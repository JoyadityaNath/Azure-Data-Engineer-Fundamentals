Storage Account:
The services that comprise Azure Storage are managed through a common Azure resource called a storage account. The storage account represents a shared pool of storage 
that can be used to deploy storage resources such as blob containers (Blob Storage), file shares (Azure Files), tables (Table Storage), or queues (Queue Storage).


Redundancy:
Azure Storage always stores multiple copies of your data to protect it from planned and unplanned events. Examples of these events include transient hardware failures,
network or power outages, and massive natural disasters. Redundancy ensures that your storage account meets its availability and durability targets even in the face
of failures.

When deciding which redundancy option is best for your scenario, consider the tradeoffs between lower costs and higher availability. The factors that help determine
which redundancy option you should choose include:

How your data is replicated within the primary region.
Whether your data is replicated from a primary region to a second, geographically distant region, to protect against regional disasters (geo-replication).
Whether your application requires read access to the replicated data in the secondary region during an outage in the primary region (geo-replication with read access).
The redundancy setting for a storage account is shared for all storage services exposed by that account. All storage resources deployed in the same storage account 
have the same redundancy setting. Consider isolating different types of resources in separate storage accounts if they have different redundancy requirements.



Types of redundancy:

1. Locally redundant storage: LRS stores three copies of your data within a single Azure datacenter in the same region. It protects against hardware failure inside
   that datacenter, but not against datacenter-wide outages.

   <img width="273" height="285" alt="image" src="https://github.com/user-attachments/assets/9f8b7bdc-879c-48f5-b08b-f8015f5844a5" />


2. Zone redundant storage: Zone-redundant storage (ZRS) replicates the data within your storage accounts to three or more Azure availability zones located in the
   primary region of your choice. Each availability zone is a separate physical location with independent power, cooling, and networking.

   <img width="503" height="501" alt="image" src="https://github.com/user-attachments/assets/867549e8-8d59-45a6-898d-cf45fa6651e0" />


3.Geo-redundant Storage: Geo-redundant storage (GRS) copies your data synchronously to one or more availability zones in the primary region using LRS. It then 
  copies your data asynchronously to a secondary region that is hundreds of miles away from the primary region

  <img width="731" height="299" alt="image" src="https://github.com/user-attachments/assets/85ac6503-e448-4b5f-9874-3e008581cb51" />


4. Geo-zone redundant Storage: Geo-zone-redundant storage (GZRS) combines the high availability provided by redundancy across availability zones with protection
   from regional outages provided by geo-replication. Data in a GZRS account is copied across three or more Azure availability zones in the primary region. In
   addition, it also replicates to a secondary geographic region for protection from regional disasters. Microsoft recommends using GZRS for applications
   requiring maximum consistency, durability, and availability, excellent performance, and resilience for disaster recovery.
With a GZRS account, you can continue to read and write data if an availability zone becomes unavailable or is unrecoverable. Additionally, your data also
remains durable during a complete regional outage or a disaster in which the primary region isn't recoverable.


<img width="960" height="541" alt="image" src="https://github.com/user-attachments/assets/40ce6fb7-f70d-4ac3-b19b-d058cf7e777b" />

