title: AWS - VPC
date: 2018-04-11 11:37:23
tags:
- AWS
- VPC
---

# VPC Deep Dive

Backgroud of VPC: 2009 aws lanched VPC and then simplified to create a default VPC to every account.

## create a VPC

```aws cli
aws ec2 describe-account-attribute
aws ec2 create-vpc --cidr 10.0.0.1/16
```

## command to create an IPSec VPN

```aws cli
aws ec2 create-vpn-gateway --type ipsec.1
aws ec2 attach-bpn-gateway --vpn vgw-f9da06e7 --vpc vpc-c15180a4
aws ec2 create-customer-gateway --type ipsec.1 --public 54.64.1.2 --bgp 6500  
aws ec2 create-vpn-connection --vpn vgw-f9da06e7 --cust cgw-f4d905ea --t ipsec.1
```

## command to create direct connect

```aws cli
aws directconnect create-connection --loc EqSE2 -b 1Gbps --con My_First
aws directconnect create-private-virtual-interface --con dxcon-fgp13h2s --new VirutalInterfaceName=foo,vlan=10,asn=60,authkey=testing, amazonAddress=192.168.0.1/24,customerAddress=192.168.0.2/24,VirtualGatewayId=vgw-f9da06e7
```

## Combine above 2 connectons

* We can setup 1 direct connect plus 1 vpn between aws vpc and on-promise network.

## Configure VPC routing table

* Each VPC will have 1 default route table connected with all subnets.

## Further step: create internet gateway to enable VPC's internet connection

```aws cli
aws ec2 create-internet-gateway
aws ec2 attach-internet-gateway --internet igw-5a1ae13f --vpc vpc-c15180a74
aws ec2 delete-route -ro rtb-ef36e58a --dest 0.0.0.0/0
aws ec2 create-route -ro rtb-ef36e58a --dest 0.0.0.0/0 --gateway-id igw-5a1ae13f
aws ec2 create-route -ro rtb-ef36e58a --dest 192.168.0.0/16 --gateway-id vgw-5a1ae13f
```

## Automatic Route Propagation from VGW

```aws cli
aws ec2 delete-route -ro rtb-ef36e58a --dest 192.168.0.0/16
aws ec2 enable-vgw-route-propapation -ro rtb-ef36e58a --gateway-id vgw-5a1ae13f
```

## Isolate some of the subnet's connection inside the VPC

* Create separate route table for the subnet

## software firewall to internet (NAT)

```aws cli
# by-default it won't work, we need to change the eni of the NAT server. Disable the source and dest check
aws ec2 modify-network-interface-attribute --net eni-f832afcc --no-source-dest-check
aws ec2 create-route --ro rtb-ef36e58a --dest 0.0.0.0/0 --instance-id i-f832afcc
aws ec2 create-route --ro rtb-ef36e31c --dest 0.0.0.0/0 --gateway-id igw-5a1ae13f
```
## VPC peering

* VPC peering support across acount, across region
* VPC peering is a service ; no Single point of failure
* Use case :
 * shared service running inside VCP and peering with other VPC.
 * Separate backend DEV/TEST/PROD; with VPC peering, all env have same ip , the service pointing different VPC by configure the iprouting table to pick the correct VPC, and each DEV/TEST/PROD VPC will have exact same IP setting.
* 2 to-be-peered VPCs can't have IP address overlap, but 1 VPC can peer with multiple VPCs which has ipaddress overlap.
* Security Groups can't be refered across VPC
* VPC peering can use similiar way as a NAT server. (routing internet connect via another VPC )
  * so one VPC don't need to have internet access, it connect to internet through another VPC's igw

# Remote connection Best Practice

* For each customer gateway create 2 VPN tunnel to 2 availability zones that VPC has .
* use 2 customer gateway on-promise for failover. Then customer gateway has 2 VPN tunnel link to different AZ.
* Use 2 direct connect, each direct connect will link to 1 AZ, then add another IPSec VPN (customer gateway) and link 2 different AZ.


# VPC Performance

__packats per second__ : important capability for instances to get high VPC performance

* EC2 has driver to better use physical network and bypass the virtualization layer


# Reference Customer Use Case


## live video

* A backpack containing multi routers to provide multi connections to internet to makesure the live vidio being sent.
* inside AWS, to transcoding and published

## TradeAir

Small banking solution on cloud.
* Use direct connect with on-promise


# VPC migration

ClassicLink : EC2-Classic instances communicate with EC2-VPC instances
Helps to migrate  EC2-Classic platform into VPC network.
* move aws managed services first
* Make ELB ready to route traffic to both classic and VPC networks
* start new VPC EC2 instances and route the traffic in
* turn off the old classic EC2 instances gracefully

## Reference

>
https://youtu.be/i6Zf9lwXRcY

> VPC deepdive 2015
> https://youtu.be/B8vnhRJDujw

#  Hosted Virtual Interface vs Hosted Connection --- when using direct connection

* VIF is direct connection (1Gpbs) -- you can share with multiple account
* Hosted Connection is direct connection but being splitted by AWS partner and garanteed speed (for example 50Mbps per connection)

## Reference

>https://youtu.be/r7zamTFGxcM

# VPC enhancements

## Elastic Network Adapter

* PCI device to support variable speed
* If you are not using AWS AMI , you need to install the driver to get best performance
* HVM instance can have access to AWS 10Gbps pysical network card
* Only high end EC2 support ENA ; you can manually build and install the driver
* There's only enable (there's no disable after enabled)

# Reference
>https://youtu.be/CBmSl3O-AhI

# 044.mp4 045.mp4 -- virtual private cloud

* Inside a certain region and spanning multiple availble zones
* Class A/B/C private network ranges; subnet mask
* Important: aws reserve first 3 and last 1 ip addresses (for example, 0,1,2,3 and 255)
   * 255 is the broadcasting address
   * one address is preserved for Internet Gateway
   * TODO: how this 4 address is preserved???
* default VPC: 172.31.0.0/16, which means (2^(32-16) - 4) availble ips
* 1 VPC can have multiple subnet;
   * min size is /28, with means 2^(32-28)-1 (for bradcassing) -1( 0 ?? IGW??) =14 available addresses

## how to connect to a VPC

* via internet gateway or via virtual private gateway and via VPN
* Route Tables: if any instances inside a VPC need access to internet, then Route Table is needed
   * Main Route Table and Custom route table
* NAT Gateway: Network Address Translation (NAT)
   * Internet Gateway is attached to VPC. NAT Gateway sits in a Public Subnet.
   * NAT gateway is not necessary. Instance in Public Subnet can directly access internet via IWG, only only when we want to configure to let instance sits in Private Subnet to have limited access to internet (only connection initiated from private subnet is allowed and must go through NAT), we can configure NAT to control this.
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
