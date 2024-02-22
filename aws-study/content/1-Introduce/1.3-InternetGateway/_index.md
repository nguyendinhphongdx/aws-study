---
title : "Internet Gateway"
date :  "`r Sys.Date()`" 
weight : 3 
chapter : false
pre : " <b> 1.3 </b> "
---

## Internet Gateway

The **Internet Gateway (IGW)** is a crucial component of Amazon VPC that facilitates communication between resources within the VPC, specifically EC2 instances, and the Internet. The IGW exhibits robust horizontal scalability, along with high levels of redundancy and availability. It operates as a designated target within the Amazon VPC's routing table, playing a vital role in directing traffic from EC2 instances to the external Internet. This process involves translating the network address of the EC2 instance into its corresponding Public IP address.

More precisely, the EC2 instances located within the VPC are only aware of their assigned Private IP addresses. However, when there is a need to transmit traffic from these EC2 instances to the Internet, the IGW intervenes by translating the originating Private IP address into the Public IP address (or Elastic IP addresses, as discussed later) assigned to the respective EC2 instance. This translation is upheld through a one-to-one mapping, which persists until the Public IP address is released.

Conversely, when the EC2 instances receive incoming traffic from the Internet, the IGW undertakes the task of translating the target address (Public IP address) into the corresponding Private IP address of the EC2 instance. Subsequently, the IGW forwards this traffic into the Amazon VPC.

![Internet Gateway](/images/1-Introduce/igw.png?featherlight=false&width=60pc)
