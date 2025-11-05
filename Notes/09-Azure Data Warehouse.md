OPENROWSET function in AZURE SQL
The OPENROWSET function in SQL Server is a powerful feature that allows ad hoc access to external data sources directly within a T-SQL query — without the need to create a linked server. It’s often used for importing, querying, or joining data from files, external databases, or OLE DB–compliant sources.
OPENROWSET in Azure Synapse allows serverless SQL pools to query external files directly from storage (CSV, Parquet, JSON, etc.).
This is highly efficient for exploratory analytics, data validation, and integration pipelines where moving data would be costly or unnecessary.


````sql
SELECT *
FROM OPENROWSET(
    BULK 'https://<storage-account>.dfs.core.windows.net/<container>/<path>/file.parquet',
    FORMAT = 'PARQUET'
) AS [result];
````

Steps to generate SQL code in Synapse:

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9d7e96da-4094-4b0f-839c-97005e6f2c58" />

<img width="1307" height="481" alt="image" src="https://github.com/user-attachments/assets/2ecae322-895a-46c9-a566-7dcda1f4e37e" />

<img width="1594" height="769" alt="image" src="https://github.com/user-attachments/assets/db6dcadb-6ad5-4b81-ae37-88b10a02724f" />


External Tables In Azure DataWarehousing:
External tables in Azure Synapse Analytics are virtual tables that let you query data stored outside your data warehouse — usually in Azure Data Lake Storage (ADLS) or Blob Storage — as if it were inside Synapse.
They don’t store the data themselves.
Instead, they point to files (like CSV, Parquet, or JSON) in external storage.

The main purpose of external tables is to:
1. Read data directly from storage without loading it into the warehouse.
2. Save storage costs by not duplicating data.
3. Enable easy integration between your data lake and data warehouse.
4. Support ETL/ELT workflows — you can query external data first, then transform or load it if needed.

When you create an external table in Azure Synapse Analytics, you’re essentially registering metadata — not moving data.
The metadata tells Synapse where the external data lives, how it’s formatted, and what its structure looks like.




A Database Scoped Credential is needed because it securely stores and manages authentication information that Synapse uses to access external data sources (like Azure Data Lake, Blob Storage, or external SQL servers).
Without it, Synapse wouldn’t know who you are or how to authenticate when connecting to data that lives outside the database.
That credential contains sensitive authentication information (like managed identity, SAS token, or storage key).
To protect that sensitive data, SQL Server and Synapse require a Database Master Key (DMK).
When you run that CREATE DATABASE SCOPED CREDENTIAL statement, Synapse needs to store authentication info securely inside your database’s metadata.
To do this, it uses encryption — specifically, it encrypts the credential using your Database Master Key (DMK).



<img width="1659" height="726" alt="image" src="https://github.com/user-attachments/assets/04c2bdea-17c1-4d62-b34c-df9746ae99d1" />

<img width="652" height="69" alt="image" src="https://github.com/user-attachments/assets/99d3b466-4b39-4051-98a8-6a0c858c170e" />



````sql
CREATE MASTER KEY ENCRYPTION BY PASSWORD ='Cr7@#007' 

--create database scoped credential for the external table

IF NOT EXISTS(SELECT * FROM sys.database_credentials WHERE name='sales_creds')
BEGIN
    CREATE DATABASE SCOPED CREDENTIAL sales_creds
    WITH 
        IDENTITY='Managed Identity'
END
````


CREATING EXTERNAL RESOURCES  for our external table:

````sql
CREATE MASTER KEY ENCRYPTION BY PASSWORD ='Cr7@#007' 

--create database scoped credential for the external table

IF NOT EXISTS(SELECT * FROM sys.database_credentials WHERE name='sales_creds')
BEGIN
    CREATE DATABASE SCOPED CREDENTIAL sales_creds
    WITH 
        IDENTITY='Managed Identity'
END

--external data source
IF NOT EXISTS(SELECT * FROM sys.external_data_sources WHERE name='gold_source')
BEGIN
    CREATE EXTERNAL DATA SOURCE gold_source
    WITH
    (
        LOCATION='https://web.azuresynapse.net/en/authoring/explore/linked/storageaccounts/AzureDataLakeStorage1-azuredestorageforjoy%2Fenriched?subFolderPath=&workspace=%2Fsubscriptions%2Ff20dc50d-1591-4f5c-ade3-50a0bd2d226b%2FresourceGroups%2FTest-resource-group%2Fproviders%2FMicrosoft.Synapse%2Fworkspaces%2Fazuresynapsejoy'
        CREDENTIAL=sales_creds
    )
END

--external file format
IF NOT EXISTS(SELECT * FROM sys.external_file_formats WHERE name="delta_format")
BEGIN
    CREATE EXTERNAL FILE FORMAT delta_format
    WITH
    (
        FORMAT_TYPE=DELTA
    )
END
````


Explanation:

1) Database Master Key (DMK)
   What it is: A symmetric key inside your Synapse SQL database that protects other sensitive secrets (credentials, certificates).
   Why you need it: When you store any secret (e.g., SAS token, storage key, or even the “use Managed Identity” instruction), SQL encrypts that metadata. The DMK     is the root of that encryption hierarchy.
   Internal mechanics:
   DMK is created and stored in your database, encrypted by the password you supplied.
   Later objects (credentials) are encrypted by the DMK.
   Without a DMK you’ll get the “create/open a master key” error when creating credentials.
   TL;DR: The DMK is the vault that secures secrets for external access.

2) Database-scoped Credential (DSC)
   What it is: A secure handle to an identity your database will use when authenticating to external systems.
   Why you need it: Synapse must prove who it is to your Data Lake. Using IDENTITY='Managed Identity' tells Synapse to use the workspace’s AAD Managed Identity       (no passwords, no keys).
   Internal mechanics:

    The DSC record is stored in sys.database_credentials and encrypted by the DMK.
    At query time, Synapse obtains an OAuth token for the Synapse workspace’s managed identity and presents it to ADLS.
    Access is permitted only if that managed identity has RBAC on the storage (e.g., Storage Blob Data Reader).
    TL;DR: The DSC is your ID badge, locked in the DMK safe.

3) External Data Source (EDS)
   What it is: A pointer that tells Synapse where the external data lives and which credential to use to get there.
   Why you need it: External tables and OPENROWSET reference an EDS to resolve storage endpoints consistently.
   Internal mechanics:
   The EDS stores the base URI and a reference to sales_creds.
   At runtime, Synapse resolves gold_source → grabs the credential → gets a token → calls the storage endpoint.
Important correction:
The LOCATION must be a storage endpoint, not a Synapse Studio URL. Use ADLS Gen2 URI format:
TL;DR: The EDS is the GPS coordinate + who to authenticate as.

4) External File Format (EFF)
   What it is: A reusable definition that tells Synapse how to interpret the files (Delta, Parquet, CSV, etc.).
   Why you need it: External tables reference a file format so the engine knows the storage layout and reader to use.
   Internal mechanics:

    The EFF stores a small metadata record (FORMAT_TYPE, optional options like field/row terminators for CSV).
    For Delta Lake, Synapse uses the Delta reader which reads the transaction log to find the current set of Parquet files.
    TL;DR: The EFF is the decoder ring for your files.




   Finally Creating our External table:

   <img width="744" height="153" alt="image" src="https://github.com/user-attachments/assets/b98fb74a-8a32-41e4-8653-4826e539313d" />

   <img width="629" height="436" alt="image" src="https://github.com/user-attachments/assets/da4b7f39-adb3-4732-8cb1-abaf0d72f853" />



<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9a54cdb2-a938-4f8e-be14-d8f5625a466c" />




