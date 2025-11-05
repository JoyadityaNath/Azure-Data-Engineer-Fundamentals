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




