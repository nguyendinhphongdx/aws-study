+++
title = "Tạo EC2 Instance"
date = 2021
weight = 2
chapter = false
pre = "<b>4.1.2 </b>"
+++

 
#### Tạo EC2 để làm Customer Gateway

1. Tạo Security Group cho **Customer Gateway**
  + Truy cập Amazon VPC console.
  + Trên thanh điều hướng bên trái, chọn **Security Group**, click **Create Security Group**.
2. Trong phần **Security group name** điền **VPN Public - SG**
  + Trong phần **Description** điền **Allow IPSec, SSH and Ping for servers in public subnet**.
  + Click chọn **VPC**, lựa chọn VPC có tên **ASG VPN**.
![Create EC2](/images/vpn/create-sg.png?width=90pc)

3. Trong **Inbound rules**, click **Add rule**.
  + Chọn **Type**: `SSH` và **Source**: `My IP`. My IP đại diện cho 1 địa chỉ public IPv4 bạn đang sử dụng.
  + Click **Add rule** để thêm 1 rule mới.
  + Chọn **Type**: `Custom ICMP v4` và **Source**: `Anywhere`. Cho phép ping từ bất kì địa chỉ IP nào.
  + Click **Add rule** để thêm 1  rule mới.
  + Chọn **Type**: `Custom UDP` ,  **Port:400 và **Source** : `Anywhere`.
  + Click **Add rule** để thêm 1  rule mới.
  + Chọn **Type**: `Custom UDP` ,  **Port:500 và **Source** : `Anywhere`.
![Create EC2](/images/vpn/create-sg2.png?width=90pc)
  + Kéo màn hình xuống dưới và Click **Create Security Group**.

4. Như vậy chúng ta đã tạo được Security Group. Tiếp theo chúng ta sẽ tiến hành tạo máy chủ EC2 đóng vai trò Customer Gateway.

5. Truy cập Amazon EC2 console tại địa chỉ https://console.aws.amazon.com/ec2/.
  + Trên thanh điều hướng bên trái, chọn **Intances**, **Launch Intance**.
  + Chọn **Select** để chọn Amazon Machine Image (AMI) **Amazon Linux 2 AMI (HVM), SSD Volume Type**.
![Create EC2](/images/vpc/create-ec2-2.png?width=90pc)

6. Giữ nguyên lựa chọn **General purpose t2.micro**, sau đó click **Next: Configure Instance Details**.
![Create EC2](/images/vpc/create-ec2-3.png?width=90pc)

7. Tại trang **Configure Instance Details**
  + **Network** chọn VPC **ASG VPN**.
  + **Subnet** chọn subnet **VPN Public**.
  + **Auto-assign Public IP** kiểm tra xem có đang **Enable** hay không. Nếu không Enable, bạn cần kiểm tra lại phần cấu hình tự động cấp phát public IP cho subnet.
  + Sau đó click **Next: Add Storage**.
![Create EC2](/images/vpn/create-vpnec2.png?width=90pc)

8. Tại trang **Add Storage**. Giữ nguyên cấu hình mặc định và Click **Next: Add Tags**
9. Tại trang **Add Tags**, Chọn **Add Tag**
  + **Key** điền `Name`.
  + **Value** điền `Customer Gateway`.
  + Sau đó bấm **Next: Configure Security Group**.

10. Tại trang **Configure Security Group**
  + **Assign a security group** click chọn **Select an existing security group**.
  + **Name** chọn security group cho các EC2 trong public cubnet có tên **VPN Public - SG**.
  + Sau đó bấm **Review và Launch**.
![Create EC2](/images/vpn/create-vpnec21.png?width=90pc)

11. Kiểm tra lại thông tin cấu hình và Click **Launch**.
  + Một prompt **Choose an existing key pair or Create new key pair** xuất hiện.
  + Chọn **Choose an existing keypair**.
  + Chọn **Key pair name**: **asg-keypair**.
  + Click **I acknowledge that I have access to the selected private key file (asg-keypair.pem), and that without this file, I won't be able to log into my instance.** để xác nhận rằng mình có thể truy cập tới Key pair bạn đã chọn.
  + Click **Launch**.
![Create EC2](/images/vpc/create-ec2-11.png?width=90pc)
  + Click **View Instances** và chờ cho tới khi EC2 khởi tạo xong.

12. Như vậy chúng ta đã tạo được máy chủ EC2 theo mô hình kiến trúc yêu cầu. Bước tiếp theo chúng ta sẽ bắt đầu khởi tạo và cấu hình kết nối VPN Site to Site.
