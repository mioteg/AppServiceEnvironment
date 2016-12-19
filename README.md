# Deploying an App Service Environment with Web/API/Function Apps
<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmioteg%2FAppServiceEnvironment%2FInProgress%2Fazuredeploy.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>
<a href="http://armviz.io/#/?load=https%3A%2F%2Fraw.githubusercontent.com%2Fmioteg%2FAppServiceEnvironment%2FInProgress%2Fazuredeploy.json" target="_blank">
    <img src="http://armviz.io/visualizebutton.png"/>
</a>

This template create an Azure Virtual Network and deploys an App Service Environment in it.
On the App Service Environment it deploys a Web App, API App, and Function App.

To full secure the environment you need to add Network Security Groups to the subnet in which the App Service Environment is deployed, as explained in [How To Control Inbound Traffic to an App Service Environment](https://docs.microsoft.com/en-in/azure/app-service-web/app-service-app-service-environment-control-inbound-traffic)

## Things to note
You need to specify the region to deploy to, separate from specifying the location of the resource group.
This is due to the fact that the App Service ARM API doesn't understand the return value of `resourceGroups().location`.
You should pick the same location as the location of the resource group you create(d).

An App Service Environment has four pools with compute nodes. Three worker pools doing the actual work, and a frontend pool to receive and distribute requests to the right worker pool. The worker pools can be differentiated in size, so you can ensure the right size for applications, and the ability to scale worker pools separately. To create the App Service Environment, you have to specify the size of each pool by setting an instance size and the number of instances. In this example the number of instances for worker pools 2 and 3 is set to 0, so only worker pool 1 is really active.
To run an application, you put it in an App Service Plan and you deploy a specific App Service Plan to nodes to a specific worker pool. In this example, all apps run on the same App Service Plan, and hence in the same worker pool. The App Service Plan is configured to run on one node, whereas two are available.

If you deploy from the Azure Portal, you may encounter an error with the following message:

`Value "[storage account name]/Microsoft.Authorization/[function app name]" used in property "name" of resource "[storage account name]/Microsoft.Authorization/[function app name]" (microsoft.storageaccount/providers/locls) is invalid. You may need to edit the template to fix this issue. The resource name "[storage account name]/Microsoft.Authorization/[function app name]" or part of the name is a trademarked or reserved word. (Code: ClientSideValidationError)`

Despite this message, the template deploys and creates the lock which from this message you would assume it can't create.

Deployment of an App Service Environment can take up to two hours.

## Parameters

### General parameters
|Parameter|Description|
|---------|-----------|
|location|The Azure region to deploy to. Match this to the Resource Group location, so that any resources deploy based on `resourceGroups().location` are deployed in the same region.

### Networking Parameters
|Parameter|Description|
|---------|-----------|
|vnetName|Name of the VNET to create and deploy the App Service Environment in. Should be unique within your subscription.|
|vnetAddressSpace|Address space of the VNET to create. The default is 10.0.0.0/16. If you intend to connect this VNET to another VNET and/or on-premises network, this address space needs to be unique.|
|defaultSubnetName|Name of the default subnet to create. The default is "default".|
|defaultSubnetAddressSpace|Address space of the default subnet. The default is 10.0.0.0/24.|
|aseSubnetName|Name of the subnet to create for the App Service Environment. The default is "aseSubnet".|
|aseSubnetAddressSpace|Address space of the subnet to create for the App Service Environment. The default is 10.0.1.0/24.|
|ipAddressCount|The number of IP Addresses to create for the environment. The default is 1.|

### App Service Environment Parameters
|Parameter|Description|
|---------|-----------|
|aseName|Name of the App Service Environment to create. This value needs to be unique within a region.|
|frontendSize|Instance size to use for the Front-end Pool. This can be one of the following values: Medium, Large, ExtraLarge, Standard_D2, Standard_D3, Standard_D4, Standard_D1_V2, Standard_D2_V2, Standard_D3_V2, Standard_D4_V2. The default is "Medium".
|frontendCount|Number of instances to deploy to the Front-end Pool. The default value is 2.|
|VmSizeWorkerPool1|Instance size to use for Worker Pool 1. This can be one of the following values: Medium, Large, ExtraLarge, Standard_D2, Standard_D3, Standard_D4, Standard_D1_V2, Standard_D2_V2, Standard_D3_V2, Standard_D4_V2. The default is "Medium".
|VmCountWorkerPool1|Number of instances to deploy to Worker Pool 1. The default is 2.|
|VmSizeWorkerPool2|Instance size to use for Worker Pool 2. This can be one of the following values: Medium, Large, ExtraLarge, Standard_D2, Standard_D3, Standard_D4, Standard_D1_V2, Standard_D2_V2, Standard_D3_V2, Standard_D4_V2. The default is "Large".
|VmCountWorkerPool2|Number of instances to deploy to Worker Pool 2. The default is 0.|
|VmSizeWorkerPool3|Instance size to use for Worker Pool 3. This can be one of the following values: Medium, Large, ExtraLarge, Standard_D2, Standard_D3, Standard_D4, Standard_D1_V2, Standard_D2_V2, Standard_D3_V2, Standard_D4_V2. The default is "ExtraLarge".
|VmCountWorkerPool3|Number of instances to deploy to Worker Pool 3. The default is 0.|

### App Service Plan Parameters
|Parameter|Description|
|---------|-----------|
|appServicePlanName|The name of the App Service plan to create on the environment for hosting the apps.  This value needs to be unique within your subscription.|
|planWorkerPool|Defines which worker pool's (WP1, WP2 or WP3) resources will be used for the app service plan. The default is WP1."
|planNumberOfWorkers|Defines the number of workers from the worker pool that will be used by the app service plan. The default is 1.

### App Parameters
|Parameter|Description|
|---------|-----------|
|websiteName|The name of the Web App to create. This value needs to be unique within your subscription.|
|apiAppName|The name of the API App to create. This value needs to be unique within your subscription.|
|functionAppName|The name of the Function App to create. This value needs to be unique within your subscription.|

## Code of Conduct
This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/). For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.
