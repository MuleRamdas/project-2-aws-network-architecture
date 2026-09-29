# Project 2 – AWS Network Architecture

## Objective

Create a complete AWS network architecture for a web application using a custom VPC, two Availability Zones, public and private subnets, Internet Gateway, NAT Gateway, route tables, and Security Groups.

## AWS Region

- Region: Asia Pacific (Mumbai) – ap-south-1

## VPC

- VPC Name: project3-web-vpc
- IPv4 CIDR: 10.0.0.0/16

## Availability Zones

- AZ-1: ap-south-1a
- AZ-2: ap-south-1b

## Subnets

| Subnet | Availability Zone | CIDR |
|---|---|---|
| public-subnet-az1 | ap-south-1a | 10.0.1.0/24 |
| private-subnet-az1 | ap-south-1a | 10.0.2.0/24 |
| public-subnet-az2 | ap-south-1b | 10.0.3.0/24 |
| private-subnet-az2 | ap-south-1b | 10.0.4.0/24 |

## Internet Gateway

- Name: project3-igw
- Attached to: project3-web-vpc

The Internet Gateway provides connectivity between the VPC and the public Internet for resources in public subnets.

## Route Tables

### Public Route Table

- Name: public-rt
- Associated with:
  - public-subnet-az1
  - public-subnet-az2

Routes:

| Destination | Target |
|---|---|
| 10.0.0.0/16 | local |
| 0.0.0.0/0 | project3-igw |

### Private Route Table

- Name: private-rt
- Associated with:
  - private-subnet-az1
  - private-subnet-az2

Routes:

| Destination | Target |
|---|---|
| 10.0.0.0/16 | local |
| 0.0.0.0/0 | project3-nat-gateway |

## NAT Gateway

- Name: project3-nat-gateway
- Availability Mode: Zonal
- Subnet: public-subnet-az1
- Availability Zone: ap-south-1a
- Elastic IP: Allocated

The NAT Gateway provides outbound Internet access to resources in private subnets without exposing them directly to the Internet.

## Security Group

- Name: project3-web-sg

Inbound rules:

| Protocol | Port | Source |
|---|---:|---|
| HTTP | 80 | 0.0.0.0/0 |
| HTTPS | 443 | 0.0.0.0/0 |

Outbound traffic is allowed by the default outbound rule.

## Network Flow

### Public Subnet

Internet → Internet Gateway → Public Subnet

### Private Subnet

Private Subnet → Private Route Table → NAT Gateway → Internet Gateway → Internet

## Architecture

The architecture contains two Availability Zones with separate public and private subnets. Public subnets use the Internet Gateway, while private subnets use the NAT Gateway for outbound Internet connectivity.

## Components and Purpose

- **VPC:** Provides an isolated AWS network.
- **Availability Zones:** Provides network distribution across two AZs.
- **Public Subnet:** Used for resources requiring direct Internet connectivity.
- **Private Subnet:** Used for resources that should not be directly reachable from the Internet.
- **Internet Gateway:** Provides Internet connectivity for public resources.
- **NAT Gateway:** Provides outbound Internet access for private resources.
- **Route Table:** Controls network traffic routing.
- **Security Group:** Acts as a virtual firewall for AWS resources.

## Testing / Verification

The following AWS resources were verified successfully:

- VPC available
- Four subnets available
- Internet Gateway attached to VPC
- Public route table associated with both public subnets
- Private route table associated with both private subnets
- NAT Gateway available
- NAT route configured for private subnets
- Security Group created with HTTP and HTTPS inbound access

## Project Status

Completed – AWS network architecture configured successfully.

## Architecture Diagram

See `architecture-diagram.png` in this repository.
