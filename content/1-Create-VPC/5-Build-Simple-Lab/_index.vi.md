+++
title = "Thực hành Cơ bản"
date = 2020
weight = 10
chapter = false
pre = "<b>1.5. </b>"
+++

Xuyên suốt bài thực hành này, chúng ta sẽ xây dựng một mô hình theo sơ đồ bên dưới:

![Mô hình Lab](/images/2-vpc/Lab-Diagram.PNG?width=80pc)

**Nội dung:**
- [Bước 1. Tạo một Amazon Virtual Private Cloud (VPC)](#bước-1-tạo-một-amazon-virtual-private-cloud-vpc)
- [Bước 2. Tạo một Private subnet](#bước-2-tạo-một-private-subnet)
- [Bước 3. Tạo các Public subnet](#bước-3-tạo-các-public-subnet)
- [Bước 4. Tạo một Internet gateway](#bước-4-tạo-một-internet-gateway)
- [Bước 5. Tạo một Route Table và added a route to the Internet](#bước-5-tạo-một-route-table-và-added-a-route-to-the-internet)
- [Bước 6. Tạo Security group cho các VM nằm trong Public & Private subnet](#bước-6-tạo-security-group-cho-các-vm-nằm-trong-public--private-subnet)
- [Bước 7. Tạo EC2 nằm trong Public subnet](#bước-7-tạo-ec2-nằm-trong-public-subnet)
- [Bước 8. Tạo EC2 nằm trong Private subnet](#bước-8-tạo-ec2-nằm-trong-private-subnet)
- [Bước 9. Test ping và SSH tới PUB-LINUX từ Internet](#bước-9-test-ping-và-ssh-tới-pub-linux-từ-internet)
- [Bước 10. Test ping và SSH tới PRI-LINUX từ PUB-LINUX](#bước-10-test-ping-và-ssh-tới-pri-linux-từ-pub-linux)
- [Bước 11. Tạo một địa chỉ Elastic IP](#bước-11-tạo-một-địa-chỉ-elastic-ip)
- [Bước 12. Tạo một NAT Gateway](#bước-12-tạo-một-nat-gateway)
- [Bước 13. Test ping tới internet từ PRI-LINUX](#bước-13-test-ping-tới-internet-từ-pri-linux)

#### Bước 1. Tạo một Amazon Virtual Private Cloud (VPC)

* Truy cập Amazon VPC console tại địa chỉ https://console.aws.amazon.com/vpc/
* Trong navigation pane, Chọn **Your VPCs**, **Create VPC**.
  * **Name tag**: `VPC-MAIN-ASG`
  * **IPv4 CIDR block**: `10.1.0.0/16`

#### Bước 2. Tạo một Private subnet

* Truy cập Amazon VPC console tại địa chỉ https://console.aws.amazon.com/vpc/
* Trên thanh điều hướng bên trái, chọn **Subnets**.
* Chọn **Create Subnet**.
  * **Name tag**: `PRISUB-MAIN-ASG`
  * **VPC**: `VPC-MAIN-ASG`
  * **Availability Zone**: `us-west-1b`
  * **IPv4 CIDR block**: `10.1.3.0/24`
  * Chọn **Yes**, **Create**

#### Bước 3. Tạo các Public subnet

* Truy cập Amazon VPC console tại địa chỉ https://console.aws.amazon.com/vpc/
* Trên thanh điều hướng bên trái, chọn **Subnets**.
* Chọn **Create Subnet**.
  * **Name tag**: `PUBSUB1-MAIN-ASG`
  * **VPC**: `VPC-MAIN-ASG`
  * **Availability Zone**: `us-west-1a`
  * **IPv4 CIDR block**: `10.1.1.0/24`
  * Chọn **Yes**, **Create**

Sau đó ta thực hiện tạo Public subnet thứ 2 cho NAT Gateway
* Chọn **Create Subnet**.
  * **Name tag**: `PUBSUB2-MAIN-ASG`
  * **VPC**: `VPC-MAIN-ASG`
  * **Availability Zone**: `us-west-1a`
  * **IPv4 CIDR block**: `10.1.2.0/24`
  * Chọn **Yes**, **Create**

Trên trang VPC consle 
* Lần lượt thực hiện với **PUBSUB1-MAIN-ASG** và **PUBSUB2-MAIN-ASG** 
* Chọn **Action**, **Modify auto-assign IP settings**
* Bấm **Enable auto-assign public IPv4 address** và Chọn **Save, Close**.

#### Bước 4. Tạo một Internet gateway

* Truy cập Amazon VPC console tại địa chỉ https://console.aws.amazon.com/vpc/
* Trên thanh điều hướng bên trái, chọn **Internet Gateways**, **Create Internet Gateways**
  * **Name tag**: `IGW-MAIN-ASG`  
  * Chọn **Create, Close**
* Bấm **IGW-MAIN-ASG**, Chọn **Action** sau đó **Attach to VPC**
  * **VPC**: `VPC-MAIN-ASG`

#### Bước 5. Tạo một Route Table và added a route to the Internet

* Truy cập Amazon VPC console tại địa chỉ https://console.aws.amazon.com/vpc/
* Trên thanh điều hướng bên trái, chọn **Route Tables**, **Create Route Table**
  * **Name tag**: `RT-MAIN-ASG`
  * **VPC**: `VPC-MAIN-ASG`
  * Chọn **Create, Close**
* Bấm **RT-MAIN-ASG**, Chọn **Action** rồi **Edit Route**
* Chọn **Add Route** 
  * **Destination**: `0.0.0.0/0`
  * **Target**: `IGW-MAIN-ASG`
  * Chọn **Save Route**
* Tiếp tục bấm on **Action**, rồi **Edit Subnet Association**
* Bấm **PUBSUB1-MAIN-ASG** và **PUBSUB2-MAIN-ASG**, sau đó bấm on **Save**

#### Bước 6. Tạo Security group cho các VM nằm trong Public & Private subnet

Đầu tiên, thực hiện tạo Security group cho Public Subnet
* Truy cập Amazon VPC console tại địa chỉ https://console.aws.amazon.com/vpc/
* Trên thanh điều hướng bên trái, chọn **Security Group**, **Create Security Group**
  * **Security group name**: `SG-PUB-MAIN-ASG`
  * **Description Info**: `security for public subnet`
  * **VPC**: `VPC-MAIN-ASG`
* Trong **Inbound rules**, bấm **Add rules**
  * **Type**: `SSH`, **Source**: `My IP`
  * **Type**: `Custom ICMP v4`, **Source**: `Anywhere` 
  * **Type**: `Custom ICMP 64`, **Source**: `Anywhere` 
  * Sau đó bấm **Create Security Group**

Tiếp theo, thực hiện tạo Security group cho Private subnet
* Trên thanh điều hướng bên trái, chọn **Security Group**, **Create Security Group**
  * **Security group name**: `SG-PRI-MAIN-ASG`
  * **Description Info**: `security for private subnet`
  * **VPC**: `VPC-MAIN-ASG`
* Trong **Inbound rules**, bấm **Add rules**
  * **Type**: `SSH`, **Source**: `SG-PUB-MAIN-ASG`
  * **Type**: `Custom ICMP v4`, **Source**: `Anywhere` 
  * **Type**: `Custom ICMP 64`, **Source**: `Anywhere` 
  * Sau đó bấm **Create Security Group**

#### Bước 7. Tạo EC2 nằm trong Public subnet

* Truy cập Amazon EC2 console tại địa chỉ https://console.aws.amazon.com/ec2/
* Trên thanh điều hướng bên trái, chọn **Intances**, **Launch Intance**
* **Step 1: Chọn an Amazon Machine Image (AMI)**: `Amazon Linux 2 AMI (HVM), SSD Volume Type`
* **Step 2: Chọn an Instance Type**: `General purpose t2.nano`, sau đó **Next: Configure Instance Details**
* **Step 3: Configure Instance Details**
	* **Network**: `VPC-MAIN-ASG`
	* **Subnet**: `PUBSUB1-MAIN-ASG`
	* **Auto-assign Public IP**: `Enable`
* Sau đó bấm **Next: Add Storage**, **Next: Add Tags**
* **Step 5: Add Tags**, Chọn **Add Tag**
	* **Key**: `Name`
	* **Value**: `PUB-LINUX`
* Sau đó bấm **Next: Configure Security Group**
* **Step 6: Configure Security Group**
	* **Assign a security group**: `Bấm an existing security group`
	* **Name**: `SG-PUB-MAIN-ASG`
* Sau đó bấm **Review và Launch**, **Launch**
* Một prompt **Choose an existing key pair or Create new key pair** xuất hiện, chọn **Create a new key pair**
	* **Key pair name**: `awskey`
* Chọn **View Instance** và chờ cho tới khi EC2 khởi tạo xong

#### Bước 8. Tạo EC2 nằm trong Private subnet

* Truy cập Amazon EC2 console tại địa chỉ https://console.aws.amazon.com/ec2/
* Trên thanh điều hướng bên trái, chọn **Intances**, **Launch Intance**
* **Step 1: Chọn an Amazon Machine Image (AMI)**: `Amazon Linux 2 AMI (HVM), SSD Volume Type`
* **Step 2: Chọn an Instance Type**: `General purpose t2.nano`, sau đó **Next: Configure Instance Details**
* **Step 3: Configure Instance Details**
	* **Network**: `VPC-MAIN-ASG`
	* **Subnet**: `PRISUB-MAIN-ASG`
	* **Auto-assign Public IP**: `Disable`
* Sau đó bấm **Next: Add Storage**, **Next: Add Tags**
* **Step 5: Add Tags**, Chọn **Add Tag**
	* **Key**: `Name`
	* **Value**: `PRI-LINUX`
* Sau đó bấm **Next: Configure Security Group**
* **Step 6: Configure Security Group**
	* **Assign a security group**: `Bấm an existing security group`
	* **Name**: `SG-PRI-MAIN-ASG`
* Sau đó bấm **Review và Launch**, **Launch**
* Một prompt **Choose an existing key pair or Create new key pair** xuất hiện, Chọn **Choose an existing key pair**
	* **Key pair name**: `awskey`
* Chọn **View Instance** và chờ cho tới khi EC2 khởi tạo xong

#### Bước 9. Test ping và SSH tới PUB-LINUX từ Internet

![Testing Ping from Internet](/images/2-vpc/Test-ping-ssh-PUBLINUX-from-Internet.png?width=90pc)

#### Bước 10. Test ping và SSH tới PRI-LINUX từ PUB-LINUX

![Testing Ping between Subnets](/images/2-vpc/Test-ping-ssh-PRILINUX-from-PUBLINUX.png?width=90pc)

Tại thời điểm này, PRI-LINUX vẫn chưa có khả năng kết nối ra bên ngoài internet do chúng ta chưa cấu hình NAT GW

#### Bước 11. Tạo một địa chỉ Elastic IP

* Truy cập Amazon VPC console tại địa chỉ https://console.aws.amazon.com/vpc/
* Trên thanh điều hướng bên trái, chọn **Elastic IPs**
* Chọn **Allocate Elastic IP address** 
	* **Elastic IP address settings**: `Amazon's pool of IPv4 addresses`
	* Chọn **Allocate**
Địa chỉ Public IP Address:`3.101.0.64`

#### Bước 12. Tạo một NAT Gateway

* Truy cập Amazon VPC console tại địa chỉ https://console.aws.amazon.com/vpc/
* Trên thanh điều hướng bên trái, chọn **NAT Gateways**.
* Chọn **Create NAT Gateway**.
  * **Subnet**: `PUBSUB2-MAIN-ASG`
  * **Elastic IP Allocation ID**: `13.52.136.34`
* Chọn **Add Tag** 
	* **Key**: `Name`
	* **Value**: `NAT-MAIN-ASG`
* Chọn **Tạo một NAT Gateway**
* Chọn **Edit Route Table**, **Create Route Table**
	* **Name tag**: `RT-NAT-ASG`
	* **VPC**: `VPC-MAIN-ASG`
	* Chọn **Create, Close**
* Bấm **RT-NAT-ASG**, Chọn **Action** rồi **Edit Route**
* Chọn **Add Route** 
  * **Destination**: `0.0.0.0/0`
  * **Target**: `NAT-MAIN-ASG`
  * Chọn **Save Route**
* Tiếp tục bấm vào **Action**, sau đó **Edit Subnet Association**
* Bấm **PUBSUB1-MAIN-ASG** và **PRISUB-MAIN-ASG**, rồi bấm  **Save**

#### Bước 13. Test ping tới internet từ PRI-LINUX

Kết quả cuối cùng của chúng ta:

![Testing Internet Connection](/images/2-vpc/Test-ping-to-Internet-from-PRILINUX.png?width=90pc)