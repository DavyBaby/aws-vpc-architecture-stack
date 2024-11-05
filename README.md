AWS VPC Architecture Stack
This repository contains a CloudFormation template that deploys a multi-VPC architecture on AWS. The setup includes a Corporate VPC and a Development VPC, each with public and private subnets, security groups, and network access control lists (NACLs). The two VPCs are connected through an AWS Transit Gateway to enable secure inter-VPC communication.

Architecture Overview
The architecture includes the following components:

1. Corporate VPC (10.0.0.0/16)
Public Subnet A (10.0.1.0/24) - Contains a bastion host for secure SSH access.
Public Subnet B (10.0.2.0/24) - Hosts web resources accessible from the internet.
Private Subnet A (10.0.3.0/24) - Used for internal resources.
Private Subnet B (10.0.4.0/24) - Used for internal resources.
2. Development VPC (192.168.0.0/16)
Public Subnet A (192.168.1.0/24) - Hosts web resources accessible from the internet.
Public Subnet B (192.168.2.0/24) - Hosts web resources accessible from the internet.
Private Subnet A (192.168.3.0/24) - Used for internal resources.
Private Subnet B (192.168.4.0/24) - Used for internal resources.
3. AWS Transit Gateway
Enables secure inter-VPC communication between the Corporate and Development VPCs.
4. Security Groups
Bastion Host Security Group: Allows SSH access from a specific IP range.
Web Security Group: Allows HTTP (port 80) and HTTPS (port 443) traffic from the internet.
Internal Security Group: Allows only internal traffic within the VPCs.
5. Network Access Control Lists (NACLs)
Internal NACL for Corporate VPC: Restricts traffic to internal IP ranges only.
Restricted NACL for Development VPC: Allows only traffic from the Corporate VPC or within the Development VPC.
6. VPC Flow Logs
Logs all traffic for monitoring and security purposes, sent to CloudWatch.
Usage
Prerequisites
AWS Account: You need an AWS account with appropriate permissions to deploy CloudFormation stacks.
IAM Role for Flow Logs: Make sure you have an IAM role with permissions to publish VPC Flow Logs to CloudWatch.
Deploying the Stack
Clone this repository to your local machine:

bash
git clone https://github.com/yourusername/aws-vpc-architecture-stack.git
cd aws-vpc-architecture-stack
Modify the vpc-architecture.yaml template as needed. For example:

Replace <your-office-IP-range> in the BastionHostSecurityGroup section with your actual IP range for secure SSH access.
Replace <account-id> with your AWS account ID in the VPC Flow Logs section.
Deploy the stack using the AWS CLI or AWS Management Console.

Using the AWS CLI:

bash
aws cloudformation create-stack --stack-name MultiVPCArchitectureStack --template-body file://vpc-architecture.yaml --capabilities CAPABILITY_NAMED_IAM
Wait for the stack to be created. You can monitor the status in the AWS CloudFormation console.

Verifying the Deployment
Once the stack is successfully deployed, you can verify the resources in the AWS Management Console:

Check the VPC section for the Corporate and Development VPCs with their associated subnets.
Check Security Groups to ensure that the Bastion Host, Web, and Internal security groups are created.
Verify the Transit Gateway to confirm connectivity between the VPCs.
Review VPC Flow Logs in CloudWatch for network traffic logs.
Clean Up
To avoid ongoing charges, delete the CloudFormation stack when it's no longer needed:

bash
Copy code
aws cloudformation delete-stack --stack-name MultiVPCArchitectureStack
This will remove all resources created by the template.
