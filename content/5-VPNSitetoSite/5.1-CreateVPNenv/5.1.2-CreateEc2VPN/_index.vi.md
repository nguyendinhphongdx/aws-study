---
title : "Tạo EC2 Instance"
date :  "`r Sys.Date()`" 
weight : 2
chapter : false
pre : " <b> 5.1.2 </b> "
---

#### Tạo EC2 để làm Customer Gateway

1. Truy cập vào **VPC**

- Chọn **Security groups**
- Chọn **Create security group**

![Create VPC](/images/5-CreateVPNenv/5.2-ec2vpn/0001-ec2vpn.png?featherlight=false&width=90pc)

2. Trong giao diện **Create security group**
- **Basic details**
	- **Security group name**: nhập `VPN Public -SG`
	- **Description**: điền `Allow IPSec, SSH and Ping for servers in public subnet`
	- **VPC**: chọn **ASG VPN** vpc


![Create VPC](/images/5-CreateVPNenv/5.2-ec2vpn/0002-ec2vpn.png?featherlight=false&width=90pc)

- Tiến hành cấu hình **Inbound rules** bằng việc thêm bốn Inbound rules.
	- Chọn **Add rule**
	- **Type**: chọn **SSH** 
	- **Source**: **My IP**. My IP đại diện cho một địa chỉ public IPv4 bạn đang sử dụng.
	- Lặp lại việc click **Add rule** để thêm một Inbound rule mới.
	- **Type**: chọn **All ICMP IPv4**  
	- **Source**: **Anywhere-IPv4**. Cho phép ping từ bất kì địa chỉ IP nào.
	- Click **Add rule** để tiếp tục thêm một Inbound rule mới.
	- **Type**: **Custom UDP** 
	- **Port range**: **400** 
	- **Source** : **Anywhere-IPv4**.
	- Click **Add rule** để thêm Inbound rule mới.
	- **Type**: **Custom TCP** 
	- **Port range**:**500** 
	- **Source** : **Anywhere-IPv4**

![Create VPC](/images/5-CreateVPNenv/5.2-ec2vpn/0003-ec2vpn.png?featherlight=false&width=90pc)

- Kiểm tra **Outbound rules** 
- Chọn **Create security group**


![Create VPC](/images/5-CreateVPNenv/5.2-ec2vpn/0004-ec2vpn.png?featherlight=false&width=90pc)

3. Hoàn thành tạo **VPN Public - SG**. 
   
	Như vậy chúng ta đã tạo được Security Group. Tiếp theo chúng ta sẽ tiến hành tạo máy chủ EC2 đóng vai trò Customer Gateway.

![Create VPC](/images/5-CreateVPNenv/5.2-ec2vpn/0005-ec2vpn.png?featherlight=false&width=90pc)

4. Truy cập vào **EC2**

- Chọn **Instances**
- Chọn **Launch instances**

![Create VPC](/images/5-CreateVPNenv/5.2-ec2vpn/0006-ec2vpn.png?featherlight=false&width=90pc)

5. Trong giao diện **Launch instances**

- **Name**: nhập `Customer Gateway`

![Create VPC](/images/5-CreateVPNenv/5.2-ec2vpn/0007-ec2vpn.png?featherlight=false&width=90pc)

- Thực hiện chọn **AMI**
	- Chọn **Quick Start**
	- Chọn **Amazon Linux**
	- Chọn **AMI**

![Create VPC](/images/5-CreateVPNenv/5.2-ec2vpn/0008-ec2vpn.png?featherlight=false&width=90pc)

- Chọn **Instance type** và chọn **Key pair**: **aws-keypair** (keypair đã tạo chung với các instance)

![Create VPC](/images/5-CreateVPNenv/5.2-ec2vpn/0009-ec2vpn.png?featherlight=false&width=90pc)

- Thực hiện cấu hình **Network**: nhấn **Edit** và sau đó
	- **VPC**: chọn **ASG VPN** vpc
	- **Subnet**: chọn **VPN Public**
	- **Auto-assign public IP**: chọn **Enable**
	- **Firewall**:  chọn **Select existing security group** và chọn **VPN Public - SG**
- Kiểm tra lại và chọn **Launch instance**

![Create VPC](/images/5-CreateVPNenv/5.2-ec2vpn/00010-ec2vpn.png?featherlight=false&width=90pc)

6. Hoàn tất tạo **EC2 Customer Gateway**

- Chọn **View all instances**

![Create VPC](/images/5-CreateVPNenv/5.2-ec2vpn/00011-ec2vpn.png?featherlight=false&width=90pc)

7. Xem chi tiết **Customer Gateway instance**


![Create VPC](/images/5-CreateVPNenv/5.2-ec2vpn/00012-ec2vpn.png?featherlight=false&width=90pc)
