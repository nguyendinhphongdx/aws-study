---
title : "Create Customer Gateway"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 5.2.2 </b> "
---

#### Create Customer Gateway

1. Access to **VPC**

- Select **Customer Gateways**
- Select **Create Customer Gateway**

![Create VPC](/images/6-VPNSitetoSite/6.2-customgw/0001-customergw.png?featherlight=false&width=90pc)

2. In the **Create Customer Gateway** interface

- **Name tag**, enter **```Customer Gateway```**
- **IP address**, enter **public IP address** of the server **EC2 Customer Gateway**.
- Select **Create Customer Gateway**


![Create VPC](/images/6-VPNSitetoSite/6.2-customgw/0002-customergw.png?featherlight=false&width=90pc)

3. Wait about 5 minutes, finish creating **Customer Gateway**

![Create VPC](/images/6-VPNSitetoSite/6.2-customgw/0003-customergw.png?featherlight=false&width=90pc)

{{% notice tip %}}
Note: according to the architectural model, the Customer Gateway will reside in the VPC in the on-premise environment. What we are currently doing is declaring to AWS that we will have a Customer Gateway with a public IP address that is the public address of the EC2 instance: Customer Gateway is in the ASG VPN VPC
{{% /notice %}}