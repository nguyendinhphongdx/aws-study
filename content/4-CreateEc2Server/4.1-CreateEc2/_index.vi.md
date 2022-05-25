---
title : "Tạo máy chủ EC2"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 4.1 </b> "
---

#### Tạo EC2 nằm trong Public subnet

1. Truy cập **AWS Management Console**

- Tìm **EC2**
- Chọn **EC2**

![Create VPC](/images/4-CreateEc2Server/4.1-ec2/0001-ec2.png?featherlight=false&width=90pc)

2. Trong giao diện **EC2**

- Chọn **Instances**
- Chọn **Launch instances**

![Create VPC](/images/4-CreateEc2Server/4.1-ec2/0002-ec2.png?featherlight=false&width=90pc)

3. **Name and tags** của instance, nhập **```EC2 Public```**

![Create VPC](/images/4-CreateEc2Server/4.1-ec2/0003-ec2.png?featherlight=false&width=90pc)

4. Thực hiện chọn **AMI**

- Chọn **Quick Start**
- Chọn **Amazon Linux 2**
- Chọn **AMI**

![Create VPC](/images/4-CreateEc2Server/4.1-ec2/0004-ec2.png?featherlight=false&width=90pc)

5. Thực hiện chọn **Instance type** và Chọn **Create new key pair**

![Create VPC](/images/4-CreateEc2Server/4.1-ec2/0005-ec2.png?featherlight=false&width=90pc)

6. Trong giao diện **Cretae key pair**

- **Key pair name**, nhập **```aws-keypair```**
- **Key pair type**, chọn **RSA**
- **Private key file format**, chọn **.pem**

![Create VPC](/images/4-CreateEc2Server/4.1-ec2/0007-ec2.png?featherlight=false&width=90pc)

7.  Thực hiện cấu hình **Network**

- **VPC**, chọn **ASG**
- **Subnet**, chọn **Public Subnet 1**
- **Auto-assign public IP**, chọn **Enable**
- **Firewall (Security Group)**, chọn **Select existing security group**
- Chọn **Public subnet -SG**
- Chọn **Launch instance**

![Create VPC](/images/4-CreateEc2Server/4.1-ec2/0006-ec2.png?featherlight=false&width=90pc)

8. Hoàn thành tạo instance

![Create VPC](/images/4-CreateEc2Server/4.1-ec2/0008-ec2.png?featherlight=false&width=90pc)

9. Đợi khoảng 5 phút **Status check** sẽ chuyển sang **2/2 checks passed**

![Create VPC](/images/4-CreateEc2Server/4.1-ec2/0009-ec2.png?featherlight=false&width=90pc)

#### Tạo EC2 nằm trong Private subnet

10. Trong giao diện **EC2**

- Chọn **Instances**
- Chọn **Launch instances**

![Create VPC](/images/4-CreateEc2Server/4.1-ec2/00010-ec2.png?featherlight=false&width=90pc)

11. **Name and tags**, nhập **```EC2 Private```**

![Create VPC](/images/4-CreateEc2Server/4.1-ec2/00011-ec2.png?featherlight=false&width=90pc)

12. Thực hiện chọn **AMI**

- Chọn **Quick Start**
- Chọn **Amazon Linux 2**
- Chọn **AMI**

![Create VPC](/images/4-CreateEc2Server/4.1-ec2/00012-ec2.png?featherlight=false&width=90pc)

13. Thực hiện chọn **instance type**

- **Key pair name**, chọn **```aws-keypair```**

![Create VPC](/images/4-CreateEc2Server/4.1-ec2/00013-ec2.png?featherlight=false&width=90pc)

14. Thực hiện cấu hình **Network**

- **VPC**, chọn **ASG** vpc
- **Subnet**, chọn **Private subnet 2**
- **Auto-assign public IP**, chọn **Disable**. Nếu không **Disable**, bạn cần kiểm tra lại phần cấu hình **tự động cấp phát public IP cho subnet.** 

![Create VPC](/images/4-CreateEc2Server/4.1-ec2/00014-ec2.png?featherlight=false&width=90pc)

15. Hoàn thành tạo instance

- Chọn **View all instance**

![Create VPC](/images/4-CreateEc2Server/4.1-ec2/00015-ec2.png?featherlight=false&width=90pc)

16. Chọn **EC2 Private**

- Chọn **Details**
- Lưu trữ **Private IPv4 addresses**


![Create VPC](/images/4-CreateEc2Server/4.1-ec2/00016-ec2.png?featherlight=false&width=90pc)

