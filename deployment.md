<div style="font-size: 40px">DotAlign Cloud deployment</div>

<br />

<div style="font-size: 20px">Table of contents</div>

<!-- TOC -->

- [Introduction](#introduction)
- [Deployment](#deployment)
  - [Deploying to an ASE](#deploying-to-an-ase)
- [Registration](#registration)
- [Relevant links](#relevant-links)

<!-- /TOC -->

## Introduction
DotAlign Cloud is a solution provided by DotAlign, Inc., deployed to your Azure tenant. It is deployed via a PowerShell script that an admin with the credentials to create a new resource group and associated resources must run. Seprately, an app registration is also to be created, which an admin with MS Graph API credentials must run. Read on for more details.  

## Deployment
To deploy DotAlign Cloud to your Azure tenant, you can run the following command in a PowerShell console:

> ` .\DeployDotAlign.ps1 -appServicePlanName <AppServicePlanToUse>`

A few points to note:

1. The app service plan name specified should be a Windows based app service plan
1. The deployment should be done in the same region as the plan. So if the app service plan was created in the `East US` region, the deployment must also be done in the `East US` region.

### Deploying to an ASE
Deploying to an ASE is also possible, but please note the following important points.

The scripts attempts to deploy the Kudu url of the function and web apps. This url usually takes the form: 

> `https://<AppName>.scm.azurewebistes.net/api/zipdeploy`

However, if you are deploying to an ASE (App Service Environment), then the Kudi url will take the form:

> `https://<AppName>.scm.<ASEName>.appserviceenvironment.net/api/webdeploy` 

And, to be able to access that url, the following must be true:

- The machine running the deployment must have access to the VNET implied by the ASE
- Valid certificates must be installed for the ILB (internal load balancer) domain

## Registration
DotAlign Cloud needs an app registration to be able to access resouces like Azure Active Directory and the MS Graph API. This can be done by running the folloinwg command:

> `.\RegisterDotAlign.ps1 -uniqueId:<UniqueIdFromDeplyment> -resourceGroupName:<ResourceGroupThatWasDeployedTo>`

## Relevant links

1. Function app Kudu/SCM url is unreachable.
   https://social.msdn.microsoft.com/Forums/en-US/3e2aa429-727c-467d-b28e-9fa15e189835/azure-functions-runtime-is-unreachable-when-deployed-on-a-isolated-asp-ilb-ase?forum=AzureFunctions

2. Network configuration for ASE.
   https://docs.microsoft.com/en-us/azure/app-service/environment/network-info#functions-and-web-jobs

{% include footer.html %}