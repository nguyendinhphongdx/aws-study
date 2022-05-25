---
title : "Create VPN Connection"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 5.2.3 </b> "
---

#### Create VPN connection

1. Access **VPC**

- Select **Site-to-Site VPN Connections**
- Select **Create VPN Connection**

![Create VPC](/images/6-VPNSitetoSite/6.3-vpnconnect/0001-vpnconnect.png?featherlight=false&width=90pc)

2. In the **Create VPN Connection** interface

- **Name tag**, enter **```VPN Connection```**
- **Target Gateway Type**: Select **Virtual Private Gateway**
- **Virtual Private Gateway**: Select **VPN Gateway**
- **Customer Gateway**: **Existing**
- **Customer Gateway ID**: Select **Customer Gateway**

![Create VPC](/images/6-VPNSitetoSite/6.3-vpnconnect/0002-vpnconnect.png?featherlight=false&width=90pc)

3. Continue to perform configuration

- **Routing Options**: **Static**
- **Static IP Prefixes**: **10.11.0.0/16**. This is the IP address resolution in a simulated On-premise environment.
- The other configurations keep the default.

![Create VPC](/images/6-VPNSitetoSite/6.3-vpnconnect/0003-vpnconnect.png?featherlight=false&width=90pc)

4. Select **Create VPN Connection**

![Create VPC](/images/6-VPNSitetoSite/6.3-vpnconnect/0004-vpnconnect.png?featherlight=false&width=90pc)

5. Wait about 5 minutes, finish creating **VPN Connection**


![Create VPC](/images/6-VPNSitetoSite/6.3-vpnconnect/0005-vpnconnect.png?featherlight=false&width=90pc)

6. Configure **propagation** for **route tables**

- In the **VPC** interface, select **Route Tables**
- Select **Route table - Public**
- Select **Route Propagation**
- Select **Edit route propagation**

![Create VPC](/images/6-VPNSitetoSite/6.3-vpnconnect/0006-vpnconnect.png?featherlight=false&width=90pc)

7. In the **Edit route propagation** interface

- Select **Enable**
- Select **Save**

![Create VPC](/images/6-VPNSitetoSite/6.3-vpnconnect/0007-vpnconnect.png?featherlight=false&width=90pc)

8. Complete and recheck **Route Propagation** has changed to **Yes**

![Create VPC](/images/6-VPNSitetoSite/6.3-vpnconnect/0008-vpnconnect.png?featherlight=false&width=90pc)