# Azure API Management CI/CD examples <!-- omit in toc -->
A few workflows to help you to get started with CI/CD for Azure API Management
It mainly contains two workflows, one to extract the configuration of an existing APIM instance and another to push the configuration to another APIM instance

This work relies heavily on the great work that can be found on the [Azure API Management DevOps Resource Kit](https://github.com/Azure/azure-api-management-devops-resource-kit)

## Table of Contents  <!-- omit in toc -->
- [Repository structure](#repository-structure)
- [How to get started](#how-to-get-started)
  - [Cloning the repository](#cloning-the-repository)
  - [Get an Azure Service Principal](#get-an-azure-service-principal)
  - [Save the credentials to Github Secrets](#save-the-credentials-to-github-secrets)
  - [Configuring the workflows](#configuring-the-workflows)

# Repository structure
The repository has a really simple structure.
There's a folder with all the workflow yaml files (.github/workflows/) and by default the resulting APIM configuration files that get extracted from an existing APIM instance will be in the APIMTemplates folder. You can configure the folder editing the GetApimConfiguration.yml file in the workflows folder.

# How to get started

## Cloning the repository
You can start by cloning this repository into your own github organization. 

## Get an Azure Service Principal
After that, you will need to set up a service principal to be able to access your subscription. 
You can do that by logging into the [Azure Portal](https://portal.azure.com), access the cloud shell (the icon looks like <svg viewBox="0 0 16 16" class="" width="16px" role="presentation" fill="#fff" focusable="false" xmlns:svg="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" id="FxSymbol0-006" data-type="333"><g><title></title><path d="M15 2v12H1V2h14m1-1H0v14h16V1z"></path><path d="M12.5 12h-4c-.3 0-.5-.2-.5-.5s.2-.5.5-.5h4c.3 0 .5.2.5.5s-.2.5-.5.5zM7.8 8.1s0-.1 0 0v-.5L3.7 4.3c-.2-.2-.5-.2-.7 0-.2.3-.1.6.1.7l3.5 3-3.5 3c-.2.2-.2.5-.1.7.1.1.2.2.4.2.1 0 .2 0 .3-.1l3.9-3.3v-.1c.2-.2.2-.2.2-.3 0 .1 0 0 0 0z"></path></g></svg> ) and type

```
az ad sp create-for-rbac --name "<ServicePrincipalName>" --role contributor --scopes /subscriptions/<SubscriptionId> --sdk-auth
```

This service principal is scoped to the subscription, but you can scope it differently (you can also explore other options  [here](https://docs.microsoft.com/en-us/cli/azure/create-an-azure-service-principal-azure-cli)).

From the output, you should copy the result (and keep in mind you won't be able to access some of these details later), which should look like this

```
{
    "clientId": "00000000-0000-0000-0000-000000000000",
    "clientSecret": "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
    "subscriptionId": "00000000-0000-0000-0000-000000000000",
    "tenantId": "00000000-0000-0000-0000-000000000000",
    "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
    "resourceManagerEndpointUrl": "https://management.azure.com/",
    "activeDirectoryGraphResourceId": "https://graph.windows.net/",
    "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
    "galleryEndpointUrl": "https://gallery.azure.com/",
    "managementEndpointUrl": "https://management.core.windows.net/"
}
```

## Save the credentials to Github Secrets
Next, let's set this as a GitHub secret. On your Github repository, go to `"Settings"`, choose `"Secrets"` from the menu and press the `"New repository secret"` button.
In the Name field type `AZURECREDENTIAL` (or whatever you want to call it, but keep in mind that this repository assumes that is the name for the credential secret), and paste the result you got from the Azure CLI into the Value field. Press `"Add secret"` to save it. You won't be able to read the value later, but you can use it in workflows/actions.

## Configuring the workflows

Most likely you will need to change the workflows to work for your specific needs and as such, these workflows are meant to be used as base rather than a complete solution.

The [GetApimConfiguration](.github/workflows/GetApimConfiguration.yml) workflow has a few configurable parameters. Details can be found [here](.github/workflows/GetApimConfiguration.md).

The [DeployApimConfiguration](.github/workflows/DeployAPIMConfiguration.yml) configuration help is [here](.github/workflows/DeployApimConfiguration.md).
