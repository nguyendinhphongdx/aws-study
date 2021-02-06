+++
title = "Cấu hình VPC Flow Logs"
date = 2020
weight = 2
chapter = false
pre = "<b>2.2. </b>"
+++

Chúng ta có thể tạo Flow log cho VPC, subnets hoặc network interfaces. Flow log có thể đẩy dữ liệu lên CloudWatch Log hoặc Amazon S3.

**Nội dung:**
- [1. Đẩy Luồng dữ liệu nhật ký lên CloudWatch Logs](#1-đẩy-luồng-dữ-liệu-nhật-ký-lên-cloudwatch-logs)
- [2. Đẩy luồng dữ liệu nhật ký lên Amazon S3](#2-đẩy-luồng-dữ-liệu-nhật-ký-lên-amazon-s3)

#### 1. Đẩy Luồng dữ liệu nhật ký lên CloudWatch Logs

Khi đẩy dữ liệu lên CloudWatch Logs, luồng dữ liệu nhật ký sẽ được đưa vào log group, và mỗi network interface có một luồng dữ liệu nhật ký riêng biệt trong log group. 
Luồng nhật ký dữ liệu chứa các bản ghi dữ liệu nhật ký lưu lượng.
Bạn có thể tạo nhiều luồng dữ liệu nhật ký đẩy vào trong cùng một log group.
Nếu một network interface hiển thị trong một hoặc nhiều luồng dữ liệu nhật ký trong cùng một log group thì nó sẽ tạo ra một luồng dữ liệu nhật ký kết hợp.
Nếu đã có một luồng dữ liệu nhật ký chỉ bắt lưu lượng bị từ chối và luồng dữ liệu nhật ký khác chỉ bắt lưu lượng được chấp nhận, thì luồng dữ liệu nhật ký kết hợp sẽ bắt dữ liệu của cả 2 luồng lưu lượng trên.

> [**Đẩy Luồng dữ liệu nhật ký lên CloudWatch Logs**](1-publish-cw-logs/)

#### 2. Đẩy luồng dữ liệu nhật ký lên Amazon S3

Trường hợp bạn muốn đẩy dữ liệu nhất ký lên Amazon S3, thì cẩn đảm bảo bucket đã được tạo sẵn trước khi sử dụng.
Các bản ghi dữ liệu nhật ký liên quan tới network interfaces sẽ được lưu trữ thành một loạt các object trên bucket. 
Trường hợp là dữ liệu nhật ký hoạt động của VPC, luồng dữ liệu nhật kỹ sẽ đẩy dữ liệu của toàn bộ the network interfaces trong VPC đó lên bucket.

> [**Publishing flow logs to Amazon S3**](2-publish-s3/)
