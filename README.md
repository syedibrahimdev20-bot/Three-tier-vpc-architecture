# Three-Tier AWS VPC Architecture

A hands-on AWS networking project to design and configure a segmented Virtual Private Cloud (VPC) for a three-tier application across two Availability Zones.

> **Project type:** AWS networking / cloud infrastructure  
> **Tools:** AWS Management Console  
> **Infrastructure as Code:** Not used (this project was built using AWS services directly)

## Project Overview

This project demonstrates how to build a VPC network that separates the application into public, application, and database tiers. The design uses two Availability Zones to support a highly available architecture and applies network segmentation to control access between tiers.

The AWS VPC console screenshot shows a VPC named **Three-Tier-VPC**, six subnets, four route tables, and two network connections. The architecture diagram represents the intended three-tier design with an Internet Gateway, Application Load Balancer, EC2 application instances, NAT Gateway, and RDS database tier.

## Architecture

### Network layout

- **VPC CIDR:** `10.20.0.0/16`
- **Availability Zones:** 2
- **Subnets:** 6 total (public, private application, and private database subnets across the two AZs)
- **Route tables:** 4
- **Network connections:** Internet Gateway and NAT Gateway are shown in the project design

### Traffic flow

```text
Internet
   |
   v
Internet Gateway
   |
   v
Public Subnets (AZ-1 and AZ-2)
   |
   v
Application Load Balancer
   |
   v
Private Application Subnets
   |
   v
EC2 Application Instances
   |
   v
Private Database Subnets
   |
   v
RDS Database
```

**Outbound access from private application instances:**

```text
Private Application Subnet --> NAT Gateway --> Internet Gateway --> Internet
```

The arrows (`-->`) show the intended traffic direction. A NAT Gateway allows resources in private subnets to initiate outbound connections; it does not provide unsolicited inbound access from the internet.

## Subnet Plan

The following CIDR plan is shown in the architecture diagram:

| Tier | Availability Zone 1 | Availability Zone 2 |
|---|---|---|
| Public | `10.20.1.0/24` | `10.20.2.0/24` |
| Private Application | `10.20.11.0/24` | `10.20.12.0/24` |
| Private Database | `10.20.21.0/24` | `10.20.22.0/24` |

## AWS Services

- **Amazon VPC** – isolated virtual network
- **Subnets** – separate public, application, and database tiers
- **Internet Gateway (IGW)** – internet connectivity for public subnet routes
- **NAT Gateway** – outbound internet access for private application resources
- **Route Tables** – route traffic between subnet tiers and network gateways
- **Security Groups** – control inbound and outbound traffic
- **Application Load Balancer (ALB)** – distributes application traffic across application instances
- **Amazon EC2** – application tier compute instances
- **Amazon RDS** – database tier shown in the architecture design

## Security Design

- Keep application and database resources in private subnets.
- Allow public web traffic only through the Application Load Balancer.
- Restrict application instance inbound traffic to the ALB security group.
- Allow database access only from the application tier security group and required database port.
- Avoid opening SSH (port 22) to the entire internet; use a trusted IP or a managed access method.
- Use least-privilege security group rules and IAM permissions.

## What I Practiced

- Creating and organizing a VPC and subnets across Availability Zones
- Planning CIDR blocks and subnet segmentation
- Working with route tables and gateway connections
- Understanding public and private subnet routing
- Applying security groups for tier-based access control
- Designing a highly available three-tier AWS network

## Validation Checklist

Before considering the architecture complete, verify in the AWS console:

- [ ] VPC and all six subnets are created with the intended CIDR blocks.
- [ ] Public subnet route tables have a route to the Internet Gateway.
- [ ] Private application subnet routes use the NAT Gateway for outbound internet access, if required.
- [ ] Database subnets do not have direct internet routes.
- [ ] Security groups allow only the required tier-to-tier traffic.
- [ ] ALB targets are healthy and application instances are reachable through the ALB.
- [ ] Database connectivity is limited to the application tier.

## Important Notes

This repository documents an AWS console-based networking project. **Terraform and other Infrastructure as Code tools were not used for this project.**

The architecture image is a design/reference diagram. Confirm that ALB, EC2, NAT Gateway, and RDS resources are actually deployed and tested in your AWS account before describing them as completed production resources. AWS resources may incur charges; remove resources that are no longer needed.

## Author

**Syed Ibrahim**  
B.Sc. Computer Science | AWS Cloud / Cloud Support (Entry Level)

- LinkedIn: [syed-ibrahim-dev](https://www.linkedin.com/in/syed-ibrahim-dev/)
- GitHub: [syedibrahimdev20-bot](https://github.com/syedibrahimdev20-bot)
