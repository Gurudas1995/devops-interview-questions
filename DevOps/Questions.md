# DevOps Interview Questions and Answers

---
### Table of Contents

<details open>
<summary>
Hide/Show table of contents
</summary>

| No. | Questions |
| --- | --------- |
| 1   | [What is DevOps?](#what-is-devops) |
| 2   | [What are the benefits of DevOps?](#what-are-the-benefits-of-devops) |
| 3   | [What is Continuous Integration?](#what-is-continuous-integration) |



1. ### What is DevOps?

    DevOps is a set of practices that combines software development (Dev) and IT operations (Ops). It aims to shorten the systems development life cycle and provide continous delivery with high software quality. DevOps is complementory with agile software development; severla DevOps aspects came from agile Methodology.

**[⬆ Back to Top](#table-of-contents)**

2. ### What are the benefits of DevOps?
     The main benefits of DevOps include:

   1. Faster delivery of features
   2. Mmore stable operating environments
   3. Improved communication and collaboration
   4. More time to innovate ( rather than fix/maintain)
   5. Reduced deployment failures and rollbacks
   6. Shorter mean time to recovery
  
**[⬆ Back to Top](#table-of-contents)**

3. ### What is Continous Integration?
    Continous Integration (CI) is a development practice where developers integrate code into shared repository frequesntly, preferably several times a day. Each integration can then be verified by an automated build and automated tests.

   Key aspects of CI:
   - Maintaining a single source repository
   - Automating the build
   - Making the build self-testing
   - Everyone commits to the baseline every day
   - Every commit builds on an integration machine
   - Keep the build fast
   - Test in a clone of the production environment
   - Make it easy to get the lastest deliverables
   - Everyone can see the results of the latest build
   - Automate deployment

**[⬆ Back to Top](#table-of-contents)**

4. ### What is landing zone?
   A cloud landing zone is a pre-configured, secure and scalable multi-account environment designed as the foundation for running workloads in providers like AWS, Azure or GCP. It includes built-in identity management, network configuration, security and governance to allow organization to deploy applications quickly and safely.

5. ### what is sesmic zone?
   A seismic xone in the context of cloud computing referes to a geographic area where data centers are located, classified by their susceptibilty to earthquake risks, ranging from low to high. Data centers in these zones muct implement specialized fondations, to ensure service continuity, high availability and physical protection of infrastructyre during seismic events.

6. ### What is serverless Computing and how does it differ from traditional cloud computing?
   Serverless computing refers to a cloud computing execution model in which the cloud provider manages the infrastructure and automatically allocates resources as needed. Serverless computing eliminates the need for server management, allowing developers to focus on writing and deploying code.\ AWS Lambda, Azure Functions which allows you to run code without provisioning or managing servers. we can write functions in various language and service automatically scales and manages infrastructure required to run your code based on incoming requests.

7. ### How would you choose between serverless and traditional (VM) for given application?
   This choice is depends on specific requirements of applications. Serverless is beneficial for applications with variable workloads and intermittent usage patterns. Specially suitable for event-driven architecture and microservices.\Traditional VMs provide more control and flexibility, making them better choice for steady workloads.

8. ### How would you implement security best practices in serverless environment?
 - By ensuring secure configurations, managing access controls and using encryptions.
 - Closely manage permissions and roles for serverless functions to ensure that only authorized users have access.
 - Use environment Specific secrets or key managment services to protect sensetive information.
 - Regularly update the serverless platform and its underlying dependencies to address any security vulnerabilites.

9. ### What are main advantages and disadvantages of using serverless computing?
- *advantages*
   - Automatic scaling
   - Pay-per-use Pricing
   - Reduced operational overhead
- *Disadvnatgaes*
   - Cold start latency
   - limited execution time

10. ### 

   
