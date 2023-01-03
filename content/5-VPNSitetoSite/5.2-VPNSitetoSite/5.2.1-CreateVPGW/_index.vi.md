---
title : "Tạo Virtual Private Gateway"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 5.2.1 </b> "
---

#### Tạo Virtual Private Gateway

1. Truy cập vào **VPC**

- Chọn **Virtual private gateways**
- Chọn **Create virtual private gateway**

![Create VPC](/images/6-VPNSitetoSite/6.1-vpgw/0001-vpgw.png?featherlight=false&width=90pc)

2. Trong giao diện **Create virtual private gateway**

- **Name tag**: nhập `VPN Gateway`
- Chọn **Amazon default ASN**
- Chọn **Create virtual private gateway**

![Create VPC](/images/6-VPNSitetoSite/6.1-vpgw/0002-vpgw.png?featherlight=false&width=90pc)

3. Chúng ta cần thực hiện **Attach to VPC**

- Chọn **Actions**
- Chọn **Attach to VPC**

![Create VPC](/images/6-VPNSitetoSite/6.1-vpgw/0003-vpgw.png?featherlight=false&width=90pc)

4. Trong giao diện **Attach to VPC**

- Chọn **VPC ASG.**
- Chọn **Attach to VPC**


![Create VPC](/images/6-VPNSitetoSite/6.1-vpgw/0004-vpgw.png?featherlight=false&width=90pc)

5. Khi trạng thái của **State** là **Attached** nghĩa là việc tạo Virtual private gateway đã hoàn tất.

![Create VPC](/images/6-VPNSitetoSite/6.1-vpgw/0005-vpgw.png?featherlight=false&width=90pc)