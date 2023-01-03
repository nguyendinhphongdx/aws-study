---
title : "Create Subnet"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 3.2 </b> "
---

#### Create subnet

1. In the **VPC** interface

   - Select **Subnets**
   - Select **Create subnet**

![Create VPC](/images/2/0001.png?featherlight=false&width=90pc)

2. In the **Create subnet** interface

   - Select **ASG** VPC

![Create VPC](/images/2/0002.png?featherlight=false&width=90pc)

3. Implement **subnet settings**

   - **Subnet name**, enter **```Public Subnet 1```**
   - Select AZ **ap-southeast-1a**
   - **IPv4 CIDR block**, import **```10.10.1.0/24```** according to architecture description
   - Select **Create subnet**

![Create VPC](/images/2/0003.png?featherlight=false&width=90pc)

4. Finish creating **subnet**

![Create VPC](/images/2/0004.png?featherlight=false&width=90pc)

5. Follow the same steps to create more subnets

   - **Public subnet 2** with **CIDR** of **10.10.2.0/24** located in **Availability Zone ap-southeast-1b**.

![Create VPC](/images/2/0005.png?featherlight=false&width=90pc)

   - **Private subnet 1** with **CIDR** of **10.10.3.0/24** located in **Availability Zone ap-southeast-1a.**


![Create VPC](/images/2/0006.png?featherlight=false&width=90pc)

   - **Private subnet 2** with **CIDR** of **10.10.4.0/24** located in **Availability Zone ap-southeast-1b**

![Create VPC](/images/2/0007.png?featherlight=false&width=90pc)


{{% notice tip %}}
You can see there is 2 columns **Availability Zone** and **Availability Zone ID**. To avoid EC2 resources being used **unevenly**, (we tend to use AZ a to run **primary and AZ** b to stand by for example) AWS will randomly assign * *Availability Zone into Availability Zone ID**. We can understand that Availability Zone is an alias, and Availability Zone ID is the identifier. For example, in the image above, Availability Zone ap-southeast-1a is assigned the Availability Zone ID apse1-az2. In another AWS account, the Availability Zone ap-southeast-1a may have an Availability Zone ID of apse1-az1.
{{% /notice %}}

![Create VPC](/images/2/0008.png?featherlight=false&width=90pc)

#### Allows automatic allocation of public IP addresses for 2 public subnets.

{{% notice tip %}}
Another point worth noting is that the subnets are the same, through configuring the routeing table and allocating a public IP address that we have just divided into Public and Private Subnet.
{{% /notice %}}

6. In the **VPC** interface

   - Select **Subnets**
   - Select **Public Subnet 1**
   - Select **Actions**
   - Select **Edit subnet settings**

![Create VPC](/images/2/0009.png?featherlight=false&width=90pc)

7. Under **Auto-assign IP settings**

   - Select **Enable auto-assign public IPv4 address**
   - Select **Save**

![Create VPC](/images/2/00010.png?featherlight=false&width=90pc)

8. Then do the same with **Public subnet 2.**

![Create VPC](/images/2/00011.png?featherlight=false&width=90pc)

![Create VPC](/images/2/00012.png?featherlight=false&width=90pc)