+++
title = "Tạo 2 EC2 Instance"
date = 2021
weight = 2
chapter = false
pre = "<b>4.1.2 </b>"
+++

 
#### Tạo 2 EC2 để làm Customer Gateway và EC2 Private 2

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


4. Tạo Security Group cho **EC2 Private 2**
  + Trên thanh điều hướng bên trái, chọn **Security Group**, click **Create Security Group**.
  + Trong phần **Security group name** điền **VPN Private - SG**
  + Trong phần **Description** điền **Allow SSH and Ping for servers in private subnet**.
  + Click chọn **VPC**, lựa chọn VPC có tên **ASG VPN**.

5. Trong **Inbound rules**, click **Add rule**.
  + Chọn **Type**: `SSH` và để nguyên **Source**: `Custom`. Click vào search box và chọn **VPN Public - SG**.Lựa chọn này cho phép tất cả những máy chủ được gán **VPN Public - SG** được SSH vào các máy chủ được gán **VPN Private - SG**.
  + Click **Add rule** để thêm 1 rule mới.
  + Chọn **Type**: `Custom ICMP v4` và **Source**: `Anywhere`. Cho phép ping từ bất kì địa chỉ IP nào.
![Create EC2](/images/vpn/create-sg3.png?width=90pc)
  + Kiểm tra cấu hình của bạn và kéo màn hình xuống dưới và Click **Create Security Group**.

6. Như vậy chúng ta đã tạo được 2 Security Group cho các máy chủ nằm trong public subnet và private subnet. Tiếp theo chúng ta sẽ tiến hành tạo 2 máy chủ EC2 đóng vai trò Customer Gateway và EC2 Private 2

7. Truy cập Amazon EC2 console tại địa chỉ https://console.aws.amazon.com/ec2/.
  + Trên thanh điều hướng bên trái, chọn **Intances**, **Launch Intance**.
  + Chọn **Select** để chọn Amazon Machine Image (AMI) **Amazon Linux 2 AMI (HVM), SSD Volume Type**.
![Create EC2](/images/vpc/create-ec2-2.png?width=90pc)

8. Giữ nguyên lựa chọn **General purpose t2.micro**, sau đó click **Next: Configure Instance Details**.
![Create EC2](/images/vpc/create-ec2-3.png?width=90pc)

9. Tại trang **Configure Instance Details**
  + **Network** chọn VPC **ASG VPN**.
  + **Subnet** chọn subnet **VPN Public**.
  + **Auto-assign Public IP** kiểm tra xem có đang **Enable** hay không. Nếu không Enable, bạn cần kiểm tra lại phần cấu hình tự động cấp phát public IP cho subnet.
  + Sau đó click **Next: Add Storage**.
![Create EC2](/images/vpn/create-vpnec2.png?width=90pc)

10. Tại trang **Add Storage**. Giữ nguyên cấu hình mặc định và Click **Next: Add Tags**
11. Tại trang **Add Tags**, Chọn **Add Tag**
  + **Key** điền `Name`.
  + **Value** điền `Customer Gateway`.
  + Sau đó bấm **Next: Configure Security Group**.

12. Tại trang **Configure Security Group**
  + **Assign a security group** click chọn **Select an existing security group**.
  + **Name** chọn security group cho các EC2 trong public cubnet có tên **VPN Public - SG**.
  + Sau đó bấm **Review và Launch**.
![Create EC2](/images/vpn/create-vpnec21.png?width=90pc)

13. Kiểm tra lại thông tin cấu hình và Click **Launch**.
  + Một prompt **Choose an existing key pair or Create new key pair** xuất hiện.
  + Chọn **Choose an existing keypair**.
  + Chọn **Key pair name**: **asg-keypair**.
  + Click **I acknowledge that I have access to the selected private key file (asg-keypair.pem), and that without this file, I won't be able to log into my instance.** để xác nhận rằng mình có thể truy cập tới Key pair bạn đã chọn.
  + Click **Launch**.
![Create EC2](/images/vpc/create-ec2-11.png?width=90pc)
  + Click **View Instances** và chờ cho tới khi EC2 khởi tạo xong.


14. Trong giao diện console EC2 , sau khi tạo xong instance **Customer Gateway**. Click **Launch Intance**.
  + Chọn **Select** để chọn Amazon Machine Image (AMI) **Amazon Linux 2 AMI (HVM), SSD Volume Type**.
  + Giữ nguyên lựa chọn **General purpose t2.micro**, sau đó click **Next: Configure Instance Details**.

15. Tại trang **Configure Instance Details**
  + **Network** chọn VPC **ASG VPN**.
  + **Subnet** chọn subnet **VPN Private**.
  + **Auto-assign Public IP** kiểm tra xem có đang **Disable** hay không.
  + Sau đó click **Next: Add Storage**.
![Create EC2](/images/vpn/create-vpnec22.png?width=90pc)

16. Tại trang **Add Storage**. Giữ nguyên cấu hình mặc định và Click **Next: Add Tags**
17. Tại trang **Add Tags**, Chọn **Add Tag**
  + **Key** điền `Name`.
  + **Value** điền `EC2 Private 2`.
  + Sau đó bấm **Next: Configure Security Group**.

18. Tại trang **Configure Security Group**
  + **Assign a security group** click chọn **Select an existing security group**.
  + **Name** chọn security group cho các EC2 trong public cubnet có tên **VPN Private - SG**.
  + Sau đó bấm **Review và Launch**.
![Create EC2](/images/vpn/create-vpnec23.png?width=90pc)

19. Kiểm tra lại thông tin cấu hình và Click **Launch**.
  + Một prompt **Choose an existing key pair or Create new key pair** xuất hiện.
  + Chọn **Choose an existing keypair**.
  + Chọn **Key pair name**: **asg-keypair**.
  + Click **I acknowledge that I have access to the selected private key file (asg-keypair.pem), and that without this file, I won't be able to log into my instance.** để xác nhận rằng mình có thể truy cập tới Key pair bạn đã chọn.
  + Click **Launch**.
  + Click **View Instances** và chờ cho tới khi EC2 khởi tạo xong.

20. Như vậy chúng ta đã tạo được 2 máy chủ EC2 theo mô hình kiến trúc yêu cầu. Bước tiếp theo chúng ta sẽ bắt đầu khởi tạo và cấu hình kết nối VPN Site to Site.
![Create EC2](/images/vpn/create-vpnec24.png?width=90pc)
