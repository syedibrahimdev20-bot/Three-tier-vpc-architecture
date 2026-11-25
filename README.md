# Three-Tier AWS VPC Architecture

A hands-on **AWS networking project** demonstrating a segmented Virtual Private Cloud (VPC) design for a three-tier application across two Availability Zones.

> **Project type:** AWS networking / cloud infrastructure  
> **Tools:** AWS Management Console  
> **Infrastructure as Code:** Not used — this project was configured using AWS services directly.

## Project Overview

This project separates a three-tier application network into public, private application, and private database subnets. The design uses two Availability Zones to demonstrate network segmentation and a highly available architecture.

The AWS console screenshot shows a VPC named **Three-Tier-VPC**, with **six subnets**, **four route tables**, and **two network connections**. The architecture diagram illustrates the intended design with an Internet Gateway, Application Load Balancer (ALB), EC2 application instances, NAT Gateway, and an Amazon RDS database tier.

## Architecture

### Network layout

- **VPC name in console:** `Three-Tier-VPC`
- **VPC CIDR shown in console:** `10.10.0.0/16`
- **Availability Zones:** 2 (us-east-1a and us-east-1b)
- **Subnets:** 6 total — 2 public, 2 private application, and 2 private database subnets
- **Route tables:** 4
- **Network connections:** 2 shown in the AWS console

### Three-tier architecture and traffic direction

The diagram below uses `>` to show the direction of traffic:

```text
                              Internet
                                 |
                                 v
                         Internet Gateway
                                 |
                   +-------------+-------------+
                   |                           |
             Public Subnet AZ-1          Public Subnet AZ-2
                   |                           |
                  ALB                         ALB
                   |                           |
                   v                           v
          Private App Subnet AZ-1    Private App Subnet AZ-2
                   |                           |
             EC2 App Tier  <------->     EC2 App Tier
                   |                           |
                   v                           v
          Private DB Subnet AZ-1     Private DB Subnet AZ-2
                   |                           |
                RDS DB                      RDS DB
```

**Traffic flow:** Internet `>` Internet Gateway `>` public subnet / ALB `>` private application tier `>` private database tier.

**Private outbound flow (when configured):** Private application subnet `>` NAT Gateway `>` Internet Gateway `>` Internet. A NAT Gateway allows private resources to initiate outbound connections; it does not allow unsolicited inbound connections from the internet.

### Subnet plan shown in the architecture diagram

The conceptual architecture image uses the following CIDR plan:

| Tier | Availability Zone 1 | Availability Zone 2 |
|---|---|---|
| Public | `10.20.1.0/24` | `10.20.2.0/24` |
| Private Application | `10.20.11.0/24` | `10.20.12.0/24` |
| Private Database | `10.20.21.0/24` | `10.20.22.0/24` |

**CIDR clarification:** The AWS console screenshot shows the actual VPC as `10.10.0.0/16`, while the conceptual diagram labels its example VPC as `10.20.0.0/16`. These are different address plans. Confirm the subnet CIDRs in the AWS console and update this table if the conceptual CIDRs were not used in your deployed VPC.

## AWS Services and Components

- **Amazon VPC** — isolated virtual network for AWS resources
- **Subnets** — separate public, application, and database tiers
- **Internet Gateway (IGW)** — internet connectivity for public subnet routes
- **Route Tables** — control traffic routing and subnet associations
- **NAT Gateway** — outbound internet access for private resources, when configured
- **Security Groups** — control inbound and outbound traffic at the resource level
- **Application Load Balancer (ALB)** — distributes incoming application traffic across targets
- **Amazon EC2** — compute instances for the application tier
- **Amazon RDS** — managed database service shown in the architecture design

## Security Design

- Keep application and database workloads in private subnets.
- Route public web traffic through the ALB rather than directly to application instances.
- Allow application instance inbound traffic only from the ALB security group, on the required application port.
- Allow database traffic only from the application tier security group and required database port.
- Avoid opening SSH (port 22) to `0.0.0.0/0`; restrict administrative access to a trusted source or managed access method.
- Follow least-privilege principles for security groups and IAM permissions.

## What I Practiced

- Creating and organizing a VPC and subnets across Availability Zones
- Planning CIDR blocks and subnet segmentation
- Working with route tables and subnet associations
- Understanding public and private subnet routing
- Reviewing gateway connections and network paths
- Designing a three-tier AWS network with controlled tier-to-tier access
- Applying high-availability principles across two Availability Zones

## Validation Checklist

Use this checklist to verify the configuration in the AWS console:

- [ ] VPC and all six subnets exist with the intended CIDR blocks.
- [ ] Public subnet route tables have a route to the Internet Gateway.
- [ ] Private application subnet routes use a NAT Gateway for outbound internet access, if required.
- [ ] Database subnets do not have direct routes to the internet.
- [ ] Security groups allow only required traffic between tiers.
- [ ] ALB targets are healthy and the application is reachable through the ALB, if deployed.
- [ ] Database connectivity is limited to the application tier, if RDS is deployed.

## Important Notes

This is an **AWS console-based networking project**. **Terraform, CloudFormation, and other Infrastructure as Code tools were not used.**

The architecture image is a design/reference diagram. The console screenshot documents the VPC and network configuration visible in the AWS account. Only list ALB, EC2, NAT Gateway, and RDS as deployed resources if you have actually created and tested them.

AWS resources may incur charges. Review billable resources and delete unused resources after practice.

## Author

**Syed Ibrahim**  
B.Sc. Computer Science | AWS Cloud / Cloud Support (Entry Level)

- LinkedIn: [syed-ibrahim-dev](https://www.linkedin.com/in/syed-ibrahim-dev/)
- GitHub: [syedibrahimdev20-bot](https://github.com/syedibrahimdev20-bot)
