# Custom Policy to extract the Content Filter results from an Azure OpenAI prompt and inference results 

```xml
 <!--
    IMPORTANT:
    - Policy elements can appear only within the <inbound>, <outbound>, <backend> section elements.
    - To apply a policy to the incoming request (before it is forwarded to the backend service), place a corresponding policy element within the <inbound> section element.
    - To apply a policy to the outgoing response (before it is sent back to the caller), place a corresponding policy element within the <outbound> section element.
    - To add a policy, place the cursor at the desired insertion point and select a policy from the sidebar.
    - To remove a policy, delete the corresponding policy statement from the policy document.
    - Position the <base> element within a section element to inherit all policies from the corresponding section element in the enclosing scope.
    - Remove the <base> element to prevent inheriting policies from the corresponding section element in the enclosing scope.
    - Policies are applied in the order of their appearance, from the top down.
    - Comments within policy elements are not supported and may disappear. Place your comments between policy elements or at a higher level scope.
-->
<policies>
    <inbound>
        <base />
    </inbound>
    <backend>
        <base />
    </backend>
    <outbound>
        <base />
        <set-header name="x-content-safety-in" exists-action="override">
            <value>@{
                var body = context.Response.Body.As<JObject>(preserveContent: true);
                var choices = body?["choices"] as JArray;
                if (choices == null || choices.Count == 0)
                {
                    return string.Empty;
                }
                var collected = new JArray();
                foreach (var choice in choices)
                {
                    var cf = choice?["content_filter_results"];
                    if (cf != null)
                    {
                        collected.Add(cf);
                    }
                }
                return collected.Count > 0 ? collected.ToString(Newtonsoft.Json.Formatting.None) : string.Empty;
            }</value>
        </set-header>
        <set-header name="x-content-safety-out" exists-action="override">
            <value>@{
                var body = context.Response.Body.As<JObject>(preserveContent: true);
                var promptFilters = body?["prompt_filter_results"] as JArray;
                if (promptFilters == null || promptFilters.Count == 0)
                {
                    return string.Empty;
                }
                var collected = new JArray();
                foreach (var pf in promptFilters)
                {
                    var cf = pf?["content_filter_results"];
                    if (cf != null)
                    {
                        collected.Add(cf);
                    }
                }
                return collected.Count > 0 ? collected.ToString(Newtonsoft.Json.Formatting.None) : string.Empty;
            }</value>
        </set-header>
    </outbound>
    <on-error>
        <base />
    </on-error>
</policies>
```

# Result Example : 

<img width="813" height="787" alt="image" src="https://github.com/user-attachments/assets/b2000169-4ef4-42e5-bb02-3db45d823ad2" />
