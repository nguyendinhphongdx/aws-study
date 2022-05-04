+++
title = "Route Table"
date = 2022
weight = 2
chapter = false
pre = "<b>1.2 </b>"
+++

#### Route Table

Route Table hay còn gọi là bảng định tuyến, cung cấp hướng dẫn định tuyến và được gán vào các Subnets. 
Ví dụ khi bạn tạo VPC với lớp mạng 10.10.0.0/16, cùng 2 subnet 10.10.1.0/24,10.10.2.0/24 thì mỗi subnets mặc định sẽ được gán 1 default route table.\
Bên trong route table sẽ có route entry **destination**:10.10.0.0/16 **target**:local. Route entry này thể hiện các tài nguyên tạo ra trong cùng 1 VPC có thể kết nối với nhau.

![Route Table](/images/architecture/routetable.png?width=40pc)