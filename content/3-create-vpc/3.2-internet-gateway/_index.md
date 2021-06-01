+++
title = "Tạo IGW và Route table"
date = 2021
weight = 2
chapter = false
pre = "<b>3.2 </b>"
+++

Sau khi tạo VPC và các subnet, bước tiếp theo chúng ta sẽ tạo Internet Gateway , tạo và cấu hình Route table. Sau khi hoàn tất các bước bên dưới, chúng ta sẽ có kiến trúc như sau:
![Mô hình Lab](/images/architecture/lab-3.2.png?width=40pc)

#### Tạo một Internet Gateway

* Truy cập Amazon VPC console.
* Trên thanh điều hướng bên trái, chọn **Internet Gateways**, **Create Internet Gateways**
![Internet Gateway](/images/vpc/create-igw.png?width=90pc)

* Đặt tên cho Internet Gateway là **Internet Gateway**.
* Click **Create Internet Gateway**.
![Internet Gateway](/images/vpc/create-igw2.png?width=90pc)
* Click **Action**.
* Click **Attach to VPC**.
![Internet Gateway](/images/vpc/create-igw3.png?width=90pc)
* Click chọn VPC **ASG** , VPC ID sẽ được tự động điền.
* Click **Attach Internet Gateway**.
![Internet Gateway](/images/vpc/create-igw4.png?width=90pc)

#### Tạo Route Table định tuyến đi ra ngoài internet thông qua Internet Gateway.

* Truy cập Amazon VPC console.
* Trên thanh điều hướng bên trái, chọn **Route Tables**, **Create Route Table**

![Internet Gateway](/images/vpc/create-route.png?width=90pc)

* Điền tên Route Table trong phần **Name tag**: `Route table - Public`.
* Chọn **VPC** có tên **ASG** , VPC id sẽ được tự động điền vào.
* Click **Create**. 
![Internet Gateway](/images/vpc/create-route2.png?width=90pc)
* Click **Close**.

* Chọn **Route table - Public **, click **Action** , click **Edit routes**.
![Internet Gateway](/images/vpc/create-route3.png?width=90pc)

* Click **Add route**.
* Điền phần **Destination** CIDR : `0.0.0.0/0` đại diện cho Internet.
* Trong phần **Target** click chọn **Internet Gateway**, sau đó chọn Internet Gateway chúng ta đã tạo. Internet Gateway ID sẽ được tự động điền.
* Chọn **Save routes**
![Internet Gateway](/images/vpc/create-route4.png?width=90pc)

* Đảm bảo **Route table - Public** đang được chọn.
* Click vào tab **Subnet Associations** bên menu bên dưới.
* Click **Edit subnet associations**.
![Internet Gateway](/images/vpc/create-route5.png?width=90pc)

* Mở rộng cột **Subnet ID** bằng cách kéo thanh ngăn sang phải.
* Chọn đúng 2 subnet public chúng ta đã tạo. 
* Click **Save**.
![Internet Gateway](/images/vpc/create-route6.png?width=90pc)

* Như vậy chúng ta đã tiến hành:
  + Tạo Internet Gateway có tên **Internet Gateway**.
  + Tạo Route table có tên **Route table - Public**
  + Chỉnh sửa route trong route table **Route table - Public**.
    + Thêm route entry Destination: 0.0.0.0/0 Target: Internet Gateway.
  + Associate **Route table - Public** vào 2 subnet **Public subnet 1** và **Public subnet 2**.