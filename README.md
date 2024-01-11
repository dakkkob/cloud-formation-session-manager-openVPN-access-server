# AWS EC2 with Session Manager and OpenVPN Setup

This repository contains AWS CloudFormation templates for setting up an AWS environment that provides  and configures an EC2 instance with Session Manager access and OpenVPN connectivity to a specified VPN. Everything is deployed in a VPC with Internet Gateway and single public subnet.

## Description

The repository includes two CloudFormation templates:

1. **bastion-bucket.yml**: Creates a secure S3 bucket for storing OpenVPN configuration file, ensuring the file is encrypted at rest and not publicly accessible. It needs to be created first as it is referenced by the other template.

2. **bastion-VPC.yml**: Sets up a VPC with single public subnet, an internet GW and an EC2 instance with OpenVPN installed, connected to a specified VPN concentrator. The EC2 instance is accessible via AWS Session Manager.

## Prerequisites

- An AWS account with necessary permissions to create resources specified in the templates
- An OpenVPN configuration file uploaded to an S3 bucket

## Deployment Instructions

### Step 1: Deploy the bastion-bucket Template

1. Navigate to the AWS CloudFormation console.
2. Choose "Create stack" and select "With new resources (standard)".
3. Upload the bastion-bucket.yml template file and proceed with the stack creation.
4. Note the name of the S3 bucket from the Outputs section after the stack is created.

### Step 2: Upload the OpenVPN configuration file

1. Generate the ovpn config file at your VPN concentrator. You can optionally remove the "remote" section from there so that the keypair is not stored along with the IP address of the VPN server.
2. Upload the ovpn file into previously created S3 bucket.

### Step 3: Deploy the bastion VPC template

1. Navigate to the AWS CloudFormation console.
2. Choose "Create stack" and select "With new resources (standard)".
3. Upload the bastion-VPC.yml template file.
4. Provide required parameters:
   - AMI ID for the EC2 instance (default is the latest Amazon Linux 2 AMI).
   - CIDR block, instance type, and name of the OpenVPN config file uploaded into the bucket
   - The IP address of the VPN concentrator.
5. Proceed with the stack creation.
6. Once the stack is created, the EC2 instance will be set up with OpenVPN and accessible via AWS Session Manager.

## Resources Created

- An EC2 instance configured with OpenVPN.
- An IAM role and instance profile for the EC2 instance.
- A VPC, public subnet, and security group.
- An S3 bucket for storing the OpenVPN configuration files.

## Security

- The EC2 instance is accessible through AWS Session Manager, providing secure access without the need for SSH keys.
- The S3 bucket is encrypted and public access is blocked to ensure the security of the OpenVPN configuration files.

For more information and detailed instructions, refer to the individual templates within the repository.
