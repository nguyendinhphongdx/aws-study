+++
title = "Tạo Security Group"
date = 2021
weight = 3
chapter = false
pre = "<b>3.3 </b>"
+++

Ở bước này chúng ta sẽ tạo 2 security group cho phép thực hiện ping và ssh để sử dụng cho 2 máy chủ EC2 nằm trong public subnet và private subnet.

![Mô hình Lab](/images/architecture/lab-3.2.png?width=40pc)

#### Tạo Security Group cho máy chủ nằm trong Public subnet

* Truy cập Amazon VPC console.
* Trên thanh điều hướng bên trái, chọn **Security Group**, click **Create Security Group**.
![Security Group](/images/vpc/create-sg.png?width=90pc)

* Trong phần **Security group name** điền **Public subnet - SG**
* Trong phần **Description ** điền **Allow SSH and Ping for servers in public subnet**.
* Click chọn **VPC**, lựa chọn VPC có tên **ASG**.
![Security Group](/images/vpc/create-sg2.png?width=90pc)

* Trong **Inbound rules**, click **Add rule**.
* Chọn **Type**: `SSH` và **Source**: `My IP`. My IP đại diện cho 1 địa chỉ public IPv4 bạn đang sử dụng.
* Click **Add rule** để thêm 1 rule mới.
* Chọn **Type**: `Custom ICMP v4` và **Source**: `Anywhere`. Cho phép ping từ bất kì địa chỉ IP nào.
![Security Group](/images/vpc/create-sg3.png?width=90pc)
* Kéo màn hình xuống dưới và Click **Create Security Group**.

#### Tạo Security Group cho máy chủ nằm trong Private subnet
* Truy cập Amazon VPC console.
* Trên thanh điều hướng bên trái, chọn **Security Group**, click **Create Security Group**.
![Security Group](/images/vpc/create-sg4.png?width=90pc)

* Trong phần **Security group name** điền **Private subnet - SG**
* Trong phần **Description** điền **Allow SSH and Ping for servers in private subnet**.
* Click chọn **VPC**, lựa chọn VPC có tên **ASG**.
![Security Group](/images/vpc/create-sg5.png?width=90pc)

* Trong **Inbound rules**, click **Add rule**.
* Chọn **Type**: `SSH` và để nguyên **Source**: `Custom`. Click vào search box và chọn **Public subnet SG**.Lựa chọn này cho phép tất cả những máy chủ được gán **Public subnet SG** được SSH vào các máy chủ được gán **Private subnet SG**.
![Security Group](/images/vpc/create-sg6.png?width=90pc)

* Click **Add rule** để thêm 1 rule mới.
* Chọn **Type**: `Custom ICMP v4` và **Source**: `Anywhere`. Cho phép ping từ bất kì địa chỉ IP nào.
![Security Group](/images/vpc/create-sg7.png?width=90pc)
* Kéo màn hình xuống dưới và Click **Create Security Group**.

* Như vậy chúng ta đã tạo được 2 Security Group cho các máy chủ nằm trong public subnet và private subnet.
* Tiếp theo chúng ta sẽ tiến hành tạo 2 máy chủ EC2.