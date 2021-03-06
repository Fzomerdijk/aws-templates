# AWS Virtual Private Cloud (VPC) templates 
Within Amazon Web Services a default VPC is available to you in every region, but all subnets are public facing 
(no public subnets are created) and ready-to-go template of a VPC with public- and private-subnets are hard to find. 

These templates will create a VPC with both public and private subnets. The public and private subnets will be created in ALL the availability zones that are present (April 2018) within the chosen region. The template is configurable via parameters so you can use the same template throughout in all environments (Dev, acceptance and production).

Currently we have both IPv4 and IPv6 versions of the VPC templates available:

* [VPC - Only IPv4 address support templates:](./README_VPC_IPv4.md)
* [VPC - Both IPv4 & IPv6 addresses support templates:](./README_VPC_IPv6.md)

## VPC template considerations 
The template uses the function "**GetAZs**", this function returns only Availability Zones that have a default subnet. If a new subnet was added after you created your AWS account (in the region) and you encounter the error "*Select cannot select nonexistent value at index ..*". Your missing one of the subnets in that region.

To create the missing default subnet in a region, see:
* <a href="https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/default-vpc.html#create-default-subnet" target="_blank">AWS - Create missing default subnet</a>

## Other Templates
* [Bastion Host / Jump Box (high available)](../bastion/)
