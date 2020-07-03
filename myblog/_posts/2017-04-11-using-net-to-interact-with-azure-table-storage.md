---
layout: post
title:  "Using .NET to interact with Azure Table Storage"
date:   2017-04-11 06:04:29 -0400
categories: azure table storage
---
#### Azure Table Storage
[Last time](http://www.jweiler.com/getting-started-with-azure-table-storage/) I wrote about how to setup a table storage account and how to get started using the [Azure Storage Explorer](http://storageexplorer.com/). This time I am going to show you how to use .NET to interact with an Azure Table storage account.

With a Storage Account you can do many things as shown by this picture.
![](https://jweiler.ghost.io/content/images/2017/04/storage-concepts.png)
I will be focusing on just tables in this post. [I will build a sample WPF app](https://github.com/jsweiler/Azure.NetTestApp) that shows you how to use table storage as the database. 

#### Getting started
You need two NuGet packages to get started: One is [Windows Azure Storage](https://www.nuget.org/packages/WindowsAzure.Storage/) and the other is [Microsoft Azure Configuration Manager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/).
You need to configure a connection string in the app.config file. I am using the storage emulator for testing so my app.config will look like this. The UseDevelopmentStorage is a shortcut to use when using the storage emulator.
```xml
<configuration>
  <startup>
    <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.7" />
  </startup>
  <appSettings>
    <add key="StorageConnectionString" value="UseDevelopmentStorage=true;" />
  </appSettings>
</configuration>
```
If you want to use the emulator you will need to [download the emulator](https://go.microsoft.com/fwlink/?linkid=717179&clcid=0x409). Once installed [see here](https://docs.microsoft.com/en-us/azure/storage/storage-use-emulator#start-and-initialize-the-storage-emulator) to start the emulator on your local machine. To use an actual account all you do is get the connection string from the Azure portal and paste in in for the value.

You need to create an instance of a `CloudStorageAccount` and `CloudTableClient`. In my test app I did that in my ViewModel constructor.
```csharp
private CloudStorageAccount storageAccount;
private CloudTableClient tableClient;

public MainViewModel()
{
    storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));
    tableClient = storageAccount.CreateCloudTableClient();
}

```
It is very easy to create a table with code like this. As you can probably guess this creates the table for you if it isn't there.
```csharp
customersTable = tableClient.GetTableReference("Customers");
customersTable.CreateIfNotExists();

```
Querying for all records is easy:
```csharp
var query = new TableQuery<CustomerEntity>();
var results = customersTable.ExecuteQuery(query);
```

When you have to update a record you need a replace `TableOperation`. Like this:
```csharp
var update = TableOperation.Replace(entity);
await customersTable.ExecuteAsync(update);

```
If you want to delete an entity it also is easy and similar.
```csharp
var deleteOperation = TableOperation.Delete(entity);
await customersTable.ExecuteAsync(deleteOperation);
```
The API for interacting with table storage is easy to use and it doesn't take very long to figure things out from my little experience with it so far. Also [the documentation](https://docs.microsoft.com/en-us/azure/storage/storage-dotnet-how-to-use-tables) is very helpful and is a must if you are new to this.

So if you want to check out my little sample app to see how you can use Table storage [do so here](https://github.com/jsweiler/Azure.NetTestApp).

Have you used the .NET api to interact with Table Storage? What did you think of it?