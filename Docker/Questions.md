# Git & GitHub Interview Questions and Answers

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



1. ### how do containerisation techniques like Docker and Kubernetes simplify application deployment and management?
  Containerization with tools like docker and kubernetes simplifies application deployment and management by providing consistency across environments, enhanced portability, automatic scaling, and self-healing capabilites.
  *Docker streamilines the process of packaging applications into standardized, isolated units called containers*
  - Consistency: Docker packages an application with all its dependancies, libraries, and configurationsinto a simple image, which elimiates the common "it works on my machine" problem by ensuring consistent behaviour across development, testing and production environments.
  - Poratbility: Containers are lightweight and share the host OS kernel, allowing them to run on any system that has docker installed, regardless of the underlying infrastructure.
  - Isolation: Each application runs in an isolated container, preventing conflicts with other applications and enhancing overall system stability and security.
  - Simplified workflows: Docker simplifies the setup process, enabling rapid prototyping and testing and integrating seamlessly into CI/CD and deployment process.

*Kubernetes is an open-source orchestration platform that automates the deployment, scaling and operational management of containerized applications at scale across a cluster of machines.*
 - Automated Deployment & scaling: Kubernetes automates the distribution and scheduling of containers across a cluster based on resource requirements and demand. It can automatically scale the number of application instances up or down to meet fluctuating traffic.
 - Self-healing: It continuously monitors the health of containers and nodes, automatically restarting or replacing failed containers and rescheduling them on healthy nodes, which ensures high availability and resilience without manual intervention.
 - Load Balancing and Service Discovery: Kubernetes provides a stable network endpoint (service) and load balances traffic across multiple container instances, ensuring that no single container is overwhlmed and enabling reliable communicartion within the application.
 - Declarative Management: Users define the desired state of their application using configuration files in YAML and kubernetes works to maintain that state, simplifying complex updates and making rollbacks easy to execute if something goes wrong.
