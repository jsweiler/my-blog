---
layout: post
title:  "Getting started with Azure Table Storage"
date:   2017-04-04 06:04:29 -0400
categories: azure table storage
---
#### Azure table storage
I will show you how to simply get started with Azure Table storage. If you have not ever looked at Table Storage you [should check it out](https://azure.microsoft.com/en-us/services/storage/tables/). If you are interested [here is a real world example](https://www.troyhunt.com/working-with-154-million-records-on/) of someone using Table Storage.
![](https://jweiler.ghost.io/content/images/2017/04/storage.PNG)
Azure Table storage is also very affordable. You can [check out the details](https://azure.microsoft.com/en-us/pricing/details/storage/blobs/) here. Here is a screen shot of the prices.
![](https://jweiler.ghost.io/content/images/2017/04/pricing.PNG)
So to get started go to the [azure portal](https://portal.azure.com) and click add new add the top left and select Storage from the list. Then you will want to select Storage account.
![](https://jweiler.ghost.io/content/images/2017/04/create.PNG)
#### Storage Explorer
An easy way to get started and create some tables is with the [Azure Storage Explorer](http://storageexplorer.com/). Once installed you will need to sign in and then you should see the storage account you created in the previous step.
![](https://jweiler.ghost.io/content/images/2017/04/explore.PNG) 
You can create a table by right clicking on Tables and clicking "Create Table". 

There are [some rules you need to follow](https://docs.microsoft.com/en-us/rest/api/storageservices/fileservices/understanding-the-table-service-data-model) when creating tables:

- Table names must be unique within an account.
- Table names may contain only alphanumeric characters.
- Table names cannot begin with a numeric character.
- Table names are case-insensitive.
- Table names must be from 3 to 63 characters long.
- Some table names are reserved, including "tables". Attempting to create a table with a reserved table name returns error code 404 (Bad Request).
These rules are also described by the regular expression "^[A-Za-z][A-Za-z0-9]{2,62}$".
Table names preserve the case with which they were created, but are case-insensitive when used.

Records in a table are called entities, and each entity can have 255 properties on it. There are three properties that need to be on each entity. They are as follows:

- PartitionKey
- RowKey
- TimeStamp

You are responsible for inserting values for the PartitionKey and RowKey, but the server manages the value for the TimeStamp property. Following I have quoted some more details from the Microsoft docs on the Partition Key and Row Key.

#### Partition Key
>Tables are partitioned to support load balancing across storage nodes. A table's entities are organized by partition. A partition is a consecutive range of entities possessing the same partition key value. The partition key is a unique identifier for the partition within a given table, specified by the `PartitionKey` property. The partition key forms the first part of an entity's primary key. The partition key may be a string value up to 1 KB in size.

>You must include the `PartitionKey` property in every insert, update, and delete operation.

##### Row Key
>The second part of the primary key is the row key, specified by the `RowKey` property. The row key is a unique identifier for an entity within a given partition. Together the `PartitionKey` and `RowKey` uniquely identify every entity within a table.
The row key is a string value that may be up to 1 KB in size.

>You must include the `RowKey` property in every insert, update, and delete operation.

#### Adding Properties
In table explorer if you select "Add" in the menu bar you then get a popup where you can select "Add Property" to add a number of properties to your entity and then insert a new entity. Once you add an entity it will show up immediately. You can then edit or delete the entities as well. You also get options to query the data as well.

#### Conclusion
The Storage explorer is a very easy way to get started and see how Azure Table storage works. Next time I am going to look at how you can interact with Table storage using the .NET api and show you an example with code.

Have you used Azure Table Storage? What did you think of it?