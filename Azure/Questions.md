# Azure Interview Questions and Answers

---
### Table of Contents

<details open>
<summary>
Hide/Show table of contents
</summary>

| No. | Questions |
| --- | --------- |
| 1   | [List All git Commands.](#List-All-git-Commands) |
| 2   | [What are the benefits of DevOps?](#what-are-the-benefits-of-devops) |
| 3   | [What is Continuous Integration?](#what-is-continuous-integration) |



1. ### What is an Azure Stroage Account?
  A container that provides a single namespace for all Azure Storage data objects, including blobs, files, queues and tables. It provides durability, scalability and High availability.
  - *Blob Storage:* Unstructured data (images, documents).
  - *File Storage:* Shared file systems (SMB/NFS)
  - *Queue Storage:* Messaging for workflow processing.
  - *Table Storage:* NoSQL key-attribute storage.

2. ### What are the different storage tiers available?
   - Hot: High-access, Higher storage cost, lower access cost.
   - Cool: Infrequent access. lower storage cost, high retrieval time.
   - Archive: Rare access, lowest storage cost, high retrieval time.

3. ### Explain the different Azure Blob types
   - Block Blobs: Used for text/binary files, documents, media files(190TB)
   - Page Blobs: Userd for random read/write, specifically for Azure VM disks (8TB)
   - Append Blobs: Optimized for append operations, ideal for logging.

4. ### How do you Secure an Azure storage account?
   Using shared access keys (Account Keys), Shared Access Signatures (SAS), with Entra ID authentication, firewall rules and virtual networks. Data is encrypted at rest and in transit.

5. ### What is SaS?
   Shared Access Signature (SaS) A URI that grants limited, temporary access rights to specific storage resources without sharing primary access keys. Service SAS delegates access to single service (Blob, File, Table or Queue). An Account SaS delegates access to resources across multiple services.
  
7. ### Explain different types of storage redundancy.
   - LRS (Locally Redundant Storage): 3 copies in one data cneter.
   - ZRS (Zone-Redundant Storage): 3 copies across availability zones.
   - GRS (Geo-Redundant Storage): LRS +3 copies in a secondary paired region.
     - RA-GRS: Read access to the secondary region of GRS.
       
8. ### What is soft delete in Azure Blob Storage?
   A feature that allows for the recovery of deleted blobs, containers, or snapshots within a specified retention period, protecting against accidental deletion.

9. ### How do you handle high performance requirements?
    By using premium storage account for lower latency, using multiple containers for parallel uploads, and enabling geo-redundancy to balance read loads.

10. ### What is Azure Resource Group?
    It is a logical container with related resources for an azure solutions allow to manage and organize related resources efficiently and share same lifecycle.

11. ### What is Azure lock?
    It is very beneficial when you want to prevent accidental deletion and modification. We can apply Azure locks at subscription, resource groups or individual resource.
     - **Read Only**: Authorized users can only read the resources, but cannot delete or modify.
     - **Delete**: Authorized users can read and modify but cannot delete resources.

12. ### Explain RBAC in Azure.
    Azure RBAC provides fine-grained access to azure resources. You can segregate duties within your team and grant users only the access they need to perform their job. Job roles are predefined sets of permission RBAC allow to grant specific permissions to users or groups at particular scope. Roles such as contributor, Owner, Reader, RBAC administrator.

13. ### What is Azure Policy?
    It is service that helps you enforce the organizational standards and assess compliance at scale.
    
14. ### What is Microsoft Entra ID? and explain microsoft entra ID role.
    It is microsoft's cloud based identity and access management service. It provides authentication and authorization for users, groups and applications.
    If another admin or non-admin needs to manage microsoft entra ID, we can assign a entra ID role that provides the permission they need. The top role is `Global Admin` which can manage all aspects of microsoft entra ID.

15. ### How do you add custom domain to microsoft entra ID?
    Microsoft entra ID comes with default domain i.e. `example.onmicrosoft.com`. Custom domain can be added to microsoft entra ID by verifying domain ownership.

16. ### What is Microsoft Entra ID connect?
    Syncs on-premises AD with Microsoft entra ID, enables unified identity across cloud and on-premises.

17. ### What is difference between an entra ID role and RBAC role?
    - **Entra ID Role**: It is directory role for manageing entra ID itself.
    - **RBAC Role**: Roles for managing Azure resources
    `Privildged Managed Identity (PIM)` handles both roles, instead of users having permanant active assignment. PIM makes them eligible for roles.

18. ### What is Azure VM?
    Azure VM is an IaaS offering that allow you to create and manage virtual machines in cloud. You have full control over Operating System and can install and run any software on VM. Gives flexibility of virtualization without need to buy and maintain physical hardware, allow you to choose wide range of configurations including various sizes, OS etc.. You still need to maintain the VM by configuring, patching and installing the software that runs on it.

19.  ### What is Azure boot diagnostic?
    Enables user to observ state of VM as it is booting up by collection serial log information and screenshots. It helps to diagnose boot failure if VM gets into non-bootable state. It's enabled by default while creating VM.

20. ### What are the major Azure VM Family?
  - **General Purpose**: Provide balanced `CPU-to-Memory` ratio. Ideal for testing and development, small to medium databases and low to medium traffic web servers. **e.g. D-series, B-series**
  - **Compute Opitimized**: Provide high `CPU-to-Memory` ratio. Suitable for medium traffic web servers, network appliances, batch processes and application servers. **e.g. F-series**
  - **Memory Optimized**: Offer high `Memory-to-CPU` ratio. Great for relational databases, medium to large cache, in-memory analytics. **e.g. E-series, M-series**
  - **Storage Optimized**: Offer high disk `throughput and I/O`. Ideal for bigdata, SQL, NoSQL databases, data warehousing and large databases.
  - **GPU Optimized**: Specially designed for compute intensive and graphic intensive workload.
  - **HPC Optimized**: For various HPC workloads such as rendering, weather simulation and financial risk analysis.

21. ### Difference between 'stopped' and 'stopped deallocated'.
    - **Stopped VM**: In this state OS is powered off, but underlying compute resources (CPU and Memory) are still allocated and reserved for that VM. Since Compute resources are still allocated, you continue to incure cost for that Vm
    - **Stopped Deallocated**: Indicates the VM has been powered off and all its compute resources have been released back to azure.
   
22. ### 

