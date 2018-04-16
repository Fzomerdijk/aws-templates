# AWS Virtual Private Cloud (VPC) templates 
Within Amazon Web Services a default VPC is available to you in every region, but all subnets are public facing 
(no public subnets are created) and ready-to-go template of a VPC with public- and private-subnets are hard to find. 

These templates will create a VPC with both public and private subnets. The public and private subnets will be created in ALL the availability zones that are present (April 2018) within the chosen region. The template is configurable via parameters so you can use the same template throughout in all environments (Dev, acceptance and production).

Currently we have both IPv4 and IPv6 versions of the VPC templates available:

* [VPC - Only IPv4 address support templates:](./README_VPC_IPv4.md)
* [VPC - Both IPv4 & IPv6 addresses support templates:](./README_VPC_IPv6.md)

## Other Templates
* [Bastion Host (high available)](../bastion/)

## Feedback & support
If you want to give feedback or need support, please contact us at: [Indivirtual Support](mailto:support@indivirtual.com)
