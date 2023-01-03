---
title : "Create Security Group"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 3.5 </b> "
---

### Create Security Group

#### Create a Security Group for servers located in the Public subnet

1. In the **VPC** interface

- Select **Security groups**
- Select **Create security group**

![Create VPC](/images/3-Prerequiste/3.3-securitygroup/0001-securitygroup.png?featherlight=false&width=90pc)


2. Configure **Security group**

- Basic details:
	- **Security Group name**: enter `Public subnet - SG`
	- **Description**: enter `Allow SSH and Ping for servers in private subnet`.
	- **VPC**: select **ASG** VPC.

![Create VPC](/images/3-Prerequiste/3.3-securitygroup/0002-securitygroup.png?featherlight=false&width=90pc)

- Configure **Inbound rules**: click **Add rule**.

	- **Type**: select **SSH**  
	- **Source type**: select **My IP**. **My IP** represents a public IPv4 address you are using (will change when you change network)
- Again, click **Add rule** to add a new Inbound rule.
	
	-  **Type**: select **All ICMP - IPv4**
	- **Source type**: select **Anywhere-IPv4**. Allow ping from any IP address.

![Create VPC](/images/3-Prerequiste/3.3-securitygroup/0003-securitygroup.png?featherlight=false&width=90pc)

- Check **Outbound rules** 
- Select **Create security group**

![Create VPC](/images/3-Prerequiste/3.3-securitygroup/0004-securitygroup.png?featherlight=false&width=90pc)

3. Complete the creation of a security group for the server located in the Public subnet.

![Create VPC](/images/3-Prerequiste/3.3-securitygroup/0005-securitygroup.png?featherlight=false&width=90pc)

#### Create a Security Group for servers located in Private subnet

4. In the **VPC** interface

- Select **Security groups**
- Select **Create security group**

![Create VPC](/images/3-Prerequiste/3.3-securitygroup/0006-securitygroup.png?featherlight=false&width=90pc)

5. Configure **Security group**

- **Basic details**:
	- In the **Security group name** field: enter `Private subnet - SG`
	
	- In the **Description** field: enter `Allow SSH and Ping for servers in private subnet`.
	
	- **VPC**: select **VPC** named **ASG**.

![Create VPC](/images/3-Prerequiste/3.3-securitygroup/0007-securitygroup.png?featherlight=false&width=90pc)

- Configure **Inbound rules**: select Add rule.
	- **Type**: select **SSH** 
	- **Source type**: **Custom**. 
	- **Source**: check the search box and select **Public subnet SG**. This option allows all servers assigned **Public subnet SG** to be **SSH** to the servers assigned to **Private subnet SG**.

![Create VPC](/images/3-Prerequiste/3.3-securitygroup/0008-securitygroup.png?featherlight=false&width=90pc)

- Again, select **Add rule** to add a new Inbound rule.
	-  **Type**: select **All ICMP IPv4** 
	- **Source type**: select **Anywhere-IPv4**. Allow ping from any IP address.
- Select **Create security group**

![Create VPC](/images/3-Prerequiste/3.3-securitygroup/0009-securitygroup.png?featherlight=false&width=90pc)

6. So we have created two **Security Group** for servers located in **public subnet and private subnet.**


	![Create VPC](/images/3-Prerequiste/3.3-securitygroup/00010-securitygroup.png?featherlight=false&width=90pc)

	
	Next, we will proceed to create two EC2 servers.