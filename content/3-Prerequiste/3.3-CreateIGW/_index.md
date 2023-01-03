---
title : "Create Internet Gateway"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 3.3 </b> "
---

#### Create an Internet Gateway

1. In the **VPC** interface

   - Select **Internet Gateways**
   - Select **Create internet gateway**

![Create VPC](/images/3/0001.png?featherlight=false&width=90pc)

2. Make a configuration

   - **Name tag**, enter **```Internet Gateway```**
   - Select **Create internet gateway**

![Create VPC](/images/3/0002.png?featherlight=false&width=90pc)

3. Finish creating **Internet Gateway**

![Create VPC](/images/3/0003.png?featherlight=false&width=90pc)


4. Implement **Attach VPC**

   - Select **Actions**
   - Select **Attach to VPC**
   - Select **ASG**, VPC ID will be automatically filled in.
   - Select **Attach internet gateway**

![Create VPC](/images/3/0004.png?featherlight=false&width=90pc)

5. When attach successfully **State** internet gateway will switch to **Attached**

![Create VPC](/images/3/0005.png?featherlight=false&width=90pc)