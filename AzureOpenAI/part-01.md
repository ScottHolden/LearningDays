# Part 1 - Taking Azure OpenAI for a spin
## Overview
In this part we will cover off the basics of Azure OpenAI so you are able to run pilots and proof of concept projects using Azure OpenAI.

Each steps contain the following sections:
- **Aim**: The goal of the step, will instruct what to do but not how
- **Questions**: A set of follow-up questions to self-test your knowledge
- **Resources**: Documentation links to help guide you if you get stuck
- **Stretch**: A secondary goal to either push yourself, or revisit later

## Step 1 - Resource Creation
### Aim
Deploy an Azure OpenAI resource, and navigate to the **Azure OpenAI Studio**. You should have the studio open with the resource you deployed selected.
### Questions
- Does an Azure OpenAI resource require a unique name? Why?
- What network connectivity restrictions does Azure OpenAI support?
### Resources
- https://learn.microsoft.com/en-us/azure/ai-services/openai/overview
- https://learn.microsoft.com/en-us/azure/ai-services/openai/how-to/create-resource
### Stretch
Create an ARM/Bicep/Terraform template to deploy an Azure OpenAI resource, **ensuring it has a uniquely named endpoint**.

## Step 2 - Model Deployment
### Aim
Deploy a chat completion and embedding model, giving them recognizable names. Make note of the controls and options that can be given to an individual deployment.

### Questions
- Can a single model be used by 2 or more deployments? Why would this be useful if available?
- What model versioning options are there?
- In what scenarios would you choose not to use Dynamic Quota?
- Does TPM (Tokens per minute) and RPM (requests per minute) scale independently?
- Would the deployments in one Azure OpenAI resource affect available quota in another resource?
- Where would you find a model's training data date?

### Resources
- https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/models
- https://learn.microsoft.com/en-us/azure/ai-services/openai/how-to/create-resource#deploy-a-model

### Stretch
Add model deployment to your template from step 1, deploying both a chat completion model and embedding model.

## Step 3 - Chat Playground - Basic Prompt Engineering
### Aim
Using the chat playground, configure a prompt for each of the following scenarios:

- When a user enters a problem, suggest a next action
- When a message is entered, return a JSON object containing the sentiment
- When given a list of items, identify a common theme
- When given a JSON object containing full name and recent purchases, generate a unique welcome message
- When given lines of code, detect any potential issues

Test each with relevant input, and see if you can spot any issues with your prompts. Explore the configuration parameters on the right hand side, and Examples on the left.

### Questions
- Was there a type of prompt, or particular wording that gave better results?
- How did the Temperature/Top P affect the responses?
- What happened when Max Response was shorter than the generated content?
- How did examples change the response?
- Does using the same prompt and message give the same response? Why/why not?

### Resources
- https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/prompt-engineering
- https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/advanced-prompt-engineering
- https://learn.microsoft.com/en-us/azure/ai-services/openai/chatgpt-quickstart?pivots=programming-language-studio

### Stretch
Design a prompt that gathers multiple pieces of data (eg Flight details, to, from, date, time, class, luggage, etc.), prompting the user for additional details as required, and then returning the gathered data in a structured format (JSON/YAML/XML).

## Step 4 - Chat Playground - Basic Prompt Injection
### Aim
Using a few of the prompts you designed in step 3, attempt to break them in the following ways:
- Answering things unrelated to the prompt topic
- Answering in a different manner (poem, song)
- Answering in a different language
- Answering as code instead of natural language and vice-versa
- Answering with details of the original prompt

Update each prompt as you break it, and try to prevent unwanted behavior. 

### Questions
- What approaches were easiest to get the model to talk in an undesired manner?
- What other approaches could be considered for prompt injection?
- Besides talking in poems, what are potential issues that prompt injection could lead to?
- Is there any difference when the prompt injection "payload" is in the system prompt vs user message?

### Resources
- https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/red-teaming
- https://learn.microsoft.com/en-us/legal/cognitive-services/openai/overview#mitigate

### Stretch
Configure a Content Filter with prompt injection filtering, does this change the results from above?

## Step 5 - BYO Data - Preparation
### Aim
Deploy an Azure AI Search and Azure Storage account to use for step 6. Once deployed you should have access to both resources (able to navigate to the Indexes tab in the Azure AI Search instance, and the Containers tab in the Azure Storage Account).

Don't worry about uploading any files just yet!

### Questions
- When would we need to move up/down a tier in Azure AI Search?
- Why would we need a Storage Account as well as Azure AI Search?
- What methods are there to get data into Azure AI Search?
- When would you need multiple indexes? Why?

### Resources
- https://learn.microsoft.com/en-us/azure/search/search-create-service-portal
- https://learn.microsoft.com/en-us/azure/storage/common/storage-account-create

### Stretch
Add the Azure AI Search and Azure Storage resources to your template from step 1 & 2

## Step 6 - BYO Data - Ingestion & Usage
### Aim
Ingest either a set of files or a webpage into Azure AI Search using the Azure OpenAI Studio BYO data option. If you don't have any data handy, try writing a prompt to generate some for you!

Configure the chat to only respond with data from the files/page you ingested.

### Questions
- What options were there to import data using the Azure OpenAI Studio?
- How else could we import data to use here?
- What search options were available when creating the index? How did these differ?
- How did the system message affect the response?
- What effect did Top K and strictness have?

### Resources
- https://learn.microsoft.com/en-us/azure/ai-services/openai/use-your-data-quickstart?pivots=programming-language-studio
- https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/use-your-data?tabs=ai-search

### Stretch
Create an index and ingest data directly within Azure AI Search, and use it within Azure OpenAI Studio.

## Step 7 - Chat Accelerator
### Aim
Deploy a prompt you have designed (optionally with BYO data turned on) into a WebApp. Explore the user experience, and try updating it with a new prompt.

### Questions
- What resources were deployed as part of the accelerator?
- How is the configuration passed though to the WebApp?
- Where is the code pulled from?
- What happens to the chat history if you refresh the page? What about restarting the WebApp? What options are there?

### Resources
- https://learn.microsoft.com/en-us/azure/ai-services/openai/chatgpt-quickstart#deploy-your-model
- https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/use-your-data?tabs=ai-search#using-the-web-app
- https://aka.ms/AOAICodeSamples

### Stretch
Modify the logo or site theming either directly on the WebApp, or locally then push to the WebApp.

## Step 8 - Using SDK's (optional)
### Aim
Using the language of your choice, invoke a chat completion using a system message/prompt, user message, and display the output to the screen.

You can also try using the REST API directly if no SDK is available.

Configure the chat competition call to use the BYO data extension pointing at the index you created in step 6.

### Questions
- How does the SDK call differ from the options in Azure OpenAI Studio?
- What differentiates a system message from user message?
- What message role is a response from the model?
- How could the Azure OpenAI SDK be used inside an existing project?
- Can a "chat completion" only be used for chat?

### Resources
- https://learn.microsoft.com/en-us/azure/ai-services/openai/chatgpt-quickstart?tabs=command-line%2Cpython&pivots=programming-language-studio#start-a-chat-session
- https://learn.microsoft.com/en-us/azure/ai-services/openai/chatgpt-quickstart?tabs=command-line%2Cpython&pivots=programming-language-csharp#create-a-sample-application
- https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/use-your-data?tabs=ai-search#using-the-api
- https://learn.microsoft.com/en-us/azure/ai-services/openai/how-to/chatgpt?tabs=python&pivots=programming-language-chat-completions
- https://learn.microsoft.com/en-us/azure/ai-services/openai/reference

### Stretch
Use the streaming option to return individual tokens in the SDK or REST API call you make.

## End of part 1!
Congratulations! You made it to the end of part 1! Take a break, and we will pick up again on part 2 shortly.