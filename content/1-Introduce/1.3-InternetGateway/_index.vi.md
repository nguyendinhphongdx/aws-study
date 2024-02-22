---
title : "Internet Gateway"
date :  "`r Sys.Date()`" 
weight : 3 
chapter : false
pre : " <b> 1.3 </b> "
---


#### Internet Gateway

- **Internet Gateway (IGW)** là một thành phần Amazon VPC giúp các tài nguyên bên trong VPC, cụ thể là EC2, có khả năng giao tiếp với Internet. IGW có khả năng co giãn mạnh theo chiều ngang, đồng thời tính dự phòng và sẵn sàng cao. Nó hoạt động như một target trong bảng định tuyến của Amazon VPC, giúp lưu lượng truy cập được định tuyến ra ngoài Internet bằng cách biên dịch địa chỉ mạng của EC2 thành địa chỉ Public IP đã được gán cho nó.  

- Cụ thể hơn, các EC2 Instance bên trong VPC chỉ biết các địa chỉ Private IP được gán cho nó, nhưng khi có lưu lượng được gửi từ EC2 ra ngoài Internet, IGW sẽ biên dịch địa chỉ Private IP đó thành địa chỉ Public IP (hoặc địa chỉ EIP, sẽ thảo luận sau) mà gán với EC2, và duy trì mapping 1-1 cho tới khi địa chỉ Public IP bị release. 

- Khi EC2 nhận được lưu lượng truy cập từ bên ngoài Internet, IGW sẽ thực hiện dịch địa chỉ Target (địa chỉ Public IP) thành địa chỉ Private IP của EC2 Instance và chuyển tiếp lưu lượng truy cập đến Amazon VPC.

![Internet Gateway](/images/1-Introduce/igw.png?featherlight=false&width=60pc)