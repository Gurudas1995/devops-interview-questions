# Jenkins Questions and Answers

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



1. ### How do you design a scalable Kenkins architecture for 100+ microservices?
 - #### High-Level Architecture (What)
   I would implement a Jenkins on Kubernetes architecture using a Master-Agent model. the core philosophy is to treat the master as a lightweight coordinator and ephemeral containerized agents for the actual build workload.
    - Jenkins Controller (Master): Deployed on Kubernetes, utilizing a persistent volume like EBS for `/var/jenkins_home`.
    - Dynamic Agents (Build Nodes): Instead of static slaves, I would use the Kubernetes plugin to dynamically provision agent pods on-demand and terminate them immediately after the build completes.
    - Infrastructure as Code (IaC): Everything like plugins, configurations, credentials and job definations will be managed via Configuration as Code.

- #### Strategy for 100+ Microservices (How):
  To Manage over 100+ microservices without creating a chaotic, unmanageable system, I would use following strategies:
  - Multibranch pipelines & Jenkinsfile: Each microservice repository will contain a `jenkinsfile`. Jenkins will be configured to automatically discover branches and build them, ensuring the build logic lives with the code.
  - Jenkins shared Libraries: To avoid code duplication across 100+ jenkinsfiles, I would create a Shared Library to standardize build, test and deploy stages.
  - Job DSL Plugin: I would use Job DSL Plugin to programmatically create Jenkins jobs via code rather than creating them manually via the UI.
  - Controller Division: If needed, I would divide the 100+ services among 2-3 master nodes, organized by functional domain or business unit to reduce the load on single controller.
- #### Key Scalability Features
  - Elastic Scaling: Since agents are kubernetes pods, the cluster autoscaler will add node capacity to AWS/AZURE based on the number of concurrent builds.
  - Docker-in-Docker: Agents will buold Docker images for microservices and push them to a registry like ECR or Dockerhub.
  - Webhook Triggers: Instead of polling, I would use GitHub Webhooks to trigger builds, ensuring near-instantaneous pipeline initiation.
  - Robust Monitoring: I Would implment Prometheus and Grafana to Monitor build times, queue times, and agent health.
 
2. ### How would you implement dynamic agents using docker or Kubernetes?
   To implement dynamic agents, specifically for CI/CD like jenkins I would prefer Kubernetes over raw Docker because it offers native auto-scaling and self-healing.
 - Define agent pod Templates: I would create container images containing all necessary build tools (Maven, Docker CLI, kubectl)
 - Configure Kubernetes plugin: In the Jenkins/CI tool, I would configure the Kubernetes cloud plugin, pointing it to my K8s cluster API URL.
 - Implement Pod Template Specs: Within the plugin, I would define `podTemplates` using YAML, specifying the container image, necessary environment variables, and resources(CPU/memory requests and limits)
 - Ephemeral Agents (Dynamic Scaling): The key is setting the agent to be ephemral, running as a pod, performing the build, and then automatically deleting itself.
 - Leverag Horizontal Scaling: I would configure kubernetes to scale up new agent pods based on demand and scale down to zero when idle.

3. ### If a pipeline succeeds but deployment fails in production, how do you handle rollback?
   I immediately prioritize  service restoration using automated rollback procedure to the last known stable version like `kubectl rollout undo` or reverting the artifact in CI/CD. Then I will perform following actions:
 - Immediate Action (Rollback): Use pre-configured automation like jenkins or AWS CodePipeline to quickly revert to the previously successful version.
 - Assessment & Communication: Evalaute the scope of the impact and notify stakeholders promptly to minimize the panic.
 - Root Cause Analysis (RCA): Review logs, monitoring tools like prometheus/ Grafana and recent changes to understand why it failed, even if the pipeline tests passed.
 - Preventive measures: Use Canary or Blue-Green Deployments to reduce blast-radious of failures
 - Database Considerations: Ensure that database changes are backward-compatible so that a code rollback doesn't break data integrity.

4. ### How do you secure secrets in jenkins?
   I secure secrets in Jenkins primarily using the built-in `Credential Plugin` which encrypts sensetive data at rest using AES. I never hardcode credentials in Jenkinsfiles; instead, I stored them securely in the controller and use Credential Binding to inject them as environment variables only during runtime. For higher security, I intefrate external tools like Hashicorp Vault, AWS Secrets Manager or Azure Key Vault to fetch secrets dynamically.
   - masking: Jenkins automatically masks secrets in console output using `****`.
   - Restricted Access: I ensure only authorized users have  access to Manage or use specific credentials via RBAC.
   - File Security: I verify that the `$JENKINS_HOME/secrets/` directory has strict permissions, allowing only the Jenkins system user to read them.
  
5. ### How would you implement parallel stages efficiently?
   Implementing parallel stages efficiently is about reducing the feedback loop without choking CI?CD infrastructure. My Approach focuses on three main pillars: Independancy, Resource Management and Visibility.
 - Identify Independant Tasks: First, I identify tasks that don't depend on each other, such as running tests across different modules, frontend vs. backend linting, or building Docker images for differnet microservices.
 - Use Native pipeline directives: In Jenkins, I use the parallel directive within a Declarative Pipeline, or `matrix` strategies in GitHub Actions. This allows me to run, for instance, testing on multiple OS environments simultaneously.
 - Ensure Proper Resource Allocation: To prevent one massive job from hogging all available runners, I use specific lables or node pools for parallel stages to distribute the load across agents, rather than running everything on the master/controller.
 - Implment `failfast`: I always add `failfast:true` to the parallel block. if one test fails, it cancels the other parallel tasks immediately, saving compute resources and letting the developer fix the issue faster.
 - Cache Dependencies: To maximize efficiency, I use caching for dependencies ( like node_module or maven repository) so that parallel jobs don't spend time re-downloading the same packages.
 - Example code snippet:
   ```
   stage('Test') {
    parallel {
        stage('Unit Tests') {
            steps { sh './run-unit-tests.sh' }
        }
        stage('Integration Tests') {
            steps { sh './run-int-tests.sh' }
        }
    }
}
  ```

6. ###
     
