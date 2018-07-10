# Amazon Web Services - Bastion Host / Jump Box - AWS Linux
Best practice for any environment is to give minimal access and minimal exposure of any (AWS) resources. To include a bastion hosts in your VPC environment enables you to securely connect to your Linux instances without exposing your environment to the Internet. After you set up your bastion hosts (also called Jump Box), you can access the other instances in your VPC through Secure Shell (SSH) connections on Linux. Bastion hosts are also configured with security groups to provide fine-grained ingress control.

This template will create a Bastion Host (Jump Box) in your VPC. The Bastion Host is added to a AWS autoscale group for high availability. Is configured to automatically install security updates and restarts every night (1:30 am - timezone of availability zone), to ensure the installed security patches are applied.

## Highlights
* An Elastic IP address (EIP) is created and assigned to your EC2 Bastion Host. This EIP is used as long as your CloudFormation Bastion stack is available. If the stack is deleted, the EIP is also removed from your account (not restrained), so you will not be charged for any unused EIP's. 
* All log of the Bastion Host is saved in CloudWatch and will be retained as long as you specify when you apply this stack.
* You can specify if you want "<a href=”https://en.wikipedia.org/wiki/Sudol” target="_blank">sudo</a>" to be enabled or not on your Bastion Host. By default sudo is NOT enabled for extra security.

## Parameters
1. **Environment Type** - A Select list (Dev, ACC, PROD) used to “tag” the resources created. Note: This option should match your VPC "Environment Type" option.
1. **ClassB subnet number** - A number between 0 and 255 used for the IPv4 IP range of the VPC and subnets (10.xxx.0.0/16). Note: This option should match your VPC "ClassB subnet number" option.
1. **SSH ip Range** - The CDIR IP range to give SSH access from to your Bastion Host. This should be the external IP range of your home / work network.
1. **EC2 Keypair** - Your Amazon SSH Key Pair, to be used to connect to your EC2 instances in this region.
1. **Instance typee** - Instance type of the SSH Bastion Host.
1. **Enable T2 CPU Unlimited burst** - Should we enable CPU Unlimited burst? Only available on T2 instance types.
1. **Log retention in days** - Should we enable <a href=”https://en.wikipedia.org/wiki/Sudol” target="_blank">sudo</a> on Bastion Hos? (No = default, more secure)
1. **Notification E-mail address** - The E-Mail address to send scaling notifications to.
1. **Resource Tagging** - A "developer" tag-name, saved with created resources.

## Other Templates
* [Virtual Private Cloud (VPC)](../vpc/)
* [Bastion Host / Jump Box (high available)](../bastion/)

## Feedback & support
If you want to give feedback or need support, please contact us at: [Indivirtual Support](mailto:support@indivirtual.com)
