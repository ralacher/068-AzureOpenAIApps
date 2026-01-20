# Challenge 00 - Prerequisites - Ready, Set, GO!

**[Home](../README.md)** - [Next Challenge >](./Challenge-01.md)

## Introduction

Thank you for participating in the Azure Open AI Apps What The Hack. Before you can hack, you will need to set up some prerequisites.

## Description

In this challenge, you will setup the necessary pre-requisites and environment to complete the rest of the hack, including:

- [Access Azure Subscription](#access-azure-subscription)
- [Setup Development Environment](#setup-development-environment)
  - [Use GitHub Codespaces](#use-github-codespaces)
- [Setup Citrus Bus Application](#setup-citrus-bus-application)
  - [Deploy Azure Resources](#deploy-azure-resources)
  - [Setup App Backend and Frontend](#setup-app-backend-and-frontend)
    - [Setup App Backend](#setup-app-backend)
    - [Setup App Frontend](#setup-app-frontend)

### Access Azure Subscription 

You will need an Azure subscription to complete this hack. If you don't have one, get a free trial here...
- [Azure Subscription](https://azure.microsoft.com/en-us/free/)

### Setup Development Environment 

You will need a set of developer tools to work with the sample application for this hack. 

You can use GitHub Codespaces where we have a pre-configured development environment set up and ready to go for you, or you can setup the developer tools on your local workstation.

A GitHub Codespace is a development environment that is hosted in the cloud that you access via a browser. All of the pre-requisite developer tools for this hack are pre-installed and available in the codespace.

- [Use GitHub Codespaces](#use-github-codespaces)

**NOTE:** We highly recommend using GitHub Codespaces to make it easier to complete this hack.

#### Use Github Codespaces

You must have a GitHub account to use GitHub Codespaces. If you do not have a GitHub account, you can [Sign Up Here](https://github.com/signup).

GitHub Codespaces is available for developers in every organization. All personal GitHub.com accounts include a monthly quota of free usage each month. GitHub will provide users in the Free plan 120 core hours, or 60 hours of run time on a 2 core codespace, plus 15 GB of storage each month.

You can see your balance of available codespace hours on the [GitHub billing page](https://github.com/settings/billing/summary).

The GitHub Codespace for this hack will host the developer tools, sample application code, configuration files, and other data files needed for this hack. Here are the steps you will need to follow:

- A GitHub repo containing the student resources and Codespace for this hack is hosted here:
  - [WTH Azure OpenAI Apps Codespace Repo](https://github.com/codespaces/new?hide_repo_select=true&ref=main&repo=1137523312&skip_quickstart=true&machine=basicLinux32gb&devcontainer_path=.devcontainer%2Fdevcontainer.json&geo=UsEast)
  - Please open this link and sign in with your personal Github account. 

**NOTE:** Make sure you do not sign in with your enterprise managed Github account.

Once you are signed in:
- Verify that the `Dev container configuration` drop down is set to `068-AzureOpenAIApps`
- Click on the green "Create Codespace" button.

Your Codespace environment should load in a new browser tab. It will take approximately 3-5 minutes the first time you create the codespace for it to load.

- When the codespace completes loading, you should find an instance of Visual Studio Code running in your browser with the files needed for this hackathon.

Your developer environment is ready, hooray! Skip to section: [Deploy Azure Resources](#deploy-azure-resources)

**NOTE:** If you close your Codespace window, or need to return to it later, you can go to [GitHub Codespaces](https://github.com/codespaces) and you should find your existing Codespaces listed with a link to re-launch it.

**NOTE:** GitHub Codespaces time out after 20 minutes if you are not actively interacting with it in the browser. If your codespace times out, you can restart it and the developer environment and its files will return with its state intact within seconds. If you want to have a better experience, you can also update the default timeout value in your personal setting page on Github. Refer to this page for instructions: [Default-Timeout-Period](https://docs.github.com/en/codespaces/setting-your-user-preferences/setting-your-timeout-period-for-github-codespaces#setting-your-default-timeout-period) 

**NOTE:** Codespaces expire after 30 days unless you extend the expiration date. When a Codespace expires, the state of all files in it will be lost.

### Setup Citrus Bus Application

Contoso Yachts, Inc has developed the Citrus Bus application in-house. The Citrus Bus application supports the company's travel agents with virtual assistants that answer questions about the Contoso Islands and can book yacht reservations. The application also supports Contoso Island educators with advanced AI services that help them grade student tests.

The Citrus Bus application was developed using Python and Typescript/Javascript, and implements a RAG architecture with Azure OpenAI, Azure AI Search, and other Azure AI platform services. These services, as well as frameworks such as Langchain, have support for both Typescript and Python.

There are three major steps to setup the Sample Application:
- [Deploy Azure Resources](#deploy-azure-resources)
- [Setup App Backend](#setup-app-backend)
- [Setup App Frontend](#setup-app-frontend)

In your codespace, or student `Resources.zip` package, you fill find the following folders containing the frontend and backend API of the sample application to help you get started:
- `/ContosoAIAppsBackend` - Contains an Azure function app that provides capabilities of processing data and interacting with Azure AI  Services like Azure OpenAI and Azure Document Intelligence.
- `/ContosoAIAppsFrontend` - Contains an Angular App that provides a user interface to some example virtual assistants.
- `/data` - Contains various artifacts and data sources that will be used by the Citrus Bus application
- `/infra` - Contains deployment script and Bicep templates to deploy Azure resources for hosting the Citrus Bus application in Azure.

The apps also contain helper utilities, functions and tools to help you speed up development as well as hints to the challenges you will be taking on.

#### Deploy Azure Resources

We have provided a deployment script and a set of Bicep templates which will deploy and configure the Azure resources required to run the sample application in the cloud. You can find these files in the `/infra` folder.

The deployment script uses the Azure PowerShell Commandlets to log into your Azure subscription. The script can be run from the terminal in your Codespace, or the terminal on your local workstation.

**NOTE:** Logging into your Azure subscription with PowerShell or the Azure CLI from a GitHub Codespace requires a Device Login Code. Some Azure subscriptions may block this method of authentication. For subscriptions where authentication with a Device Login Code is not permitted, you will need to create and use an Azure Service Principal to login from GitHub Codespaces.

##### Provisioning Azure Resources

Execute the following commands in your GitHub Codespace or local workstation terminal window:

```bash
cd infra
chmod +x deploy.sh
./deploy.sh --subscription-id "[subscription-id]" --resource-group-name "[resource-group-name]" --tenant-id "[tenant-id]" 
```

- `subscription-id`: The ID of the Azure Subscription where you want to deploy the resources
- `resource-group-name`: The name of the resource group where you want to deploy the resources. It will be created for you when you run the deployment script. 
- `tenant-id`: The Tenant ID associated with your Azure subscription where you want to deploy the resources

The deployment process takes about 30 minutes to complete.

###### Capacity Issues

At the time this hack was authored (June 2024), the Azure AI resources required for the solution are not all available in the same region. By default, the deployment script above will attempt to deploy most Azure resources in `East US 2` and the Azure Document Intelligence resource in `East US`.

If you have any errors with capacity or quota issues, expand the hidden section below for troubleshooting instructions.

<details markdown="1">
<summary markdown="span">Click to expand/collapse Troubleshoot Capacity Issues</summary>

If you have any errors with capacity or quota issues, you may need to re-deploy the solution using one or more of the optional location parameters below. Note the resource type and region that failed to deploy in any error messages, and choose a different region based on the information below.

- `location`: The Azure region where you want to deploy all resources EXCEPT Azure Open AI & Azure Document Intelligence. (Default value is `eastus2`)
- `openai-location`: The Azure region where the Azure OpenAI resource will be deployed. (Default value is `eastus2`)
- `document-intelligence-location`: The Azure region where the Azure Document Intelligence resource will be deployed. (Default value is `eastus`)

**NOTE:** The hack requires the Azure OpenAI Assistant API feature which is currently in preview and NOT available in ALL regions *where Azure OpenAI is available*!
 
As of June 2024, Azure OpenAI with the Assistant API preview feature is available in the following regions: `eastus2`, `australiaeast`, 
`francecentral`, `norwayeast`, `swedencentral`, `uksouth`, `westus`, `westus3`

This information is subject to change over time, for the most up to date list of available locations see [Azure OpenAI Service Models - Assistants (Preview) Availability](https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/models#assistants-preview)

**NOTE:** This hack uses Python to interact with the Azure Document Intelligence API. Python is supported with the `2024-02-29-preview` version of the Document Intelligence API.  The `2024-02-29-preview` version of the API is currently NOT available in ALL regions *where Azure Document Intelligence is available*!

As of June 2024, Azure Document Intelligence with support for API version `2024-02-29-preview` (with Python support) is available in the following regions: `eastus`, `westus2`, `westeurope`

This information is subject to change over time, for the most up to date list of available locations see [What is Azure AI Document Intelligence? - API `2024-02-29-preview` Availability](https://learn.microsoft.com/en-us/azure/ai-services/document-intelligence/overview?view=doc-intel-4.0.0)

</details>

#### Setup App Backend and Frontend

Now that the Azure resources to support the hack's sample application have been deployed, you will setup and run both the backend and frontend components in your development environment.  These components will run in your development environment (GitHub Codespace OR local workstation) throughout the duration of the hack as you work on the challenges.

Here are some things to know about the developer environment:
- Once you start each component, it will hold the Terminal session while the component is running.
- Therefore, each component will require its own Terminal session in VS Code.
- If you are using GitHub Codespaces and the Codespace times out while you are hacking, you will need to re-run the steps here to restart each component.

##### Setup App Backend

The sample application's Backend is implemented as an Azure Function. Azure Functions can be run in a local developer environment for testing before eventual deployment into Azure.  For this hack, you will run the application's Backend function code in your developer environment without publishing it to Azure.

In a Terminal session in VSCode, navigate to the `/ContosoAIAppsBackend` folder and Start up the Backend function app by running the following commands:

```
cd ContosoAIAppsBackend
pip install -r requirements.txt
func start 
```

##### Setup App Frontend

The sample application's Frontend is implemented as an Angular web application. For this hack, you will only run the application's Frontend in your development environment without publishing it to Azure.  

Open another Terminal session in VScode and then navigate into the `/ContosoAIAppsFrontend` folder. Install the application dependencies and start the Frontend by running the following commands:

```
# Navigates into the folder 
cd ContosoAIAppsFrontend

# Installs the node packages required for the frontend
npm install

# Starts up the web application on your local machine
npm start
```

**NOTE:** The Frontend application is configured to connect to the Backend function's URL via a setting in the `/ContosoAIAppsFrontend/src/environments/environment.ts` file. If your Backend function app is running on a different port or machine, please update the `environment.ts` config file accordingly

Open another terminal session in VSCode so that you can continue the rest of the challenges. The terminal sessions you opened to run the Frontend and Backend should remain running in the background. 

## Success Criteria

To complete this challenge successfully, you should be able to:

- Verify that you have a bash shell with the Azure CLI available.
- Verify that you have deployed the following resources in Azure:

  - Azure OpenAI Service
  - Azure Cognitive Search
  - Two Azure Storage Accounts with Azure Blob Storage
  - Azure Cosmos DB service with databases and containers
  - Azure Service Bus with at least one queue set up
  - Azure Redis Cache Instance
  - Azure Document Intelligence Service (formerly Azure Form Recognizer)
 
Your Azure Function Backend and Front End applications should be up and running and reachable via HTTP (Browser)

You should also be able to ask Murphy and Priscilla for their name from the front-end and they should respond correctly with the correct name configured in the app's system prompts. The other assistants will not currently respond correctly and will be fixed in a later challenge.

## Learning Resources

Here are some resources that should provide you with background information and educational content on the resources you have just deployed

- [Azure OpenAI Service](https://learn.microsoft.com/en-us/azure/cognitive-services/openai/)
- [Document Intelligence Region/API Version Availability](https://learn.microsoft.com/en-us/azure/ai-services/document-intelligence/overview?view=doc-intel-4.0.0)
