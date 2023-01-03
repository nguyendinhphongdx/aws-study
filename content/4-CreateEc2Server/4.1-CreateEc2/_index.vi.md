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

3. **Name and tags** của instance, nhập `EC2 Public`

![Create VPC](/images/4-CreateEc2Server/4.1-ec2/0003-ec2.png?featherlight=false&width=90pc)

4. Thực hiện chọn **AMI**

- Chọn tab **Quick Start**
- Chọn **Amazon Linux**
- Chọn **Amazon Linux 2 ....Free tier eligible**

![Create VPC](/images/4-CreateEc2Server/4.1-ec2/0004-ec2.png?featherlight=false&width=90pc)

5. Tiến hành chọn **Instance type** và chọn **Create new key pair**

![Create VPC](/images/4-CreateEc2Server/4.1-ec2/0005-ec2.png?featherlight=false&width=90pc)

6. Trong giao diện **Cretae key pair**

- **Key pair name**, nhập `aws-keypair`
- **Key pair type**, chọn **RSA**
- **Private key file format**, chọn **.pem**
- Rồi chọn **Create key pair**

![Create VPC](/images/4-CreateEc2Server/4.1-ec2/0007-ec2.png?featherlight=false&width=90pc)

7.  Thực hiện cấu hình **Network settings**, click **Edit**

- **VPC**: chọn **ASG**
- **Subnet**: chọn **Public Subnet 1**
- **Auto-assign public IP**: chọn **Enable**
- **Firewall (Security Group)**: chọn **Select existing security group** rồi chọn **Public subnet -SG**
- Sau cùng, chọn **Launch instance**

![Create VPC](/images/4-CreateEc2Server/4.1-ec2/0006-ec2.png?featherlight=false&width=90pc)

8. Hoàn thành tạo instance

![Create VPC](/images/4-CreateEc2Server/4.1-ec2/0008-ec2.png?featherlight=false&width=90pc)

9. Đợi khoảng 5 phút, **Status check** sẽ được chuyển thành **2/2 checks passed**

![Create VPC](/images/4-CreateEc2Server/4.1-ec2/0009-ec2.png?featherlight=false&width=90pc)

#### Tạo EC2 nằm trong Private subnet

10. Trong giao diện **EC2**

- Chọn **Instances**
- Chọn **Launch instances**

![Create VPC](/images/4-CreateEc2Server/4.1-ec2/00010-ec2.png?featherlight=false&width=90pc)

11. **Name and tags**: nhập `EC2 Private`

![Create VPC](/images/4-CreateEc2Server/4.1-ec2/00011-ec2.png?featherlight=false&width=90pc)

12. Thực hiện chọn **AMI**

- Chọn tab **Quick Start**
- Chọn **Amazon Linux**
- Chọn **Amazon Linux 2 ... Free tier eligible**

![Create VPC](/images/4-CreateEc2Server/4.1-ec2/00012-ec2.png?featherlight=false&width=90pc)

13. Tiến hành chọn **instance type**

- **Key pair name**: chọn **aws-keypair**

![Create VPC](/images/4-CreateEc2Server/4.1-ec2/00013-ec2.png?featherlight=false&width=90pc)

14. Thực hiện cấu hình **Network settings**, click **Edit**

- **VPC**: chọn **ASG** vpc
- **Subnet**: chọn **Private subnet 2**
- **Auto-assign public IP**, chọn **Disable**. Nếu không thể chọn **Disable**, bạn cần kiểm tra lại phần cấu hình **tự động cấp phát public IP cho subnet.** 
- **Firewall (Security Group)**: chọn **Select existing security group** rồi chọn **Private subnet -SG**
-  Sau cùng, chọn **Launch instance**

![Create VPC](/images/4-CreateEc2Server/4.1-ec2/00014-ec2.png?featherlight=false&width=90pc)

15. Hoàn thành tạo instance

- Chọn **View all instance**

![Create VPC](/images/4-CreateEc2Server/4.1-ec2/00015-ec2.png?featherlight=false&width=90pc)

16. Chọn **EC2 Private**

- Chọn tab **Details**
- Lưu lại thông tin **Private IPv4 addresses**


![Create VPC](/images/4-CreateEc2Server/4.1-ec2/00016-ec2.png?featherlight=false&width=90pc)

