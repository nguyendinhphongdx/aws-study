---
title : "Security Group"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 2.1 </b> "
---

#### Security Group

Some basic features of Security group:

* Only Allow rules can be added, Deny rules cannot be added.
* Separate rules for outgoing or incoming traffic can be specified.
* A newly created Security group has no Inbound rules available.
  Therefore, at the beginning Instance will not allow any traffic to enter, requiring us to add an Inbound rule to allow access.
* By default, the Security group has an Outbound rule that allows all traffic to go out of the Instance.
This rule can be edited (deleted) and added with specific Outbound rules, specifying which traffic originating from the Instance is allowed to go out.
If the SG does not have an Outbound rule, then no traffic is allowed to leave the Instance.
* Security groups is a Stateful service - meaning that if traffic going into the Instance is allowed, then the traffic can also go out of the Instance, and vice versa, regardless of the Outbound rule.
* Instances can only communicate with each other if and only if they are associated with a Security group that allows connections, or a Security group to which the Instance is associated contains a Rule that allows traffic (except for the default Security group. with default rules that allow all traffic to be accessed).
* Security groups are associated with network interfaces.
After the Instance has been initialized, you can still change the Security group assigned to the Instance, which also changes the security group assigned to the corresponding primary network interface.

#### Security group Rule

Rule is generated to grant access to traffic entering or leaving the Instance. This access can be applied to a specific CIDR or to a Security group located in the same VPC or located in another VPC but with a peering connection already initiated.

Basic components of Security group rule:
* (Inbound rules only) includes the origin (source) of the traffic and the destination port or port range.
The source can be another security group, an IPv4 or IPv6 CIDR range, or simply an IPv4 or IPv6 address.
* (Outbound rules only) includes the destination of the traffic and the destination port or destination port range.
The destination can be another security group, an IPv4 or IPv6 CIDR range, or simply an IPv4 or IPv6 address, or a service that begins with a prefix (e.g. igw_xxx) in the prefix ID list (a service is identified by the prefix ID - the name and ID of the service available in the region).
* Every protocol has some standard protocol. Example: SSH is 22,..