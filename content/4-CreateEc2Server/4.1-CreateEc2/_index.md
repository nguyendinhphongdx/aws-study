---
title : "Create EC2 Server"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 4.1 </b> "
---

#### Create EC2 in Public subnet

1. Access **AWS Management Console**

- Find **EC2**
- Select **EC2**

![Create VPC](/images/4-CreateEc2Server/4.1-ec2/0001-ec2.png?featherlight=false&width=90pc)

2. In the **EC2** interface

- Select **Instances**
- Select **Launch instances**

![Create VPC](/images/4-CreateEc2Server/4.1-ec2/0002-ec2.png?featherlight=false&width=90pc)

3. **Name and tags** of the instance, enter **```EC2 Public```**

![Create VPC](/images/4-CreateEc2Server/4.1-ec2/0003-ec2.png?featherlight=false&width=90pc)

4. Executing **AMI** Selection

- Select **Quick Start**
- Select **Amazon Linux 2**
- Select **AMI**

![Create VPC](/images/4-CreateEc2Server/4.1-ec2/0004-ec2.png?featherlight=false&width=90pc)

5. Make **Instance type** and Select **Create new key pair**

![Create VPC](/images/4-CreateEc2Server/4.1-ec2/0005-ec2.png?featherlight=false&width=90pc)

6. In the **Cretae key pair** interface

- **Key pair name**, enter **```aws-keypair```**
- **Key pair type**, select **RSA**
- **Private key file format**, select **.pem**

![Create VPC](/images/4-CreateEc2Server/4.1-ec2/0007-ec2.png?featherlight=false&width=90pc)

7. Configure **Network**

- **VPC**, select **ASG**
- **Subnet**, select **Public Subnet 1**
- **Auto-assign public IP**, select **Enable**
- **Firewall (Security Group)**, select **Select existing security group**
- Select **Public subnet -SG**
- Select **Launch instance**

![Create VPC](/images/4-CreateEc2Server/4.1-ec2/0006-ec2.png?featherlight=false&width=90pc)

8. Complete instance creation

![Create VPC](/images/4-CreateEc2Server/4.1-ec2/0008-ec2.png?featherlight=false&width=90pc)

9. Wait about 5 minutes **Status check** will change to **2/2 checks passed**

![Create VPC](/images/4-CreateEc2Server/4.1-ec2/0009-ec2.png?featherlight=false&width=90pc)

#### Create EC2 in Private subnet

10. In the **EC2** interface

- Select **Instances**
- Select **Launch instances**

![Create VPC](/images/4-CreateEc2Server/4.1-ec2/00010-ec2.png?featherlight=false&width=90pc)

11. **Name and tags**, enter **```EC2 Private```**

![Create VPC](/images/4-CreateEc2Server/4.1-ec2/00011-ec2.png?featherlight=false&width=90pc)

12. Executing **AMI** Selection

- Select **Quick Start**
- Select **Amazon Linux 2**
- Select **AMI**

![Create VPC](/images/4-CreateEc2Server/4.1-ec2/00012-ec2.png?featherlight=false&width=90pc)

13. Make **instance type** selection

- **Key pair name**, select **```aws-keypair```**

![Create VPC](/images/4-CreateEc2Server/4.1-ec2/00013-ec2.png?featherlight=false&width=90pc)

14. Configure **Network**

- **VPC**, select **ASG** vpc
- **Subnet**, select **Private subnet 2**
- **Auto-assign public IP**, select **Disable**. If not **Disable**, you need to check the configuration **automatically allocate public IP for subnet.**

![Create VPC](/images/4-CreateEc2Server/4.1-ec2/00014-ec2.png?featherlight=false&width=90pc)

15. Complete instance creation

- Select **View all instances**

![Create VPC](/images/4-CreateEc2Server/4.1-ec2/00015-ec2.png?featherlight=false&width=90pc)

16. Select **EC2 Private**

- Select **Details**
- Store **Private IPv4 addresses**


![Create VPC](/images/4-CreateEc2Server/4.1-ec2/00016-ec2.png?featherlight=false&width=90pc)