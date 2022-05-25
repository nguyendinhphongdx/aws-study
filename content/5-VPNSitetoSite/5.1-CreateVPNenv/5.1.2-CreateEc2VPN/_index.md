---
title : "Creating EC2 Instance"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 5.1.2 </b> "
---

#### Create EC2 as a Customer Gateway

1. Access to **VPC**

- Select **Security Group**
- Select **Create security group**

![Create VPC](/images/5-CreateVPNenv/5.2-ec2vpn/0001-ec2vpn.png?featherlight=false&width=90pc)

2. In the **Create security group** interface

- **Security group name**, enter **```VPN Public -SG```**
- In the **Description** section enter **Allow IPSec, SSH, and Ping for servers in public subnet**.
- **VPC**, select **ASG VPN** vpc


![Create VPC](/images/5-CreateVPNenv/5.2-ec2vpn/0002-ec2vpn.png?featherlight=false&width=90pc)

3. Configure **Inbound rules**

- Select **Add rule**
- Select **Type**: **SSH** and **Source**: **My IP**. My IP represents a public IPv4 address you are using.
- Click **Add rule** to add a new rule.
- Select **Type**: **All ICMP IPv4** and **Source**: **Anywhere**. Allow ping from any IP address.
- Click **Add rule** to add a new rule.
- Select **Type**: **Custom UDP** , **Port:400** and **Source** : **Anywhere**.
- Click **Add rule** to add a new rule.
- Select **Type**: **Custom TCP** , **Port:500** and **Source** : **Anywhere**

![Create VPC](/images/5-CreateVPNenv/5.2-ec2vpn/0003-ec2vpn.png?featherlight=false&width=90pc)

4. Check **Outbound rules** and select **Create security group**


![Create VPC](/images/5-CreateVPNenv/5.2-ec2vpn/0004-ec2vpn.png?featherlight=false&width=90pc)

5. Complete creation of **VPN Public - SG**. So we have created a Security Group. Next, we will proceed to create an EC2 server that plays the Customer Gateway role.

![Create VPC](/images/5-CreateVPNenv/5.2-ec2vpn/0005-ec2vpn.png?featherlight=false&width=90pc)

6. Access to **EC2**

- Select *Instances**
- Select **Launch instances**

![Create VPC](/images/5-CreateVPNenv/5.2-ec2vpn/0006-ec2vpn.png?featherlight=false&width=90pc)

7. In the **Launch instances** interface

- **Name**, enter **```Customer Gateway```**

![Create VPC](/images/5-CreateVPNenv/5.2-ec2vpn/0007-ec2vpn.png?featherlight=false&width=90pc)

8. Executing **AMI** Selection

- Select **Quick Start**
- Select **Amazon Linux**
- Select **AMI**

![Create VPC](/images/5-CreateVPNenv/5.2-ec2vpn/0008-ec2vpn.png?featherlight=false&width=90pc)

9. Select **Instance type** and select **Key pair**: **aws-keypair**(keypair created with instances)

![Create VPC](/images/5-CreateVPNenv/5.2-ec2vpn/0009-ec2vpn.png?featherlight=false&width=90pc)

10. Configure **Network**

- **VPC**, select **ASG VPN** vpc
- **Subnet**, select **VPN Public**
- **Auto-assign public IP**, select **Enable**
- **Firewall**, select **Select existing security group**
- Select **VPN Public - SG**
- Check again and select **Launch instance**

![Create VPC](/images/5-CreateVPNenv/5.2-ec2vpn/00010-ec2vpn.png?featherlight=false&width=90pc)

11. Finish creating **EC2 instance**

- Select **View all instances**

![Create VPC](/images/5-CreateVPNenv/5.2-ec2vpn/00011-ec2vpn.png?featherlight=false&width=90pc)

12. View details **Customer Gateway instance**


![Create VPC](/images/5-CreateVPNenv/5.2-ec2vpn/00012-ec2vpn.png?featherlight=false&width=90pc)