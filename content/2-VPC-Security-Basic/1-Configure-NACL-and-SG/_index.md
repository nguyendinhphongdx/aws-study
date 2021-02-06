+++
title = "Configure NACL & SG"
date = 2020
weight = 2
chapter = false
pre = "<b>2.1.2. </b>"
+++

A security group acts as a virtual firewall for your instance to control inbound and outbound traffic. When you launch an instance in a VPC, you can assign up to 5 security groups to the instance. 

{{% notice note %}}
Security groups act at the Instance level, not the Subnet level. Therefore, each instance in a subnet in your VPC can be assigned to a different set of security groups. 
{{% /notice %}}

A network access control list (ACL) is an optional layer of security for your VPC that acts as a firewall for controlling traffic in and out of one or more subnets. You might set up network ACLs with rules similar to your security groups in order to add an additional layer of security to your VPC. 

**Contents:**
- [Security groups for your VPC](#security-groups-for-your-vpc)
- [Network ACLs for your VPC](#network-acls-for-your-vpc)
