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

