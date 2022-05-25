---
title : "Tạo VPC"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 3.1 </b> "
---

#### Tạo VPC

1. Truy cập giao diện **AWS Management Console**

- Tìm **VPC**
- Chọn **VPC**

![Create VPC](/images/3-Prerequiste/3.1-vpcandsubnet/0001-createvpcandsubnet.png?featherlight=false&width=90pc)

2. Trong giao diện **VPC**

- Chọn **Yours VPC**
- Chọn **Create VPC**

![Create VPC](/images/3-Prerequiste/3.1-vpcandsubnet/0002-createvpcandsubnet.png?featherlight=false&width=90pc)

3. Tiến hành các bước tạo VPC

- **Resource**, chọn **VPC only**
- **Name tag**, nhập **```ASG```**
- **IPv4 CIDR**, nhập **``10.10.0.0/16``**

![Create VPC](/images/3-Prerequiste/3.1-vpcandsubnet/0003-createvpcandsubnet.png?featherlight=false&width=90pc)

{{% notice warning %}}
Phần cấu hình **Tennacy** chúng ta sẽ để ở cơ chế mặc định. Nếu chúng ta chuyển sang Dedicated sẽ có một số **EC2 Instance type** không phù hợp và sẽ không tạo được trong VPC với **tennacy mode** là **Dedicate**
{{% /notice %}}

4. Chọn **Create VPC**

![Create VPC](/images/3-Prerequiste/3.1-vpcandsubnet/0004-createvpcandsubnet.png?featherlight=false&width=90pc)

5. Hoàn thành tạo **VPC** 

![Create VPC](/images/3-Prerequiste/3.1-vpcandsubnet/0005-createvpcandsubnet.png?featherlight=false&width=90pc)

