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



Reource Group Hierarchy

Azure resource groups: A resource group is a container that holds related resources for an Azure solution. The resource group can include all the resources for the
solution or only those resources that you want to manage as a group. You decide how to allocate resources to resource groups based on what makes the most sense 
for your organization. Generally, add resources sharing the same lifecycle to the same resource group so you can easily deploy, update, and delete them as a group.


<img width="860" height="609" alt="image" src="https://github.com/user-attachments/assets/cbe3e3d8-9be0-420c-9973-4082e0060223" />


1. Management Groups
Definition: A governance layer used to centrally apply compliance, access policies, and security standards across the organization.
Use Case: Align corporate policy enforcement across business units or environments (e.g., Production vs. Development).
Impact: Policies applied here inherit downward to every subscription beneath it.

2. Subscriptions
Definition: A billing and quota boundary that encapsulates Azure usage and cost tracking.
Purpose: Divide workloads by team, environment, cost center, or customer.
Example:
Subscription A → Corporate IT
Subscription B → Data Analytics Dept.
Identity and role assignments can also be scoped here.

3. Resource Groups
Definition: A logical container for grouping resources that share a common lifecycle, purpose, or management boundary.
Enables:
Unified role-based access control (RBAC) at group level
Joint monitoring and tagging
Coordinated deployment and deletion
Rule: A resource belongs to exactly one resource group.

4. Resources
Definition: Actual Azure service instances provisioned to run workloads, such as:
Virtual Machines
SQL Databases
Storage Accounts
Functions
Databricks Workspaces
These represent the tangible compute, storage, and platform components your solution consumes.
Enterprise Interpretation
This hierarchy enables:
Controlled governance (policies roll down the hierarchy)
Cost segregation and chargeback (subscription-level billing)
Operational clarity (resource grouping aligned to workloads)
Scalable identity and access control (RBAC inheritance across scopes)




Example Usage of a Resource Group

Assume you deploy a production web application. It includes an App Service, a SQL Database, a Storage Account, a Virtual Network, and monitoring components. 
Placing all of these into a single Resource Group—for example, RG-CustomerPortal-Prod—creates a unified management boundary. This means security roles, deployment 
automation, cost tracking, monitoring configuration, and lifecycle operations are all performed against the group as a cohesive unit rather than against individual 
resources.


If the same resources were scattered independently across the subscription:
Permission scoping becomes broad, increasing operational risk.
Cost visibility becomes diluted and difficult to align to business ownership.
Deleting or updating workloads becomes error-prone, as components must be located and managed individually.
Governance becomes inconsistent, encouraging configuration drift and complicating compliance.
