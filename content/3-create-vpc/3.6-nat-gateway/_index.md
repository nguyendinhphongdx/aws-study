+++
title = "NAT GW"
date = 2021
weight = 6
chapter = false
pre = "<b>3.6 </b>"
+++

Ở bước này chúng ta sẽ tạo NAT Gateway để cho phép máy chủ **EC2 Private** có thể ra ngoài internet theo mô hình dưới đây.

![Mô hình Lab](/images/architecture/vpc.png?width=75pc)

#### Tạo một địa chỉ Elastic IP

1. Truy cập Amazon VPC console.
2. Trên thanh điều hướng bên trái, chọn **Elastic IPs**.
3. Click **Allocate Elastic IP address** .
![Create NATGW](/images/vpc/create-natgw.png?width=90pc)
4. Click **Allocate**.
5. Chúng ta vừa tạo thành công một địa chỉ Public IP Address: **54.251.117.148**
![Create NATGW](/images/vpc/create-natgw2.png?width=90pc)

#### Tạo một NAT Gateway

1. Truy cập Amazon VPC console.
2. Trên thanh điều hướng bên trái, chọn **NAT Gateways**.
3. Chọn **Create NAT Gateway**.
![Create NATGW](/images/vpc/create-natgw3.png?width=90pc)
4. Tại trang **Create NAT Gateway**:
  + Mục **Name** điền **NAT Gateway**.
  + Mục **subnet** chọn **Public subnet 2**.
  + Mục **Elastic IP Allocation ID** chọn Elastic IP address chúng ta vừa tạo.
  + Click **Create NAT Gateway**
![Create NATGW](/images/vpc/create-natgw4.png?width=90pc)

#### Tạo **Route table - Private** và gáng vào các private subnet.

1. Truy cập Amazon VPC console. 
2. Trên thanh điều hướng bên trái, chọn **Route Tables**.
3. Click **Create route table**
![Create NATGW](/images/vpc/create-natgw5.png?width=90pc)
4. Tại trang Create route table:
  + Mục **Name tag** điền **Route table - Private**.
	+ Mục VPC chọn VPC **ASG**.
	+ Click **Create**
![Create NATGW](/images/vpc/create-natgw6.png?width=90pc)
  + Sau khi tạo route table thành công, click **Close**.
5. Chọn **Route table - Private**.
  + Click tab **Subnet Associations**.
  + Click **Edit subnet associations**.
![Create NATGW](/images/vpc/create-natgw7.png?width=90pc)
  + Chọn 2 private subnet và click **Save**.

6. Chọn **Route table - Private**.
  + Click tab **Route**.
  + Click **Edit route**.
![Create NATGW](/images/vpc/create-natgw8.png?width=90pc)

  + Chọn **Add Route** 
  + **Destination**: `0.0.0.0/0`
  + **Target**: **NAT Gateway**
  + Chọn **Save routes**
![Create NATGW](/images/vpc/create-natgw9.png?width=90pc)
  + Click **Close** sau khi thêm route entry thành công.

7. Kiểm tra ping amazon.com thành công từ **EC2 Private**.
![Create NATGW](/images/vpc/create-natgw10.png?width=90pc)

8. Chúc mừng các bạn, như vậy chúng ta đã hoàn thành mô hình lab như bên dưới. Trong phần kế tiếp chúng ta sẽ cùng khởi tạo và cấu hình kết nối VPN Site to Site để kết nối môi trường onpremise lên AWS Cloud nhé.
![Mô hình Lab](/images/architecture/vpc.png?width=75pc)





