---
layout: post
title:  "Azure Quick Tips: Create a CosmosDB"
date:   2018-06-15 06:04:29 -0400
categories: azure cosmosdb
---
I'm going to give some quick tips on how to get started with Azure Cosmos DB. Head over to [the azure portal](https://portal.azure.com) and click "Create a resource", and select Databases > Azure Cosmos DB.
![](https://jweiler.ghost.io/content/images/2018/06/2018-06-14-12_40_02-Window.png)
Enter the information including selecting which api you want to use. Azure Cosmos DB is a [multi model database service](https://docs.microsoft.com/en-us/azure/cosmos-db/introduction#key-capabilities) so you have several options to pick from.
![](https://jweiler.ghost.io/content/images/2018/06/2018-06-14-12_43_03-Window.png) 

Click create and wait till the service is set up. 

Then you are ready to start using the database. [Here](https://docs.microsoft.com/en-us/azure/cosmos-db/) is documentation for Cosmos DB. Just select the API type you chose to learn about it.

#### Using the Azure CLI
You can also create a CosmosDB with the Azure cli. [Here](https://docs.microsoft.com/en-us/azure/cosmos-db/scripts/create-database-account-collections-cli?toc=%2fcli%2fazure%2ftoc.json) is a sample script that shows you how to do it.



#### Using Storage Explorer
You can use the [Azure Storage Explorer](https://azure.microsoft.com/en-us/features/storage-explorer/) to interact with your new CosmosDB service. Just connect you Azure account and you will be able to see the CosmosDB you just created and can add tables right from there.

Now you have your CosmosDB service set up and are ready to do amazing things.

![Image ](/assets/images/2023-01-06 15_04_46-GitDemo.png)
<img src="https://github.com/jsweiler/my-blog/blob/master/assets/images/2023-01-06%2015_04_46-GitDemo.png" alt="Untitled2" />