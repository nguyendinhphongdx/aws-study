---
title : "Introduction"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 1. </b> "
---

#### Introduction of Amazon VPC

Amazon Virtual Private Cloud (Amazon VPC) is a “Virtual Private Cloud” which is a custom virtual network located inside the AWS Cloud and isolated from the entire outside world. The concept is similar to designing and implementing a separate standalone network environment in an on-premise data center, which is still very popular today in many countries of the world.

Inside that custom VPC, you have full control over your virtual network environment, meaning both the ability to initialize and run AWS resources and the ability to select IP address ranges, create networks subnets and configure routing tables and network gateways. You can use both IPv4 and IPv6 for secure and easy access to resources and applications in the VPC.

Region is a concept that describes many extremely large clusters of AWS data centers located in a certain territory. Within a region, you can create multiple VPCs, and each VPC is distinguished by different ranges of IP address spaces. You specify the IPv4 address range by selecting a Classless Inter-Domain Routing (CIDR), such as 10.0.0.0/16. The Amazon VPC address range cannot be changed once it has been created. Amazon VPC address ranges can be as large as /16 (ie 65536 available addresses) or as small as /28 (ie 16 available addresses) and they must not overlap with any other networks to which they will be connected. connect to.

The Amazon VPC service was launched after the Amazon EC2 service, so at some point, AWS offered two different networking platforms, EC2-Classic and EC2-VPC. EC2-Classic is the first networking platform where all Amazon EC2 is created in a single flat network, sharing connectivity among AWS customers. As of December 2013, AWS only supports EC2-VPC with default VPC created in each Region and a default subnet with a CIDR block of 172.31.0.0/16.

#### Content

- [Subnets](1.1-subnets/)
- [Route Table](1.2-routetable/)
- [Internet Gateway](1.3-internetgateway/)
- [NAT Gateway](1.4-natgateway/)

Now we will go through the most basic concepts of VPC in the next sections.