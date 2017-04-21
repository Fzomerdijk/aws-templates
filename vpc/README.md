# AWS Virtual Private Cloud (VPC) template (IPv4)
Within Amazon Web Services a default VPC is available to you, but all subnets are public facing (no public subnets are created) and ready-to-go template of a VPC with public- and private-subnets are hard to find. 
This template will create a VPC with both public and private subnets. 
These public and private subnets will be created in ALL the availability zones that are present (April 2017) within the chosen region. The template is configurable via parameters so you can use the same template throughout in all environments.

Currently we have these versions of the template:

* [VPC with open Access Control Lists (ACL)](./AWS_VPC_open_ACL_template.json)

This is a template that contains a VPC with public and private Access Control Lists, but these ACL's do not have any restriction. So it is up to you to setup appropriate rules!  

* [VPC with strict Access Control List (ACL)](./AWS_VPC_strict_ACL_template.json)

In this template the Access Control Lists (ACL) are accoding to the <a href="https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Appendix_NACLs.html#VPC_Appendix_NACLs_Scenario_2?raw=true" target="_blank">AWS recommended settings</a>, 
for a VPC with both public and private subnets.

### Explanation of terms used
* Public subnet – can connect to the internet through an Internet Gateway. Instances in a public subnet require public IP's to connect to the internet.
* Private subnet – cannot directly connect to the internet by default. Traffic needs to be routed via a NAT gateway (placed in a public subnet). Private subnet instances only need a private IP.
* <a href="https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Internet_Gateway.html" target="_blank">AWS Internet Gateway</a> - An AWS Internet Gateway is a horizontally scaled, redundant, and highly available VPC component that allows communication between instances in your VPC and the Internet.
* <a href="https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpc-nat-gateway.html" target="_blank">AWS NAT Gateway </a> - You can use a network address translation (NAT) gateway to enable instances in a private subnet to connect to the Internet or other AWS services, but prevent the Internet from initiating a connection with those instances.

Note: This template cannot be used if your instances only have a IPv6 network interface. In that case, a <a href="https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/egress-only-internet-gateway.html" target="_blank">AWS Egress-Only Internet Gateway</a> should be used instead of a (IPv4) NAT instance.

## Parameters
1. **Environment Type** - A Select list (Dev, ACC, PROD) used to “tag” the resources created. Note: once VPC is created, this option cannot be changed.
1. **ClassB subnet number** - A number between 0 and 255 used for the IPv4 IP range of the VPC and subnets (10.xxx.0.0/16). Note: once VPC is created, this option cannot be changed.
1. **Add external IP by default** - No / Yes - Should we (by default) add an external IP in PUBLIC subnet(s)? 
1. **Add NAT Gateway** - No / Yes - Should we add a NAT gateway, so instances in the PRIVATE subnet can connect to internet.
1. **High Available Nat Gateway setup** - Only one NAT gateway or High Available setup (in all AZ's a NAT-GW). This option only applies when the option Add NAT Gateway is set to “Yes”
1. **Resource Tagging** - A name, saved with created resources.

See also <a href="./images/Create-Stack-Parameters.png?raw=true" target="_blank">this screen shot</a> for the options.


## VPC with public and private subnets - no NAT gateway
If you want to create a VPC with both public and private subnets in all availability zones in a region. But the private subnets do NOT need any internet access. You can use the template with the parameter **Add NAT Gateway** set to **No**

![Architecture](./images/VPC-Private-No-Internet-access-four-regions.png?raw=true "VPC, private subnets has no internet connectivity")

## VPC with public and private subnets - one NAT gateway
If you set the parameter **Add NAT Gateway** to “Yes” but the parameter **High Available Nat Gateway setup** to “Only one”; all private subnets can connect to the internet through one AWS NAT Gateway. This NAT Gateway will be created in the first availability zone of that region. If that region fails (for whatever reason), the private subnets will lose their internet connectivity. This option should be used for DTA environments to save costs. Or for production environments where internet connectivity from private subnets is not always required.

![Architecture](./images/VPC-One-NAT-GW-four-regions.png?raw=true "VPC with one NAT gateway")

## VPC with private subnets - high available NAT gateways
If you set the parameter **Add NAT Gateway** to “Yes” but the parameter **High Available Nat Gateway setup** to “Highly Available”; all private subnets can connect to the internet via their own AWS NAT Gateway. So, in every availability zone a AWS NAT Gateway is created, ensuring the internet connectivity of that private subnet. This option should only be used if the private subnets are depending on an internet connection. This is also the most expensive option and should be used carefully.

![Architecture](./images/VPC-HA-four-regions.png?raw=true "VPC high available setup")

## Other Templates
* [Bastion Host (high available)](../bastion/)

## Feedback & support
If you want to give feedback or need support, please contact us at: [Indivirtual Support](mailto:support@indivirtual.com)
