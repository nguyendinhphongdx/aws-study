+++
title = "Setup Site to Site VPN"
date = 2020
weight = 1
chapter = false
pre = "<b>2.3. </b>"
+++

You can connect an existing data center to Amazon VPC using either hardware or software VPN connections, which will make Amazon VPC an extension of the data center. Amazon VPC offers two ways to connect a corporate network to a VPC: **VPG** and **CGW**.
* A Virtual Private Gateway (**VPG**) is the virtual private network (VPN) concentrator on the AWS side of the VPN connection between the two networks. 
* A Customer Gateway (**CGW**) represents a physical device or a software application on the customer’s side of the VPN connection.

The **VPN tunnel** is established after traffic is generated from the customer’s side of the VPN connection.  
You must specify the type of routing that you plan to use when you create a VPN connection. If the CGW supports Border Gateway Protocol (BGP), then configure the VPN connection for dynamic routing. Otherwise, configure the connections for static routing. If you will be using static routing, you must enter the routes for your network that should be communicated to the VPG. Routes will be `propagated` to the Amazon VPC to allow your resources to route network traffic back to the corporate network through the VGW and across the VPN tunnel.

Amazon VPC also supports multiple CGWs, each having a VPN connection to a single VPG (many-to-one design). In order to support this topology, the CGW IP addresses must be unique within the region. Amazon VPC will provide the information needed by the network administrator to configure the CGW and establish the VPN connection with the VPG. The VPN connection consists of two Internet Protocol Security (IPSec) tunnels for higher availability to the Amazon VPC.  
Following are the important points to understand about **VPG**, **CGW**, and **VPN**:
* The VPG is the AWS end of the VPN tunnel. 
* The CGW is a hardware or software application on the customer’s side of the VPN tunnel.
* You must initiate the VPN tunnel from the CGW to the VPG.
* VPGs support both dynamic routing with BGP and static routing.
* The VPN connection consists of two tunnels for higher availability to the VPC.

Let’s build a lab together then we have a better understanding about VPN Site-to-site.  
In this lab, assume that we have Main office and Branch office lies on 2 VPCs belonging to 2 different AZs to make network difference from 2 sites. On each VPC, we create 2 EC2 to allow SSH from outside, but not able to connect and ping each other using the Private IP address of each EC2. What we need to do is configure the VPN so that Private IP addresses can ping each other using Site-to-Site VPN.

![Lab Diagram](/images/2-vpc/Lab-Diagram-Site-to-Site.PNG?width=80pc)

**Contents:**
- [1. Configure Network](#1-configure-network)
- [2. Initialize EC2 on each VPC](#2-initialize-ec2-on-each-vpc)
- [3. Configure Site to Site VPN at Main office](#3-configure-site-to-site-vpn-at-main-office)
- [4. Configure Site to Site VPN at Branch office](#4-configure-site-to-site-vpn-at-branch-office)
- [5. Test ping from Pub-Linux to Pub-Linux-Bra and vice versa](#5-test-ping-from-pub-linux-to-pub-linux-bra-and-vice-versa)

#### 1. Configure Network

* Create 2 VPC on 2 seperate AZ 
  * **VPC-Main-ASG** with CIDR block `10.1.0.0/16`
  * **VPC-Bra-ASG** with CIDR block `10.2.0.0/16`
* Create Public Subnets on each VPC
	* **Pubsub1-Main-ASG** with CIDR block `10.1.1.0/24`
	* **Pubsub-Bra-ASG** with CIDR block `10.2.1.0/24`
* Create Internet GW on each VPC 
	* **IGW-Main-ASG** attached to **VPC-Main-ASG**
  	* **IGW-Bra-ASG** attached to **VPC-Bra-ASG**
* Modify Default Route Table for each VPC
	* Names: **RT-Main-ASG**; Routes: `Local,0.0.0.0/0`  **IGW-Main-ASG**
	* Names: **RT-Bra-ASG**; Routes: `Local,0.0.0.0/0`  **IGW-Bra-ASG**

#### 2. Initialize EC2 on each VPC

At the beginning, two EC2 Private IP addresses will not be able to ping each other. 

**Create Security Group for EC2 in Main Office**

* Security group: Create a new security group: **SG-Pub-Main-ASG**
* Type: **SSH**; Source: **My IP**
* Type: **All TCP**; Source: Custom, `10.2.0.0/16`	
* Type: **All UDP**; Source: Custom, `10.2.0.0/16` 
* Type: **All ICMP - IPv4**; Source: Custom, `10.2.0.0/16` 

**Create Security Group for EC2 in Branch Office**

* Security group: Create a new security group: **SG-Pub-Bra-ASG**
* Type: **SSH**; Source: **My IP** 
* Type: **All TCP**; Source: Custom, `10.1.0.0/16`	
* Type: **All UDP**; Source: Custom, `10.1.0.0/16` 
* Type: **All ICMP - IPv4**; Source: Custom, `10.1.0.0/16` 
 
**Create EC2 in Main Office network**

* AMI: **Amazon Linux 2**
* Instance type: **t2.medium**
* Network: **VPC-Main-ASG**
* Subnet: **Pubsub1-Main-ASG**
* Auto-assign Public IP: **Enable**
* Security group: **SG-Pub-Main-ASG**
* Key pair: **Create a new key pair**: `awskey2`

**Create EC2 in Branch Office network**

* AMI: **Amazon Linux 2**
* Instance type: **t2.medium**
* Network: **VPC-Bra-ASG**
* Subnet: **Pubsub-Bra-ASG**
* Auto-assign Public IP: **Enable**
* Security group: **SG-Pub-Bra-ASG**
* Key pair: **Create a new key pair**: `awskey2`
 

#### 3. Configure Site to Site VPN at Main office

**Create Virtual Private Gateway at Main Office**
* Open the Amazon VPC console at https://console.aws.amazon.com/vpc/ 
* Chọn **Virtual Private Gateway**
* Chọn **Create VPG**
* Name: **VPG-MainBranch-ASG**
 
**Create & configure Customer Gateway at Main Office**

* Name: **CGW-MainBranch-ASG**
* Routing: **Static**
* IP Address: **13.56.76.108** 
 
**Creat & configure Site-to-Site connection at Main Office**

* Name: **VPN-MainBranch-ASG**
* Virtual Private Gateway: **VPG-MainBranch-ASG**
* Customer Gateway: Exist (**CGW-MainBranch-ASG**)
* Routing Options: **Static**
* IP Prefixes: `10.2.0.0/16` 
 
Enable propagate for Route table of VPC-Main-ASG: 
* Open the Amazon VPC console at https://console.aws.amazon.com/vpc/
* Choose Main Office Route table: **RT-Main-ASG**
* Click **Route Propagation**, **Edit Route Propagation**
* Check **Enable, Yes**


#### 4. Configure Site to Site VPN at Branch office

On **EC2-Bra-ASG** do followings:

* Install OpenSwan
```javascript
sudo su
yum install openswan -y
```

* Configure /etc/ipsec.conf
```javascript
include /etc/ipsec.d/*.conf
```

* Configure /etc/sysctl.conf
```javascript
net.ipv4.ip_forward = 1
net.ipv4.conf.all.accept_redirects = 0
net.ipv4.conf.all.send_redirects = 0
```

* Configure /etc/ipsec.d/aws-vpn.conf 
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
**Note:**  
```text
leftid: IP Public Address of Branch office.
right: IP Public Address of AWS VPN Tunnel
leftsubnet: CIDR from Branch site 
rightsubnet: CIDR from Main site
```

* Configure /etc/ipsec.d/aws-vpn.secrets
```javascript
13.56.76.108 52.53.71.108: PSK "vYiouHnJ1Q2itTl9NCy8zSuOOWciVmz2"
```

* Restart Network service & IPSEC service
```javascript
service network restart 
chkconfig ipsec on
service ipsec start
service ipsec status
```

![VPN Information](/images/2-vpc/VPN-MainBranch-ASG.png?width=90pc)

#### 5. Test ping from Pub-Linux to Pub-Linux-Bra and vice versa

![Testing Ping between Subnets](/images/2-vpc/Test-ping-PubLinux-to-PubLinuxBra.png?width=90pc)

![Testing Ping between Subnets](/images/2-vpc/Test-ping-PubLinuxBra-to-PubLinux.png?width=90pc)
