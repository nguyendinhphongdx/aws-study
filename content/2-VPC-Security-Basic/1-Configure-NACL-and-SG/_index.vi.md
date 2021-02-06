+++
title = "Cấu hình NACL & SG"
date = 2020
weight = 1
chapter = false
pre = "<b>2.1. </b>"
+++

Một security group hoạt động như một tường lửa ảo cho Instance, giúp kiểm soát lưu lượng truy cập. 
Một Instance trong VPC có thể được gán tối đa 5 Security group do SG chỉ hoạt động ở tầng Instance mà không họat động ở tầng Subnet. 

{{% notice note %}}
Security groups hoạt động ở mức máy ảo, không phải ở mức subnet. Tuy nhiên, mỗi máy ảo trong một subnet có thể được gán với nhiều bộ Security group khác nhau,
{{% /notice %}}

Danh sách kiểm soát truy cập mạng (ACL) là lớp bảo mật tùy chọn cho VPC, nó hoạt động như một tường lửa để kiểm soát lưu lượng ra và vào cho một hoặc nhiều subnet. 
Ta có thể thiết lập network ACL với các rule tương tự như security groups, nhằm bổ sung thêm một lớp bảo mật nữa cho VPC.

**Nội dung:**
- [Security groups](#security-groups)
- [Network ACLs cho VPC](#network-acls-cho-vpc)
