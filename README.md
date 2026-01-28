README - EC2 Snapshot Cleanup Using AWS Lambda


**1. Overview**


This project automates the cleanup of EC2 snapshots that are older than one year.
The solution uses AWS Lambda and is fully managed using Infrastructure as Code (Terraform).

The Lambda function runs on a schedule and deletes old snapshots automatically,
helping reduce storage cost and manual maintenance.


**2. Chosen IaC Tool – Terraform**


Terraform is used to define and deploy all AWS resources because:

- It allows infrastructure to be defined as code
- It is easy to version control
- It supports automated and repeatable deployments
- It is widely used in production environments

Terraform is used to create:
- VPC
- Private subnet
- IAM role
- Lambda function
- EventBridge (CloudWatch) schedule


**3. Infrastructure Created**


The following resources are created using Terraform:

1. VPC
   - Used to host the Lambda securely

2. Private Subnet
   - Lambda runs inside a private subnet

3. IAM Role
   - Allows Lambda to:
     - Describe EC2 snapshots
     - Delete EC2 snapshots
     - Write logs to CloudWatch

4. Lambda Function
   - Executes Python code
   - Deletes snapshots older than 365 days

5. EventBridge Rule
   - Triggers Lambda once per day automatically


**4. Steps to Deploy the Infrastructure**


**Prerequisites:**
- AWS account
- AWS CLI installed and configured
- Terraform installed
- IAM permissions to create AWS resources

Step 1: Initialize Terraform
$ terraform init

Step 2: Deploy the infrastructure
$ terraform apply

Type 'yes' when prompted.

Terraform will create:
- VPC
- Subnet
- IAM role
- Lambda function
- EventBridge schedule


**5. Deploying the Lambda Function**


Step 1: Create the Lambda file
------------------------------
Create a file named:

lambda_function.py

Step 2: Zip the file
--------------------
zip lambda.zip lambda_function.py

Step 3: Deploy
--------------
Terraform automatically uploads the Lambda code during:
terraform apply


**6. Running Lambda Inside a VPC**


The Lambda function is configured with:

- Private Subnet ID
- Security Group with outbound access
- IAM role with required permissions

Terraform configuration:
- subnet_ids
- security_group_ids

This ensures:
- Secure execution
- No public exposure
- Access to AWS services


****7.** Assumptions****


- AWS Region: us-east-1
- Only self-owned snapshots are deleted
- Snapshots older than 365 days are removed
- Lambda runs once per day
- Terraform is used for deployment
- Internet access is available via AWS networking


**8. Monitoring and Logging**


CloudWatch Logs:
- View Lambda execution logs
- Track snapshot deletions
- Debug errors

Log Path:
CloudWatch → Logs → /aws/lambda/delete-old-snapshots

CloudWatch Metrics:
- Invocation count
- Errors
- Duration

