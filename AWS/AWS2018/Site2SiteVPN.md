title: AWS - Site to Site VPN
date: 2019-05-11 08:37:23
tags:
- AWS
- Site2SiteVPN
---


# Basic Steps

## Cloudformation
   * VPC with only private subnet; route table declared
   * VGW created and attached to VPC;
   * Propagation allowed via vgw to route table
   * CGW information declared;
## Create Site2SiteVPN
   * Pay attention to IPSec Tunnel Interconnection IP CIDR
        https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-ec2-vpnconnection-vpntunneloptionsspecification.html

   * Download configuration and run from client side
       * Pay attention to propagation CIDR

Client Side
1) Confirm the Client Gateway support BGP
2) Allocate the IpSec tunnel interconnection ip cidr
3) Allocate AWS VPC IP range
4) Confirm Data Centre Propagating IP Rages (default will be 0.0.0.0)
