---
title : "Internet Gateway"
date :  "`r Sys.Date()`" 
weight : 3 
chapter : false
pre : " <b> 1.3 </b> "
---

#### Internet Gateway

- The Internet Gateway (IGW) is an Amazon VPC component that enables resources within the VPC, specifically EC2, to communicate with the Internet. IGW has strong horizontal elasticity, and high redundancy and availability. It acts as a target in the Amazon VPC's routing table, helping traffic to be routed outside the Internet by translated EC2's network address into the Public IP address it has been assigned.

- More specifically, EC2 Instances inside the VPC only know the Private IP addresses assigned to it, but when there is traffic sent from EC2 out to the Internet, the IGW will translate that Private IP address into a Public IP address ( or EIP addresses, discussed later) that are assigned to EC2, and maintain a 1-1 mapping until the Public IP address is released. 

- When EC2 receives traffic from outside the Internet, the IGW translates the Target address (Public IP address) into the EC2 Instance Private IP address and forwards the traffic to the Amazon VPC.

![Internet Gateway](/images/1-Introduce/igw.png?featherlight=false&width=60pc)