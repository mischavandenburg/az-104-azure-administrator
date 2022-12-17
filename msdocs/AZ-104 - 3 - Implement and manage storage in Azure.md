# AZ-104 - 3 - Implement and manage storage in Azure

**Binary Large Object**
Also referred to as object storage or container storage. So it's the same as our Object Store in OpenStack.

Azure Blob Storage is a service for storing large amounts of unstructured object data. Unstructured data is data that doesn't adhere to a particular data model or definition, such as text or binary data.

- any type of text or binary data
- uses 3 resources:
	- azure storage account
	- containers in storage acount
	- blobs in container

==Blob cannot exist by itself. Must be inside a container resource.

Azure account can have unlimited containers and containers can have unlimited blobs.

## use case examples

-   **browser uploads**. Use Blob Storage to serve images or documents directly to a browser.
-   **distributed access**. Blob Storage can store files for distributed access, such as during an installation process.
-   **streaming data**. Stream video and audio by using Blob Storage.
-   **archiving and recovery**. Blob Storage is a great solution for storing data for backup and restore, disaster recovery, and archiving.
-   **application access**. You can store data in Blob Storage for analysis by an on-premises or Azure-hosted service.

## tiers

- premium
- hot
- cool (min 30 days)
- archive (min 180 days, offline)

## lifecycle management rules

You can set up rules with if-then clauses.

If blob older than 30 days, move to archive storage.
If blob older than 100 days, delete blob. 

## blob object replication

asynchronous replication of containers across regions

- requires versioning to be enabled on source and destination
- doesn't support snapshots
- ==only for hot and cool tiers

For which tiers can you enable blob object replication?::Only for hot and cool tiers. Requires versioning to be enabled on source and destination. 
ID: 1670597879288


## blob types

- block blob
	- blocks of data that are assembled to make blob
	- most scenarios use block blobs
	- ideal for text and binary data: files, images, videos
- page blob
	- up to 8tb in size
	- efficient for frequent read/write operations
	- Azure virtual machines use page blobs for OS disks and data disks
- append blob
	- similar to block, optimized for appending
	- useful for logging scenarios

## upload tools

- AzCopy
	- CLI tool for windows and Linux
- Azure Data Factory
- blobfuse
	- can acces blobs in storage account through linux filesystem

# Storage Security

## SAS

Secure way to share storage resources without compromising account keys. 

Grants access to a resource for a specified period of time. 

- granular control
- can grant access to multiple Azure Storage services: blobs, files, tables
- can specify time interval + expiration
- can specify permissions

## encryption

Azure Storage encryption is enabled for all new and existing storage accounts and can't be disabled.

Can you disable storage account encryption?::Enabled on default for all new and existing accounts. Cannot be disabled.
ID: 1670597879298

# Configure Storage Account

## types of storage

- virtual machine data
	- VM harddisks
	- max 32767 GB
	- number of disks depends on vm tier
- Unstructured data
	- Least organized
	- mixed
	- **non relational
	- Azure Blob Storage
	- Azure Data Lake Storage
		- hadoop distributed file system as a service
- Structured Data
	- relational format
	- shared schema
	- F. ex database table
		- rows
		- columns
		- keys
	- Azure Cosmos DB
		- globally distributed database service
	- Azure SQL Database
		- fully managed SQL database as a service

## storage account tiers

- standard
	- HDD
	- lowest cost
	- used for bulk storage that is infrequently accessed
- Premium
	- SSD
	- VM disks or databases

>[!NOTE]
>You can't convert standard tier storage account to premium storage or vice versa

Can you convert a standard tier storage account to premium?::You can't convert standard tier storage account to premium storage or vice versa.
ID: 1670597879305


## Azure Storage Services

- Blob storage (containers)
	- streaming video audio
	- backup & restore
	- can be accessed from anywhere: url, cli, api
	- nfs protocol
- Azure Files
	- highly available network file shares
	- acces by Server Message Block (SMB) or NFS protocol
	- authenticated with storage account credentials
- Azure Queue Storage
	- store and retrieve messages
- Azure Table Storage (Azure Cosmos DB)
	- fully managed NoSQL database
	- automatic management, updates, patching
	- automatic scaling
- Disks



>[!info]
>**Azure Queue Scenario**
>Consider a scenario where you want your customers to be able to upload pictures, and you want to create thumbnails for each picture. You could have your customer wait for you to create the thumbnails while uploading the pictures. An alternative is to use a queue. When the customer finishes the upload, you can write a message to the queue. Then you can use an Azure Function to retrieve the message from the queue and create the thumbnails. Each of the processing parts can be scaled separately, which gives you more control when tuning the configuration.

## replication strategy

- locally redundant storage (LRS)
	- replicated within same data center
- zone redundant (ZRS)
	- replicated in multiple data centers in the same region
- Geo redundant (GRS)
	- replicates to secondary region
	- RA-GRS: based on GRS, but with read access at all times
	- with GRS you only have read access from second region in case of a failover
- geo-zone redundant storage (GZRS)
	- combines ZRS with GRS
		- replicated across 3 in ZRS
		- replicated to second region GRS
	- also RA-GZRS

## access storage

>[!info]
> every object you store has a unique URL address


`//`**`mystorageaccount`**`.blob.core.windows.net/`**`mycontainer`**`/`**`myblob`**.

Can configure custom domains



# Azure Storage Tools

- Azure Storage Explorer
	- gui tool
	- does not need azure portal
- AzCopy
	- cli
	- can run in background
- Azure import/export service
	- physically send disks to be transferred to/from storage account

# creating a storage account

Storage Account::A container that groups a set of Azure Storage services together. Can only contain these resources. Deleting an account deletes all data inside of it. Part of a resource group.
ID: 1669705497235

## Storage Account
- container that groups a set of Azure Storage services
- **Can only contain storage resources
- ==Deleting account deletes all data inside of it
- Part of resource group
- Azure SQL and Cosmos DB cannot be included in storage account
- account itself is free

What happens when you delete a storage account?::Deleting account deletes all data inside of it. Can only contain storage resources.
ID: 1670597879311


# Shared Access Signatures

Grants granular access to files in Azure Storage with time limit. 

>[!quote]
>With a SAS, you control what a client can do with the files and for how long.

three types:
- User delegation SAS
	- only for blob, secured with Azure AD
- Service SAS
	- storage account key
	- to access Blob, Queue, Table or File
- Account SAS
	- same as service SAS
	- but can also control acess to service-level operations, such as Get Service Stats

SAS has two components
- URI that points to storage resource
- token that determines permissions

`https://medicalrecords.blob.core.windows.net/patient-images/patient-116139-nq8z7f.jpg?sp=r&st=2020-01-20T11:42:32Z&se=2020-01-20T19:42:32Z&spr=https&sv=2019-02-02&sr=b&sig=SrW1HZ5Nb6MbRzTbXCaPm%2BJiSEn15tC91Y4umMPwVZs%3D`

**uri**
`https://medicalrecords.blob.core.windows.net/patient-images/patient-116139-nq8z7f.jpg?`
**token**
`sp=r&st=2020-01-20T11:42:32Z&se=2020-01-20T19:42:32Z&spr=https&sv=2019-02-02&sr=b&sig=SrW1HZ5Nb6MbRzTbXCaPm%2BJiSEn15tC91Y4umMPwVZs%3D`

## stored access policy

A stored access policy is basically a template for creating sas tokens. For example you can use the c# code just by referencing the identifier and it will create the token

```c#
// Create a user SAS that only allows reading for a minute
BlobSasBuilder sas = new BlobSasBuilder
{
    Identifier = "stored access policy identifier"
};
```

Can be used on all 4 types of storage: blob, file, queue, table

Properties
- identifier
	- name
- start time
- expiry time
- permissions

# anki


LRS::Locally Redundant Storage
ID: 1669550890051

ZRS::Zone Redundant Storage. Multi datacenter in same region.
ID: 1669550890055

GRS::Geo Redundant Storage. Replicates to secondary region. RA-GRS
ID: 1669550890059

GZRS::Geo Zone Redundant Storage. 3 ZRS and to second GRS. Also RA-GZRS
ID: 1669550890062

Storage Account uniqueness?::Storage account must be unique.
ID: 1669550890066

Storage object URL::mystorageaccount.blob.core.windows.net/mycontainer/myblob Je moet weten hoe de URL is opgebouwd. Er is ook table.core.windows.net, queue.core.windows.net, file.core.windows.net
ID: 1669705497240



URI::Uniform Resource Identifier. This is basically a large url containing the parameters and acces key to access the storage through GET requests
ID: 1669496089442


SAS::Shared Access Signature. Grants restricted access rights to resources within a specified timelimit. 
ID: 1669496089450


Blob Types::Block blob, page blob, append blob.
ID: 1669496089455


Block blobs::Blocks that are assembled. Used in most scenarios. Ideal for text and binary data: files, images and videos. 
ID: 1669496089459


Page blob::Up to 8tb. Best for frequent read/write operations. VM's use these for OS and data disks.
ID: 1669496089462


Append blob::Similar to block, optimized for appending. Useful for logging scenarios. 
ID: 1669496089467




Azure Blob Storage::Binary Large Object. Service for storing large amoutns of unstructured object data, it does not have a particular data model or definition. Same as Object Store. 
ID: 1669496089471


How many blobs can you store?::Accounts can have unlimited containers and containers can have unlimited blobs.
ID: 1669496089474
