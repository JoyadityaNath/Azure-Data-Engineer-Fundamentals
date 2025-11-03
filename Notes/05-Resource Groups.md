Azure resource groups: A resource group is a container that holds related resources for an Azure solution. The resource group can include all the resources for the
solution or only those resources that you want to manage as a group. You decide how to allocate resources to resource groups based on what makes the most sense 
for your organization. Generally, add resources sharing the same lifecycle to the same resource group so you can easily deploy, update, and delete them as a group.

Reource Group Hierarchy


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



How to make a new resource inside a resource group:
1. Enter a resource group and just create a new resource for example: a storage account.

