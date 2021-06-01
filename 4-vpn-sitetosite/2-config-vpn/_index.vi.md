+++
title = "Cấu hình Site-to-Site VPN"
date = 2020
weight = 2
chapter = false
pre = "<b>2.3.2. </b>"
+++

![Lab Diagram](/images/3/0.png)

**Nội dung:**
- [1. Cấu hình Site to Site VPN](#1-cấu-hình-site-to-site-vpn)
- [2. Cấu hình Site-to-Site VPN trên Customer Gateway (OpenSwan)](#2-cấu-hình-site-to-site-vpn-trên-customer-gateway-openswan)

#### 1. Cấu hình Site to Site VPN

**Tạo Virtual Private Gateway**

1. Truy cập trang **Amazon VPC console** tại địa chỉ https://console.aws.amazon.com/vpc/, chọn **Virtual Private Gateway**.

![Network](/images/3/8.png?width=90pc)

2. Chọn **Create Virtual Private Gateway** và tạo với thông tin.
   - **Name tag:** ```VPNGateway```
   
![Network](/images/3/9.png?width=90pc)

3. Trong danh sách các **Virtual Private Gateway**, chọn vào **VPNGateway** vừa tạo.
4. Chọn **Actions** > **Attach to VPC**.
5. Chọn VPC đã tạo ở bước trước. (**LabVPC**)

![Network](/images/3/10.png?width=90pc)

**Tạo & cấu hình Customer Gateway**

1. Truy cập trang **Amazon VPC console** tại địa chỉ https://console.aws.amazon.com/vpc/, chọn **Customer Gateway**.

![Network](/images/3/11.png?width=90pc)

2. Chọn **Create Customer Gateway** và tạo với thông tin:
   - **Name:** ```CustomerGateway```
   - **Routing:** Static
   - **IP Address:** Nhập địa chỉ IP Public của VPN Gateway ở phía Onprem của bạn.
3. Chọn **Create Customer Gateway**.

![Network](/images/3/12.png?width=90pc)

**Tạo & cấu hình kết nối Site-to-Site trên AWS**

1. Truy cập trang **Amazon VPC console** tại địa chỉ https://console.aws.amazon.com/vpc/, chọn **Site-to-Site VPN Connections**.
2. Chọn **Create VPN Connection** và tạo với thông tin:
   * **Name tag:** ```VPNConnection```
   * **Target Gateway Type:** Chọn **Virtual Private Gateway**
   * **Virtual Private Gateway:** Chọn **VPNGateway**
   * **Customer Gateway:** Existing
   * **Customer Gateway ID:**	Chọn **CustomerGateway**
   * **Routing Options:** Static
   * **Static IP Prefixes:** ```10.28.0.0/16``` 
   * Các cấu hình khác giữ nguyên mặc định.
3. Chọn **Create VPN Connection**.

**Cấu hình Route Table**

1. Truy cập trang **Amazon VPC console** tại địa chỉ https://console.aws.amazon.com/vpc/, chọn **Route Tables**.
2. Chọn các **Route Table** trong **LabVPC**.
3. Chọn tab **Route Propagation**
4. Chọn **Edit route propagation**
5. Chọn vào ô **Propagate**
6. Chọn **Save** để lưu Route Table.

![Network](/images/3/13.png?width=90pc)

#### 2. Cấu hình Site-to-Site VPN trên Customer Gateway (OpenSwan)

Để tải tập tin cấu hình cho VPN Connection ở phía On-premise. Bạn có thể tải Template từ phía AWS cung cấp:
1. Truy cập trang **Amazon VPC console** tại địa chỉ https://console.aws.amazon.com/vpc/, chọn **Site-to-Site VPN Connection**.
2. Chọn VPN Connection đã tạo, sau đó chọn Download Configuration.
3. Trong hộp thoại Download Configuration, lựa chọn appliance phù hợp với bạn: Trong bài thực hành này, chúng ta sẽ sử dụng OpenSwan.
   - **Vendor:**	Chọn **OpenSwan**
   - **Platform:**	Chọn **OpenSwan**
   - **Software:**	Chọn **OpenSwan 2.6.38+**

![Network](/images/3/17.png?width=90pc)

Sau đó dựa vào cấu hình được cung cấp, bạn thay đổi các thông tin phù hợp và cấu hình cho thiết bị của mình.

* Cài đặt **OpenSwan**
```bash
sudo su
yum install openswan -y
```

![Network](/images/3/14.png?width=90pc)

* Cấu hình ```/etc/ipsec.conf```
Bỏ dấu # phía trước cấu hình bên dưới.
```bash
include /etc/ipsec.d/*.conf
```

* Cấu hình ```/etc/sysctl.conf```
Thêm cấu đoạn sau vào cuối tập tin cấu hình.
```bash
net.ipv4.ip_forward = 1
net.ipv4.conf.all.accept_redirects = 0
net.ipv4.conf.all.send_redirects = 0
```

![Network](/images/3/15.png?width=90pc)

Sau đó để áp dụng cấu hình này, bạn chạy lệnh:
```bash
sysctl -p
```

* Cấu hình ```/etc/ipsec.d/aws.conf```
Thêm cấu đoạn sau vào tập tin cấu hình. Chúng ta sẽ tạo 2 Tunnel với thông tin được lấy từ VPN Connection đã tạo trước đó.

```bash
conn Tunnel1
	authby=secret
	auto=start
	left=%defaultroute
	leftid=54.249.66.228
	right=13.114.170.224
	type=tunnel
	ikelifetime=8h
	keylife=1h
	phase2alg=aes128-sha1;modp1024
	ike=aes128-sha1;modp1024
	auth=esp
	keyingtries=%forever
	keyexchange=ike
	leftsubnet=10.28.0.0/16
	rightsubnet=10.10.0.0/16
	dpddelay=10
	dpdtimeout=30
	dpdaction=restart_by_peer

conn Tunnel2
	authby=secret
	auto=start
	left=%defaultroute
	leftid=54.249.66.228
	right=52.198.189.177
	type=tunnel
	ikelifetime=8h
	keylife=1h
	phase2alg=aes128-sha1;modp1024
	ike=aes128-sha1;modp1024
	auth=esp
	keyingtries=%forever
	keyexchange=ike
	leftsubnet=10.28.0.0/16
	rightsubnet=10.10.0.0/16
	dpddelay=10
	dpdtimeout=30
	dpdaction=restart_by_peer
```

**Ghi chú:**  
```text
leftid: IP Public Address phía Onprem.
right: IP Public Address phía AWS VPN Tunnel.
leftsubnet: CIDR của Mạng phía Local (Nếu có nhiều lớp mạng, bạn có thể để là 0.0.0.0/0).
rightsubnet: CIDR của Mạng phía Private Subnet trên AWS.
```

![Network](/images/3/16.png?width=90pc)

* Cấu hình ```/etc/ipsec.d/aws.secrets```
Tạo tập tin mới với cấu hình sau để thiết lập chứng thực cho 2 Tunnel.
```bash
54.249.66.228 13.114.170.224: PSK "ZQbWr7AXk1Wy4hv1Y3KcO4orck.DiQz0"
54.249.66.228 52.198.189.177: PSK "8OrmB8M6Jspp6v1yv0ekd89oFM3dNAyC"
```

![Network](/images/3/18.png?width=90pc)

* Khởi động lại Network service & IPSEC service
```bash
service network restart 
chkconfig ipsec on
service ipsec start
service ipsec status
```

![Network](/images/3/19.png?width=90pc)
