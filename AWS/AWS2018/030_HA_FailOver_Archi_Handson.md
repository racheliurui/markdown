title: AWS - High Available and Fault Tolerant Architecture
date: 2018-04-19 15:07:00
tags:
- AWS Architecture
- Faut Torlerant Architecture
---

# 085.mp4 --- HA and Fault Torerant Architecture hands on overview

## 086.mp4 -- focusing on VPC

### Advanced VPC Architecture

* The Advanced VPC Architecture can use CloudFormer to duplicate into different regions
  * __Route53__ (Global) handle cross Region requests to IGW sitting in each region and the CloudFront Distribution
      * Route53 is Global service
  * __CloudFront__ (Global) caching source linked to S3 bucket.
      * CloudFront is Global service
  * 2 __VPC__ : sitting in different regions.
      * VPC can't span region
  * 2 __S3__ : each region has 1 S3 bucket and using "Service Endpoint" linking to VPC in same rigion; one of the S3 bucket is used as the other S3's replica.
      * S3 can't span region
      * S3 can connect to VPC via "service endpoint"
      * S3 can have replica in another region
  * 2 __ELB__ : each region has one ELB, receiving request from IWG and banlance the request to Instances sitting accross AZs.
      * ELB can span AZ and balancing request accross AZ
      * ELB can't span Region
  * 4 __Availability Zones__ : each region has 2 AZ. EC2 instances and Aurora DB services are splitted into 2 Regions and 2 AZs in each region.
  * 2 __AutoScaling Group__ : each region has 1 auto scaling group to contain the EC2 clusters. Each AutoScaling Group is spanning 2 AZs.
  * 4 __Public Subnet__: each region has 1 VPC and each VPC has 2 public subnet sitting in different AZs.
      * subnet can't span AZs. (Subnet has property to specify its AZ id)
      * each of the public subset contains partial of the EC2 cluster ( which belongs to the AutoScaling Group)
  * 2 __NAT Service__ : each region has 1 NAT service sits in one of the public subnet to provide NAT service to instances sitting in Private subnet.
  * 4 __Private Subnet__ : Each region has 2 private subnet sitting in different AZs. These subnet used to contain data services.
      * subnet can't span AZs.
  * 5 __Aurora service__ : each AZ has at least one Aurora service.
      *  The main Aurora  and standby Aurora sits in one Region but splitted into different AZ.
      *  One AZ has the main Aurora, and all the other AZ has the read replica.
      * Aurora support cross Region Read Replica
        * https://aws.amazon.com/blogs/aws/new-cross-region-read-replicas-for-amazon-aurora/
  * 2 __DB Subnet Group__ : each region has one


### hands on by creating one VPC in one region that meet the Advanced VPC archi design

* Use wizard to create the VPC and then review the config
  * choose the wizard to create VPC with 1 pubic subnet and 1 private subnet
  * select CIDR for each subnet and select same AZ for both subnet
  * Specify NAT instance type and key pair
  * Specify S3 Service Endpoint access level (none / public only / private only / both; full access / custom )
* understanding the Route Table being used / created in Public and Private Subnet
  * public subnet的路由表，0.0.0.0/0指向IWG; Private subnet，0.0.0.0/0指向NAT的ENI
  * 一条s3的请求指向特定的VPC（service endpoint）
  * 一条本地VPC内部的局部路由
  * route table is explicitly associated to public subnet; route table is implicitly associated to private subnet
* Check默认的ACL ：allow inbound/outbound everything
* Check the NAT service being created via wizard
  * Virtulization is using paravirtual ( new EC2 instances are more using HVM )
  * NAT security group by defaut is allow all
* To fix above issues,
  * Change the network interface to "not being deleted after termination"
  * terminate the current NAT instance and check the network interface's status become "available"
  * create a new VPC security group to be used by NAT instance
     * allow inbound http(s) from private subnet
     * allow inbound ssh from current client ip
     * allow outbound http(s) to anywhere
     * allow all inbound traffic from current security group (!!! ???)
  * change the network interface's security group to use the new security group we just created.
     * VPC security group is binding with instance (attach to network interface is the same to attach to instance)
  * review the VPC security group vs ACL (access control list)

## 087.mp4 -- going on with re-create the NAT instance

* when creating the new NAT instance, search from community AMI with key word "NAT HVM" to select the existing Image
* select the existing public subnet to contain this new NAT instance
* Disable "Assign public IP" because we will attach existing network interface to it
* Network Interfaces section: attach existing one to this new NAT instance
* Select the newly created Security Group , the NAT security group
* Review and launch the instance

## 088.mp4 -- In the same region, create subnet in another AZ and create ACL for all subnets

* Create the new private subnet sitting in Same VPC but different AZ. (design the size accordingly)
* Create the new public subnet sitting in Same VPC but different AZ. (design the size accordingly)
* Review the route table being created for both new Subnets
  * the route table attached by default with public VPC is wrong , it's the route table being used by private subnet; change it to the other one that routing internet traffic to iwg.
* Create new ACL called "Public NACL" which sits inside existing VPC.
  * ACL rule has an number, it will be applied using Number sequence
  * allow inbound http(s) from internet,inbound ssh from client
  * 1024-65535 (by ELB health check) from internet
    * https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/elb-security-groups.html
  * allow outbound http(s) to internet, outbound  ssh to all private subnets, outbound 1024-65535 to __internet__, port 3306(mysql ) to all private subnets
  * associate newly created ACL to 2 public subnets
* Create new ACL called "private NACL" which sits inside existing VPC
  * Allow inbound MySQL (3306) from both public subnets ; allow inbound ssh from both public subnets; __inbound 32768-61000 from internet (NAT)__
  * Allow outbound http(s) to internet; allow __outbound 32768-61000 (mysql response)__ to public subnets;
  * Associate newly created ACL to 2 private subnets
* the ACL inbound and outbound rule will have a default deny rule at the end.

* If ping doesn't work, check the ICMP protocol at security group and ACL level
* If ssh doesn't work, check ACL outbound protocol allow 32768-61000 to internet: which means the ssh respond to the ssh client.

## 089.mp4 -- creating AutoScaling group in existing VPC ; create ELB to dispatch requests

* Create security group for the web server EC2 instances
  * inbound http(s) from internet; ssh from local client; all traffic from itself
  * outbound all traffic to anywhere
* Create Load Balancer (EC2 --> Network & Security --> Load Balancers)
  * select current VPC for this new Load balancer to sit in
  * select dispatch HTTP protocol from 80 to 80 port (if https select , we need to upload ssl cert to ELB)
  * select both of our public subnets as the dispatching targets
  * select newly created security group for web server to attach with
  * configure the health check which used by ELB
  * select none of the existing EC2 instance (because we will use AutoScaling group) and enable cross zone load balancing and connection draining

* Edit the load balancer's default configs :
  * (this configuration moved to ELB group): enable loadbalancer generated stickyness with expiration 60 seconds
* Create AutoScaling Group by create launch configuration & AutoScaling Group
  * create launch configuration
    * select AMI (search for worldpress AMI)
    * configure the instance configurations (monitoring / role / script ), disable assign public ip
    * attach newly created web security group
  * create AutoScaling group using existing launch configuration
    * starting with 2 instances
    * select existing VPC
    * multi-select 2 public subnet
    * select "receive traffic from Elastic Load Balancer(s)" and select newly created ELB
    * Health check type select "ELB"
    * "configure scaling policies" :
      * scale from 2-10;
      * scale up : create a cpu usage alarm to trigger increase ; wait 600 before allow another activity
      * scale down : similiar
    * optional : add notification if scaling happened
  * review the result
    * 2 web instances being created
    * scaling history showing the action being done.
    * review and edit ELB's health check configuration (interval from 30 to 60 sec)
  * terminate one webserver manually and check another one being launched automatically

## 090.mp4 -- RDS Aurora service creation and configuration

* Create security group for RDB servcie
  * allow inbound MySQL(3306) from webserver security group
  * allow outbound http(s) to internet
* Edit previous web security group
  * allow outbound to newly created DB security group
* Switch to RDB service , create "DB Subnet Group " to launch multi AZ db instances
  * create the group and select existing VPC
  * multi select our 2 private subnets
* Launch RDB instance
  * select RDB type; enable is enable multizone; select db engine version; select instance type;
  * give instance name ;user and password;
  * select existing VPC and newly created DB subnet group
  * disable public access to the db instance
  * attach newly created DB security group
  * give database name and other config (port/encryption/backup and retention config/ maintainance config )
* review the result
* create read replica for the database
  * select newly created "DB subnet group" and select the AZ where we want the read replica sits (instead of select subnet, here we need to select the AZ which already being binded with subnet when we create the DB subnet group)
* review the result : the main database and read replica will have different endpoint , how to handle from web application
  * we can't use ELB to dispatch request to RDB
  * possible solution is create HAProxy
