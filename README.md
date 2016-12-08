# Deploying an App Service Environment with Web/API/Function Apps
This template create an Azure Virtual Network and deploys an App Service Environment in it.
On the App Service Environment it deploys a Web App, API App, and Function App.

To full secure the environment you need to add Network Security Groups to the subnet in which the App Service Environment is deployed, as explained in [How To Control Inbound Traffic to an App Service Environment](https://docs.microsoft.com/en-in/azure/app-service-web/app-service-app-service-environment-control-inbound-traffic)

## Things to note
You need to specify the region to deploy to, separate from specifying the location of the resource group.
This is due to the fact that the App Service ARM API doesn't understand the return value of `resourceGroups().location`.
You should pick the same location as the location of the resource group you create(d).



## Code of Conduct
This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/). For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.