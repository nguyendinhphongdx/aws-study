+++
title = "VPC Subnet"
date = 2020
weight = 2
chapter = false
pre = "<b>1.2. </b>"
+++

A subnet is a segment of an Amazon VPC’s IP address range where you can launch Amazon EC2 instances, Amazon Relational Database Service (Amazon RDS) databases, and other AWS resources. CIDR blocks define subnets (for example, 10.0.1.0/24 and 192.168.0.0/24). The smallest subnet that you can create is a /28 (16 IP addresses). AWS reserves the first four IP addresses and the last IP address of every subnet for internal networking purposes. For example, a subnet defined as a /28 has 16 available IP addresses; subtract the 5 IPs needed by AWS to yield 11 IP addresses for your use within the subnet.

After creating an Amazon VPC, you can add one or more subnets in each Availability Zone. Subnets reside within one Availability Zone and cannot span zones, so remember that one subnet equals one Availability Zone. You can have multiple subnets in one Availability Zone.

Subnets can be classified as **Public**, **Private**, or **VPN-only**. 
* A **Public subnet** is one in which the associated route table (discussed later) directs the subnet’s traffic to the Amazon VPC’s IGW (also discussed later). 
* A **Private subnet** is one in which the associated route table does not direct the subnet’s traffic to the Amazon VPC’s IGW. 
* A **VPN-only subnet** is one in which the associated route table directs the subnet’s traffic to the Amazon VPC’s VPG (discussed later) and does not have a route to the IGW.

Regardless of the type of subnet, the internal IP address range of the subnet is always private (that is, non-routable on the Internet). Default Amazon VPCs contain one public subnet in every Availability Zone within the region, with a netmask of /20.

