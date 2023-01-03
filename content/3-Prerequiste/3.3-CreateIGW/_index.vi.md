---
title : "Tạo Internet Gateway"
date :  "`r Sys.Date()`" 
weight : 3 
chapter : false
pre : " <b> 3.3 </b> "
---

#### Tạo Internet Gateway

1. Trong giao diện **VPC**

- Chọn **Internet gateways**
- Chọn **Create internet gateway**

![Create VPC](/images/3-Prerequiste/3.2-igwandroutetable/0001-igwandroutetable.png?featherlight=false&width=90pc)

2. Thực hiện cấu hình

- **Name tag**: nhập `Internet Gateway`
- Chọn **Create internet gateway**

![Create VPC](/images/3-Prerequiste/3.2-igwandroutetable/0002-igwandroutetable.png?featherlight=false&width=90pc)

3. Hoàn thành tạo **Internet Gateway**

![Create VPC](/images/3-Prerequiste/3.2-igwandroutetable/0003-igwandroutetable.png?featherlight=false&width=90pc)

4. Thực hiện **Attach VPC**

- Chọn **Actions**
- Chọn **Attach to VPC**

![Create VPC](/images/3-Prerequiste/3.2-igwandroutetable/0004-igwandroutetable.png?featherlight=false&width=90pc)

5. Chọn **ASG**, VPC ID sẽ được tự động điền.

- Chọn **Attach intermet gateway**

![Create VPC](/images/3-Prerequiste/3.2-igwandroutetable/0005-igwandroutetable.png?featherlight=false&width=90pc)

6. Khi attach thành công, **State** của internet gateway được chuyển thành **Attached**

![Create VPC](/images/3-Prerequiste/3.2-igwandroutetable/0006-igwandroutetable.png?featherlight=false&width=90pc)


