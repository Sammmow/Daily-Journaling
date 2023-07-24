**Data Warehouse**:
I searched up Pentaho from the following link: https://help.hitachivantara.com/Documentation/Pentaho/8.3/Products/Learn_about_the_PDI_client#r_pentaho_data_integration_perspective_learn_about_the_pdi_client_spoon_pdi

I am reading on it. There is nothing written about connectors it provides.
It has some tools for locally extracting,transforming and loading.
Similarly, for bigger data, it uses spark engine. Ref here :https://help.hitachivantara.com/Documentation/Pentaho/8.3/Products/Adaptive_Execution_Layer
The pricing of products are not clearly listed or I have not been able to find them.

I looked at AWS Data Pipeline. The users can specify tasks to extract,transform.
It features drag and drop console for ease of use.
I read on the following link : https://aws.amazon.com/datapipeline/
However, AWS Data Pipeline is mostly used for storing, transforming and processing Amazon's services.


<hr/>

**9th June,2023**:
Using stitch:
Stitch is an ETL tool by Talend.
Create an account. There is a free trial for 30 days.
I am integrating a small google sheet into my stitch.

More about google sheets integration in here:
https://www.stitchdata.com/docs/integrations/saas/google-sheets

<br/>

**How to use stitch?**

Sign into stitch account.
Then go to Add integration button.
Click google sheets icon.
Enter name for integration and then add url of google sheets.

**Create replication schedule:**
There is Replication frequency settings found in integration.
It extracts data from the integration every time limit one sets.

For eg, if the replication schedule is set to 30 mins, then stitch will connect and extract data every 30 mins.

After adding name and url of sheets, then give permission to stitch that they can see all of the user's sheets and drive data.Only then is the integration complete.
After this, one can choose to select specific columns or the whole table to be loaded. User can select this as per requirement.
In the stitch dashboard, user sees the extraction, preparation and the  Loaded part. 
If the destination is not set i.e. ware house is not set then , the loaded part is empty.
Currently, I have not specified which data warehouse to load the extracted data to.

I am using snowflake.
Snowflake:
more info here: https://www.stitchdata.com/docs/destinations/snowflake/connecting-a-snowflake-data-warehouse-to-stitch#create-data-warehouse

CREATE WAREHOUSE CUSTOMERINFO
WITH
AUTO_RESUME = TRUE
WAREHOUSE_SIZE = XSMALL
AUTO_SUSPEND = 240;

CREATE WAREHOUSE <name>
WITH
{properties}

**AUTO_SUSPEND** : 
Specifies the number of seconds of inactivity after which a warehouse is automatically suspended.

Valid values: 
Any integer 0 or greater, or NULL:

Setting a value less than 60 is allowed, but might not result in the desired/expected behavior because the background process that suspends a warehouse runs approximately every 60 seconds and, therefore, is not intended for enabling exact control over warehouse suspension.

Setting a 0 or NULL value means the warehouse never suspends.

Default
600 (the warehouse suspends automatically after 10 minutes of inactivity)

**AUTO_RESUME**:
Specifies whether to automatically resume a warehouse when a SQL statement (e.g. query) is submitted to it.

Valid values
TRUE: The warehouse resumes when a new query is submitted.

FALSE: The warehouse only resumes when explicitly resumed using ALTER WAREHOUSE or through the Snowflake web interface.

Default
TRUE (the warehouse resumes automatically when a SQL statement is submitted to it)

11:34 am:
I had a question that I am putting sensitive data to cloud.I asked Nishant dai and Madhav dai. It is better that you a on premise data warehouse, they said because for now,even if I alter the data and remove sensitive information, other organizations will not allow me to put sensitive information in the cloud.
I am looking for on-premise data warehouses.
I searched on on premise data warehouse. I read on premise vs cloud warehouse. In on-premise warehouses, there are a few warehouse i.e. Oracle database,Microsoft SQL server,IBM Db2,Teradata, SAP HANA,etc.
searched for open source data warehouses. Found Pentaho, Talend Open Studio,Oracle Autonomous Database,


July 12th,2023:

Azure Blob Storage: 
used for : 
Serving images or documents directly to a browser.
Storing files for distributed access.
Streaming video and audio.
Writing to log files.
Storing data for backup and restore, disaster recovery, and archiving.
Storing data for analysis by an on-premises or Azure-hosted service.

Users or client applications can access objects in Blob Storage via HTTP/HTTPS, from anywhere in the world. Objects in Blob Storage are accessible via the Azure Storage REST API, Azure PowerShell, Azure CLI, or an Azure Storage client library. Client libraries are available for different languages, including:

.NET
Java
Node.js
Python
Go

13th July,2023: 

Data Box gateway:For copying hundreds of TBs of dat to Azure Storage, use DataBox Gateway. Data can be ingested one time or ongoing basis.
supports continuous data ingestion, initial bulk transfer followed by incremental transfer as well.
supports SMB protocol:
-> Server Message Block(SMB) : client server communication protocol used for shared access to files,directories,printers,serial ports and other resources on network.


**Azure Blob Storage**: stores files for distributed access, backup, restore, data recovery and archiving.
data can be accessed using http/https.
accessible by Azure Storage REST API, Azure PowerShell and Azure CLI.
Client libraries are available in python,.net, GO, etc.

Azure Blob contains:
1.Storage
2.Container
3. Blobs

Storage: contains a set of containers.
Containers: contains a set of Blobs.
Blos: 
i. Block Blobs: stores text and binary data
ii. Append blob: block blobs but user can append data.
iii. Page blob: stores random access files upto 8Tin in size, stores VHD(virtual hard drive), serves as disks for azure virtual machines.

**Azure NetApp Files**:
SMB,NFS protocol
Use Cases: file sharing,databases, high power computing
Built in availability, data protection and disaster recovery capabilities and is scalable.

**Azure Data Factory**:
Azure's ETL tool.
works with:   
Azure Databricks: It provides a notebook-based environment for building and executing ETL workflows using Spark. With Databricks, you can process large volumes of data in parallel, leverage machine learning capabilities, and integrate with various data sources.

Azure Synapse Analytics: Azure Data Warehouse   

**Working of Azure Data Factory**:

1.Connect to Data Source and then, load the data to a Centralized Data Store.
2.Collect data in Synapse Analytics and transform.
3. Alter data in data store, process transformed data using **DATA FLOWS**. 
**Data Flows**: Data Flows inherently use spark cluster, thus user does not need to know spark programming.
Or, if One prefers code transformation by hand, ADF(Azure Data Factory) supports coding platformss like HDInsight Hadoop, Spark, Data Lake Analytics.

**Azure Data Factory concepts/components**:

1. Pipelines:
Create pipelines that orchestrate workflows.

2. Data Flows:
Create, manage data transformation logic to transform any sized data.  
Build Reusable library of data transformation routines.
DF executes login on Spark Cluster which user does not need to maintain.

3. Activites:
Activities are each process step in pipeline.
Eg, a copy activity to copy data from one data store to another.
Similarly, use Hive activity, which runs on Hive query on Azure HDInsight cluster to transform or analyze your data.
DF supports Data movement, transformation and control activities.

4. Datasets:
Datasets are data structures within data stores.
It points t odata you want to use in your activites as input or outputs.

5. Linked Services:
There are much like connection strings.
They Define connection information needed by DF to connect to external sources.
A linked sources defines connection to data source.

For example, an Azure Storage-linked service specifies a connection string to connect to the Azure Storage account. Additionally, an Azure blob dataset specifies the blob container and the folder that contains the data.

Linked services are used for two purposes in Data Factory:

To represent a data store that includes, but isn't limited to, a SQL Server database, Oracle database, file share, or Azure blob storage account. For a list of supported data stores, see the copy activity article.

To represent a compute resource that can host the execution of an activity. For example, the HDInsightHive activity runs on an HDInsight Hadoop cluster. For a list of transformation activities and supported compute environments, see the transform data article.

6. Integration Runtime:
In DF, an activity defines the action to be performed. 
Linked service define target data store or compute service.
Integration runtime provides bridge between the activity and linked services.
Provides compute environment where activity either runs or gets dispatched from.
This way, the activity can be performed in the region closest possible to the target data store or compute service in the most performant way while meeting security and compliance needs.
-> can be created from  Azure Data Factory and Azure Synapse UI via the management hub directly as well as activities, datasets, or data flows that reference them.

functions of Integration Runtime:
https://learn.microsoft.com/en-us/azure/data-factory/concepts-integration-runtime#azure-integration-runtime

Azure IR network environment:
connects to data stores and compute services with public accessible endpoints.
**Enabling Managed Virtual Network**, Azure IR connects to data stores using private link service in private network environment.

-> Activity Dispatch in Azure IR: operation to route the activity to target compute service, so there isn't ned to scale up the computer size for this scenario.

**Imp**:
When copying data between stores that are accessible through Internet from any IP, Azure Integration runtime is used becaue it is secure,reliable,scalable and globally available.

When copying data to and from data stores that are located on premise or in a network with access control(for eg. Azure Virtual Network), a self-hosted integration runtime needs to be set up.

**Control flow**:
Control flow is an orchestration of pipeline activities that includes chaining activities in a sequence, branching, defining parameters at the pipeline level, and passing arguments while invoking the pipeline on-demand or from a trigger. It also includes custom-state passing and looping containers, that is, For-each iterators.


Supported file formats for Data Sets:
https://learn.microsoft.com/en-us/azure/data-factory/connector-overview#supported-file-formats

Azure IR network Environment:
https://learn.microsoft.com/en-us/azure/data-factory/concepts-integration-runtime#azure-ir-network-environment

Enabling Managed Virtual Network, Azure Integration Runtime supports connecting to data stores using private link service in private network environment.



**July 17th,2023**:
I am considering using Azure Blob Storage or Azure SQL Database.
I would prefer to use Azure SQL Database but it provides 250 gb of data.
Considering the data is not more that 10 gbs, I am considering using Azure Blob Storage.

I am creating an Azure Storage account using PowerShell.
https://learn.microsoft.com/en-us/azure/storage/common/storage-account-create?tabs=azure-powershell#prerequisites

Azure Storage :
https://learn.microsoft.com/en-us/azure/storage/common/storage-redundancy#redundancy-in-the-primary-region

Locally redundant storage (LRS) copies your data synchronously three times within a single physical location in the primary region. LRS is the least expensive replication option, but isn't recommended for applications requiring high availability or durability.

Zone-redundant storage (ZRS) copies your data synchronously across three Azure availability zones in the primary region. For applications requiring high availability, Microsoft recommends using ZRS in the primary region, and also replicating to a secondary region.

Configuring Storage:
https://learn.microsoft.com/en-us/azure/storage/common/storage-account-create?tabs=azure-portal#prerequisites

**Networking tabs**:
https://learn.microsoft.com/en-us/azure/storage/common/storage-account-create?tabs=azure-portal#networking-tab
In Networking tabs, the connection access can be chosen as from 
enable public access from all networks.
enable public access from selected virtual networks and IP address.
disable public access and use private access.

For network routing, I have chosen Microsoft network routing. We can use other way too.


**Encryption tab**:
https://learn.microsoft.com/en-us/azure/storage/common/storage-account-create?tabs=azure-portal#encryption-tab

By default, data in the storage account is encrypted by using Microsoft-managed keys. You can rely on Microsoft-managed keys for the encryption of your data, or you can manage encryption with your own keys

Customer managed keys for Azure Storage encryption: Currently, I am  using the default i.e. blobs and file only for the "Enable support for customer managed keys option"
https://learn.microsoft.com/en-us/azure/storage/common/customer-managed-keys-overview

**Resource naming and tagging**:
Naming and tagging is required to distinguish the Azure resources.

**Baseline Naming Conventions**:https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-best-practices/resource-naming-and-tagging-decision-guide?toc=%2Fazure%2Fazure-resource-manager%2Fmanagement%2Ftoc.json#baseline-naming-conventions

Purpose of naming and tagging: 
https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-best-practices/naming-and-tagging#purpose-of-naming-and-tagging

->Tried to set up Azure Powershell. There were some errors.

**July 18th,2023**:

**Imp**:
self-hosted integration runtime can only be shared with multiple data factories, it can't be shared across Synapse workspaces or between data factory and Synapse workspace.

###Create and configure a self hosted integration runtime:



###Azure PowerShell Installation:
https://learn.microsoft.com/en-us/powershell/azure/install-azure-powershell?view=azps-10.1.0#install

**Installed Azure Powershell.**

By using powershell, I am creating a data Factory.

To create data factory,
$SubscriptionName = "Client-kyc Azure Data Factory" 
$ResourceGroupName = "clientInfo-kyc" 
$DataFactoryLocation = "Southeast Asia" 

# Shared Self-hosted integration runtime information. This is a Data Factory compute resource for running any activities 
# Data factory name. Must be globally unique 
$SharedDataFactoryName = "[Shared Data factory name]" 
$SharedIntegrationRuntimeName = "[Shared Integration Runtime Name]" 
$SharedIntegrationRuntimeDescription = "[Description for Shared Integration Runtime]"

# Linked integration runtime information. This is a Data Factory compute resource for running any activities
# Data factory name. Must be globally unique
$LinkedDataFactoryName = "[Linked Data factory name]"
$LinkedIntegrationRuntimeName = "[Linked Integration Runtime Name]"
$LinkedIntegrationRuntimeDescription = "[Description for Linked Integration Runtime]"

PS C:\Users\sujit> Connect-AzAccount

Account                          SubscriptionName   TenantId                          
-------                          ----------------   --------                          
shambhav.acharya@deerwalk.edu.np Azure for Students 8ccf04ed-2580-4d2b-a35f-b10552f...

PS C:\Users\sujit> $SubscriptionName = "Azure for Students" //changed subscription name to Azure for Students.

**Command to create data Factory**:
Link here : [https://learn.microsoft.com/en-us/azure/data-factory/create-shared-self-hosted-integration-runtime-powershell#create-a-data-factory]


Command:
New-AzResourceGroup -Location $DataFactoryLocation -Name $ResourceGroupName

But, I already have an existing Resource Group. So, I will use the following command.
Set-AzDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                         -Location $DataFactoryLocation `
                         -Name $SharedDataFactoryName


There is naming convention for SharedDataFactoryname i.e. it must start and end with alphanumeric character. Also, for a name like clientinfo-kyc Azure Data Factory , it should be modified as **"clientinfo-kyc-Azure-Data-Factory"**

**Create SHARED Self Hosted Integration Runtime**:
https://learn.microsoft.com/en-us/azure/data-factory/create-shared-self-hosted-integration-runtime-powershell#create-a-data-factory

$SharedIR = Set-AzDataFactoryV2IntegrationRuntime `
    -ResourceGroupName $ResourceGroupName `
    -DataFactoryName $SharedDataFactoryName `
    -Name $SharedIntegrationRuntimeName `
    -Type SelfHosted `
    -Description $SharedIntegrationRuntimeDescription`

    **Get the integration runtime authentication key and register a node:**
    Get-AzDataFactoryV2IntegrationRuntimeKey `
    -ResourceGroupName $ResourceGroupName `
    -DataFactoryName $SharedDataFactoryName `
    -Name $SharedIntegrationRuntimeName

**Create a Shared Integration Runtime**:
https://learn.microsoft.com/en-us/azure/data-factory/create-self-hosted-integration-runtime?tabs=data-factory#create-a-self-hosted-ir-via-azure-powershell

After you make and register your node,

**Service account for self-hosted integration runtime**:
https://learn.microsoft.com/en-us/azure/data-factory/create-self-hosted-integration-runtime?tabs=data-factory#service-account-for-self-hosted-integration-runtime


What are the integration runtime and data store locations?
The integration runtime location defines the location of its back-end compute, and where the data movement, activity dispatching and data transformation are performed. To obtain better performance and transmission efficiency, the integration runtime should be closer to the data source or sink.


Private Endpoint:
Private Link:


On where I am in work:
https://learn.microsoft.com/en-us/azure/purview/manage-integration-runtimes#prerequisites


July 21st,2023
**Creating an Azure IR via Powershell**:

To create a Azure IR via Azure PowerShell : 
Set-AzDataFactoryV2IntegrationRuntime -DataFactoryName "SampleV2DataFactory1" -Name "MySampleAzureIR" -ResourceGroupName "ADFV2SampleRG" -Type Managed -Location "West Europe"

For Azure IR, the type must be set to Managed. You do not need to specify compute details because it is fully managed elastically in cloud.

https://learn.microsoft.com/en-us/azure/data-factory/create-azure-integration-runtime?tabs=data-factory

Before setting the IR, I have to set up the linked services.
Before you create a dataset, you must create a linked service to link your data store to the Data Factory or Synapse Workspace.

**To create Linked Service**:
Create a JSON file named AzureStorageLinkedService.json in C:\ADFv2QuickStartPSH folder with the following content: (Create the folder ADFv2QuickStartPSH if it does not already exist.)

10:49 pm: Installing Azure CLI

then 
az login
az storage account keys list --account-name "kycblob" --resource-group "clientInfo-kyc"

11:31 am: still on configuring linked service...

11:50 am: resumed from break and 

1:47 am :the "File System" linked service in Azure Data Factory requires a username and password when using it to access files located on the local system, network share, or on-premises location. However, as you rightly mentioned, you may not have explicit credentials for an Excel file stored on your local C drive.

In your local development environment, you can still use Azure Data Factory to work with Excel files by taking an alternative approach. Instead of using the "File System" connector, you can use the "Binary" connector to read the Excel file as binary data and then use data flows to process the data and store it in your destination, such as Azure Blob storage.

When using Binary dataset, the service does not parse file content but treat it as-is.
https://learn.microsoft.com/en-us/azure/data-factory/format-binary

Roadblock:
could not copy excel file directly to Azure Data Factory
Then,made a blob storage to store excel files on it. The files stored will be transformed and then, copied to another blob storage.
Another roadblock is it is confusing to configure the container and the directory for the local file path.

**July 23rd,2023**:

clientinfo.xlxs is in Source container of the Storage Account.
It needs to be copied from source to sink.
Using copy tool, clientinfo.xlxs was copied from source to sink.

Now, I need to figure out a way to extract data from sink. 
I need to to put another excel sheet, i.e. kyc in the source. 
Then, merge the tables such that there is only one table. Tools for transformation/merging: Azure HDInsight, Databricks, Synapse Analytics


**July 24th,2023**:
I am setting up databricks to use for data merge, edit, transformation.

I am searching for which Databricks plan to use.

Searched on databricks documentation from Azure documentation and Stack Overflow.
I have added sources to the data flow. The data from the files were too big ,Dataflow put error on timeout.

So, I added sample data i.e. pieces of data.
Now, I need to somehow make the two datasets merge without conflicts.