---
title : "Subnets"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 1.1 </b> "
---

#### Subnets

**Subnet** là một phân đoạn của **dải địa chỉ IP** mà bạn sử dụng khi **khởi tạo Amazon VPC**, cung cấp trực tiếp dải mạng hoạt động cho các tài nguyên AWS có thể chạy bên trong nó như **Amazon EC2, Amazon RDS (CSDL Quan hệ Amazon),...** Các subnet cũng được xác định thông qua **CIDR block** (ví dụ: **10.0.1.0/24** và **192.168.0.0/24**) và bắt buộc các **CIDR của subnet** phải nằm trong **CIDR của VPC**. **Subnet nhỏ nhất** có thể tạo được là **/28** (16 địa chỉ IP). AWS lưu trữ **4 địa chỉ IP đầu tiên và 1 địa chỉ IP cuối cùng** của mỗi subnet cho các mục đích kết nối mạng nội bộ. Ví dụ: subnet  /28 có 16 địa chỉ IP khả dụng, nhưng loại bỏ 5 reserved IP cho AWS, như vậy còn lại 11 địa chỉ IP có thể sử dụng cho các tài nguyên hoạt động bên trong subnet này.

**Availability Zone** hay được viết tắt thành AZ là **một trung tâm dữ liệu con**, nằm bên trong Region và được xác định dựa treo vị trí địa lý. Bên trong AZ có thể có một hoặc nhiều subnet, nhưng một subnet chỉ có thể nằm trong duy nhất một AZ mà không thể mở rộng sang AZ khác.  

Các subnet được chia thành các loại như **Public**, **Private**, hoặc **VPN-only**. 
+ **Public subnet** là subnet có route table (sẽ thảo luận sau) điều hướng lưu lượng truy cập bên trong subnet đi tới VPC IGW (cũng sẽ thảo luận sau) 
+ **Private subnet** thì ngược lại với Public subnet, nó không có route table điều hướng lưu lượng truy cập tới VPC IGW. 
+ **VPN-only subnet** là subnet mà có route table điều hướng lưu lượng truy cập tới VPG của Amazon VPC (sẽ thảo luận sau). 

Bất kể loại mạng con nào, dải địa chỉ IP nội bộ của subnet luôn là private (nghĩa là từ bên ngoài Internet không thể kết nối trực tiếp tới các địa chỉ thuộc dải này).


![Subnets](/images/1-Introduce/subnet.png?featherlight=false&width=50pc)
