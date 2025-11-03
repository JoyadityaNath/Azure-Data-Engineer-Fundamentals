Azure Data Factory(ADF): 
Azure Data Factory (ADF) is Microsoft Azure's cloud-based data integration service. It enables you to move, transform, and manage data from various sources 
in an automated and orchestrated manner.
ADF is positioned as a complete ETL/ELT orchestration and data integration service in the Azure ecosystem.


Linked Service: 
A Linked Service in Azure Data Factory represents a connection configuration to an external system. It defines where your data comes from 
or where your data is going, along with the authentication and connectivity parameters required to access that system.
In other words, Linked Services are ADF’s connection endpoints to data sources, compute engines, and storage platforms.

Datasets:
A Dataset represents the data structure or reference that ADF works with.
It does not hold data itself, but describes where the data is stored and how it is formatted.
Eg: Linking to github using linked service, then the file within the github reposioty will be the dataset.


Activities:
An Activity is an individual step inside a Pipeline.
Each Activity performs a specific operation such as copying data, running a transformation, executing a stored procedure, or calling a Databricks notebook.
Example Activities inside a pipeline:
Copy Activity (move data)
Data Flow Activity (transform data)
Notebook Activity (run code in Databricks)


Pipelines:
A Pipeline is the orchestration layer in Azure Data Factory.
It defines the workflow—the sequence in which tasks run, how data moves, and how logic is coordinated.


