+++
title = "Cấu hình Site-to-Site VPN"
date = 2020
weight = 1
chapter = false
pre = "<b>2.3. </b>"
+++

Mặc định từ bên ngoài có thể kết nối TTDL On-premise tới Amazon VPC sử dụng VPN cứng hoặc mềm tùy thuộc mục đích và nhu cầu sử dụng thực thế.  
Cụ thể Amazon VPC cung cấp 2 cách để kết nối với môi trường mạng của doanh nghiệp đó là **VPG** and **CGW**.  
* Virtual Private Gateway (**VPG**) là một trung tâm điều khiển kết nối the virtual private network (VPN) được cài đặt ở đầu AWS.  
* A Customer Gateway (**CGW**) là thành phần đại diện cho thiết bị VPN cứng hoặc mềm được cài đặt ở đầu Khách hàng.

**VPN tunnel**  sẽ được thiết lập ngay sau khi lưu lượng dữ liệu được truyền tải giữa AWS và hệ thống mạng của khách hàng. Trong kết nối đó, ta phải chỉ rõ loại định tuyến sẽ được sử dụng để đảm bảo an toàn cũng như chất lượng về mặt truyền tải dữ liệu.  
Nếu CGW ở phía khách hàng có hỗ trợ Border Gateway Protocol (BGP), thì trong cấu hình VPN connection ta bắt buộc phải đặt định tuyến là dynamic routing.  
Còn ngược lại ta phải cấu hình định tuyến kết nối là static routing. Trường hợp sử dụng static routing, ta phải nhập chính xác những định tuyến cần thiết cho việc kết nối từ phía Khách hàng tới VPG được thiết lập ở đầu AWS. Đồng thời định tuyến cho VPC cũng phải được cấu hình `propagated` để cho phép các tài nguyên có thể trao đổi dữ liệu đi ra và đi vào trong kết nối VPN tunnel giữa AWS với hệ thống mạng của Khách hàng

Amazon VPC cung cấp nhiều loại CGWs, và từng CGW được gán với một VPG nhưng 1 VPG có thể kết hợp với nhiều CGW (many-to-one design). Để hỗ trợ mô hình này thì địa chỉ IP của CGW phải là duy nhất trong một region.  
Amazon VPC cũng cung cấp các thông tin cần thiết cho Nhân viên quản trị mnagj có thể cấu hình CGW và thiết lập kết nối VPN tới VPG trên AWS. Kết nối VPN luôn bao gồm 2 Internet Protocol Security (IPSec) tunnels để đảm bảo tính sẵn sàng cao của kết nối.  
Bên dưới là những đặc điểm quan trọng mà ta cần nắm về **VPG**, **CGW**, and **VPN**:
* VPG là thành phần đầu cuối của VPN tunnel nằm trên AWS.
* CGW có thể là thiết bị phần cứng hoặc ứng dụng phần mềm nằm ở đầu Khách hàng trong kết nối VPN tunnel.
* Bạn phải khởi tạo kết nối VPN tunnel từ CGW tới VPG.
* VPG hỗ trợ cả dynamic routing (BGP) và static routing.
* Kết nối VPN luôn có 2 tunnels nhằm đảm bảo tính sẵn sàng cao cho kết nối với VPC từ site Khách hàng.

Bài lab giúp chúng ta học được cách thiết lập một kết nối Site to Site VPN trong AWS. Trong thực tế, giải pháp này khá được ưa chuộng do ưu điểm giá thành rẻ, đồng thời rất dễ cấu hình do AWS cung cấp hướng dẫn cho từng loại thiết bị phía đầu Customer. Việc Cust bận tâm duy nhất đó là chuẩn bị đường internet để từ đó tạo đường hầm an toàn bí mật (sử dụng ipsec) kết nối tới AWS thông qua AWS VPN tunnel.  
Trong phạm vi bài lab, giả lập rằng chúng ta có Main office và Branch office đặt tại 2 VPC thuộc 2 AZ khác nhau để có sự khác biệt về mặt network. Trên mỗi VPC thực hiện tạo 2 EC2 cho phép SSH từ bên ngoài, nhưng không có khả năng kết nối và ping lẫn nhau sử dụng địa chỉ Private IP của mỗi EC2. Việc ta cần làm là cấu hình VPN để các địa chỉ Private IP có thể ping được lẫn nhau sử dụng VPN Site-to-Site.

![Lab Diagram](/images/2-vpc/Lab-Diagram-Site-to-Site.PNG?width=80pc)

**Nội dung:**
- [1. Cấu hình Network](#1-cấu-hình-network)
- [2. Khởi tạo EC2 trên mỗi VPC](#2-khởi-tạo-ec2-trên-mỗi-vpc)
- [3. Cấu hình Site to Site VPN tại Main office](#3-cấu-hình-site-to-site-vpn-tại-main-office)
- [4. Cấu hình Site to Site VPN tại Branch office](#4-cấu-hình-site-to-site-vpn-tại-branch-office)
- [5. Test ping từ Pub-Linux tới Pub-Linux-Bra và ngược lại](#5-test-ping-từ-pub-linux-tới-pub-linux-bra-và-ngược-lại)

#### 1. Cấu hình Network 

* Tạo 2 VPC nằm trên 2 AZ khác nhau
  * **VPC-Main-ASG** với CIDR block `10.1.0.0/16`
  * **VPC-Bra-ASG** với CIDR block `10.2.0.0/16`
* Tạo Public Subnets trên mỗi VPC
	* **Pubsub1-Main-ASG** với CIDR block `10.1.1.0/24`
	* **Pubsub-Bra-ASG** với CIDR block `10.2.1.0/24`
* Tạo Internet GW trên mỗi VPC 
	* **IGW-Main-ASG** được gắn với **VPC-Main-ASG**
  	* **IGW-Bra-ASG** được gắn với **VPC-Bra-ASG**
* Sửa lại Default Route Table cho mỗi VPC
	* Names: **RT-Main-ASG**; Routes: `Local,0.0.0.0/0`  **IGW-Main-ASG**
	* Names: **RT-Bra-ASG**; Routes: `Local,0.0.0.0/0`  **IGW-Bra-ASG**

 
#### 2. Khởi tạo EC2 trên mỗi VPC

Tại thời điểm ban đầu, 2 địa chỉ Private IP của 2 EC2 đều không thể ping được lẫn nhau.

**Tạo Security Group cho EC2 thuộc Main Office**

* Security group: Tạo security group mới: **SG-Pub-Main-ASG**
* Type: **SSH**; Source: **My IP**
* Type: **All TCP**; Source: Custom, `10.2.0.0/16`	
* Type: **All UDP**; Source: Custom, `10.2.0.0/16` 
* Type: **All ICMP - IPv4**; Source: Custom, `10.2.0.0/16` 

**Tạo Security Group cho EC2 thuộc Branch Office**

* Security group: Tạo security group mới: **SG-Pub-Bra-ASG**
* Type: **SSH**; Source: **My IP** 
* Type: **All TCP**; Source: Custom, `10.1.0.0/16`	
* Type: **All UDP**; Source: Custom, `10.1.0.0/16` 
* Type: **All ICMP - IPv4**; Source: Custom, `10.1.0.0/16` 
 
**Tạo EC2 thuộc mạng của Main Office**

* AMI: **Amazon Linux 2**
* Instance type: **t2.medium**
* Network: **VPC-Main-ASG**
* Subnet: **Pubsub1-Main-ASG**
* Auto-assign Public IP: **Enable**
* Security group: **SG-Pub-Main-ASG**
* Key pair: **Create a new key pair**: `awskey2`

**Tạo EC2 thuộc mạng của Branch Office**

* AMI: **Amazon Linux 2**
* Instance type: **t2.medium**
* Network: **VPC-Bra-ASG**
* Subnet: **Pubsub-Bra-ASG**
* Auto-assign Public IP: **Enable**
* Security group: **SG-Pub-Bra-ASG**
* Key pair: **Create a new key pair**: `awskey2`
 

#### 3. Cấu hình Site to Site VPN tại Main office

**Tạo Virtual Private Gateway tại Main Office**
* Truy cập trang Amazon VPC console tại địa chỉ https://console.aws.amazon.com/vpc/ 
* Chọn **Virtual Private Gateway**
* Chọn **Tạo VPG**
* Name: **VPG-MainBranch-ASG**
 
**Tạo & cấu hình Customer Gateway tại Main Office**

* Name: **CGW-MainBranch-ASG**
* Routing: **Static**
* IP Address: **13.56.76.108** 
 
**Tạo & cấu hình kết nối Site-to-Site tại Main Office**

* Name: **VPN-MainBranch-ASG**
* Virtual Private Gateway: **VPG-MainBranch-ASG**
* Customer Gateway: Exist (**CGW-MainBranch-ASG**)
* Routing Options: **Static**
* IP Prefixes: `10.2.0.0/16` 
 
Bật propagate cho Route table của VPC-Main-ASG: 
* Truy cập trang Amazon VPC console tại địa chỉ https://console.aws.amazon.com/vpc/
* Chọn Route table thuộc Main Office : **RT-Main-ASG**
* Bấm **Route Propagation**, **Edit Route Propagation**
* Check **Enable, Yes**


#### 4. Cấu hình Site to Site VPN tại Branch office

Trên máy chủ EC2-Bra-ASG thực hiện các bước sau:

* Cài đặt OpenSwan
```javascript
sudo su
yum install openswan -y
```

* Cấu hình /etc/ipsec.conf
```javascript
include /etc/ipsec.d/*.conf
```

* Cấu hình /etc/sysctl.conf
```javascript
net.ipv4.ip_forward = 1
net.ipv4.conf.all.accept_redirects = 0
net.ipv4.conf.all.send_redirects = 0
```

* Cấu hình /etc/ipsec.d/aws-vpn.conf 
```javascript
conn Tunnel1
	authby=secret
	auto=start
	left=%defaultroute
	leftid=13.56.76.108 
	right=52.53.71.108
	type=tunnel
	ikelifetime=8h
	keylife=1h
	phase2alg=aes128-sha1;modp1024
	ike=aes128-sha1;modp1024
	keyingtries=%forever
	keyexchange=ike
	leftsubnet=10.2.0.0/16
	rightsubnet=10.1.0.0/16
	dpddelay=10
	dpdtimeout=30
	dpdaction=restart_by_peer
```
**Chú ý:**  
```text
leftid: IP Public Address of Branch office.
right: IP Public Address of AWS VPN Tunnel
leftsubnet: CIDR from Branch site 
rightsubnet: CIDR from Main site
```

* Cấu hình /etc/ipsec.d/aws-vpn.secrets
```javascript
13.56.76.108 52.53.71.108: PSK "vYiouHnJ1Q2itTl9NCy8zSuOOWciVmz2"
```

* Khởi động lại Network service & IPSEC service
```javascript
service network restart 
chkconfig ipsec on
service ipsec start
service ipsec status
```

![Thông tin VPN đã thiết lập](/images/2-vpc/VPN-MainBranch-ASG.png?width=90pc)

#### 5. Test ping từ Pub-Linux tới Pub-Linux-Bra và ngược lại

![Testing Ping giữa các Subnet](/images/2-vpc/Test-ping-PubLinux-to-PubLinuxBra.png?width=90pc)

![Testing Ping giữa các Subnet](/images/2-vpc/Test-ping-PubLinuxBra-to-PubLinux.png?width=90pc)
