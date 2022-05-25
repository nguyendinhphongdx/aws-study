---
title : "Subnets"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 1.1 </b> "
---

#### Subnets

A subnet is a segment of the IP address range that you use when you provision your Amazon VPC, which directly provides the active network range to the AWS resources that may run within it, such as Amazon EC2, Amazon RDS (Amazon Relationship Database). ),... Subnets are also identified through CIDR blocks (eg 10.0.1.0/24 and 192.168.0.0/24) and the subnet's CIDRs must be in the VPC's CIDR. The smallest subnet that can be created is /28 (16 IP addresses). AWS stores the first 4 IP addresses and the last 1 IP address of each subnet for intranet networking purposes. For example, subnet /28 has 16 available IP addresses, but removes 5 reserved IPs for AWS, leaving 11 usable IP addresses for resources operating within this subnet.

Availability Zone or abbreviated to AZ is a one or multi data center, located within Region and identified based on geographical location. Within an AZ there can be one or more subnets, but a subnet can only reside in a single AZ and cannot extend to other AZs.

Subnets are divided into categories such as **Public**, **Private**, or **VPN-only**.
**Public subnet** is a subnet with a route table (discussed later) that directs traffic within the subnet to the VPC IGW (discussed later)
The **Private subnet** is the opposite of the Public subnet, which does not have a route table that directs traffic to the VPC IGW.
**VPN-only subnet** is a subnet that has a route table that directs traffic to Amazon VPC's VPG (discussed later).

Regardless of the type of subnet, the subnet's internal IP address range is always private (that is, from outside the Internet it is not possible to directly connect to addresses in this range).

![Subnets](/images/1-Introduce/subnet.png?featherlight=false&width=50pc)