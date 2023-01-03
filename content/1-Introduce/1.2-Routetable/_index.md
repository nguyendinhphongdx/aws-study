---
title : "Route Table"
date :  "`r Sys.Date()`" 
weight : 2 
chapter : false
pre : " <b> 1.2 </b> "
---

#### Route Table

The Route Table, also known as the routing table, provides routing instructions and is assigned to the Subnets.
For example, when you create a VPC with network layer 10.10.0.0/16, with 2 subnets 10.10.1.0/24,10.10.2.0/24, each default subnet will be assigned a default route table.\
Inside the route table there will be a route entry **destination**:10.10.0.0/16 **target**:local. This route entry shows that resources created in the same VPC can be connected to each other.

![Route Tables](/images/1-Introduce/routetable.png?featherlight=false&width=50pc)