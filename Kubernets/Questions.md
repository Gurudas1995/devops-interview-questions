# Kubernetes Interview Questions and Answers

---
### Table of Contents

<details open>
<summary>
Hide/Show table of contents
</summary>

| No. | Questions |
| --- | --------- |
| 1   | [What is Static Pods](#What-is-Static-Pods) |
| 2   | [What are the benefits of DevOps?](#what-are-the-benefits-of-devops) |
| 3   | [What is Continuous Integration?](#what-is-continuous-integration) |




1. ### What is Static Pods?
  This are special kubernetes pods managed directly by the kublet on a specific node, bypassing the Kubernetes API server, making them ideal for critical system components like control plane service (i.e. API server, controller-manager, scheular) that need to run locally on a node for bootstrapping or reliability, though they can't use standard API-driven management features.
  We define them via manifest files on the node's filesystem, and the kubelet watches this directory to start/stop/restart them, crating a "Mirror" pod in the API server for visibility, but not control. 

**[⬆ Back to Top](#table-of-contents)**

2. ### What exactly happens technically when you run `kubectl apply` against a Kubernetes cluster?
   When we execute `kubectl apply` the command parses YAML manifest file, checks the desired state(declared resources and configuration), and sends a RESTful API request to kubernetes API server. The server evakuates if the resource already exists.
   - If it exists, it performs patch operation to update only chnaged fields.
   - If it's new the server creates the resource from scratch. Controllers then reconcile the cluster towards the desired state, e.g.. deploying pods, services, etc.. changes are persisted in etcd, the backing data store.
  
3. ### What role do docker and kubernetes play in a microservice architecture?
   - Dokcer used to package microservices and their dependancies into portable container.
   - Kubernetes orchestrate containers, automating deployment, scaling and management and providing features like service discovery and load balancing.

4. ### What is Kubernetes? and why do we need container orchestration?
   Kubernetes is an open-source platform for managing, automating the deployment scaling and operation of containerized applications. container orchestration is necessary to manage large, dynamic environemnets with many containers across multiple hosts.
   main task of kubernetes:
   - Provisioning and deployments
   - Load balancing and service discovery
   - Resource allocation and monitoring
   - Self-healing

5. ### Explain key components of Kubernetes architecture.
   Kubernetes architecture is divded into two main parts: the **Control plane** and **Worker Nodes**.
   - **Control Plane Components**: Control plane makes global decisions about the cluster and responds to cluster events.
     - **Kube-apiserver**: The front end for the kubernetes control plane, all components communicate through it.
     - **etcd**: A consistent and highly-available key-value store as Kubernetes backing store for all cluster data.
     - **Kube-schedular**: Watches for newly created Pods with no assigned node and select a node for them to run on.
     - **Kube-controller-manager**: Runs controller process, such as the node controller and job controller.
     - **Cloud-controller-manager**: Links your cluster into your cloud provider's API.
   - **Worker node Components**
     - **kubelet**: An agent runs on each node in the cluster, ensuring that containers are running in a pod
     - **Kube-proxy**: A network proxy that maintains network rules on nodes, allowing network communication to your pods.
     - **Container runtime**: The software responsible for running containers like docker, containerd, CRI-O.
     
6. ### Have you upgraded any Kuberenetes cluster?
   Yes, I have upgraded Kubernetes clusters, typically following a structured, phased apporach to ensure zero downtime. Mainly i focus on taking `etcd` backups, upgrading the control plane first, followed by worker noded one-by-one using rolling update strategy. for self managed cluster I have used `kubeadm` and for EKS, AKS for managed clusters.
   - **Preparation and Pre-checks**:
     - Backup: Perform a full snapshots of etcd and backup critical manifests/configurations.
     - Version check: Review release notes for deprecated APIs and breaking changes. Kubernetes supports upgrading one minor version at a time e.g. 1.29 --> 1.30 --> 1.31
    - **Upgrade Procedure**
      - Control plane upgrade: Upgrade the first master node: update `kubeadm`, then run `kubeadm upgrade plan` and `kubeadm upgrade apply v1.x.y`. Upgrade remaining master node if applicable using `kubeadm upgrade node`.
      - Woker node upgrade:
        - *Cordon*: Mark node as unschedulable using `kubectl cordon <node-name>`.
        - *Drain*: Evacuate running pods to other nodes using `kubectl drain <node-name> --ignore-daemonsets`.
        - *Upgrade*: Update `kubelet` and `kubeadm` to the new version.
        - *Uncordon*: Bring the node back online using `kubectl uncordon <node-name>`.
        - *Repeat*: repeat for all worker nodes sequnetially.
      - Post-Upgrade Verification:
        - Verify nodes are running the new version using `kubectl get nodes`
        - Check that all system components like CoreDNS, Calico/Flannel, Ingress Controller are healty
        - Validate application functionallity using staging/smoketest traffic.
- **To upgrade managed Cluster(EKS/AKS/GKE)**: Use the cloud provider console or CLI to upgrade the control plane e.g `az aks get-upgrades --resource-group <RG_name> --name <Cluster_name> --output table`, and upgrade managed node groups sequentially to ensure availability.

7. ### How do you deploy an application in a kubernetes cluster?
      Deploying an application in kubernetes involves containerizing the app, pushing the image to a registry, and applying YAML manifests using `kubectl apply -f`. For complex apps, we can use Helm Charts or Kustomize for multi environements deployment. Key components of a standard application deployment is as below:
   - **Containerization**: The application is packaged into a Docker image and pushed to container registry. e.g. Docker hub. ECR, ACR etc.
   - **Kubernets Manifests(YAML)**:
     - **Deploument**: Defines the container image, replicas and update strategy.
     - **Service**: Provides networking to expose the application (internal or external)
     - **Ingress**: Manages external access to services, typically HTTP.
     - **Deployment Command**: `kubectl apply -f <filename.yaml>` is the standard command to create or update resources.
     - **Helm**: Use for managing complex applications by packaging YAMLs into charts for templating and easier version control.

8. ### How do you communicate with a jenkins server and a kubernetes cluster?
   Communication between Jenkins and Kubernetes is primarily achieved using the **Jenkins Kubernetes Plugin**, which enables Jenkins to dynamically provision agents as pods within the cluster. Secure integration is managed using **kubeconfig credentials**, **Service accounts** and **Jenkinsfile Pipelines** that use `kubectl` or helm to deploy applications.
   Key Aspects of integration:
   - Dynamic Agents: the Jenkins Kubernetes plugin provisions temporary worker pods in kubernetes to run build jobs, which are deleted upon completion, maximizing resource efficiency.
   - Authentication and Access: The Kubernetes API server is accessed using a `kubeconfig` file stored in Jenkins Credentials, or by assigning a service account with appropriate Role-Based Access Control permissions to the Jenkins pod.
   - CI/CD Pipeline Workflow: A `Jenkinsfile` is used to automate the process which involves code commit which triggers jenkins job, build and Push the docker image, and deployment using `kubectl apply` or `helm upgrade` inside jenkins pipeline to update kubernetes manifests.
   - Network Interaction: Jenkins connects to the kubernetes API server directly, while built applications use kubernetes services and ingress for deployment.

9. ### Do you only update docker images in kubernetes or do you also update replicas, storage levels and CPU allocations?
    **No**, we do not only update Docker images. A kubernetes deployment is a declarative way to manage the entire desired state of your application, and professional manitenance involves updating several key parameters to ensure stability and performance.
   - Key resources updated in kubernetes:
     - **Container images**: Changing the image tag is the most common way to roll out new application code or security pathces.
     - **Replics**: We frequently update the number of replicas to handle changes in traffic. This is done manually via `kubectl scale` or automatically through a HPA.
     - **CPU and Memory Allocations**: We must update resource `requests` and `limits` to prevent performance issues like CPU throttling or OOMkilled. Recent version like v1.27+ even support `in-place resizing` allowing you to update these without restarting the container.
     - **Storage Levels**: Storage requirements can be updated by expanding persistent volume claims (PVCs) if the underling storage class supports dynamic expansion.
     - **Configurations & Secrets**: We also update ConfigMaps and Secrets to change envirponment variables or application settings without rebuilding the docker image.
    
10. ### A node fails. What happnes to the pods managed by a deployment, and how does Kubernetes recover them?
  When a node fails in a K8s cluster, 
  - *node becomes NotReady*: The Kubelet on the node stops sending heartbeats to the control plane. After a default timeout (typically 5 min), the node status changes to NotReady.
  - *Pods are considered lost*: K8s automatically marks the Pods running on the failed node as `unavailable` or `unknown`. These Pods are not immediately deleted but are considerd non-functional.
  - *Deployment Ensure desired state*: The deployment controller detects that the number of available pods is below the desired replica count.
  - *New pods are scheduled*: K8s creates replacement pods on healty nodes to maintain desired state. these are fresh pods; they don't retain the state of the lost ones unless you're using persistent volumes or stateful workloads.
  - *Self-heaing in action*: so this is great example of `self-healing` capabilites; it monitors, detects, and automatically recovers from failures without manual intervention.

11. ### What are Persistent Volumes (Pvs) and Persistent Volume Claim (PVCs)?
    This are API objects that decouple storage provisioning from storage consumption, allowing data to persist beyound the lifecycle of a Pod. If a Pod crashes or is rescheduled to a differnet node, the data remains safe on the PV, whereas `emptyDir` or standard volume data is lost.Developers don't need to know the specific storage backend, they only need to know how much storage they need. allows for easy scalling and portability of stateful applications.
- **Persistent Volume**: A PV is a Piece of storage in the cluster that has been provisioned by an `admin` or dynamically provisioned using storage classes. It is a resource in the cluster, just like a node is a cluster resource. PVs have a lifecycle independant of any individual pod that uses them. It contains the actual storage implementation details such as NFS or cloud specific storage like AWS EBS, Azure Disk, GCE PD.
- **Persistent Volume Claim (PVC)**: A PVC is a request for storage by a user or developer. Similar to how pods consume node resources (CPU/memory), PVCs consume PV resources. It allows user to request specific size and access modes like `ReadWriteOnce`, `ReadWriteMany`. When a PVC is created, Kubernetes searches for a matching PV and binds them together in a 1:1 mapping.

12. ### What are init containers and when should we use them?
    Init containers are specialized containers that run and complete their tasks before the main application containers start within a Pod. They are designed specifically for initialization, configuration and setup, ensuring the environment is fully prepared before the primary application starts running.
- **Key Characteristics**
  - Run to completion: init containers must exit with success code 0.
  - Sequential execution: multiple init container run one by one in the order they defined.
  - Sequential Startup Block: if init container fails, K8s restarts it unitil it succeeds unless `restartPolicy` is set to `Never`.
  - Shared Volume: They can use shared volumes `emptyDir` to share data with the app container.
- Example: Uses `busybox` image to wait for `mysql` service to be ready and creates a shared config file.

13. ###  What is Kubernetes networking and how does it work?
Kubernetes networking is a flat, cluster-wide system ensuring all pods can communicate with each other without NAT. It works through a IP-per-pod-model, where every pod receives a unique IP, and networking plugins (CNI) facilitate between pods, nodes and external services.
- *IP-per-pod Model*: Every pod gets its own IP address, allowing contianers within a pod to share the same network namespace and IP.
- *Container Network Interface(CNI)*: Plugins like callico, flanner are used to configure the network, providing connectivity across nodes.
- *Service Discovery and Load Balancing*: Kubernetes Services provide stable Ips and DNS names for sets of pods, load-balancing traffic among them.
- *Ingress-Controllers*: Ingress handles external access to services, directing HTTP/HTTPS traffic to internal services using routing rules.
-*Pod Communication*:
  - *Pod-to-Pod*: Direct IP communication or via CNI (for cross node)
  - *Pod-to-Service*: Traffic sent to a stable ClusterIP, Which load-balances to backing pods.
  - *External-to-Pod*: Via NodePort, LoadBalancer or Ingress.

14. ### What is Role-Based-Access-Control (RBAC) in Kubernetes?
    It is a security mechanism that regulates access to the API server based on roles assigned to users, groups or service accounts. It controls `who` can perform `what` actions on `which` resources within a specific namespace or the entire cluster.
- **Key Components**:
  - **Role and ClusterRole**: Defines permissions like what actions are allowed on which resources. A `Role` is namespaced, while `ClusterRole` is cluster wide.
  - **RoleBinding and ClusterRoleBinding**: Assigns roles to specific subjects like users, groups or service accounts. A `RoleBinding` grants access within a specific namespace; a `ClusterRoleBinding` grants access cluster-wide.
Some Built-in roles are **Cluster-admin** (Superuser), **Admin**(Namespace admin), **edit**(read/write) and **View**(read-only).

15. ### How does Kubernetes autoscaling work?
    - **Types of Autoscaling**:
      - **Horizontal Pod Autoscaler(HPA)**: Scales the number of pod replicas in a Deployment. replicaSet or StateFulSet based on CPU/ Memory usage or custom metrics. HPA runs as control loop (15s default interval). It fetches metrics from the **Metrics Server**, compares current utilization against a target, and calculates the necessary replica count. If 5 pods are at 75% CPU and the target is 50%, HPA increases the pod count.
      - **Vertical Pod Autoscaler(VPA)**: Adjusts the resource requests and limits (CPU and memory) of a container in a pod, making pods larger or smaller rather than adding more. VPA has a Recommender (monitors usage) and un Updater (terminates pods to update their resource requests). VPA right-sizes pods based on actual usage.
      -  **Cluster Autoscaler (CA)**: Scales the number of nodes in the cluster. It adds nodes when pods cannot be scheduled due to insufficient capacity and removes nodes when they are underutilized. It works with cloud provider APIs like AWS autoscaling groups, Azure VMSS or GKE to provision or remove infrastructure.
     
16. ### What is difference between a Pod and Container in Kubernetes?
    - Container - is a single isolated application process unit.
    - Pod - is a smallest deployable unit in kubernetes, a higher abstraction that acts as logical host for one or more containers that are tightly coupled and share the same network namespaces and storage volumes.

17. ### How does kubernetes handle service discovery and load balancing?
    Load balancning and service discovery primarily handled through a resoruce called **service**, which provides a stable network endpoint for a dynamic set of ephemeral pods.
    - **Service Discovery: CoreDNS**: Every service created in the cluster is automatically assigned a DNS name (e.g. `my-service.namespace.svc.cluster.local`). Pods can reach each other using this human-readable names instead of IPs.
    - **Load Balancing:Kube-proxy**: The Kube-proxy daemon running on every node manages traffic routing. it uses either `iptables` or `IPVS` to intercept traffic sent to a service's virtual IP and distribute it across healthy backend pods, typically using a round-robin algorithm.
   
18. ### 
      
    
