# AWS-Photo-sharing-App-

## VPC Configuration

A custom Virtual Private Cloud (VPC) was created with the following configuration:

- Name: kloudtask-vpc
- Region: us-east-1 (N. Virginia)
- IPv4 CIDR Block: 10.16.0.0/16
- DNS Resolution: Enabled
- DNS Hostnames: Enabled

The /16 CIDR block provides 65,536 IP addresses, allowing subnet segmentation for future scaling across multiple availability zones.



## Subnet Creation

To segment the VPC network, two subnets were created within the kloudtask-vpc.

### Public Subnet
- Name: public-subnet
- Availability Zone: us-east-1a
- CIDR Block: 10.16.1.0/24

### Private Subnet
- Name: private-subnet
- Availability Zone: us-east-1a
- CIDR Block: 10.16.2.0/24

The /24 subnet mask provides 256 IP addresses per subnet (251 usable), allowing logical separation between internet-facing resources (public subnet) and internal application resources (private subnet).

Both subnets are deployed within the same Availability Zone (us-east-1a) for this lab setup. In a production architecture, subnets would typically span multiple Availability Zones for high availability.


## Internet Connectivity Configuration

To enable internet access for resources in the public subnet, an Internet Gateway and a custom route table were configured.

### Internet Gateway

- Name: kloudtask-igw
- Attached to: kloudtask-vpc

The Internet Gateway enables communication between the VPC and the public internet.

### Public Route Table

- Name: public-rt
- Associated VPC: kloudtask-vpc

#### Route Configuration

| Destination | Target |
|------------|--------|
| 0.0.0.0/0  | kloudtask-igw |

The default route (0.0.0.0/0) directs all outbound internet traffic to the Internet Gateway.

### Subnet Association

The route table `public-rt` is associated only with:

- public-subnet (10.16.1.0/24)

The private subnet remains unassociated with this route table, keeping it isolated from direct internet access.

A subnet is considered public when:
1. It has a route to 0.0.0.0/0
2. The route target is an Internet Gateway
3. The subnet is associated with that route table

Without all three conditions, a subnet remains private.
