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

![Create VPC](/images/1/0001.png?featherlight=false&width=90pc)

2. In the **VPC** interface

   - Select **Yours VPC**
   - Select **Create VPC**


![Create VPC](/images/1/0002.png?featherlight=false&width=90pc)

3. Follow the steps to create a VPC

   - **Resource**, select **VPC only**
   - **Name tag**, enter **```ASG```**
   - **IPv4 CIDR**, enter **``10.10.0.0/16``**


![Create VPC](/images/1/0003.png?featherlight=false&width=90pc)

{{% notice warning %}}
For the **Tenancy** configuration, we will leave the default mechanism. If we switch to Dedicated, there will be some **EC2 Instance type** that is not suitable and will not be created in VPC with **tenancy mode** as **Dedicate**
{{% /notice %}}

4. Select **Create VPC**

![Create VPC](/images/1/0004.png?featherlight=false&width=90pc)

5. Finish creating **VPC**

![Create VPC](/images/1/0005.png?featherlight=false&width=90pc)


6. View the details of the newly created VPC. Check if not **Enable DNS resolution and DNS Hostname**

   - Go to **Edit VPC settings**
   - Select **DNS settings**
   - Select and **Save**.

![Create VPC](/images/1/0006.png?featherlight=false&width=90pc)