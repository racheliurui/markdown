title: AWS - Handson NodeJs
date: 2018-04-09 09:57:23
tags:
- AWS
- AWS developer
---

# 027.mp4 , 028.mp4, 029.mp4 -- Set up dev environments


* create users and download their access credentials
* create A group stands for developer, attach policy to group and add users into the group

* create a role to stands for the EC2 instance
* create security group --- attach to VPC / define inbound and outbound rules (open http/https/ssh)
* launch instance --- select community AMI; enable public ip; attach the role created;protect against accidental termination; advanced details (put bash script) ; tag it; attach security group ; create and download keypair to access the instance
* install 2 useful plugins for atom: remote-edit git-plus ; edit EC2 server file ,save refresh the page
