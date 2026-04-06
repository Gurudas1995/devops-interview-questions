1. ### What is scaling Policy?

   It is a set of instruction used by AWS Auto Scaling to automatically adjust the capacity of your application's resourcesin response to changing demand.\
   **Four types of scaling Policies**
     #### I. Target Tracking Scaling
     - This is recommended and simplest method. You select a metric and target value. AWS automatically add or removes capacity as needed to keep the metric as close to the target value as possible.\
     - Use case - ideal for steady, predicatble performance needs where you want to maintain a specific utilization level.
     #### II. Step Scaling
     - This policy used cloudwatch alarms to trigger scaling adjustments based on the magnitude of the alram breach. you define a 'step' for how much capacity to add or remove depending on how far the metric vaule is from the threshold.
     #### III. Simple Scaling
     - It is similar to step scaling but responds to a single alarm breach with a single scaling action. A key difference is that after a simple scaling activity is initated, the policy waits for a 'cooldown' period to end before responding to any further alarms.
     #### IV. Predctive scaling
     - This uses machine learning to analyze historical traffic patterns and forecast future capacity need up to 48 hours in advance. It proactively schedules scaling actions to ensure capacity us available before demand increases, which helps to avoid performance issues.
   

   **[⬆ Back to Top](#table-of-contents)**
2. ### What are the types of endpoints in AWs?
   AWS endpoints vary by service, but broadly fall into Regional, Global and VPC endpoints. VPC endpoints further split into Interface Endpoints(PrivateLink) for most services and Gateway Endpoints for S3/DynamoDB.
   - Regional Endpoints: 
     Standard endpoints located in specific AWS Regions(s3.us-east-1.amazon.com) for most services.
   - Global Endpoints:
     Endpoints that aren't tied to a specific region, like IAM, route53, and cloudfront, for unified management of content delivery.
   - Interface Endpoints(PrivateLink):
     Create an Elastic Network interface (ENI) with a private IP in your subnet, acting as an entry point for many srvices(lambda, KMS, API gateway).
   - Gateway Endpoints:
     A route table target for specific services like S3 and DynamoDB, routing traffic directly without ENI.


6. ### How would you access data in an S3 bucket from account A, when your application is running on an EC2 instance in Account B?
   Accessing an S3 bucket in Account A from an EC2 instace in Account B requires a cross account IAM role in Account A and a bucket policy that trusts account B. The EC2 instance in account B assumes this role- to obtain temporary credentials to access the bucket, ensuring secure, authorized access. Below are the steps.
- Create a Cross account IAM role in Account A:
  - Create an IAM role (e.g. S3AccessRole) in Account A.
  - Set the trust relationship to allow Account B's IAM entity or specific user/role to assume this role
  - Attach a policy to this role that grants the necessary S3 permissions (GetObject, ListBucket) on the target bucket.
- Configure the S3 Bucket Policy in Account A:
  - Update the bucket policy in account A to explicitly grant access to the role created above or directly to the IAM role attached to the EC2 instance in Account B.
  ```
       {
        "Effect": "Allow",
        "Principal": { "AWS": "arn:aws:iam::AccountB-ID:role/EC2Role" },
        "Action": ["s3:GetObject", "s3:ListBucket"],
        "Resource": ["arn:aws:s3:::bucket-a", "arn:aws:s3:::bucket-a/*"]
        }
  ```
  - Ensure the EC2 instance is running with an IAM instance profile that has permission to perform the sts:AssumeRole action on the cross-account role created in step 1.

7. ### What is the AWS Shared Responsibility Model?
   AWS manages security of the cloud (infrastructure, hardware, networking). The customer is responsible for security in the cloud (OS patching, IAM policies, network configuration, data encryption)

8. ### How do you manage and monitor AWS infrastructure to ensure high availability and performance?
 - Using AWS Cloudwatch to monitor resource utilization, system-wide performance metrics, transaction volumes, and latency, setting alarms based on specific thresholds.
 - Implmenting AWS Auto Scaling to maintain application availabilty and balance capacity.
 - Utilizing AWS Trusted Advisor to optimize AWS environment, reduce costs, boost performance and increase security by inspecting your AWS environment.
 - Employing AWS CloudTrail to enable governance, compliance, operational auditing and risk auditiong of your AWS account.

9. ### What is VPC and why is it used?
    A Virtual Private Cloud (VPC) is an isolated section of the AWS cloud where you launch resources. It Allows for custom IP address ranges, subnets, route tables, and network gateways to ensure security and network isolation. Some security benifites are:
 - Network segmentation.
 - Control over inbound and outbound network traffic via network access control list (NACL) and security groups.
 - VPN connections for secure communication with your corporate network.
 - Dedicated, Hardware-based VPN appliance for enhanced security.
 - The ability to use AWS IAM to set up and enforce policies for VPC-related resources.
   
11. ### Explain difference between Public and private subnets
    A public subnet has a route to an internet Gateway (IGW) for direct internet access. A private subnet does not have a direct route to the internet; it typically uses a NAT instance or NAT Gateway to access the internet securely.

12. ### How can you automate OS patching on EC2 instances?
    Using AWS systems manager - Patch Manager, which allows schedulling maintenance windows to apply patches to both Windows and Linux instances.

13. ### What is the best way to handle DDoS attacks?
    Utilize AWS Shield (standard or Advanced) for DDoS protection, AWS WAF (Web App Firewall) to filter malicious traffic and Amazon CloudFront for content delivery.

14. ### How do you ensure high availability for a web application?
    Deploy instances across multiple Availability Zones using an Auto Scaling Group behind an Application Load Balancer (ALB).

15. ### What are IAM roles and how are they used?
    IAM roles are identity structures with permissions that can be assumed by AWS services like EC2 or users, allowing them to perform actions without hardcoding security credentials.

16. ### How do you optimize cost for AWS infrastructure?
    Implement AWS Cost Explorer and Budgets. Use Reserved Instances or Savings Plans for steady state, spot Instances for batch jobs, and delete unused resources like unattached EBS Volumes, old snapshots.

17. ### What is the difference between EBS, EFS and S3?
 - EBS (Elastic Block Store): Block storage for a single EC2 instance (high performance).
 - EFS (Elastic File System): Shared file system for multiple EC2 instances.
 - S3 (Simple Storage Service): Object storage for flat files, Highly durable, internet-accessible.

17. ### How do you securely access an EC2 instance in private subnet?
    Use AWS Systems Manager Session Manager, which allows shell access without opening SSH port 22 or using a Bastion host.

18. ### What is AWS CloudFormation?
    A service that models and sets up AWS resources based on JSON or YAML temlates, enabling infrastrucutre as Code for consistent, automated deployments.

19. ### How do you handle a failing EC2 Instance?
    Check Cloudwatch status checks (system or instance checks). If it's a system failure, I stop/start the instance to move it to healthy hardware. For instance failures, I analyze logs and potentially replace the instance via auto scaling.

20. ### How do you ensure data inegrity and security in AWS S3?
 - Using **S3 Versioning** to keep multiple variants of an object.
 - Implementing **SSL/TLS** to encrypt data in transit to S3.
 - Empolying **S3 Server-Side Encryption (SSE)** for data at rest.
 - Leveraging **S3 Bucket Policices and IAM Policies** to manage access to S3 bucktes.
 - Utilizing **AWS KMS** to manage encryption keys used by SSE.
 - Implementing **MFA delete** to provide an additional layer of security by requiring MFA to delete S3 objects.

21. ### What is SCP?
   AWS Servce Control Policies (SCPs) are JSON based policies used in AWS Organizations to set maximum permission guardrails across multiple AWS accounts. They act as central, organization-wide restriction layer, defining which IAM actions and services are allowed or denied, overriding individual IAM policies. Common use cases are - 
 - Restricting Regions: Blocking services in specific regions to comply with data residency requirements.
 - Preventing Service Usage: Disabling services like S3, Lambda or IAM in specific environment.
 - Security Guardrails: Preventing the deletion of CLoudtrail logs, diabling GuardDuty, or blocking root user usage.
 - Cost Management - Restrictring the ability to launch expensive instance types.

22. ### How can instance 2 with a static IP communicate with instance 1 which is in a private subnet and mapped to a multi-az load balancer?
    Instance 2 with staic IP can communicate with instance 1 by sending traffice to the load balancer's DNS name via HTTPS/HTTP. The ALB acts as a proxy, routing requests from public IPs to private instances, provided the security groups permit traffic between them. Here are the specific steps to enable this communication.
 - **Configure Load Balancer Security Group**: The ALB's SG must allow inbound traffic from instance 2's static IP or its security group on the listner port i.e. 80 or 443.
 - **Configure instance 1 Security Group**: the Security group for instance 1 must allow inbound traffic from the ALB's security group.
 - **Use Proper Routing**: Ensure the VPC subent routing tables allow traffic to flow between the public subnet and the private subnet.
 - **Internal Vs. Public ALB**: If instance 2 is inside same VPC, use an internal ALB. if isntance 2 is on the internet, use internet facing ALB.

23. ### For an ec2 instance in a private subnet, how can it verify and download required packages from the internet without using NAT gateway or bastion host? Are there any other AWS services that can facilitate this?
    We can use **VPC endpoints (AWS PrivateLink)** or **AWS Systems Manager (SSM)**. The instance must have an IAM role that allows it to communicate with SSM endpoints.
 - **VPC Endpoints**: VPC endpoints allow private, secure connectivity to AWS services like S3, ECR, or systems Manager without traversing the public internet.
 - **AWS Systems Manager**: If the goal is to install software package, we can use `SSM run command` or patch Manager to execute commands on the instance without needing SSH access or public IP addresses.

24. ### What is the typical latency for load balancer, and if you encounter high latency, what monitoring steps would you take?
    Typical load balancer latency is very low, often under 1ms to 20ms depending on the type (layer4 or layer7) and geographic distribution. High latency usually indicates backend bottlenecks, netwoek congestion or improper configuration. Key monitoring steps include checking backend target response time, RequestCount, healthy host counts and error logs.

25. ### If your application is hosted in S3 and users are in different geographic locations, how can you reduce latency?
   - Use Amazon Cloudfront: Configure a CloudFront distribution with your S3 bucket as the origin to serve content from edge locations worldwide.
   - Enable Transfer Acceleration: Use S3 transfer acceleration for faster uploads to S3, utilizing edge locations for the upload path.
   - Leverage edge Caching: Set appropriate `Cache-Control` headers so assets are cached at edge locations, reducing requests back to source S3 bucket.

26. ### Which services can be integrated with a CDN (Content Delivery Netowrk)?
    A CDN integrates with services that host, deliver, or process web content to reduce latency and improve performance. Here is a breakdown of commonly integrated services:
 - **Static Object Storage**: Amazon S3, Azure Blob Storage, Google Cloud Storage. best for images CSS, JS.
 - **Compute & Load Balancing**: AWS EC2, Elastic Load Balancing, or Azure VM.
 - **API Gateways**: To accelerate API requests and reduce load on backend services.
 - **Media Streaming Services**: AWS Elemental MediaPackage, IVS(Interactive Video Services) for secure video delivery.
 - **Custom Origins**: On-premise servers on third-party web servers  via public IP or domain.
 - **Security and Optimizing Services**: WAF (Web application Firewalls), DDoS protection and certficate managers for HTTPS.

27. ### How do you dynamically retrieve VPC details from AWS to create an EC2 Instance using IaC, can you write the code?
    In IaC, we dynamically retrieve VPC details using `Data Sources` in Terraform or `ImportValue/Parameters` in CloudFormation. This Allows us to reference existing infrastructure without hardcoding Ids, which is essential for environments that change or are managed by different teams. Below is the terraform code, which uses `data` block to query the AWS API.
```
# Retrieve the existing VPC dynamically using a tag
data "aws_vpc" "selected_VPC"{
  filter {
    name = "tag:name"
    values = ["my-prod-vpc"]
   }
}
# Retrieve a subnet within that VPC
data "aws_subnets" "VPC_subnets"{
  filter {
    name = "VPC-id"
    values = [data.aws_vpc.selected_vpc.id]
   }
}
# Create the EC2 instance using above retrieved ID
resource "AWS_instance" "My_instance"{
  ami = "ami-sc395428488934"
  instance_type = "t2. micro"
  subnet_id = data.aws_subnets.vpc_subnets.ids[0]
  tags = {
    Name = "DynamicInstance"
   }
}
```

    
    
   



   
