---
title : "Route Table"
date :  "`r Sys.Date()`" 
weight : 2 
chapter : false
pre : " <b> 1.2 </b> "
---

## Route Table

The **Route Table**, also referred to as the *routing table*, is responsible for providing routing instructions within a network and is associated with specific subnets.

For instance, in the scenario where a Virtual Private Cloud (VPC) is established with the network layer `10.10.0.0/16`, along with two subnets, `10.10.1.0/24` and `10.10.2.0/24`, each default subnet will be allocated a default route table.

Inside the route table, there will exist a route entry with the following details:
- **Destination**: `10.10.0.0/16`
- **Target**: `local`

This particular route entry signifies that resources created within the same VPC can communicate with each other.

![Route Tables](/images/1-Introduce/routetable.png?featherlight=false&width=50pc)
