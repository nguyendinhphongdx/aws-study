+++
title = "Triển khai hạ tầng"
date = 2020
weight = 1
chapter = false
pre = "<b>2.3.1. </b>"
+++

![Lab Diagram](/images/3/0.png)

**Nội dung:**
- [1. Cấu hình Network](#1-cấu-hình-network)
- [2. Khởi tạo EC2 trên mỗi VPC](#2-khởi-tạo-ec2-trên-mỗi-vpc)

#### 1. Cấu hình Network 

Đầu tiên chúng ta sẽ tạo Elastic IP chuẩn bị cho NAT Gateway:
1. Truy cập **VPC Console**, chọn **Elastic IPs** và chọn **Allocate Elastic IP address**.
2. Chọn **Allocate** để tạo Elastic IP.

Bây giờ truy cập vào **VPC Dashboard** để tạo một VPC mới:
1. Chọn **Launch VPC Wizard**

![Network](/images/3/1.png?width=90pc)

2. Trong trang **Step 1: Select a VPC Configuration**, chọn **VPC with Public and Private Subnets**
3. Chọn **Select**
4. Trong trang **Step 2: VPC with Public and Private Subnets**, nhập thông tin bên dưới:
   - **IPv4 CIDR block:** ```10.10.0.0/16```
   - **VPC name:**  ```LabVPC```
   - **Public subnet's IPv4 CIDR**: ```10.10.0.0/24```
   - **Public subnet name:**  ```Public subnet```
   - **Private subnet's IPv4 CIDR:** ```10.10.3.0/24```
   - **Private subnet name:** ```Private subnet```
   - **Elastic IP Allocation ID:**  Chọn Elastic IP đã tạo trước đó.
   - Để các thiết lập khác ở giá trị mặc định.

![Network](/images/3/2.png?width=90pc)

5. Chọn **Create VPC** để tạo VPC.

#### 2. Khởi tạo EC2 trên mỗi VPC

**Tạo Security Group cho EC2**

1. Truy cập **EC2 Console**, chọn **Create security group**.
2. Trong trang **Create security group**, nhập thông tin bên dưới:
   - **Security group name:** ```Private-EC2-SG```
   - **Description:** ```Private-EC2-SG```
   - **VPC:** Chọn VPC đã tạo trước đó
   - Ở mục **Inbound rules**, chọn **Add rule** và thêm các rule sau:
     - Type: **SSH** | Source: **My IP**
     - Type: **All Traffic** | Source: **Custom** ```10.28.0.0/16```	
3. Chọn **Create security group**.

![Network](/images/3/3.png?width=90pc)

**Khởi chạy EC2 trong LabVPC**

1. Truy cập **EC2 Console**, chọn **Instances** và chọn **Launch instances**.

![Network](/images/3/4.png?width=90pc)

2. Thực hiện cấu hình theo các bước hướng dẫn và nhập các thông tin bên dưới:
   * **AMI:** Amazon Linux 2 AMI (HVM), SSD Volume Type
   * **Instance type:** t2.micro
   * **Network:** LabVPC
   * **Subnet:** Private subnet
   * **Auto-assign Public IP:** Disable
   * **Network interfaces** > **Primary IP:** ```10.10.3.100```
   * **Security group:** Private-EC2-SG
   * Để các thiết lập khác ở giá trị mặc định.
3. Chọn **Review and Launch**
4. Kiểm tra thông tin cấu hình EC2 và chọn **Launch**.

![Network](/images/3/6.png?width=90pc)

5. Trong hộp thoại **Select an existing key pair or create a new key pair**, tạo mới một keypair:
   - Chọn **Create a new key pair**
   - **Key pair name:** ```ec2-keypair```

![Network](/images/3/5.png?width=90pc)

6. Chọn **Launch Instances**

![Network](/images/3/7.png?width=90pc)
