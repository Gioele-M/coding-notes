# Azure storage
 
*Azure Storage Accounts*
This is storage in the cloud (instead of Disk), paired with services, such as:
- Azure Blob Storage
	+ Storage for unstructured data, grows automatically on demand and stores backups
- Azure File Shares
	+ Share files within the company
- Azure Table Storage
	+ Store non-relational structured data (SCHEMALESS DESIGN!!!)	
- Azure Queue Storage
	+ Messaging based service (for apps to communicate data)



*Create account from 'Storage account' in marketplace*
(It is not a real account, it is still a service)
You have the choice for Standard and Premium accounts.
You get all the aforementioned services when you open a new storage account




*Blob storage*
Stores files as '_objects_', you group objects into CONTAINERS. (Object based storage service)
You get a url for each object to access it via web (this had restrictions obviously, not everyone can access) -> change access level inside container setting!


*Access tiers*
You will be charged based on:
- Volume of data
- Quantity and type of operations (access-deposit operation)
- Data redundancy is enabled (backup)

You also have access tiers, basically the type of subscription you do, which costs more or less based on services required. The tiers top to bottom are:
- Premium
- Hot
	+ Frequently accessed objects (high storage cost, low access cost)
- Cool
	+ Best for objects accessed and modified infrequently (less storage, more access) (accessible after 30 days online)
- Archive
	+ Low storage cost but high access cost (accessible after 180 days online) (You can access data after a 're-hyration' eg. change tier again which takes a while)

You can mix and match the tiers for all your data, and swap states if you don't need ot anymore

You can set Hot and Cool tiers at account level, and those 2 + Archive at blob level




*Data redundancy*
Data redundancy is a protection against failiures and loss of data by having multiple copies of the same data, you have options:
- Locally redundant (for non critical data)
- Geo-redundant (for backup - in a different region in the world just in case)
	+ You can do also _read access_ so that you can access the other region data just on a read level
- Zone redundant (for high availability, they deploy in 3 different Availability zones)
- Geo-Zone-redundant (mix of the other two)





*Azure File shares*
_In storage account_
You can create a new File Share, very easily. Can then create directories and upload files (not treated as a blob anymore but as file)

To get the data you can click on 'connect', get a Powershell script to download on local machine thorugh terminal (exceptions due to firewall apply)
Through this connection you can also upload files through terminal



*Azure File Sync*
It's basically a service that gives a server that caches the most frequently used files for ready access



*Azure Queue Storage*
It's a service for apps to send messages on a queue


*Azure Table Storage*
Used to store non-relational schema data. Data can be accessed in the 'Storage Browser', here we can add 'entities' (rows) with add data types. Data can have different numbers of columns



*Premium storage accounts*
You create a Premium Block Blob. It's much faster as it's stored in solid-state drivers, optimised for latency. Costs a lot


*Azure Storage Explorer*
A downloadable tool to manage Storage Accounts. You can download and upload files from there, as well as manage them.



*Migrating data to Azure Storage*
You get a device 'Azure Data Box', where you tranfer in Terabytes of data, send it back and the engineers of Azure copy it in the server


*AzCopy*
Command-line utility used to copy blobs or files to or from a storage account. 


*Virtual network service endpoints*
Provides secure and direct connectivity to azure services over the Azure backbone network.
In virtual network settings you can add a Service Endpoint, for a variety of services such as storage.







Recap
*Azure storage accounts*
- Allowst storage of objects in the cloud
- Can use blob-queue-file and table
- Different types of accounts 


















































