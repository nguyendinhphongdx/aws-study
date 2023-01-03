---
title : "Create Security Group"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 3.5 </b> "
---

#### Create Security Group

#### Create Security Group for servers located in Public subnet

1. In the **VPC** interface

   - Select **Security Group**
   - Select **Cretae security group**

![Create VPC](/images/5/0001.png?featherlight=false&width=90pc)


2. Configure **Security group**

   - **Security Group name**, enter **```Public subnet - SG```**
   - **Description**, enter **Allow SSH and Ping for servers in private subnet.**
   - Select **ASG** VPC

![Create VPC](/images/5/0002.png?featherlight=false&width=90pc)

3. Configure **Inbound rules**

   - In **Inbound rules**, click **Add rule**.

   - Select **Type**: **SSH** and **Source**: **My IP**. **My IP** represents 1 public IPv4 address you are using (will change when you change network)

   - Select **Add rule** to add a new rule.

   - Select **Type**: **All ICMP - IPv4** and **Source**: **Anywhere**. Allow ping from any IP address.

![Create VPC](/images/5/0003.png?featherlight=false&width=90pc)

4. Check **Outbound rules** and select **Cretae security group**

![Create VPC](/images/5/0004.png?featherlight=false&width=90pc)

5. Complete the creation of security group for the server located in the Public subnet

![Create VPC](/images/5/0005.png?featherlight=false&width=90pc)

#### Create a Security Group for a server located in a Private subnet

6. In the **VPC** interface

   - Select **Security Groups**
   - Select **Create security group**

![Create VPC](/images/5/0006.png?featherlight=false&width=90pc)

7. Configure **Security group**

   - In the **Security group name** field enter **Private subnet - SG**

   - In the **Description** section enter **Allow SSH and Ping for servers in private subnet.**

   - select **VPC**, select **VPC** named **ASG**.

![Create VPC](/images/5/0007.png?featherlight=false&width=90pc)

8. Configure **Inbound rules**

   - In Inbound rules, select Add rule.

   - Select **Type**: **SSH** and leave **Source**: **Custom**. Check the search box and select **Public subnet SG**. This option allows all servers assigned **Public subnet SG** to be **SSH** to the servers assigned to **Private subnet SG**. **.

![Create VPC](/images/5/0008.png?featherlight=false&width=90pc)

9. Select **Add rule** to add a new rule.

   - Select **Type**: **All ICMP IPv4** and **Source**: **Anywhere**. Allow ping from any IP address.
   -
![Create VPC](/images/5/0009.png?featherlight=false&width=90pc)

10. Select **Create security group**


![Create VPC](/images/5/00010.png?featherlight=false&width=90pc)

11. So we have created **2 Security Group** for servers located in **public subnet and private subnet.**

   - Next we will proceed to create 2 EC2 servers.

![Create VPC](/images/5/00011.png?featherlight=false&width=90pc)