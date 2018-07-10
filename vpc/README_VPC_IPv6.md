# AWS Virtual Private Cloud (VPC) template IPv4 and IPv6 support
Within Amazon Web Services a default VPC is available to you, but all subnets are public facing (no public subnets are created) and ready-to-go template of a VPC with public- and private-subnets are hard to find. 

These templates will create a VPC with both public and private subnets. These public and private subnets will be created in all the availability zones that are present (April 2017) within the chosen region. The template is configurable via parameters so you can use the same template throughout in all environments.

IPv6 addresses are globally unique, and are therefore public by default. If you want your instance to be able to access the Internet, but you want to prevent resources on the Internet from initiating communication with your instance, you should use an egress-only Internet gateway. 

These VPC templates add a egress-only Internet gateway by default. This will prevents the Internet from initiating an IPv6 connection with your private instances. Still you can switch off the IPv4 (IP version four) NAT gateways, so private instances are not able to connect to the Internet via IPv4. But this does not prevent your private instances to communicate to the internet over IPv6. If you want to block all IPv6 communication to private subnets, you need to do this via ACL and route tables.

Currently we have these versions of the template:

* [VPC with open Access Control Lists (ACL)](./AWS_VPC_open_IPv6_ACL_template.json)

This is a template that contains a VPC with public and private Access Control Lists, but these ACL's do not have any restriction. So it is up to you to setup appropriate rules!  

* [VPC with strict Access Control List (ACL)](./AWS_VPC_strict_IPv6_ACL_template.json)

In this template the Access Control Lists (ACL) are accoding to the <a href="https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Appendix_NACLs.html#VPC_Appendix_NACLs_Scenario_2?raw=true" target="_blank">AWS recommended settings</a>, 
for a VPC with both public and private subnets.

### Explanation of terms used
* Public subnet – can connect to the internet through an Internet Gateway. Instances in a public subnet require public IPv4's to connect to the internet. 
* Private subnet – cannot directly connect to the internet by default. Traffic needs to be routed via a NAT gateway (placed in a public subnet). Private subnet instances only need a private IP.
* IPv4 vs IPv6 - Where IPv6 network addresses are globally unique and are therefore public by default. A extra "Egress-Only Internet Gateways" is nessasary to block incoming traffic while still allowing outbound traffic for private subnets / instances.
* <a href="https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Internet_Gateway.html" target="_blank">AWS Internet Gateway</a> - An Internet gateway is a horizontally scaled, redundant, and highly available VPC component that allows communication between instances in your VPC and the Internet. It therefore imposes no availability risks or bandwidth constraints on your network traffic.
An Internet gateway serves two purposes: to provide a target in your VPC route tables for Internet-routable traffic, and to perform network address translation (NAT) for instances that have been assigned public IPv4 addresses.
An Internet gateway supports IPv4 and IPv6 traffic for your public subnets / instances.
* <a href="https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpc-nat-gateway.html" target="_blank">AWS NAT Gateway </a> - You can use a network address translation (NAT) gateway to enable instances in a private subnet to connect to the Internet or other AWS services, but prevent the Internet from initiating a connection with those instances.
* <a href="https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/egress-only-internet-gateway.html" target="_blank">AWS Egress-Only Internet Gateways</a> - An egress-only Internet gateway is a horizontally scaled, redundant, and highly available VPC component that allows outbound communication over IPv6 from instances in your VPC to the Internet, and prevents the Internet from initiating an IPv6 connection with your instances.

## Parameters
1. **Environment Type** - A Select list (Dev, ACC, PROD) used to “tag” the resources created. Note: once VPC is created, this option cannot be changed.
1. **ClassB subnet number** - A number between 0 and 255 used for the IPv4 IP range of the VPC and subnets (10.xxx.0.0/16). Note: once VPC is created, this option cannot be changed.
1. **Add IPv4 NAT Gateway** - No / Yes - Should we add a IPv4 NAT gateway, so instances in the PRIVATE subnet can connect to internet.
1. **High Available IPv4 Nat Gateway setup** - Only one IPv4 NAT gateway or High Available setup (in all AZ's a NAT-GW). This option only applies when the option Add NAT Gateway is set to “Yes”
1. **Resource Tagging** - A name, saved with created resources.

See also <a href="./images/Create-Stack-Parameters.png?raw=true" target="_blank">this screen shot</a> for the options.

### IPv4 adress ranges
Where "**xxx**" is the ClassB subnet number option as given in the template parameter section

|        Name       |      Value        |     Example    | Additional description  |
| ----------------- | ----------------- | -------------- | ----------------------- |
| VPC               | 10.*xxx*.0.0/16   | 10.33.0.0/16   |                         |
| PublicSubnet1a    | 10.*xxx*.0.0/24   | 10.33.2.0/24   | Max. 251 IPv4 adresses  |
| PublicSubnet1b    | 10.*xxx*.1.0/24   | 10.33.2.0/24   | Max. 251 IPv4 adresses  |
| PublicSubnet1c    | 10.*xxx*.2.0/24   | 10.33.2.0/24   | Max. 251 IPv4 adresses  |
| PublicSubnet1d    | 10.*xxx*.3.0/24   | 10.33.2.0/24   | Max. 251 IPv4 adresses  |
| PublicSubnet1e    | 10.*xxx*.4.0/24   | 10.33.2.0/24   | Max. 251 IPv4 adresses  |
| PrivateSubnet1a   | 10.*xxx*.5.0/24   | 10.33.5.0/24   | Max. 251 IPv4 adresses  |
| PrivateSubnet1b   | 10.*xxx*.6.0/24   | 10.33.6.0/24   | Max. 251 IPv4 adresses  |
| PrivateSubnet1c   | 10.*xxx*.7.0/24   | 10.33.7.0/24   | Max. 251 IPv4 adresses  |
| PrivateSubnet1d   | 10.*xxx*.8.0/24   | 10.33.8.0/24   | Max. 251 IPv4 adresses  |
| PrivateSubnet1e   | 10.*xxx*.9.0/24   | 10.33.9.0/24   | Max. 251 IPv4 adresses  |
	
### IPv6 adress ranges
Note that the IPv6 ranges can not be constructed but are dynamicaly created by AWS.

|        Name       |          Value          |          Example         | 
| ----------------- | ----------------------- | ------------------------ | 
| VPC               | ...:...:...:..00::/56   | 2a05:d014:640:8100::/56  | 
| PublicSubnet1a    | ...:...:...:..01::/64   | 2a05:d014:640:8101::/64  |
| PublicSubnet1b    | ...:...:...:..02::/64   | 2a05:d014:640:8102::/64  |
| PublicSubnet1c    | ...:...:...:..03::/64   | 2a05:d014:640:8103::/64  | 
| PublicSubnet1d    | ...:...:...:..04::/64   | 2a05:d014:640:8104::/64  | 
| PublicSubnet1e    | ...:...:...:..05::/64   | 2a05:d014:640:8105::/64  | 
| PrivateSubnet1a   | ...:...:...:..06::/64   | 2a05:d014:640:8106::/64  | 
| PrivateSubnet1b   | ...:...:...:..07::/64   | 2a05:d014:640:8107::/64  | 
| PrivateSubnet1c   | ...:...:...:..08::/64   | 2a05:d014:640:8107::/64  | 
| PrivateSubnet1d   | ...:...:...:..09::/64   | 2a05:d014:640:8109::/64  | 
| PrivateSubnet1e   | ...:...:...:..10::/64   | 2a05:d014:640:8110::/64  | 
	
## VPC with public and private subnets - no NAT gateway
If you want to create a VPC with both public and private subnets in all availability zones in a region. But the private subnets do NOT need any IPv4 internet access. You can use the template with the parameter **Add NAT Gateway** set to **No**

![Architecture](./images/VPC-Private-No-Internet-access-four-regions.png?raw=true "VPC, private subnets has no internet connectivity")

## VPC with public and private subnets - one NAT gateway
If you set the parameter **Add NAT Gateway** to “Yes” but the parameter **High Available Nat Gateway setup** to “Only one”; all private subnets can connect to the internet through one AWS NAT Gateway. This NAT Gateway will be created in the first availability zone of that region. If that region fails (for whatever reason), the private subnets will lose their internet connectivity. This option should be used for DTA environments to save costs. Or for production environments where internet connectivity from private subnets is not always required.

![Architecture](./images/VPC-One-NAT-GW-four-regions.png?raw=true "VPC with one NAT gateway")

## VPC with private subnets - high available NAT gateways
If you set the parameter **Add NAT Gateway** to “Yes” but the parameter **High Available Nat Gateway setup** to “Highly Available”; all private subnets can connect to the internet via their own AWS NAT Gateway. So, in every availability zone a AWS NAT Gateway is created, ensuring the internet connectivity of that private subnet. This option should only be used if the private subnets are depending on an internet connection. This is also the most expensive option and should be used carefully.

![Architecture](./images/VPC-HA-four-regions.png?raw=true "VPC high available setup")

### Template output values
The templates will output the next values and export these values also in your AWS (region) account, so they can be imported and used in another template. See also the <a href=”https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-importvalue.html” target="_blank">AWS documentation</a> about this.
The "Export Name" values in the table below, are from creation of a template with **Environment Type** set to *Dev* and *55* as the **ClassB subnet number**.   

|        Key        |          Value          |     Description     |    Export Name (example)     |
| ----------------- | ----------------------- | ------------------- | ---------------------------- |
| VPCName           | aws-vpc-id              | VPC Name            | *Dev*-VPC-*55*               |
| PrivateRouteTable | aws-rtb-id              | Private Route Table | *Dev*-PrivateRouteTable-*55* |
| PublicRouteTable  | aws-rtb-id              | Public Route Table  | *Dev*-PublicRouteTable-*55*  |
| VPCGateWay        | aws-igw-id              | VPC Gateway         | *Dev*-GateWay-*55*           |
| PublicSubnet1a    | subnet-id               | Public Subnet 1a    | *Dev*-PublicSubnet1a-*55*    |
| PublicSubnet1b    | subnet-id               | Public Subnet 1b    | *Dev*-PublicSubnet1b-*55*    |
| PublicSubnet1c    | subnet-id or "publ. 1a" | Public Subnet 1c    | *Dev*-PublicSubnet1c-*55*    |
| PublicSubnet1d    | subnet-id or "publ. 1a" | Public Subnet 1d    | *Dev*-PublicSubnet1d-*55*    |
| PublicSubnet1e    | subnet-id or "publ. 1a" | Public Subnet 1e    | *Dev*-PublicSubnet1e-*55*    |
| PrivateSubnet1a   | subnet-id or "publ. 1a" | Private Subnet 1a   | *Dev*-PrivateSubnet1a-*55*   |
| PrivateSubnet1b   | subnet-id or "publ. 1a" | Private Subnet 1b   | *Dev*-PrivateSubnet1b-*55*   |
| PrivateSubnet1c   | subnet-id or "publ. 1a" | Private Subnet 1c   | *Dev*-PrivateSubnet1c-*55*   |
| PrivateSubnet1d   | subnet-id or "publ. 1a" | Private Subnet 1d   | *Dev*-PrivateSubnet1d-*55*   |
| PrivateSubnet1e   | subnet-id or "publ. 1a" | Private Subnet 1e   | *Dev*-PrivateSubnet1e-*55*   |

## Other Templates
* [Bastion Host / Jump Box (high available)](../bastion/)

## Feedback & support
If you want to give feedback or need support, please contact us at: [Indivirtual Support](mailto:support@indivirtual.com)
