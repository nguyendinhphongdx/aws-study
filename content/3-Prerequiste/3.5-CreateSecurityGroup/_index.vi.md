---
title : "Tạo Security Group"
date :  "`r Sys.Date()`" 
weight : 5 
chapter : false
pre : " <b> 3.5 </b> "
---

### Tạo Security Group

#### Tạo Security Group cho máy chủ nằm trong Public subnet

1. Trong giao diện **VPC**

- Chọn **Security groups**
- Chọn **Create security group**

![Create VPC](/images/3-Prerequiste/3.3-securitygroup/0001-securitygroup.png?featherlight=false&width=90pc)


2. Thực hiện cấu hình **Security group**
- Basic details:
    - **Security Group name**: nhập `Public subnet - SG`
    - **Description**: nhập `Allow SSH and Ping for servers in private subnet`.
    - **VPC**: chọn **ASG** VPC.

![Create VPC](/images/3-Prerequiste/3.3-securitygroup/0002-securitygroup.png?featherlight=false&width=90pc)

- Thực hiện cấu hình **Inbound rules**: click **Add rule**.

    - **Type**: select **SSH**  
    - **Source type**: chọn **My IP**. **My IP** đại diện cho 1 địa chỉ public IPv4 bạn đang sử dụng (sẽ thay đổi khi bạn đổi mạng)
- Lặp lại việc click **Add rule** để tạo một  Inbound rule mới.
    -  **Type**: chọn **All ICMP - IPv4**
    - **Source type**: chọn **Anywhere-IPv4**. Cho phép ping từ bất kì địa chỉ IP nào.

![Create VPC](/images/3-Prerequiste/3.3-securitygroup/0003-securitygroup.png?featherlight=false&width=90pc)

- Kiểm tra **Outbound rules** 
- Chọn **Create security group**

![Create VPC](/images/3-Prerequiste/3.3-securitygroup/0004-securitygroup.png?featherlight=false&width=90pc)

3. Hoàn thành tạo security group cho máy chủ nằm trong Public subnet

![Create VPC](/images/3-Prerequiste/3.3-securitygroup/0005-securitygroup.png?featherlight=false&width=90pc)

#### Tạo Security Group cho máy chủ nằm trong Private subnet

4. Trong giao diện **VPC**

- Chọn **Security groups**
- Chọn **Create security group**

![Create VPC](/images/3-Prerequiste/3.3-securitygroup/0006-securitygroup.png?featherlight=false&width=90pc)

5. Thực hiện cấu hình **Security group**

- **Basic details**:
    - Trong **Security group name** field: điền `Private subnet - SG`
    
    - Trong **Description** field: điền `Allow SSH and Ping for servers in private subnet`.
    
    - **VPC**: chọn **VPC** có tên **ASG**.

![Create VPC](/images/3-Prerequiste/3.3-securitygroup/0007-securitygroup.png?featherlight=false&width=90pc)


- Thực hiện cấu hình **Inbound rules**: chọn Add rule.
    - **Type**: chọn **SSH** 
    - **Source type**: **Custom**. 
    - **Source**: click vào search box và chọn **Public subnet SG**. Lựa chọn này cho phép tất cả những máy chủ được gán **Public subnet SG** được **SSH** vào các máy chủ được gán **Private subnet SG**.

![Create VPC](/images/3-Prerequiste/3.3-securitygroup/0008-securitygroup.png?featherlight=false&width=90pc)

- Ta lại chọn **Add rule** để tạo một Inbound rule mới.
    -  **Type**: chọn **All ICMP IPv4** 
    - **Source type**: chọn **Anywhere-IPv4**. Cho phép ping từ bất kì địa chỉ IP nào.
- Chọn **Create security group**

![Create VPC](/images/3-Prerequiste/3.3-securitygroup/0009-securitygroup.png?featherlight=false&width=90pc)

6. Như vậy chúng ta đã tạo được hai Security Group cho các máy chủ nằm trong **public subnet và private subnet.**


![Create VPC](/images/3-Prerequiste/3.3-securitygroup/00010-securitygroup.png?featherlight=false&width=90pc)

- Tiếp theo chúng ta sẽ tiến hành tạo hai máy chủ EC2.