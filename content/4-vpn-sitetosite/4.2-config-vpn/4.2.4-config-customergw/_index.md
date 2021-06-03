+++
title = "Cấu hình Customer GW "
date = 2021
weight = 4
chapter = false
pre = "<b>4.2.4 </b>"
+++


1. Để tải tập tin cấu hình cho VPN Connection ở phía On-premise. Bạn có thể tải Template từ phía AWS cung cấp:
  + Truy cập trang **Amazon VPC console** tại địa chỉ https://console.aws.amazon.com/vpc/
  + Kéo thanh điều hướng bên trái xuống dưới, click **Site-to-Site VPN Connection**.
  + Chọn VPN Connection đã tạo, sau đó chọn Download Configuration.
![Configure VPN](/images/vpn/create-vpn6.png?width=90pc)

2. Trong hộp thoại Download Configuration, lựa chọn appliance phù hợp với bạn: Trong bài thực hành này, chúng ta sẽ sử dụng OpenSwan.
   + **Vendor:**	Chọn **OpenSwan**
   + **Platform:**	Chọn **OpenSwan**
   + **Software:**	Chọn **OpenSwan 2.6.38+**
   + Click **Download**.

![Configure VPN](/images/vpn/create-vpn8.png?width=90pc)
   + Lưu thông tin file câu hình vào thư mục chúng ta sử dụng lưu trữ key pair và công cụ cho bài lab.
  
3. Sau đó dựa vào cấu hình được cung cấp, bạn thay đổi các thông tin phù hợp và cấu hình cho thiết bị của mình.
4. Kết nối ssh vào EC2 **Customer Gateway**.
![Configure VPN](/images/vpn/configure-cgw.png?width=90pc)
  + Cài đặt **OpenSwan**
```bash
sudo su
yum install openswan -y
```
![Configure VPN](/images/vpn/configure-cgw2.png?width=90pc)


5. Kiểm tra cấu hình file **/etc/ipsec.conf**
 + Chạy lệnh **vi /etc/ipsec.conf**
 + Kiểm tra cấu hình đang như hình dưới.
![Configure VPN](/images/vpn/configure-cgw3.png?width=90pc)
 + Ấn phím ESC và tổ hợp :q! để thoát khỏi trình chỉnh sửa **vi**.

6. Cấu hình file **/etc/sysctl.conf**
 + Chạy lệnh **vi /etc/sysctl.conf**
 + Chuyển xuống vị trí cuối cùng trong file cấu hình. Ấn phím *i* để tiến hành chỉnh sửa file.
 + Thêm cấu đoạn sau vào cuối tập tin cấu hình.
```bash
net.ipv4.ip_forward = 1
net.ipv4.conf.all.accept_redirects = 0
net.ipv4.conf.all.send_redirects = 0
```
 + Ấn phím ESC và tổ hợp :wq! để lưu file cấu hình.
![Configure VPN](/images/vpn/configure-cgw4.png?width=90pc)

7. Sau đó để áp dụng cấu hình này, chạy lệnh:
```bash
sysctl -p
```
8. Tiếp theo chúng ta sẽ cấu hình file **/etc/ipsec.d/aws.conf**
 + Chạy lệnh **vi /etc/ipsec.d/aws.conf**
 + Ấn phím **i** để tiến hành chỉnh sửa file.
 + Thêm cấu đoạn sau vào tập tin cấu hình. Chúng ta sẽ tạo 2 Tunnel với thông tin được lấy từ file cấu hình VPN Connection bạn đã tải về và lưu chung vào thư mục chứa key pair trước đó.

```bash
conn Tunnel1
	authby=secret
	auto=start
	left=%defaultroute
	leftid=13.212.233.95
	right=52.220.15.74
	type=tunnel
	ikelifetime=8h
	keylife=1h
	phase2alg=aes128-sha1;modp1024
	ike=aes128-sha1;modp1024
	keyingtries=%forever
	keyexchange=ike
	leftsubnet=10.11.0.0/16
	rightsubnet=10.10.0.0/16
	dpddelay=10
	dpdtimeout=30
	dpdaction=restart_by_peer
	overlapip=yes

conn Tunnel2
	authby=secret
	auto=start
	left=%defaultroute
	leftid=13.212.233.95
	right=54.255.91.142
	type=tunnel
	ikelifetime=8h
	keylife=1h
	phase2alg=aes128-sha1;modp1024
	ike=aes128-sha1;modp1024
	keyingtries=%forever
	keyexchange=ike
	leftsubnet=10.11.0.0/16
	rightsubnet=10.10.0.0/16
	dpddelay=10
	dpdtimeout=30
	dpdaction=restart_by_peer
	overlapip=yes
```
 + Ấn phím ESC và tổ hợp :wq! để lưu file cấu hình.

{{%notice tip%}}
Đảm bảo bạn chỉnh sửa địa chỉ IP và lớp mạng phù hợp trước khi copy đoạn cấu hình trên.\
Đối với Amazon Linux thì chúng ta sẽ bỏ dòng **auth=esp** trong file cấu hình gốc.\
Vì chúng ta chỉ có 1 public IP addres cho **Customer Gateway** nên sẽ cần thêm cấu hình **overlapip=yes** .\
{{%/notice%}}

{{%notice note%}}
**leftid**: IP Public Address phía Onprem. ( Ở đây chính là IP public của EC2 **Customer Gateway** trong **ASG VPN** VPC) .\
**right**: IP Public Address phía AWS VPN Tunnel. .\
**leftsubnet**: CIDR của Mạng phía Local (Nếu có nhiều lớp mạng, bạn có thể để là 0.0.0.0/0). .\
**rightsubnet**: CIDR của Mạng phía Private Subnet trên AWS. 
{{%/notice%}}

![Configure VPN](/images/vpn/configure-cgw5.png?width=90pc)

9. Kiểm tra bước tiếp theo trong file cấu hình chúng ta đã tải xuống.
![Configure VPN](/images/vpn/configure-cgw6.png?width=90pc)

10. Tạo mới và cấu hình file **etc/ipsec.d/aws.secrets**
Tạo tập tin mới với cấu hình sau để thiết lập chứng thực cho 2 Tunnel.
 + Chạy lệnh **touch /etc/ipsec.d/aws.secrets** để tạo tập tin.
 + Chạy lệnh **vi /etc/ipsec.d/aws.secrets**
 + Ấn phím **i** để tiến hành chỉnh sửa file.
 + Thêm cấu đoạn sau vào cuối tập tin cấu hình.
```bash
13.212.233.95 52.220.15.74: PSK "2ned6kIANuA8nPtYnwvQVwak_fJcmSP5"
13.212.233.95 54.255.91.142: PSK "jdkPQG140TeUacw3rnKlyavxnrNaAkZ7"
```
 + Ấn phím ESC và tổ hợp :wq! để lưu file cấu hình.
 + Chạy lệnh **cat /etc/ipsec.d/aws.secrets** để kiểm tra nội dung file cấu hình.
![Configure VPN](/images/vpn/configure-cgw7.png?width=90pc)


11. Khởi động lại Network service & IPSEC service
```bash
service network restart 
chkconfig ipsec on
service ipsec start
service ipsec status
```

![Configure VPN](/images/vpn/configure-cgw8.png?width=90pc)

{{%notice tip%}}
Nếu status tunnel vẫn chưa chạy đúng, sau khi kiểm tra và cập nhật cấu hình bạn sẽ cần chạy lệnh để restart lại service network và ipsec : .\
**sudo service network restart** .\
**sudo service ipsec restart**
{{%/notice%}}


12. Sau khi hoàn tất cấu hình.Hãy thử thực hiện lệnh ping từ phía máy chủ **Customer Gateway** tới máy chủ **EC2 Private**. Nếu cấu hình VPN thành công bạn sẽ được kết quả như dưới đây.

![Configure VPN](/images/vpn/testping.png?width=90pc)