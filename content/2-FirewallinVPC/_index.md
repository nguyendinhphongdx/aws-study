---
title : "Firewall in VPC"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 2. </b> "
---

#### Firewall in VPC

In this section, we will learn the basic security features in Amazon VPC such as the Security Group firewall feature and Network Access Control Lists.

A security group (SG) acts as a virtual firewall for the EC2 Instance, helping to control network traffic. An Instance in VPC can be assigned up to 5 Security groups because SG only works at the Instance layer and not at the Subnet layer.

{{% notice note %}}
Security groups operate at the virtual machine level, not the subnet level. However, each virtual machine in a subnet can be assigned to different security groups.
{{% /notice %}}

A network access control list (NACL or network ACL) is an optional security layer for VPCs that acts as a firewall to control incoming and outgoing traffic for one or more subnets.
You can set up network ACLs with the same rules as security groups, to add an extra layer of security to the VPC.

#### Content

- [Security groups](2.1-securitygroup/)
- [Network ACLs](2.2-networkacls/)