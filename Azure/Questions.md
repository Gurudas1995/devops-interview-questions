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

10. 

