---
title : "NAT Gateway"
date :  "`r Sys.Date()`" 
weight : 4 
chapter : false
pre : " <b> 1.4 </b> "
---

#### NAT Gateway

- Mặc định, mọi EC2 chạy bên trong Private subnet thì sẽ không có khả năng giao tiếp với Internet thông qua IGW. Từ đó vấn đề phát sinh khi EC2 đó cần truy cập ra ngoài Internet để áp dụng các bản cập nhật bảo mật, tải xuống các bản vá hoặc cập nhật phần mềm ứng dụng.  
Nắm bắt được nhu cầu đó, AWS cung cấp 2 phương thức cho phép các EC2 bên trong Private subnet có quyền được truy cập Internet, đó là NAT Instance và NAT Gateway. Với các trường hợp thông thường, thì ta nên sử dụng NAT Gateway thay cho NAT Instance. NAT Gateway đảm bảo tính sẵn sàng và băng thông cao hơn, đồng thời đòi hỏi ít nỗ lực quản trị hơn so với NAT Instance.

- Để tạo một một NAT gateway, ta phải chỉ định một mạng con (public) và một địa chỉ Elastic IP. Cần đảm bảo địa chỉ Elastic IP đang không được liên kết với bất cứ Instance hoặc một Network interface nào khác.

- Trường hợp ta muốn migrate từ NAT instance sang NAT gateway, ta có thể sử dụng lại địa chỉ Elastic IP của NAT instance. Nhưng trước hết ta cần phải tách địa chỉ IP ra khỏi NAT Instance. 


![NAT Gateway](/images/1-Introduce/natgw.png?featherlight=false&width=70pc)

{{%notice tip%}}
NAT Gateway và NAT instance đều không hỗ trợ traffic chiều vào trực tiếp từ internet.
{{%/notice%}}