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
   - Docker images: To
   

