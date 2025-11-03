Azure Data Lake is a service which can be used to store any kind of data- structured (tables), semi-structured(JSON, CSV), and unstructured(images) data.


Block

A block is the fundamental storage unit used to persist data segments within Azure Blob Storage. Large objects are partitioned into multiple blocks to enable
parallelized write operations, optimized network throughput, and efficient reassembly on retrieval. Block-based storage is foundational to high-performance 
ingestion pipelines and scalable object management.

Blob (Binary Large Object)

A Blob is an addressable, versionable object stored within Azure Blob Storage, composed of one or more blocks. Blobs support unstructured and semi-structured data 
at petabyte scale, with durability, lifecycle policy integration, replication configuration, and fine-grained access control via RBAC and SAS tokens. Blob Storage 
provides the core object persistence layer underpinning analytical and operational data workloads.

Data Lake (Azure Data Lake Storage Gen2)

Azure Data Lake Storage Gen2 is a high-throughput, distributed data repository built on top of Azure Blob Storage. It incorporates a hierarchical namespace to enable 
enterprise-grade data management, directory-level security, ACID-compliant metadata operations, and cost-optimized analytics integration with services such as Azure
Synapse, Databricks, Fabric, and Spark-based engines. It supports multi-format data ingestion (structured, semi-structured, unstructured) and is designed for 
large-scale analytical processing across heterogeneous data domains.



So basically, everything is stored on form of small units called blocks(managed by user), then it appears as binary objects when uploaded to cloud called BLOBS, and 
when these blobs are stored in a heirarchical manner then it is called a Data Lake.



<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d16841d8-e9ab-430a-9a05-2a677ad5284e" />




While creating a storage account in Azure resource group, ensure this option is ticked. Hierarchical namespace ensures that a data lake is being created. Else it 
is just a blob storage.


<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0d155819-8d67-4e0f-92cd-f6931f2bec7d" />

In the left panel, in data storga we can see 4 types of storages. Containers(Store files and large binary data at scale. Data Lakes), File(Cloud network drives for shared file access. Also mounts local files), Queues(Message-based communication between components) and Tables(Fast NoSQL key-value storage for structured/semi-structured data).


