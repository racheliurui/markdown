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
   * 255 is the broadcasting address
   * one address is preserved for Internet Gateway
   * TODO: how this 4 address is preserved???
* default VPC: 172.31.0.0/16, which means 2^(32-16) - 4 availble ips
* 1 VPC can have multiple subnet;
   * min size is /28, with means 2^(32-28)-1 (forbradcassing) -1( 0 ?? IGW??) =14 available addresses

## how to connect to a VPC

* via internet gateway or via virtual private gateway and via VPN
* Route Tables: if any instances inside a VPC need access to internet, then Route Table is needed
   * Main Route Table and Custom route table
* NAT Gateway: Network Address Translation (NAT)
   * Internet Gateway is attached to VPC. NAT Gateway sits in a Public Subnet.
   * NAT gateway is not necessary. instance in Public Subnet can directly access internet via IWG, only only when we want to configure to let instance sits in Private Subnet to have limited access to internet (only connection initiated from private subnet is allowed and must go through NAT), we can configure NAT to control this.
   * For any instance sits in a private subnet, they access internet via 0.0.0.0/0 pointing to NAT sits in public subnet; then from NAT instance, they can visit internet.
   * https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpc-nat-gateway.html

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
* To have internet access to EC2 : have route to igw,  ACL allowed, security group allowed


CIDR block ; when a subnet designed is too small
https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Subnets.html
https://aws.amazon.com/premiumsupport/knowledge-center/vpc-ip-address-range/

Basic : ClassABC private network address
https://en.wikipedia.org/wiki/Private_network


VPC Peering
https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpc-peering.html

VPN-Only Subnet
https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Scenario3.html

VPC Security Group
https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_SecurityGroups.html

Two types of security groups: for EC2-classic / for EC2-VPC https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-network-security.html#ec2-classic-security-groups
Difference between 2 types of Security Groups
https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_SecurityGroups.html#VPC_Security_Group_Differences

VPC Primary Private IP Address ; Secondary Private IP address
https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpc-ip-addressing.html
* Primary Private IP Address is the main IP address binding with eth0
* Secondary Private IP Addreess is additional manually assigned IP address. It can be reassigned
* Both Primary and Secondary Private IP addresses, once assigned will be attached to instance until it's terminted  

ENI: Elastic Network Interface
https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-eni.html
* 虚拟网卡。 每个instance默认有eth0
* 根据instance类型不同，允许attach不同数目的虚拟网卡
* ENI是instance能够有secondary private IP address的基础。
* 常用场景是一个instance坐在两个subnet上，一个网卡（例如eth0）连subnet A，一个连subnet B。
    * Subnet A 是public subnet，通过安全控制使得该instance可以提供http网络服务
    * Subnet B 是private subnet，通过安全控制使得该instance只能连VPN Gateway，通过该gateway允许指定内网ip地址提供ssh连接进行管理。

ClassicLink : EC2-Classic instances communicate with EC2-VPC instances
https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/vpc-classiclink.html
what is a EC2-Classic platform?
https://docs.rightscale.com/faq/clouds/aws/What_is_an_EC2-Classic_network.html

BGP-capable VPN device (Border Gateway Protocol)
https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_VPN.html

Tenancy property for EC2 instance has 2 values to choose from : default ; dedicated
https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/dedicated-instance.html

Activate VPC Peering connection
https://docs.aws.amazon.com/AmazonVPC/latest/PeeringGuide/create-vpc-peering-connection.html

VPC limitations,
https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Appendix_Limits.html


VPN Pricing
https://aws.amazon.com/vpc/pricing/
