title: AWS - Workspace
date: 2018-07-02 19:38:23
tags:
- AWS
- AWS Workspace
---

# AWS WorkSpaces and WorkSpaces Application Manager

* One user one VM; data based on EBS
* AWS workspaces is for desktops; AWS EC2 is for servers
* Integrate with existing tools: AD;Intranet; MFA; SCCM (System Center Configuration Manager)
* Work well with a lot of patterns
   * BYOD (bring your own devices)

## Updates

* For standard bundle, no upgrade cost
* support BYOL (bring your won lisence)
* Volumn Encryption with AWS KMS
* FMA (Radius )
   * Remote Authentication Dial-In User Service (RADIUS) is a networking protocol, operating on port 1812 that provides centralized Authentication, Authorization
* Certification - SOC 1,2 ISO9001 and 27001

## Demo

* after setting up, connect to existin AD
* Using wizard to launch workspace
  * create user in AD
  * select bundle ( laptop with certain images )
  * wait 20 min till workspace fully launched
* User use workspace client to connect to the server

# WAM (workspaces Application manager)

* Deploy track and update apps on user's workspaces
* bring your won apss or subscribe apps from aws marketpalce
* Gain availabity and control over app usage
* support app versioning

## Demo

* WAM has a catelog containing all apps the current account owns
* select muti apps from catelog and assign to AD user or groups
* during assign wizard, configure options allows
   * assign certain version of the apps
   * installation type (optional or required);
   * auto install or optional install
* uninstall from WAM
   * by removing the user from app subscription


# References

>
https://youtu.be/uE4x5kWVox4

# Workspaces Best Practises

## background

* 2000 users
* have aws direct connect

## AWS Account Structure

* Payer/Linked Account structure
* Only cerntral logging in Payer Account
* WOrkSpaces in separate account
* Consistent tagging standards across all acounts
* Set up IAM access to allow L1 helpdesk to reboot the workspace
* Use a dedicated AWS account for user management

## Network Deployment considerations

### VPC design best practise

* Rule 1: Eliminate IP waste (be frugal with what you use)
* Rule 2: Minimum 2 subnets
* Rule 3: Be flexible to accommodate for future.

* Based on 2k users, we design /20 and get 4k ip addresses
* use subnet to isolate DEV/PROD env
* Set up cross-account VPC peering. (AWS CLI)

![aws workspace network topo](https://github.com/racheliurui/markdown/blob/master/Trending/AWS/images/Extra_AWS_Workspace_Diagram.png?raw=true)



## Directory Services Design Consideration

* AWS AD instances sits in management account hosting AD
* Use VPC peering to peer to another account which hosting the VPC with workspace instances running (and with AD connector deployed).
* Set up AD sites and services ( avoid routing back to on-promise to authenticate) --- setting inside AD (add region name in site naming convention)

## Demo with configuration

* Enable / Disable MFA
  * Specify the RADIUS server;
  * Provision muti RADIUS for HA purpose; Can host RADIUS on EC2; Implement multi ADC to archive support for multi RADIUS standards
* Setting to connect with AD
* Local Admininistrator enable/ disable
* Use AD SItes and Services to correct opration of Directory Service
* Sparated workspace OU to apply group policy
* Check ingress ports are open for Directory Service communications
* Use AD Group Policy to manage workspace
* Use AWS Cli to spin up workspace


# References

>
https://youtu.be/9Q-ahnw2Lsc
