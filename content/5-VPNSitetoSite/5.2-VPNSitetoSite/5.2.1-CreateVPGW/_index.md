---
title : "Create Virtual Private Gateway"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 5.2.1 </b> "
---

#### Create Virtual Private Gateway

1. Access to **VPC**

- Select **Virtual Private Gateway**
- Select **Create Virtual Private Gateway**

![Create VPC](/images/6-VPNSitetoSite/6.1-vpgw/0001-vpgw.png?featherlight=false&width=90pc)

2. In the **Create Virtual Private Gateway** interface

- **Name tag**, enter **```VPN Gateway```**
- Select **Amazon default ASN**
- Select **Create virtual private gateway**

![Create VPC](/images/6-VPNSitetoSite/6.1-vpgw/0002-vpgw.png?featherlight=false&width=90pc)

3. We need to implement **Attach to VPC**

- Select **Actions**
- Select **Attach to VPC**

![Create VPC](/images/6-VPNSitetoSite/6.1-vpgw/0003-vpgw.png?featherlight=false&width=90pc)

4. In the **Attach to VPC** interface

- Select **VPC ASG.**
- Select **Attach to VPC**


![Create VPC](/images/6-VPNSitetoSite/6.1-vpgw/0004-vpgw.png?featherlight=false&width=90pc)

5. Finish and see **State** as **Attached**

![Create VPC](/images/6-VPNSitetoSite/6.1-vpgw/0005-vpgw.png?featherlight=false&width=90pc)