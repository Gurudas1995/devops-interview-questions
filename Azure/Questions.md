# Azure Interview Questions and Answers

---
<details>
<Summary>1. ### What is an Azure Stroage Account? </Summary>  
  A container that provides a single namespace for all Azure Storage data objects, including blobs, files, queues and tables. It provides durability, scalability and High availability. We can store upto 500TB of data.
  - **Blob Storage**: Unstructured data (images, documents).
  - **File Storage**: Shared file systems (SMB/NFS)
  - **Queue Storage**: Messaging for workflow processing.
  - **Table Storage**: NoSQL key-attribute storage.
</details>

2. ### What are the different storage tiers available?
   - **Hot tier**: High-access, Higher storage cost, lower access cost. Designed for data that is accessed frequently.
   - **Cool**: Infrequent access. lower storage cost, high retrieval time. Designed for infrequnetly accessed data but stored atleast 30 days.
   - **Archive**: Rare access, lowest storage cost, high retrieval time. Designed for rarely accessed data and stored for long periods.

3. ### Explain the different Azure Blob types
   - **Block Blobs**: Used for text/binary files, documents, media files(190TB)
   - **Page Blobs**: Userd for random read/write, specifically for Azure VM disks (8TB)
   - **Append Blobs**: Optimized for append operations, ideal for logging.

4. ### How do you Secure an Azure storage account?
   Using shared access keys (Account Keys), Shared Access Signatures (SAS), with Entra ID authentication, firewall rules and virtual networks. Data is encrypted at rest and in transit.

5. ### What is SaS?
   Shared Access Signature (SaS) A URI that grants limited, temporary access rights to specific storage resources without sharing primary access keys. Service SAS delegates access to single service (Blob, File, Table or Queue). An Account SaS delegates access to resources across multiple services.
  
7. ### Explain different types of storage redundancy.
   - **LRS (Locally Redundant Storage)**: 3 copies in single data center. Cheapest
   - **ZRS (Zone-Redundant Storage)**: Data is replicated across three different Availability zone within a single AZ.
     - **G-ZRS**: Data replicated across two regions. primary region using ZRS and secondary region using LRS. 
   - **GRS (Geo-Redundant Storage)**: LRS +3 copies in a secondary paired region.
     - **RA-GRS**: Read access to the secondary region of GRS.
       
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
   
22. ### What is Vnet?
    It's an user created isolated network in cloud. We can accomplish communication with internet, between azure resources and with on-prem resources.
    - Vnet Ranges:
      - `10.0.0.0 - 10.255.255.255` - Class A
      - `172.16.0.0 - 172.31.255.255` - Class B
      - `192.168.0.0 - 192.168.255.255` - Class C
23. ### How many IP's Are reserved in each subnet?
    Total 5 IP's are reserved in each subnet i.e. first four and last one.
     - 1st: Network address
     - 2nd: Default Gateway
     - 3rd and 4th: Reserved to map azure DNS IP to Vnet
     - Last: Network broadcast

24. ### What is Vnet Peering?
    By default 2 Vnet cannot communicate with each other. To enable communication between them we need to create peering between two Vnet. Peering between two Vnet withing same region knon as `local peering`, while Peering between Vnet in differnet region is known as `Global Peering`. Both Vnet address space should be different.

25. ### Difference between `Availability Zone` and `Availability Set`.
    - **Availability zone**: Isolated locations within a region, designed to provide high availability and fault tolerance, also known as `data center`. Protect against entire data center failure.
    - **Availability Set**: It is ensure that Azure VM are created across different physical rack and host in same data center. Protect against hardware failure within single datacenter.

26. ### Explain Fault domain and update domain.
    `Fault domain` prevent single points of hardware failure, while `update domain` allow update without disrupting VMs in availability set.

27. ### What is Managed disk in Azure?
    This are block level storage volumes that are managed by Azure and used with VM. Managed disk are like physical disk in on on-prem server, but they are virtualized. Available types of managed disks are ultra-disks, premium solid state disk(SSD), Standard SSD, and standard Hard disk drive (HDD).

28. ### What is Network Security Group (NSG)?
    It is act as virtual firewall used to filter in and out traffic. It can be attached to subnet of VM or NIC of VM or both. When it attached to subnet then all NSG rules applied to VMs in that Subnet.

29. ### What is Azure Cost Management?
   - This Azure resource helps organizations to monitor control and optimize their spending on Azure resources.
   - It helps to define budgets for subscriptions, resource groups or specific resources and set alerts to notify you when spending approaches or exceeds limits.
   - Identify underutilized resources and resize, stop or delete them to reduce Cost.
   - Pre-purchase reserved instances or savings plans for VMs or databases.

30. ### Explain VM Scaleset in Azure.
    VM Scaleset provide high availability and ability to handle increase and decrease in demand. You can also use your custom image you need to set minimum and maximum VM count for autoscaling to happen.

31. ### What are basic, standard and Gateway Load Balancer?
    - **Basic Load Balancer (Retired-2025)**: Designed for small scale applications  that do not require high availability, zone redundancy, or strict security, often used for development or testing.
    - **Standard Load Balancer**: A high-performance secure, and reliable option suitable for production workloads. It supports zone-redundant, cross-region traffic routing, and features a 'scure by default' model. (NSGs are required)
    - **Gateway Load Balancer**: Specifically designed to transperently deploy, scale and manage third-party Network Virtual Appliances, such as firewalls or deep packet inspection tools, without requiring complex traffic engineering.

32. ### Explain Azure Traffic Manager.
    It is DNS based traffic load balancer. It controls traffic distrubution to ensure low latency access and provide failover support. Traffic manager uses DNS to direct client requests to the appropriate endpoints based on traffic routing method as below:
    - **Weighted**: Client traffic is load balanced across multiple endpoints. Higher number means more weight, more traffic on that endpoints.
    - **Performance**: Client traffice is routed to lowest latency endpoint
    - **Priority**: Traffic sends to primary endpoints, if that fails, traffic redirected to secondary endpoint.
    - **Geographic**: Traffic routed based on geographic location.
    - **Multivalue**: configured multiple healthy endpoints and client can choose any of them to send traffic.
    - **Subnet**: Client traffic is sent to Specific iendpoints based on source public IP subnet.

33. ### What is route table in Azure?
    Route tables controls how traffic is directed in Vnet. It contains set of rules called routes. User defined routes are known as `UDR`.

34. ### What is NAT Gateway?
    It Provides internet connectivity to a VM. This can be used where users not want to provide individual public IP to VM but they want outbound internet access from VM. NAT gateway do not support inbound connections coming from internet to VM.

35. ### What is Azure CDN?
    Content Delivery Network improves performance by caching content at edge locations globally and improves content load times.

36. ### What is Azure Backup?
    It is cloud based service that provides reliable backup and restore capabilities for your VM. It helps to protect your critical data from accidental deletion, corruptions or ransomware. Using this we can perform complete VM restore.

37. ### What is instant restore data and soft delete in Azure backup?
    - **Instant Restore**: This allows restoring data from backup snapshot instantly which helps in reducing restore times.
    - **Soft Delete**: It retains deleted backups for a configurable period, protecting against accidental deletions.

38. ### What is Azure Site Recovery?
    Azure Site Recovery offers disaster recovery by replicating resources between primary and secondary regions.

39. ### Explain the difference between failover and failback?
    - Failover: Switches to a secondary locations
    - Failback: Returns services to the original primary location.

40. ### What is Azure Monitor?
    It helps to understand how VMs and applications are performing and proactivelly identify issues and helps in responding to critical situations that may affect them.

41. ### What is Azure Expressroute?
    It is private, dedicated connection between Azure and on-prem infrastructure for faster and secure data transfer

42. ### What is site-to-site VPN?
    It connects on-premises networks to azure Vnet over the internet, allowing secure communication between environment.

43. ### What is an Azure Bastion?
    azure Bastion provides secure RDP and SSH access to VMs without exposing them to public internet.

44. ### What is Point-to-site VPN?
    Allows individual clients to securely connect to azure Vnet from there devices.

45. ### What are the main types of Azure support plan?
    - **Basic**: Free for general billing and subscription support.
    - **Developer**: For trial and non-prod environment.
    - **Standard**: For prod workload with faster response times.
    - **Professional Direct**: For buisness critical workloads with proactive guidance.

46. ### What are Azure Tags?
    Azure tags are lables (Key Value Pair) that can be applied to azure resources for better organization, tracking and cost management. tags help to categorize resources by department, environment or by project and help to manage them effectively.

47. ### How does priority work in Azure NSG?
    NSG rules in Azure have a priority number between 100 and 4096. Lower the number, higher the priority and it get processed first. This allows admin to control the order of rule execution.

48. ### What is Application Security Groups (ASG)?
  ASG allow you to create a group and add virtual machines to that group. so we can use this group in inbound and outbound rule of NSG.
  We can call this group multiple times in NSG. and do not need to add individual IP addresses in NSG. we can add and remove VMs from ASG anytime.

49. ### What is snapshot in Azure?
    Snapshot is point-in-time backup of a disk in Azure. It's used to quickly backup VM disk before performing changes, allowing for easy recovery if needed.

50. ### What is host caching in Azure?
    Host cachin temporarily stores frequently accessed data on the VM's local storage to improve read/write performance. It's commenly used for OS and data disks on VMs, with options like read-only or read/write for better performance in scenario like db applications.

51. ### 
    
    
    

