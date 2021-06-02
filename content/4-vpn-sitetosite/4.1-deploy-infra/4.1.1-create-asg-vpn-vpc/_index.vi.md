+++
title = "Tạo ASG VPN VPC"
date = 2021
weight = 1
chapter = false
pre = "<b>4.1.1 </b>"
+++

 
#### Tạo ASG VPN VPC 

1. Truy cập Amazon VPC console tại địa chỉ https://console.aws.amazon.com/vpc/
2. Trong navigation pane, Chọn **Your VPCs**, **Create VPC**.
  + **Name tag** điền tên VPC: `ASG VPN`
  + **IPv4 CIDR block** điền dãy IP block của VPC : `10.11.0.0/16`
  + Click **Create VPC**.
![Create VPN VPC](/images/vpn/create-asgvpn.png?width=90pc)

{{%notice warning%}}
Phần cấu hình Tennacy chúng ta sẽ để ở cơ chế mặc định. Nếu chúng ta chuyển sang **Dedicated**  sẽ có một số EC2 Instance type không phù hợp và sẽ không tạo được trong VPC với tennacy mode là **Dedicated**
{{%/notice%}}

3. Trên thanh điều hướng bên trái, chọn **Subnets**.
4. Chọn **Create Subnet**.
  + Lựa chọn VPC có tên là **ASG VPN** chúng ta vừa tạo.
  + **Name tag** điền tên subnet: `VPN Public`
  + Lựa chọn **Availability Zone**: `ap-southeast-1a`
  + Chọn **IPv4 CIDR block** là `10.11.1.0/24` theo kiến trúc mô tả.
![Create VPN VPC](/images/vpn/create-asgvpn2.png?width=90pc)
  + Kéo màn hình xuống dưới, click **Create subnet** để tiến hành tạo **VPN Public** với CIDR là **10.11.1.0/24** nằm trong Availability Zone **ap-southeast-1a**.

5. Làm tương tự để tạo thêm 1 subnet nữa bao gồm :
  + **VPN Private** với CIDR là **10.11.2.0/24** nằm trong Availability Zone **ap-southeast-1b**.
![Create VPN VPC](/images/vpn/create-asgvpn3.png?width=90pc)

6. Trong giao diện quản lý subnet. 
  + Click chọn **VPN Public**.
  + Click action.
  + Click chọn **Modify auto-assign IP settings**.
7. Click **Enable auto-assign public IPv4 address** và Click **Save**.

#### Tạo một Internet Gateway cho ASG VPC VPN

1. Truy cập Amazon VPC console.
2. Trên thanh điều hướng bên trái, chọn **Internet Gateways**, **Create Internet Gateways**
  + Đặt tên cho Internet Gateway là **Internet Gateway**.
  + Click **Create Internet Gateway**.
3. Tiếp theo chúng ta cần Attach Internet Gateway vào VPC **ASG VPN**. 
  + Click **Action**.
  + Click **Attach to VPC**.
  + Click chọn VPC **ASG VPN** , VPC ID sẽ được tự động điền.
  + Click **Attach Internet Gateway**.
![Create VPN VPC](/images/vpn/create-asgvpn4.png?width=90pc)

4. Tiếp theo chúng ta cần tạo Route Table định tuyến đi ra ngoài internet thông qua Internet Gateway.
  + Trên thanh điều hướng bên trái, chọn **Route Tables**, **Create Route Table**

5. Điền tên Route Table trong phần **Name tag**: `Route table VPN - Public`.
  + Chọn **VPC** có tên **ASG VPN** , VPC id sẽ được tự động điền vào.
  + Click **Create**. 
![Create VPN VPC](/images/vpn/create-asgvpn5.png?width=90pc)
  + Click **Close**.

6. Chọn **Route table VPN - Public**, click **Action** , click **Edit routes**.

  + Click **Add route**.
  + Điền phần **Destination** CIDR : `0.0.0.0/0` đại diện cho Internet.
  + Trong phần **Target** click chọn **Internet Gateway**, sau đó chọn Internet Gateway chúng ta đã tạo. Internet Gateway ID sẽ được tự động điền.
  + Chọn **Save routes**

7. Đảm bảo **Route table - Public** đang được chọn.
  + Click vào tab **Subnet Associations** bên menu bên dưới.
  + Click **Edit subnet associations**.
  + Mở rộng cột **Subnet ID** bằng cách kéo thanh ngăn sang phải.
  + Chọn subnet **VPN Public**.
  + Click **Save**.
![Create VPN VPC](/images/vpn/create-asgvpn6.png?width=90pc)

* Như vậy chúng ta đã tiến hành:
  + Tạo Internet Gateway có tên **Internet Gateway**.
  + Tạo Route table có tên **Route table VPN - Public**.
  + Chỉnh sửa route trong route table **Route table VPN - Public**.
    + Thêm route entry Destination: 0.0.0.0/0 Target: Internet Gateway.
  + Associate **Route table VPN - Public** vào subnet **VPN Public**.
