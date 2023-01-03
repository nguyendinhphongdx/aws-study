---
title : "Create Internet Gateway"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 3.3 </b> "
---

#### Create Internet Gateway

1. In the **VPC** interface

- Select **Internet gateways**
- Select **Create internet gateway**

![Create VPC](/images/3-Prerequiste/3.2-igwandroutetable/0001-igwandroutetable.png?featherlight=false&width=90pc)

2. Make configuration

- **Name tag**: enter `Internet Gateway`
- Select **Create internet gateway**

![Create VPC](/images/3-Prerequiste/3.2-igwandroutetable/0002-igwandroutetable.png?featherlight=false&width=90pc)

3. Finish creating **Internet Gateway**

![Create VPC](/images/3-Prerequiste/3.2-igwandroutetable/0003-igwandroutetable.png?featherlight=false&width=90pc)

4. Implement **Attach VPC**

- Select **Actions**
- Select **Attach to VPC**

![Create VPC](/images/3-Prerequiste/3.2-igwandroutetable/0004-igwandroutetable.png?featherlight=false&width=90pc)

5. Select **ASG**, and VPC ID will be automatically filled in.

- Select **Attach internet gateway**

![Create VPC](/images/3-Prerequiste/3.2-igwandroutetable/0005-igwandroutetable.png?featherlight=false&width=90pc)

6. When attach successfully, **State** internet gateway will switch to **Attached**

![Create VPC](/images/3-Prerequiste/3.2-igwandroutetable/0006-igwandroutetable.png?featherlight=false&width=90pc)