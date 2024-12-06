
# AWS Architecture with Terraform

This project provisions an AWS infrastructure using Terraform. The architecture consists of a Virtual Private Cloud (VPC), two EC2 instances in public subnets, and an S3 bucket for storage and AWS Load balancer. Below is a detailed explanation of the components and their functionality.

![Images/Architecture.png]

## Architecture Overview

- **VPC**:
  - A Virtual Private Cloud (VPC) is created with a custom CIDR block (`172.16.0.0/16`).
  - Two public subnets are defined within this VPC.

- **Public Subnets**:
  - Each public subnet hosts one EC2 instance.
  - Subnets are configured to allow external access for specific use cases.

- **EC2 Instances**:
  - Two instances are deployed in the public subnets.
  - The instances are connected to an S3 bucket for data storage or processing tasks.
  - Elastic IPs (EIPs) are allocated to ensure each instance has a fixed public IP address.

- **S3 Bucket**:
  - Used as centralized storage for the instances.
  - Facilitates data upload, retrieval, and backup operations.

- **Network Access**:
  - Instances are configured with proper IAM roles and security groups to allow communication with the S3 bucket securely.
  - Inbound and outbound rules are managed to restrict unnecessary access.

## Terraform Structure

The project is organized as follows:

```
.
├── main.tf          # Contains the main infrastructure resources
├── variables.tf     # Defines input variables for customization
├── outputs.tf       # Specifies outputs for useful resources (e.g., instance IDs, bucket name)
├── providers.tf     # Configures the AWS provider
├── README.md        # Documentation for the project
```

### Key Terraform Modules and Resources

- **VPC Module**: Creates the VPC, public subnets, and associated networking components.
- **EC2 Resources**:
  - AMI selection and instance type customization.
  - Key pair generation for SSH access.
- **S3 Bucket**:
  - Configured with versioning and optional lifecycle policies.

## Prerequisites

- Terraform installed locally (>= v1.0.0).
- AWS CLI configured with appropriate credentials and permissions.
- SSH key pair for accessing the instances.
- IAM user/role with permissions to manage the following AWS services:
  - EC2
  - S3
  - VPC
  - IAM

## Setup and Deployment

1. **Clone the Repository**:
   ```bash
   git clone <repository_url>
   cd <repository_directory>
   ```

2. **Customize Variables**:
   Modify the `terraform.tfvars` file to suit your environment:
   ```hcl
   aws_region      = "us-east-1"
   instance_type   = "t2.micro"
   key_name        = "<your_key_pair>"
   vpc_cidr        = "172.16.0.0/16"
   public_subnets  = ["172.16.1.0/24", "172.16.2.0/24"]
   ```

3. **Initialize Terraform**:
   ```bash
   terraform init
   ```

4. **Plan the Deployment**:
   Review the resources to be created:
   ```bash
   terraform plan
   ```

5. **Apply the Deployment**:
   Deploy the infrastructure:
   ```bash
   terraform apply
   ```

6. **Output Resources**:
   After applying, Terraform outputs details such as:
   - EC2 instance public IPs
   - S3 bucket name

## Diagram

![AWS Architecture Diagram](./Screenshot 2024-12-06 180620.png)

## Cleanup

To delete the resources and avoid unnecessary charges, run:
```bash
terraform destroy
```

## Notes

- Ensure your AWS account has sufficient limits for the services being provisioned.
- Update IAM roles and policies to restrict access according to the principle of least privilege.
- Use a secure mechanism to store and manage sensitive information like SSH keys and AWS credentials.

## License

This project is licensed under the MIT License.