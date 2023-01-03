---
title : "Tạo Route Table"
date :  "`r Sys.Date()`" 
weight : 4 
chapter : false
pre : " <b> 3.4 </b> "
---

#### Tạo Route Table để định tuyến đi ra internet thông qua Internet Gateway.


1. Trong giao diện **VPC**

- Chọn **Route tables**
- Chọn **Create route table**

![Create VPC](/images/3-Prerequiste/3.2-igwandroutetable/0007-igwandroutetable.png?featherlight=false&width=90pc)

2. Tiến hành cấu hình **Route table**

- **Name**: nhập `Route table-Public`
- **VPC**: chọn **ASG** VPC. VPC ID sẽ được tự động điền vào.
- Chọn **Cretae route table**

![Create VPC](/images/3-Prerequiste/3.2-igwandroutetable/0008-igwandroutetable.png?featherlight=false&width=90pc)

3. Hoàn thành tạo **Route table**

![Create VPC](/images/3-Prerequiste/3.2-igwandroutetable/0009-igwandroutetable.png?featherlight=false&width=90pc)

4. Thực hiện **Edit route**

- Chọn **Actions**
- Chọn **Edit routes**

![Create VPC](/images/3-Prerequiste/3.2-igwandroutetable/00010-igwandroutetable.png?featherlight=false&width=90pc)

5. Trong giao diện **Edit routes**

- Chọn **Add route**
- Kế đến điền vào phần **Destination CIDR**: `0.0.0.0/0` đại diện cho Internet.
- Trong phần **Target**  chọn **Internet Gateway**, sau đó chọn **Internet Gateway** chúng ta đã tạo. **Internet Gateway ID** sẽ được tự động điền.
- Chọn **Save changes**

![Create VPC](/images/3-Prerequiste/3.2-igwandroutetable/00011-igwandroutetable.png?featherlight=false&width=90pc)

6. Hoàn thành và kiểm tra lại **Routes**

![Create VPC](/images/3-Prerequiste/3.2-igwandroutetable/00012-igwandroutetable.png?featherlight=false&width=90pc)

7. Đảm bảo **Route table - Public** đang được chọn.

- Chọn tab **subnet associations**
- Chọn **Edit subnet associations**

![Create VPC](/images/3-Prerequiste/3.2-igwandroutetable/00013-igwandroutetable.png?featherlight=false&width=90pc)

8. Trong bước **Edit subnet associations**

- Mở rộng cột **Subnet ID** bằng cách kéo thanh ngăn sang phải.
- Chọn đúng **Public subnet 1** và **Public subnet 2** chúng ta đã tạo.
- Chọn **Save associations**

![Create VPC](/images/3-Prerequiste/3.2-igwandroutetable/00014-igwandroutetable.png?featherlight=false&width=90pc)

9. Hoàn thành và kiểm tra lại **Subnet associations**

![Create VPC](/images/3-Prerequiste/3.2-igwandroutetable/00015-igwandroutetable.png?featherlight=false&width=90pc)
