+++
title = "Setup Site to Site VPN"
date = 2020
weight = 2
chapter = false
pre = "<b>2.3.2. </b>"
+++

![Lab Diagram](/images/3/0.png)

**Contents:**
- [1. Configure Site to Site VPN](#1-configure-site-to-site-vpn)
- [2. Configure Site-to-Site VPN cho Customer Gateway (OpenSwan)](#2-configure-site-to-site-vpn-cho-customer-gateway-openswan)

#### 1. Configure Site to Site VPN

**Create Virtual Private Gateway**

1. Open the Amazon VPC console at https://console.aws.amazon.com/vpc/, click **Virtual Private Gateway**

![Network](/images/3/8.png?width=90pc)

2. Click **Create VPG**
   - **Name tag:** ```VPNGateway```

![Network](/images/3/9.png?width=90pc)

3. In **Virtual Private Gateway** list, select created **VPNGateway**.
4. Click **Actions** > **Attach to VPC**.
5. Select previous created VPC. (**LabVPC**)

![Network](/images/3/10.png?width=90pc)

**Create & configure Customer Gateway**

1. Go to **Amazon VPC console** at https://console.aws.amazon.com/vpc/, navigate to **Customer Gateway**.

![Network](/images/3/11.png?width=90pc)

2. Click **Create Customer Gateway** and create a new Customer Gateway with below information:
   - **Name:** ```CustomerGateway```
   - **Routing:** Static
   - **IP Address:** Public IP of your On-premise Gateway.
3. Click **Create Customer Gateway**.

![Network](/images/3/12.png?width=90pc)

**Create & configure Site-to-Site VPN Connection on AWS**

1. Go to **Amazon VPC console** at https://console.aws.amazon.com/vpc/, navigate to **Site-to-Site VPN Connections**.
2. Click **Create VPN Connection** and create a new connection with below information:
   * **Name tag:** ```VPNConnection```
   * **Target Gateway Type:** Select **Virtual Private Gateway**
   * **Virtual Private Gateway:** Select **VPNGateway**
   * **Customer Gateway:** Existing
   * **Customer Gateway ID:**	Select **CustomerGateway**
   * **Routing Options:** Static
   * **Static IP Prefixes:** ```10.28.0.0/16``` 
   * Keep other settings as default.
3. Click **Create VPN Connection**.

**Config Route Table**

1. Go to **Amazon VPC console** at https://console.aws.amazon.com/vpc/, navigate to **Route Tables**.
2. Select **Route Table** in **LabVPC**.
3. Select tab **Route Propagation**
4. Select **Edit route propagation**
5. Tick on **Propagate**
6. Click **Save** to save Route Table.

![Network](/images/3/13.png?width=90pc)

#### 2. Configure Site-to-Site VPN cho Customer Gateway (OpenSwan)

To download the configuration for VPN Connection on On-premise site. You can download the template provide from AWS:
1. Go to **Amazon VPC console** at https://console.aws.amazon.com/vpc/, navigate to **Site-to-Site VPN Connection**.
2. Select the created **VPN Connection** in the list, then click **Download Configuration**.
3. In **Download Configuration** dialog, select your suitable appliance. In this lab, we use OpenSwan:
   - **Vendor:**	Select **OpenSwan**
   - **Platform:**	Select **OpenSwan**
   - **Software:**	Select **OpenSwan 2.6.38+**

![Network](/images/3/17.png?width=90pc)

Then you can base on this template to change some configuration that suitable for you and apply it to your Appliance.

* Install OpenSwan
```bash
sudo su
yum install openswan -y
```

![Network](/images/3/14.png?width=90pc)

* Configure ```/etc/ipsec.conf```
Remove the # from the configuration line.
```bash
include /etc/ipsec.d/*.conf
```

* Configure ```/etc/sysctl.conf```
Add below configuration to the file.
```bash
net.ipv4.ip_forward = 1
net.ipv4.conf.all.accept_redirects = 0
net.ipv4.conf.all.send_redirects = 0
```

![Network](/images/3/15.png?width=90pc)

To apply the configuration, run below command:
```bash
sysctl -p
```

* Configure ```/etc/ipsec.d/aws-vpn.conf```
Add the connfiguration to config file. We will define 2 Tunnel with the information from downloaded configuration.

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
**Note:**  
```text
leftid: IP Public Address of Onprem System.
right: IP Public Address of AWS VPN Tunnel.
leftsubnet: CIDR of Local Network behind OpenSwan.
rightsubnet: CIDR of AWS Subnets.
```

![Network](/images/3/16.png?width=90pc)

* Configure /etc/ipsec.d/aws-vpn.secrets
Create the new file and add the configuration for authentication of 2 Tunnels.
```bash
54.249.66.228 13.114.170.224: PSK "ZQbWr7AXk1Wy4hv1Y3KcO4orck.DiQz0"
54.249.66.228 52.198.189.177: PSK "8OrmB8M6Jspp6v1yv0ekd89oFM3dNAyC"
```

![Network](/images/3/18.png?width=90pc)

* Restart Network service & IPSEC service
```bash
service network restart 
chkconfig ipsec on
service ipsec start
service ipsec status
```

![Network](/images/3/19.png?width=90pc)
