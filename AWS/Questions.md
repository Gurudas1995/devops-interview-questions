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

3. ###  What is RTO and why is it important?
   Recovery Time Objective (RTO) is the target time for resuming operations after an incident (e.g. 2 hours). It is crucial becuase it directly impacts business revenue, reputation, and SLA compliance.

4. ### What is RPO and what does it measure?
   Recovery Point Objective (RPO) is the maximum allowable data loss measured in time (e.g. last 15 minutes of data).It determines how frequently backups or snapshots must be taken.

5. ### what is the difference between RTO and RPO and what are commenly used metrics?
RTO relates to downtime (time to restore systems), while RPO relates to data loss (amount of data that needs re-entry).
   - RTO : Seconds (hot site) to days (Tape restore)
   - RPO : Zero (synchronus data replication) to 24 hours (daily backup)

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

7. ###


   
