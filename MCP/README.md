# Snippet to connect an MCP Server in the settings of a VSCode .Code-Workspace file

```json
{
  "folders": [
    {
      "path": "..",
      "name": "root"
    }
  ],
  "settings": {
    "debug.internalConsoleOptions": "neverOpen",
    "debugpy.debugJustMyCode": false,
    "mcp": {
      "servers": {
        "MCP Server Name": {
          "url": "https://host.com/mcp/sse",
          "headers": {
            "Ocp-Apim-Subscription-Key": "${input:apim-key}"
          }
        }
      },
      "inputs": [
        {
          "id": "apim-key",
          "type": "promptString",
          "description": "Enter your APIM subscription key",
          "password": true
        }
      ]
    }
  }
}
```
