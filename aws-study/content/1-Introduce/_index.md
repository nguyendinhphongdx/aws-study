---
title : "Introduction"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 1. </b> "
---

## Introduction to Amazon VPC

Amazon Virtual Private Cloud (Amazon VPC) is a **Virtual Private Cloud**—a customized virtual network hosted within the AWS Cloud and isolated from the external world. This concept resembles the design and implementation of a distinct standalone network environment in an on-premise data center, a practice still widely employed across many countries.

Within this dedicated VPC, users possess comprehensive control over their virtual network environment. This control encompasses the initiation and operation of AWS resources, the ability to choose IP address ranges, establish network subnets, and configure routing tables and network gateways. Secure and convenient resource and application access within the VPC is facilitated through both IPv4 and IPv6 protocols.

The term "Region" refers to vast clusters of AWS data centers situated within specific territories. Multiple VPCs can be established within a single region, with each VPC differentiated by its unique IP address space range. The IPv4 address range is defined by selecting a Classless Inter-Domain Routing (CIDR) notation, such as 10.0.0.0/16. Once created, the Amazon VPC address range remains immutable. These ranges can span from as extensive as /16 (equivalent to 65536 available addresses) to as limited as /28 (allowing for 16 available addresses). Crucially, these ranges must not overlap with any other connected networks.

The Amazon VPC service was introduced subsequent to the launch of Amazon EC2. Consequently, AWS provided two distinct networking platforms for a period: EC2-Classic and EC2-VPC. EC2-Classic established a single flat network where all Amazon EC2 instances operated, enabling shared connectivity among AWS clients. However, as of December 2013, AWS exclusively supports EC2-VPC. Each region includes a default VPC along with a default subnet featuring a CIDR block of 172.31.0.0/16.

## Contents

- [Subnets](1.1-subnets/)
- [Route Table](1.2-routetable/)
- [Internet Gateway](1.3-internetgateway/)
- [NAT Gateway](1.4-natgateway/)

In the following sections, we will delve into the fundamental concepts of VPC.
