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

3. ### What is Continous Integration, Continous Delivery and Continous deployment?
    **Continous Integration (CI)** is a development practice where developers integrate code into shared repository frequesntly, preferably several times a day. Each integration can then be verified by an automated build and automated tests.

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

   **Continous Delivery(CD)** Extends CI by automatically preparing code changes for prod release after building and testing which requires manual approval
   **Continous Deployment(CD)** An advancement in software release where code changes passing all tests without manual intervention gives faster feedback loop.

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

10. ### What methods do you use to check for code vulnerabilites?
    Methods for checking code vulnerabilities include automated scanning (SAST, DAST, IAST), Software Composition Analysis (SCA) to check dependancies, and manual code reviews.
- Static Application Security Testing (SAST): Analyzes source code, binaries without executing the application to find vulnerabilities early in the development lifecycle. Tools like Snyk Code are commonly used.
- Dynamic Application Security Testing (DAST): Tests running applications from the outside by simulating attacks (e.g. inputtng malicious data) to detect runtime issues like SQL injection or XSS.
- Interactive Application Security Testing (IAST): Combines SAST and DAST, monitoring application behaviour from within during testing to identify vulnerabilities.
- Software Composition Analysis (SCA): Scans open source dependancies and liabraries to identify known vulnerabilites (CVEs) and outdated packages.

11. ### How does CI/CD pipeline works end-to-end in context of Azure DevOps pipeline?
    In Azure DevOps, the pipeline is often defined in a YAML file stored in the repository (Pipeline-as-code) or configured via a classic editor in the UI.
 - Source - Code is committed to a repositroy (azure Repos, GitHub).
 - Trigger - Azure pipelines automatically detects the change and starts the defined pipline.
 - CI (Build/test) - The pipeline runs jobson hosted or private agents to compile the code, run tests and create artifacts. It can integrate with various languages and platforms (node.js, Python, Java).
 - CD (Deploy) - The resulting artifacts are stored (e.g. in ACR for docker images) and then deployed to target Azure environments like Azure App Service, kubernetes using release pipelines.
 - Approvals: Optional pre-deployment approval gates can be set to require human validation before deploying to production.

12. ### How do you design a production-grade CICD pipeline?
    Designing a production-grade CICD pipeline involves implementing a highly automated, secure, multi-stage workflow with robust quality gates and monitoring.
- **Core Principles**
   - Automation: Automate every repetitive task to minimize manual errors
   - Version Control: Store all application code, IaC and pipeline configuration in a unified version control system like GitHub.
   - Consistency: Use containers to ensure a consistent environment across development, testing, staging and production.
   - Security: Integrate security scans (SAST, DAST, Snyk, Trivy) early in pipeline and manage secrets securely.
   - Observability: Implment centralized monitoring and logging like Dynatrace, Prometheus and Grafana to quickly detect and troubleshoot issues in production.
   - Reliability: Design for failuer with rollback mechanisms and manual approval gates for critical stages.
 - **Key Stages**
   - **Source Stage**:
     - Trigger: The pipeline is automatically triggered by a code commit or a pull request to the version control system.
     - Checks: Implement pre-commit hooks locally and branch protection rules in the repository to enforce formatting and basic linting before the code even enters the pipeline.
  - **Build Stage (CI)**
    - Compile: Compile the source code and manage dependencies.
    - Artifact Creation: Package the application into an immutable artifact, such as Docker container image.
    - Static Analysis: Perform static code analysis and security scans on the codebase and container image.
  - **Test Stage**:
     - Unit Test: Run fast-running unit tests on the build artifact.
     - Integration Tests: Run more extensive intgration and end-to-end tests in a dedicated testing environment.
     - Quality Gates: The pipeline fails if code coverage drops below a certain threshold or tests fail, providing quick feedback to developers.
 - **Staging Stage (CD)**:
   - Deploy to staging: Automatically deploy the artifact to a staging environment that mirrors production
   - Acceptance Testing: Run user acceptance tests or manual review for final validation.
   - Manual Approval: Implement a manual approval gate before deploying to production.
- **Production Stage (CD)**
   - Deploy to Production: Deploy the approved artifact to the production environment using robust strategies like blue-green or canary release.
   - Configuration: Manage environment-specific configurations and secrets securely using dedicated tools or built-in CI?CD features.
- **Monitoring and Feedback**
    - Observe: Continuously monitor the application in production for performance metrics and errors.
    - Feedback loop: Ensure automated feedback loops (notifcations, alerts) are in place to notify the team about issue immediately, enabling quick resolution and potential automated rollbacks.
 
13. ### What are some common diployment stratagies?
    - **Blue-green Deployment**: Two identical environments (Blue and green) run concurrently, traffic is instantly switched to the new version minimizing downtime and enable easy rollback
    - **Canary Deployment**: A new version is released to a small group of users initially, if stable, it's progressivelly rolled out to more users, mitigating the impact of potential failure.
    - **A/B Testing**: Different usrs see different versions at same time. this is used to compare new features.
    - **Rolling Update**: Gradually replace old version with newer version, keeping the service available.
    - **Recreate**: Completely removing old version, then start new version. this encure downtime.
    - **Shadow Deployment**: Duplicate real traffic to shadow version users only see the old version. use to test performance scaling silently in production conditions.
   
14. ### How do you handle a pipeline failure and ensure a quick recovery?
    - Implement monitoring and alerts for immediate failure detection
    - Analyze logs to identify the root cause
    - For prouction automate rollbacks using blue-green or canary or rolling update deployment stratagies.
    - Allow for quick restoration of the previous version.
    - store artifacts and IaC codes in source code repositories like GitHub.

15. ### What are the different types of cloud services?

    The main types of cloud services are:

    1. **IaaS (Infrastructure as a Service):**
       - Provides virtualized computing resources
       - Examples: AWS EC2, Azure VMs

    2. **PaaS (Platform as a Service):**
       - Provides platform allowing customers to develop, run, and manage applications
       - Examples: Heroku, Google App Engine

    3. **SaaS (Software as a Service):**
       - Provides software applications over the internet
       - Examples: Salesforce, Google Workspace

    4. **FaaS (Function as a Service):**
       - Provides serverless computing capabilities
       - Examples: AWS Lambda, Azure Functions

16. ### What is microservice architecture? what are benifits and drawbacks?
    It is a way that structure an applcation as a collection of small, autonomus, and loosely coupled specific buisness capabilities. Each service runs its own process and communication with others using lightweight mechanisms like HTTP/REST or message queues.
    - **Benifits**:
      - **Scalability**: Individual services can be scaled horizontally based on demand optimizing resource usage.
      - **Flexibility**: Teams can choose different technology stacks for each service.
      - **Fault Isolation**: The failure of one service does not necessarily bring down the entire system.
      - **Faster Development and Deployment**: Smaller independant codebases allow for faster development cycle and CI/CD.
    - **Drwabacks**:
      - **Complexity**: Managing large number of independant services requires sophisticated tools for orchestration, monitoring and logging.
      - **Inter-service Communication**: Network latency and potenital communication failure between services can introduce new challanges.
      - **Data Consistency challange**: Ensuring data consistency acorss multiple independant codebases is complex and often requires different patterns like eventual consistency.

17. ### Can you share an example of a complex automation script you have written?
    In my current role. I developed a complex Bash script to automate the cleanup of stael Azure App Registrations and Service Principals. the goal was to identify and report on applications that had not been used in the last 90 days to reduce attack surface.
    How the Script Works:
     - **Data Extraction**: It uses the Azure CLI to fetch all application registrations in the tenant.
     - **Usage Verification**: For each application, it queries the Microsoft graph API (or we can use `az ad sp signin-activity`) to retrieve the `lastSignInDateTime`.
     - **Owner Identification**: It specifically pulls the Owner details using az ad app owner list to ensure accountability.
     - **Secret Expiry tracking**: The script parses the `passwordCredentials` array within the JSON output to find the nearest `endDate` highlighting secrets that are already expired or nearing expiry.
     - **Logic and Reporting**: It calculates the date 90 days ago using the `date` command and compares it against the `lastSignInDateTime`. It compiles detils like App Name, Client ID, Owner, Secret Exipry and Last Used Date into a formatted CSV report.
Complex Bash Elements Used:
 - **`jq` Integration**: To parse deeply nested JSON arrays like secrets and Owners that standard CLI output can't easily handle.
 - **Parallel Processing**: I used background jobs or `xargs` to speed up the API calls for tenants with hundreds of applications.
 - **Error Handling**: Implemented checks for service principal permissions to ensure the script doesn't fail silently if logs are inaccessible.
   ##TODO: write a script for above question using chatgpt.

18. ### How do you approach troubleshooting and debugging automation scripts?
    For troubleshooting and debugging I follow systematic approach:
 - **Analyze the Failure Context**: Start by reviewing execution logs, stack traces and error messages to pinpoint exactly where and why the script failed.
 - **Classify the Failure**: Determine if it is a 'True Failure' like an actual bug in application or 'False Failure' like issue with script, environment or test data.
 - **Reporduce the Issue**:
   - *Manual Reproduction*: Attempt to perform the same steps manually in same environment to see if the issue is automation-specific or a genuine application bug.
   - *Isolated Execution*: Run only the failing test case or specific stpes to rule out dependencies or 'flaky' behavior caused by pevious tests.
- **Isolate the Root Cause**:
   - *Synchronization issues*: Check for timing problems. Use explicit waits instead of static sleeps to handle dynamic loading.
   - *Test Data & Environment*: Ensure the environment is stable and the test data like credentials, URLs hasn't expired or changed.
- **Use Debugging Tools**:
   - *Breakpoints and Stepping*: Use IDE (CursorAI, VSCode) to set breakpoints and step through the code line-by-line to inspect variable values.
   - *Dry Runs*: For infrastructure scripts like Terraform or Ansible, use 'dry run' or 'Plan' modes to validate logic without making changes.
- **Fix and Verify**: Once the fix is implemented, re-run the script in multiple environments to ensure it is robust and doesn't cause regressions.


     
    
  

