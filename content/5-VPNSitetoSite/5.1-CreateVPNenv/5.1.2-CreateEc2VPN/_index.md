---
title : "Creating EC2 Instance"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 5.1.2 </b> "
---

#### Create EC2 as a Customer Gateway

1. Access to **VPC**

- Select **Security groups**
- Select **Create security group**

![Create VPC](/images/5-CreateVPNenv/5.2-ec2vpn/0001-ec2vpn.png?featherlight=false&width=90pc)

2. In the **Create security group** interface
- **Basic details**
	- **Security group name**, enter `VPN Public -SG`
	- **Description**: enter `Allow IPSec, SSH, and Ping for servers in public subnet`.
	- **VPC**: select **ASG VPN** vpc


![Create VPC](/images/5-CreateVPNenv/5.2-ec2vpn/0002-ec2vpn.png?featherlight=false&width=90pc)

- Configure **Inbound rules** by adding four Inbound rules 
	- Select **Add rule**
	- **Type**: select **SSH**
	- **Source**: **My IP**. My IP represents a public IPv4 address you are using.
	- Again, click **Add rule** to add a new Inbound rule.
	- **Type**: select **All ICMP IPv4**
	- **Source**: **Anywhere-IPv4**. Allow ping from any IP address.
	- Next, click **Add rule** to add a new rule.
	- **Type**: **Custom UDP**
	- **Port range**: **400** 
	- **Source** : **Anywhere-IPv4**.
	- Finally, click **Add rule** to add the fourth Inbound rule.
	- **Type**: **Custom TCP** 
	- **Port range**: **500** 
	- **Source** : **Anywhere-IPv4**

![Create VPC](/images/5-CreateVPNenv/5.2-ec2vpn/0003-ec2vpn.png?featherlight=false&width=90pc)

- Check **Outbound rules** 
- Select **Create security group**

![Create VPC](/images/5-CreateVPNenv/5.2-ec2vpn/0004-ec2vpn.png?featherlight=false&width=90pc)

3. Complete creation of **VPN Public - SG**. 
   
	So we have created a Security Group. Next, we will proceed to create an EC2 server that plays the Customer Gateway role.

![Create VPC](/images/5-CreateVPNenv/5.2-ec2vpn/0005-ec2vpn.png?featherlight=false&width=90pc)

4. Access to **EC2**

- Select **Instances**
- Select **Launch instances**

![Create VPC](/images/5-CreateVPNenv/5.2-ec2vpn/0006-ec2vpn.png?featherlight=false&width=90pc)

5. In the **Launch instances** interface

- **Name**: enter `Customer Gateway`

![Create VPC](/images/5-CreateVPNenv/5.2-ec2vpn/0007-ec2vpn.png?featherlight=false&width=90pc)

- Executing **AMI** Selection
	- Select **Quick Start**
	- Select **Amazon Linux**
	- Select **AMI**

![Create VPC](/images/5-CreateVPNenv/5.2-ec2vpn/0008-ec2vpn.png?featherlight=false&width=90pc)

- Select **Instance type** and select **Key pair**: **aws-keypair** (keypair created with instances)

![Create VPC](/images/5-CreateVPNenv/5.2-ec2vpn/0009-ec2vpn.png?featherlight=false&width=90pc)

- Configure **Network**: click **Edit** and 
	- **VPC**: select **ASG VPN** vpc
	- **Subnet**: select **VPN Public**
	- **Auto-assign public IP**: select **Enable**
	- **Firewall**: select **Select existing security group** and select **VPN Public - SG**
- Check again and select **Launch instance**

![Create VPC](/images/5-CreateVPNenv/5.2-ec2vpn/00010-ec2vpn.png?featherlight=false&width=90pc)

6. Finish creating **EC2 Customer Gateway**

- Select **View all instances**

![Create VPC](/images/5-CreateVPNenv/5.2-ec2vpn/00011-ec2vpn.png?featherlight=false&width=90pc)

7. View details **Customer Gateway instance**


![Create VPC](/images/5-CreateVPNenv/5.2-ec2vpn/00012-ec2vpn.png?featherlight=false&width=90pc)