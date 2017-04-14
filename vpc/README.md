# AWS Virtual Private Cloud (VPC) template (IPv4)
Within Amazon Web Services a default VPC is available to you, but all subnets are public facing and ready-to-go examples of a VPC with public and private subnets are hard to find. This template will create in an AWS region a VPC with both public- and private-subnets. These public- and private-subnets will extend to any of the regions available (April 2017) availability zones. The template is configurable via some parameters so you can use one and the same template through your DTAP street.

## Parameters
1. **EnvironmentType** - A Select list (Dev, ACC, PROD) used to “tag” the resources created.
1. IPv4 subnet **class** - A number between 0 and 255 used for the IPv4 IP range of the VPC and subnets (10.xxx.0.0/16)
1. **AddExternalIP** - No / Yes - Should we (by default) add an external IP in PUBLIC subnet(s)? 
1. **AddNatGateway** - No / Yes - Should we add a NAT gateway, so PRIVATE subnet can connect to internet.
1. **DeveloperName** - A name, saved with created resources.

See also <a href="./images/Create-Stack-Parameters.png?raw=true" target="_blank">this screen shot</a> for the options.

## Other Templates
* [Bastion Host (high available)](../bastion/)

## VPC with publice and private subnets - no NAT gateway
ToDo - update comment here

![Architecture](./images/VPC-Private-No-Internet-access-four-regions.png?raw=true "VPC, private subnets has no internet connectivity")


## VPC with publice and private subnets - one NAT gateway
ToDo - update comment here

![Architecture](./images/VPC-One-NAT-GW-four-regions.png?raw=true "VPC with one NAT gateway")

## VPC with private subnets - high available NAT gateways
ToDo - update comment here

![Architecture](./images/VPC-HA-four-regions.png?raw=true "VPC high available setup")

