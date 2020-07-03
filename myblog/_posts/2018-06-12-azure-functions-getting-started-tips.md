---
layout: post
title:  "Azure Functions Getting Started Tips"
date:   2018-06-12 06:04:29 -0400
categories: azure functions
---
I'm going to give some quick tips on the different ways you can create your first Azure Function.
#### Azure Portal
The first is to go to [the azure portal](https://portal.azure.com) and sign in. Click "Create a resource" in the top left corner. Click on the Compute tab and select Function app.
![](https://jweiler.ghost.io/content/images/2018/06/2018-06-07-05_16_37-Clipboard.png)
Fill in the information and click Create.
![](https://jweiler.ghost.io/content/images/2018/06/2018-06-07-05_19_48-Function-App---Microsoft-Azure.png)
You will be directed to the dashboard with a toast giving you the status of your function app creation. When finished go to the resource and click the plus beside Functions and you can use a premade function or make your own.
![](https://jweiler.ghost.io/content/images/2018/06/2018-06-07-05_22_43-Clipboard.png)

#### Azure CLI
A function app can be created with the Azure CLI as well. To do this go to [the azure portal](https://portal.azure.com) and click on the Cloud Shell button.
![](https://jweiler.ghost.io/content/images/2018/06/2018-06-07-05_27_28-Dashboard---Microsoft-Azure.png)
The command window will pop open in the bottom of the window and you can select Powershell or Bash. The first thing to do is create a resource group like this
```
az group create --name myResourceGroup --location eastus
```
Azure functions need a general-purpose Azure Storage account, so to create one run the following command. storage_name needs to be globally unique and be 3 to 24 characters in length and contain only numbers and lowercase letters.
```
az storage account create --name <storage_name> --location westeurope --resource-group myResourceGroup --sku Standard_LRS
```

Now the next step is to create a Function app with the following code. The app_name needs to be globally unique and for storage_name use the name of the Storage account you just created.
```
az functionapp create --deployment-source-url https://github.com/Azure-Samples/functions-quickstart --resource-group myResourceGroup --consumption-plan-location eastus --name <app_name> --storage-account  <storage_name>  
```
The github repository has a few sample functions to get you started.
You can test the function by calling the following command in the Azure Cloud shell.
```
curl http://<app_name>.azurewebsites.net/api/HttpTriggerJS1?name=<yourname>
```
#### Create a function in Visual Studio
Ensure that you have downloaded Visual studio 2017 and that the Azure development workload is installed. Also install the latest [Azure Function tools](https://docs.microsoft.com/en-us/azure/azure-functions/functions-develop-vs#check-your-tools-version).

Do a file new project and select the Azure Functions from the cloud group in VS.
![](https://jweiler.ghost.io/content/images/2018/06/2018-06-12-05_09_50-New-Project.png)
Select the type of function you want to create and hit OK. Use the storage account dropdown to select or create a new storage account.
![](https://jweiler.ghost.io/content/images/2018/06/2018-06-12-05_12_27-New-Project---FunctionApp1.png)
Visual studio creates a function based on which type you selected. To test it locally just hit F5 and copy the Function Url into a browser.
![](https://jweiler.ghost.io/content/images/2018/06/2018-06-12-05_14_49-C__Program-Files_dotnet_dotnet.exe.png)
Paste the url into a browser and append the name variable like this:
```
http://localhost:7071/api/Function1?name=MyName
```
You should see Hello, MyName in the browser.
The next step is to publish it to Azure. Right click on the project in Visual Studio and select **Publish**.
![](https://jweiler.ghost.io/content/images/2018/06/2018-06-12-05_19_46-.png) When you hit publish you will need to  enter information about the function just like you would when creating it in Azure. Once the publish is complete hit the Url and you should see Hello, Name again.
#### Create a function with Java/Maven
Java for Azure Functions is still in Preview. For more details see [here](https://docs.microsoft.com/en-us/azure/azure-functions/functions-create-first-java-maven).
#### Create a function - Linux
This is very similar to how we created the function with the Azure CLI except it uses a Linux app service plan. You will want to open the Azure cloud shell again to get started.

You will need to create a resource group like so:
```
az group create --name myResourceGroup --location eastus
```
Since functions use a storage account we need to create one. The storage_name needs to be a globally unique name.
```
az storage account create --name <storage_name> --location eastus --resource-group myResourceGroup --sku Standard_LRS
```
The next step is to create an Azure Linux App Service plan. The following command will do it.
```
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku S1 --is-linux
```
Then we just need to create a function app on Linux. The app_name needs to be globally unique.
```
az functionapp create --name <app_name> --storage-account  <storage_name>  --resource-group myResourceGroup \
--plan myAppServicePlan --deployment-source-url https://github.com/Azure-Samples/functions-quickstart-linux
```
When that is finished you can use cURL to test the new function you just created.
```
curl http://<app_name>.azurewebsites.net/api/HttpTriggerJS1?name=<yourname>
```
There you have it, a simple overview of different ways to create Azure functions.

Which way do you like to create functions?
