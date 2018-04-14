title: AWS - VPC
date: 2018-04-11 11:37:23
tags:
- AWS
- VPC
---

# 044.mp4 045.mp4 -- virtual private cloud


* Inside a certain region and spanning multiple availble zones
* Class A/B/C private network ranges; subnet mask
* Important: aws reserve first 3 and last 1 ip addresses (for example, 0,1,2,3 and 255)
* default VPC: 172.31.0.0/16, which means 2*2*..... * 2 - 4 availble ips
* 1 VPC can have multiple subnet; min size is 28, 2*2*2*2-4= ??? 14 addresses

## how to connect to a VPC

* 2 ways: via internet gateway or via virtual private gateway and via VPN
* Route Tables: if any instances inside a VPC need access to internet, then Route Table is needed
   * Main Route Table and Custom route table
* NAT Gateway: Network Address Translation (NAT)

## VPC Security

* Security Groups : Instance Level
* ACL (Access Controle Lists): Subnet level
* Flow Logs (Capture as CloudWatch logs)


# 046.mp4 -- VPC handon demo

* VPC wizard to create VPC
* Select one of the option to use "VPC with public and private subnet"
* Select the NAT Gateway (rates apply) -- use the service or start an EC2 instance acting as NAT (Here t2.micro is selected)
* Service Endpoints : AWS S3 not sits inside VPC, when EC2 in VPC need access S3, instead of define role for each EC2, we can define a Service Endpoint to allow EC2 access the S3 without assign a role.
  * select service -- only S3 Available
  * Select which subnet
  * select access policy
  * can create multiple endpoints for a VPC
* Check the ACL being created and binded with the Subnets
  * check and edit the inbound/outbound rules
  * ACL rule is stateless compared with security group (Security group don't define in/outbound separatedly)
  * ACL rule has both allow and deny rules compared to security group
* Check subnets --- check the route being configured for public and private subnet
* To have internet access to EC2 : have rout to igw,  ACL allowed, security group allowed


CIDR block ; when a subnet designed is too small
https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Subnets.html
https://aws.amazon.com/premiumsupport/knowledge-center/vpc-ip-address-range/

VPC Peering
https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpc-peering.html

VPN-Only Subnet
https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Scenario3.html

VPC Security Group
https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_SecurityGroups.html

Two types of security groups: for EC2-classic / for EC2-VPC https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-network-security.html#ec2-classic-security-groups


VPC Primary Private IP Address ; Secondary Private IP address
https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpc-ip-addressing.html

ENI: Elastic Network Interface
https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-eni.html

ClassicLink : EC2-Classic
https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/vpc-classiclink.html

BGP-capable VPN device (Border Gateway Protocol)
https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_VPN.html

Tenancy
https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/dedicated-instance.html

Activate VPC Peering connection
https://docs.aws.amazon.com/AmazonVPC/latest/PeeringGuide/create-vpc-peering-connection.html

VPC limitations,
https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Appendix_Limits.html


VPN Pricing
https://aws.amazon.com/vpc/pricing/
