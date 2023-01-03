---
title : "Create VPC"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 3.1 </b> "
---

#### Create VPC

1. Access the **AWS Management Console** interface

- Find **VPC**
- Select **VPC**

![Create VPC](/images/3-Prerequiste/3.1-vpcandsubnet/0001-createvpcandsubnet.png?featherlight=false&width=90pc)

2. In the **VPC** interface

- Select **Yours VPC**
- Select **Create VPC**

![Create VPC](/images/3-Prerequiste/3.1-vpcandsubnet/0002-createvpcandsubnet.png?featherlight=false&width=90pc)

3. Follow the steps to create a VPC

- **Resource**, select **VPC only**
- **Name tag**, enter `ASG`
- **IPv4 CIDR**, enter `10.10.0.0/16`

![Create VPC](/images/3-Prerequiste/3.1-vpcandsubnet/0003-createvpcandsubnet.png?featherlight=false&width=90pc)

{{% notice warning %}}
In the **Tenancy** configuration, we will leave the default mechanism. If we switch to Dedicated, there will be some **EC2 Instance type** that is not suitable and will not be created in VPC with **tenancy mode** as **Dedicate**
{{% /notice %}}

4. Select **Create VPC**

![Create VPC](/images/3-Prerequiste/3.1-vpcandsubnet/0004-createvpcandsubnet.png?featherlight=false&width=90pc)

5. Finish creating **VPC**

![Create VPC](/images/3-Prerequiste/3.1-vpcandsubnet/0005-createvpcandsubnet.png?featherlight=false&width=90pc)