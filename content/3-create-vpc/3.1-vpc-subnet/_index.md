+++
title = "Tạo VPC và Subnet"
date = 2021
weight = 1
chapter = false
pre = "<b>3.1 </b>"
+++

Đầu tiên chúng ta sẽ tiến hành tạo VPC và 4 subnet, như kiến trúc dưới đây.
![Mô hình Lab](/images/architecture/lab-3.1.png?width=40pc)

#### Tạo một Amazon Virtual Private Cloud (VPC)

* Truy cập Amazon VPC console tại địa chỉ https://console.aws.amazon.com/vpc/
* Trong navigation pane, Chọn **Your VPCs**, **Create VPC**.

![Tạo VPC](/images/vpc/create-vpc.png?width=90pc)

* **Name tag** điền tên VPC: `ASG`
* **IPv4 CIDR block** điền dãy IP block của VPC : `10.10.0.0/16`
* Click **Create VPC**.

![Tạo VPC](/images/vpc/name-ip-create.png?width=90pc)
{{%notice warning%}}
Phần cấu hình Tennacy chúng ta sẽ để ở cơ chế mặc định. Nếu chúng ta chuyển sang **Dedicated**  sẽ có một số EC2 Instance type không phù hợp và sẽ không tạo được trong VPC với tennacy mode là **Dedicated**
{{%/notice%}}

#### Tạo các subnet

* Truy cập Amazon VPC console.
* Trên thanh điều hướng bên trái, chọn **Subnets**.
* Chọn **Create Subnet**.
![Tạo VPC](/images/vpc/create-subnet.png?width=90pc)

* Lựa chọn VPC có tên là **ASG** chúng ta vừa tạo.
* **Name tag** điền tên subnet: `Public Subnet 1`
* Lựa chọn **Availability Zone**: `ap-southeast-1a`
* Chọn **IPv4 CIDR block** là `10.10.1.0/24` theo kiến trúc mô tả.
![Tạo VPC](/images/vpc/name-ip-subnet.png?width=90pc)
* Kéo màn hình xuống dưới, click **Create subnet** để tiến hành tạo **Public subnet 1** với CIDR là **10.10.1.0/24** nằm trong Availability Zone **ap-southeast-1a**.

* Làm tương tự để tạo thêm 3 subnet nữa bao gồm :
  + **Public subnet 2** với CIDR là **10.10.2.0/24** nằm trong Availability Zone **ap-southeast-1b**.
  + **Private subnet 1** với CIDR là **10.10.3.0/24** nằm trong Availability Zone **ap-southeast-1a**.
  + **Private subnet 2** với CIDR là **10.10.4.0/24** nằm trong Availability Zone **ap-southeast-1b**.
![Tạo VPC](/images/vpc/done-subnet.png?width=90pc)


{{%notice tip%}}
Bạn có thể thấy có 2 cột là Availability Zone và Availability Zone ID. Để tránh việc tài nguyên EC2 được sử dụng không đồng đều, (chúng ta thường có xu hướng dùng AZ a để chạy primary và AZ b để stand by chẳng hạn) nên AWS sẽ gán ngẫu nhiên Availability Zone vào Availability Zone ID. Chúng ta có thể hiểu rằng Availability Zone là 1 dạng alias , còn Availability Zone ID mới chính là yếu tố định danh.
Ví dụ ở hình trên Availability Zone **ap-southeast-1a** được gán Availability Zone ID là	**apse1-az2**. Ở một AWS account khác , Availability Zone **ap-southeast-1a** có thể có Availability Zone ID là **apse1-az1**.
{{%/notice%}}


{{%notice tip%}}
Một điểm đáng chú ý nữa là các subnet về cơ bản đều giống nhau, thông qua cấu hình route table và cấp phát public IP address mà chúng ta mới phân chia ra Public và Private Subnet.
{{%/notice%}}

#### Cho phép tự động cấp phát public ip address cho 2 public subnet.

* Click chọn **Public subnet 1**.
* Click action.
* Click chọn **Modify auto-assign IP settings**.
![Tạo VPC](/images/vpc/modify-public-ip.png?width=90pc)

* Click **Enable auto-assign public IPv4 address** và Click **Save**.
![Tạo VPC](/images/vpc/modify-public-ip2.png?width=90pc)

* Làm tương tự cho **Public subnet 2**.