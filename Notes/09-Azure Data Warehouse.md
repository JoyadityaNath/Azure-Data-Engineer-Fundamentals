OPENROWSET function in AZURE SQL
The OPENROWSET function in SQL Server is a powerful feature that allows ad hoc access to external data sources directly within a T-SQL query — without the need to create a linked server. It’s often used for importing, querying, or joining data from files, external databases, or OLE DB–compliant sources.
OPENROWSET in Azure Synapse allows serverless SQL pools to query external files directly from storage (CSV, Parquet, JSON, etc.).
This is highly efficient for exploratory analytics, data validation, and integration pipelines where moving data would be costly or unnecessary.


'''sql
SELECT *
FROM OPENROWSET(
    BULK 'https://<storage-account>.dfs.core.windows.net/<container>/<path>/file.parquet',
    FORMAT = 'PARQUET'
) AS [result];
'''
