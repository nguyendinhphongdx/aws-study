---
title : "Giới thiệu"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 1. </b> "
---

#### Giới thiệu Amazon VPC

**Amazon Virtual Private Cloud (Amazon VPC)** là **“Đám mây Riêng tư Ảo”** là một mạng ảo tùy chỉnh nằm bên trong **AWS Cloud** và tách biệt với toàn bộ thế giới bên ngoài. Khái niệm này tương tự như việc thiết kế và triển khai một mạng độc lập riêng biệt hoạt động trong một trung tâm dữ liệu on-premise, loại hình vẫn còn rất phổ biến hiện nay tại nhiều nơi trên thế giới.

Bên trong VPC tùy chỉnh đó, bạn có toàn quyền kiểm kiểm soát môi trường mạng ảo của mình, nghĩa là vừa có khả năng khởi tạo và chạy các tài nguyên AWS, vừa có thể lựa chọn phạm vi địa chỉ IP, tạo các mạng con và cấu hình các bảng định tuyến và cổng kết nối mạng. Bạn có thể sử dụng cả IPv4 và IPv6 để truy cập an toàn và dễ dàng vào tài nguyên và ứng dụng trong VPC. 

**Region** là khái niệm mô tả nhiều cụm trung tâm dữ liệu cực lớn của AWS đặt tại một vùng lãnh thổ nhất định. Trong một region, ta có thể tạo ra nhiều VPC và mỗi VPC được phân biệt nhau bởi những dải không gian địa chỉ IP khác nhau. Ta chỉ định phạm vi địa chỉ IPv4 bằng cách lựa chọn một **Classless Inter-Domain Routing (CIDR)**, chẳng hạn như **10.0.0.0/16**. Phạm vi địa chỉ của Amazon VPC không thể thay đổi sau khi nó đã được tạo. Phạm vi địa chỉ Amazon VPC có thể lớn bằng /16 (tức 65536 địa chỉ khả dụng) hoặc nhỏ bằng /28 (tức 16 địa chỉ khả dụng) và chúng không được phép trùng với bất kỳ mạng nào khác mà chúng sẽ được kết nối tới.

Dịch vụ Amazon VPC được ra mắt sau dịch vụ Amazon EC2, vì vậy mà có thời điểm AWS cung cấp hai nền tảng mạng khác nhau đó là EC2-Classic và EC2-VPC. EC2-Classic là nền tảng mạng đầu tiên, trong đó tất cả Amazon EC2 được tạo ra đều nằm trong một mạng phẳng duy nhất, chia sẻ kết nối giữa các khách hàng của AWS. Cho tới tháng 12 năm 2013, AWS chỉ còn hỗ trợ EC2-VPC với VPC mặc định được tạo ra ở mỗi Region cùng một subnet mặc định với CIDR block có giá trị là 172.31.0.0/16.

#### Nội dung

- [Subnets](1.1-subnets/)
- [Route Table](1.2-routetable/)
- [Internet Gateway](1.3-internetgateway/)
- [NAT Gateway](1.4-natgateway/)

Bây giờ chúng ta sẽ cùng nhau đi qua các khái niệm cơ bản nhất của VPC nhé.
