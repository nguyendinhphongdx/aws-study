---
title : "Create EC2 Server"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 4.1 </b> "
---

#### Create EC2 in the Public subnet

1. Access **AWS Management Console**

   - Find **EC2**
   - Select **EC2**

![Create VPC](/images/6/0001.png?featherlight=false&width=90pc)

2. In the **EC2** interface

   - Select **Instances**
   - Select **Launch instances**

![Create VPC](/images/6/0002.png?featherlight=false&width=90pc)

3. **Name and tags** of the instance, enter **```EC2 Public```**

![Create VPC](/images/6/0003.png?featherlight=false&width=90pc)

4. Executing **AMI** Selection

   - Select **Quick Start**
   - Select **Amazon Linux 2**
   - Select **AMI**

![Create VPC](/images/6/0004.png?featherlight=false&width=90pc)

5. Select **Instance type** and Select **Create new key pair**

![Create VPC](/images/6/0005.png?featherlight=false&width=90pc)

6. In the **Create key pair** interface

   - **Key pair name**, enter **```aws-keypair```** (optional name, you can set any).
   - **Key pair type**, select **RSA**
   - **Private key file format**, select **.pem**

![Create VPC](/images/6/0006.png?featherlight=false&width=90pc)

7. Configure **Network**

    - **VPC**, select **ASG**
    - **Subnet**, select **Public Subnet 1**
    - **Auto-assign public IP**, select **Enable**
    - **Firewall (Security Group)**, select **Select existing security group**
    - Select **Public subnet -SG**
    - Select **Launch instance**

![Create VPC](/images/6/0007.png?featherlight=false&width=90pc)

8. Complete instance creation

![Create VPC](/images/6/0008.png?featherlight=false&width=90pc)

9. Wait about 5 minutes **Status check** will change to **2/2 checks passed**

![Create VPC](/images/6/0009.png?featherlight=false&width=90pc)

#### Create EC2 in a Private subnet

10. In the **EC2** interface

    - Select **Instances**
    - Select **Launch instances**

11. **Name and tags**, enter **```EC2 Private```**

![Create VPC](/images/6/00010.png?featherlight=false&width=90pc)

![Create VPC](/images/6/00011.png?featherlight=false&width=90pc)

12. Executing **AMI** Selection

     - Select **Quick Start**
     - Select **Amazon Linux 2**
     - Select **AMI**

![Create VPC](/images/6/00012.png?featherlight=false&width=90pc)

13. Make **instance type** selection. **Key pair name**, select **```aws-keypair```**

![Create VPC](/images/6/00013.png?featherlight=false&width=90pc)

14. Configure **Network**

      - **VPC**, select **ASG** vpc
      - **Subnet**, select **Private subnet 2**
      - **Auto-assign public IP**, select **Disable**. If not **Disable**, you need to check the configuration **automatically allocate public IP for the subnet.**

![Create VPC](/images/6/00014.png?featherlight=false&width=90pc)

15. Complete instance creation

      - Select **View all instances**

![Create VPC](/images/6/00015.png?featherlight=false&width=90pc)

16. Select **EC2 Private**

      - Select **Details**
      - Store **Private IPv4 addresses**

![Create VPC](/images/6/00016.png?featherlight=false&width=90pc)