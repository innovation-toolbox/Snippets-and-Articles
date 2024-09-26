# GPT to generate a mermaid graph of a flow 

GPT (from GPT4) is quite fluent in Markdown and mermaid diagrams, it's easy to ask it to generate such a graph by doing the following : 

## System Message 
```markdown
You are a Product Manager responsible for facilitating the onboarding of new developers on an existing code base repository.
Your job is to create the *mermaid* compatible diagram based on the explanations that are given to you regarding the flux between Azure Services, AI Models and Azure Dependancies.
```

### User Message : 

```markdown
I have a Python Notebook that is responsible for reading the content of a file in a folder and loops upon each file. 
The notebook asks a Society of Mind composed of two agents to extract the main informations out of the source code file : 
- A first agent (Software Architect) to extract the coding language used in the source code file, as well as the specificities of the language (object oriented programming, functional language, methods, dependencies, etc.) as well as the rules the second agent will need to be following for the extraction specifically.
- A second agent (Developer) will then extract a code description, code summary, business rules, mermaid flow diagram,  as well as a list of features and data flows with their technical and functional description.
To extract these informations, the agents need to work with an Azure OpenAi GPT4-o where they send their extraction requests.

Once all the files are parsed, they are batch imported in an Azure AI Search that will then be used for the documentation generation process
```

### Assistant Message : 

```markdown
Great, thank you for the detailed explanation. Based on your description, the flow involves several components and interactions between them. Here's a high-level overview of the process, which we'll convert into a mermaid diagram:

Python Notebook reads the content of files in a folder and loops through each file.
Society of Mind involves two agents:
Software Architect Agent: Extracts the coding language and specificities of the language.
Developer Agent: Extracts a code description, code summary, business rules, mermaid flow diagram, features, and data flows.
Both agents send extraction requests to Azure OpenAI GPT-4.
Once all files are parsed, the results are batch imported into Azure AI Search for documentation generation.

Here's how you can represent this workflow in a mermaid diagram:
    ```mermaid
    graph TD  
        A[Python Notebook] -->|Reads files| B[Society of Mind]  
        B -->|Extract info| C[Software Architect Agent]  
        B -->|Extract info| D[Developer Agent]  
        C -->|Send request| E[Azure OpenAI GPT-4]  
        D -->|Send request| E[Azure OpenAI GPT-4]  
        E -->|Return info| C  
        E -->|Return info| D  
        C -->|Provide rules| D  
        D -->|Gathered info| F[Batch Import]  
        F -->|Import data| G[Azure AI Search]  
    ```
In this diagram:

A represents the Python Notebook that initiates the process.
B represents the Society of Mind, which includes two agents.
C and D represent the Software Architect Agent and Developer Agent, respectively.
E represents Azure OpenAI GPT-4, which processes the requests from the agents.
F represents the batch import process.
G represents Azure AI Search, where the final data is stored for documentation generation.

If you have any further details or specific interactions that need to be included, please let me know!

```

With further details in the following chat : 
```markdown 
Can you please add the details around the kind of information that are manipulated by the software architect and developer ?

[...]

Also, the dev and the software architect can discuss between each other until the dev extracted all the software architect feels is relevant

[...]

Perfect, can you make it a sequence diagram ? 

```

### Assistant answer : 

```markdown
Certainly! 
A sequence diagram will illustrate the interactions between the components over time. Below is the mermaid sequence diagram for the described process:\n\n

```mermaid
\nsequenceDiagram\n    participant Notebook as Python Notebook\n    participant Society as Society of Mind\n    participant Architect as Software Architect Agent\n    participant Developer as Developer Agent\n    participant GPT4 as Azure OpenAI GPT-4\n    participant Import as Batch Import\n    participant AI_Search as Azure AI Search\n\n    Notebook->>Society: Reads files\n    Society->>Architect: Extract coding language, specificities, and rules\n    Architect->>GPT4: Send extraction request\n    GPT4-->>Architect: Return extracted information\n\n    Architect->>Developer: Provide rules and language specifics\n    Developer->>GPT4: Send extraction request\n    GPT4-->>Developer: Return extracted information\n\n    loop Until all relevant information is extracted\n        Developer->>Architect: Discuss extracted information\n        Architect-->>Developer: Provide additional rules/feedback\n        Developer->>GPT4: Send additional extraction request\n        GPT4-->>Developer: Return additional extracted information\n    end\n\n    Developer->>Import: Provide extracted information\n    Import->>AI_Search: Batch import data\n\n    Note over AI_Search: Data is now available for documentation generation\n```\n\n### Breakdown:\n- **Python Notebook (Notebook)**: Initiates the process by reading files and invoking the Society of Mind.\n- **Society of Mind (Society)**: Manages the interaction between the Software Architect Agent and Developer Agent.\n- **Software Architect Agent (Architect)**: \n  - Extracts coding language, specificities, and rules.\n  - Sends extraction requests to Azure OpenAI GPT-4 and receives extracted information.\n  - Provides rules and specifics to the Developer Agent.\n- **Developer Agent (Developer)**:\n  - Extracts code description, summary, business rules, mermaid flow diagram, features, and data flows.\n  - Sends extraction requests to Azure OpenAI GPT-4 and receives extracted information.\n  - Engages in iterative discussions with the Software Architect Agent until all relevant information is extracted.\n- **Azure OpenAI GPT-4 (GPT4)**: Processes extraction requests and returns the necessary information.\n- **Batch Import (Import)**: Collects and batch imports the extracted information.\n- **Azure AI Search (AI_Search)**: Stores the final data for documentation generation.\n\nThis sequence diagram represents the temporal flow of interactions between the different components in your process. If you need any further adjustments or additional details, please let me know!"
```

### User Message 
```markdown
Can you remove the Batch Import part and integrate it directly in an interaction between the society of mind and azure ai search ? 
Also, i want the developer to return to the society of mind the result of the extraction after the discussions with the software architect before it is sent to Azure Search
```

### Assistant Message 
```markdown 
Certainly! I'll update the sequence diagram to reflect the changes: removing the Batch Import part and integrating the interaction directly with Azure AI Search, as well as ensuring the Developer returns the result to the Society of Mind before sending it to Azure AI Search.\n\n```mermaid\nsequenceDiagram\n    participant Notebook as Python Notebook\n    participant Society as Society of Mind\n    participant Architect as Software Architect Agent\n    participant Developer as Developer Agent\n    participant GPT4 as Azure OpenAI GPT-4\n    participant AI_Search as Azure AI Search\n\n    Notebook->>Society: Reads files\n    Society->>Architect: Extract coding language, specificities, and rules\n    Architect->>GPT4: Send extraction request\n    GPT4-->>Architect: Return extracted information\n\n    Architect->>Developer: Provide rules and language specifics\n    Developer->>GPT4: Send extraction request\n    GPT4-->>Developer: Return extracted information\n\n    loop Until all relevant information is extracted\n        Developer->>Architect: Discuss extracted information\n        Architect-->>Developer: Provide additional rules/feedback\n        Developer->>GPT4: Send additional extraction request\n        GPT4-->>Developer: Return additional extracted information\n    end\n\n    Developer->>Society: Return extracted information\n    Society->>AI_Search: Send extracted information for documentation generation\n\n    Note over AI_Search: Data is now available for documentation generation\n```\n\n### Breakdown:\n- **Python Notebook (Notebook)**: Initiates the process by reading files and invoking the Society of Mind.\n- **Society of Mind (Society)**: Manages the interaction between the Software Architect Agent and Developer Agent.\n- **Software Architect Agent (Architect)**: \n  - Extracts coding language, specificities, and rules.\n  - Sends extraction requests to Azure OpenAI GPT-4 and receives extracted information.\n  - Provides rules and specifics to the Developer Agent.\n- **Developer Agent (Developer)**:\n  - Extracts code description, summary, business rules, mermaid flow diagram, features, and data flows.\n  - Sends extraction requests to Azure OpenAI GPT-4 and receives extracted information.\n  - Engages in iterative discussions with the Software Architect Agent until all relevant information is extracted.\n  - Returns the extracted information to the Society of Mind.\n- **Azure OpenAI GPT-4 (GPT4)**: Processes extraction requests and returns the necessary information.\n- **Azure AI Search (AI_Search)**: Receives the final extracted information from the Society of Mind for documentation generation.\n\nThis updated sequence diagram reflects the new interactions and should align with your requirements. If there are any further adjustments or additional details needed, please let me know!
```

### User message : 

```markdown
Can you remove the Azure OpenAI Gpt 4 agent and their interaction with the developer and software architect to simplify the diagram ? 
```
### Assistant message : 

```markdown
Sure! I'll simplify the sequence diagram by removing the Azure OpenAI GPT-4 agent and its interactions. This will focus the diagram on the interactions between the Python Notebook, Society of Mind, Software Architect Agent, Developer Agent, and Azure AI Search.


sequenceDiagram  
    participant Notebook as Python Notebook  
    participant Society as Society of Mind  
    participant Architect as Software Architect Agent  
    participant Developer as Developer Agent  
    participant AI_Search as Azure AI Search  
  
    Notebook->>Society: Reads files  
    Society->>Architect: Extract coding language, specificities, and rules  
    Architect->>Developer: Provide rules and language specifics  
  
    loop Until all relevant information is extracted  
        Developer->>Architect: Discuss extracted information  
        Architect-->>Developer: Provide additional rules/feedback  
    end  
  
    Developer->>Society: Return extracted information  
    Society->>AI_Search: Send extracted information for documentation generation  
  
    Note over AI_Search: Data is now available for documentation generation  
 

Breakdown:
Python Notebook (Notebook): Initiates the process by reading files and invoking the Society of Mind.
Society of Mind (Society): Manages the interaction between the Software Architect Agent and Developer Agent.
Software Architect Agent (Architect):
Extracts coding language, specificities, and rules.
Provides rules and specifics to the Developer Agent.
Developer Agent (Developer):
Extracts code description, summary, business rules, mermaid flow diagram, features, and data flows.
Engages in iterative discussions with the Software Architect Agent until all relevant information is extracted.
Returns the extracted information to the Society of Mind.
Azure AI Search (AI_Search): Receives the final extracted information from the Society of Mind for documentation generation.

This simplified sequence diagram focuses on the core interactions and processes. If there are any further adjustments or additional details needed, please let me know!


```

### User Message : 

```markdown
That last one was perfect, please reset the app context now for the following and generate another mermaid diagram : 
I have a Python Notebook responsible for the orchestration of a society of mind made of 2 different types of writers for summary writing and feature detailed writing. Both will discuss with a Style Critic.
All these agents will interact (Summary writer to style critic and feature writer to style critic) with each other until reaching a consensus on the chapter to write. 

Can you take the following code to draw a simplified workflow like the one above focusing on : Python Notebook Orchestration, Summary Writer Agent (Gpt-4o), Feature Writer (Gpt-4o), Society of Mind (Gpt-4o), Style Critic (Gpt-4o) and Azure Search as an index used for the source code content to summarize.
Python Code below : 
```

```python
# Load Data from Index : 
__AZURE_SEARCH_INDEX_SOURCECODE_NAME = os.getenv("AZURE_SEARCH_INDEX_SOURCECODE_NAME", "documentor-source")
__AZURE_SEARCH_SERVICE_NAME = os.getenv("AZURE_SEARCH_SERVICE_NAME")
__API_KEY = os.getenv("AZURE_SEARCH_SERVICE_KEY")

azure_search_client = AzureSearchIndexClient(
    azure_search_service_name=__AZURE_SEARCH_SERVICE_NAME,
    index_name=__AZURE_SEARCH_INDEX_SOURCECODE_NAME,
    api_key=__API_KEY)

azure_search_client.load_data()

code_summaries = azure_search_client.get_code_summaries()
code_descriptions = azure_search_client.get_code_descriptions()

# Writing the global description : 
_ = AoaiClients()

output_records: OutputRecords = OutputRecords()

// get the user prompt from the Writer Class
task = SummaryWriterAgentPrompts._SummaryWriterAgentPrompts__USER_PROMPT

try:
    message = task.format(
        app_name=app_name,
        context=json.dumps(code_summaries),
    )
    
    // The actual Autogen chat process with the society of mind agent
    user_proxy.initiate_chat(
        summary_society_of_mind_agent,
        message=message,
        cache=None
    )
    
    //Retrieving the Json output from the society of mind agent
    output = user_proxy.last_message()['content']

    // Clearing History for the next iteration
    user_proxy.clear_history()
except Exception as e:
    print(
        f"Error while retrieving an answer from agents for the file : {file.name}")
    // resetting output to None to force the retry of the agentic process
    output = None

# Loop on the features and generate content : 

// load json as object
result = json.loads(json_features)
features = result['functionalities']

codefiles = dict((file["code_file"], file["code_description"]) for file in code_descriptions)

// Define a function to generate the documentation for a chapter
def generate_documentation(chapter):
    print("start generation for chapter: " + chapter["title"])
    code_description = ""
    // loop on files to build the prompt
    for file in chapter["files"]:
        if codefiles.get(file):
            code_description += """
                FILENAME: {file}
                DESCRIPTION: 
                {description}
                ========================================
            """.format(file=file, description=codefiles[file])
            
    // sometimes the model gives bad reference and the code_description is empty
    if code_description == "":
        print("No code description found for chapter: " + chapter["title"])
        return chapter['title'], ""
    
    // get the user prompt from the Writer Class
    task = FeatureWriterAgentPrompts._FeatureWriterAgentPrompts__USER_PROMPT

    // generate the documentation for the chapter
    try:
        message = task.format(
            functionality=chapter["title"],
            code_descriptions=code_description,
        )
        
        // The actual Autogen chat process with the society of mind agent
        user_proxy.initiate_chat(
            feature_society_of_mind_agent,
            message=message,
            cache=None
        )
        
        // Retrieving the Json output from the society of mind agent
        output = user_proxy.last_message()['content']

        // Clearing History for the next iteration
        user_proxy.clear_history()
    except Exception as e:
        print(
            f"Error {json.dumps(e)}")
        # resetting output to None to force the retry of the agentic process
        output = None


    
    # return the title and the content
    return chapter['title'], output

# Split chapters into batches of 4 to parrallelize the generation
# without triggering the TPM limit of the API
batch_size = 2
batches = [features[i:i+batch_size] for i in range(0, len(features), batch_size)]

# Create an executor for parallel execution
with concurrent.futures.ThreadPoolExecutor() as executor:
    # Iterate over batches and submit tasks for parallel execution
    futures = []
    for batch in batches:
        for chapter in batch:
            future = executor.submit(generate_documentation, chapter)
            futures.append(future)

    # Wait for all tasks to complete and get the results in the original order
    results = []
    for future in concurrent.futures.as_completed(futures):
        results.append(future.result())

# Append the chapters to the app_name.md file in the original order
with open(filepath, 'a') as file:
    for title, content in results:
        file.write("
# {title}
".format(title=title))
        file.write(content)
        print(title + " documentation generated & added to the document")

```

```markdown
Certainly! Based on the provided Python code and the described interactions, here's a mermaid sequence diagram that focuses on the key components: Python Notebook Orchestration, Summary Writer Agent, Feature Writer Agent, Society of Mind, Style Critic, and Azure Search.

    ```mermaid
    sequenceDiagram
        participant Notebook as Python Notebook
        participant Society as Society of Mind
        participant SummaryWriter as Summary Writer Agent (Gpt-4o)
        participant FeatureWriter as Feature Writer Agent (Gpt-4o)
        participant StyleCritic as Style Critic (Gpt-4o)
        participant AI_Search as Azure Search

        Notebook->>AI_Search: Load data from Azure Search Index
        AI_Search-->>Notebook: Return code summaries and descriptions

        Notebook->>Society: Initialize Society of Mind
        Society->>SummaryWriter: Task for summary writing
        SummaryWriter->>StyleCritic: Discuss summary style
        StyleCritic-->>SummaryWriter: Provide feedback

        loop Until consensus on summary
            SummaryWriter->>StyleCritic: Discuss summary style
            StyleCritic-->>SummaryWriter: Provide feedback
        end

        SummaryWriter->>Society: Return agreed summary

        loop Until all chapters are generated
            Notebook->>Society: Task for feature writing
            Society->>FeatureWriter: Task for detailed feature writing
            FeatureWriter->>StyleCritic: Discuss feature style
            StyleCritic-->>FeatureWriter: Provide feedback

            loop Until consensus on feature writing
                FeatureWriter->>StyleCritic: Discuss feature style
                StyleCritic-->>FeatureWriter: Provide feedback
            end

            FeatureWriter->>Society: Return agreed feature writing
        end

        Society->>Notebook: Return final documentation content

        Note over Notebook: Documentation is saved to file
    ```
Breakdown:
- **Python Notebook (Notebook)**:
  - Orchestrates the entire process.
  - Loads data from Azure Search Index.
  - Initializes the Society of Mind.
  - Tasks the Society of Mind for both summary and feature writing.
- **Azure Search (AI_Search)**:
  - Provides code summaries and descriptions to the Python Notebook.
- **Society of Mind (Society)**:
  - Manages interactions between the Summary Writer Agent, Feature Writer Agent, and Style Critic.
- **Summary Writer Agent (SummaryWriter)**:
  - Writes summaries.
  - Engages in discussions with the Style Critic to refine the summary style until a consensus is reached.
- **Feature Writer Agent (FeatureWriter)**:
  - Writes detailed features.
  - Engages in discussions with the Style Critic to refine the feature writing style until a consensus is reached.
- **Style Critic (StyleCritic)**:
  - Provides feedback on both summary and feature writing styles.
  - Engages in iterative discussions with both writers until a consensus is reached.
- **Documentation Generation**:
  - The final agreed content is returned to the Python Notebook and saved to a file.

This sequence diagram provides a clear overview of the orchestration and interactions between the different agents involved in the summary and feature writing process. If there are any further adjustments or additional details needed, please let me know!"
```