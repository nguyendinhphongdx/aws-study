---
title : "Dọn dẹp tài nguyên"
date :  "`r Sys.Date()`" 
weight : 6
chapter : false
pre : " <b> 6. </b> "
---
#### Dọn dẹp tài nguyên

Chúng ta sẽ tiến hành xóa các tài nguyên theo thứ tự sau

### Terminate các EC2 Instance.

- Truy cập Amazon EC2 console tại địa chỉ https://console.aws.amazon.com/ec2/.
- Trên thanh điều hướng bên trái, chọn Intances
- Chọn tất cả EC2 Instance liên quan tới bài lab.
- Click Actions.
- Click Manage Instance State.
- Chọn Terminate.
- Click Change State

![Create VPC](/images/7-cleanup/0001-cleanup.png?featherlight=false&width=90pc)

#### Xóa NAT Gateway và Elastic IP Address

- Xóa NAT Gateway và Elastic IP Address. AWS sẽ thu tiền cho các EIP lãng phí nên bạn cần kiểm tra kỹ để tránh bị trừ chi phí ngoài ý muốn.
- Truy cập trang Amazon VPC console tại địa chỉ https://console.aws.amazon.com/vpc/
- Trên thanh điều hướng bên trái , click NAT Gateway.
- Chọn NAT Gateway.
- Click Action.
- Click Delete NAT Gateway.
- Gõ delete.
- Click Delete để xác nhận xóa NAT Gateway


![Create VPC](/images/7-cleanup/0002-cleanup.png?featherlight=false&width=90pc)

#### Xóa xóa Elastic IP Address.
- Tiếp tục xóa Elastic IP Address.
- Truy cập trang Amazon VPC console tại địa chỉ https://console.aws.amazon.com/vpc/
- Trên thanh điều hướng bên trái , click Elastic IP.
- Chọn Elastic IP Address chúng ta đã tạo.
- Click Action.
- Click Release Elastic IP Address
- Click Release.

![Create VPC](/images/7-cleanup/0003-cleanup.png?featherlight=false&width=90pc)

#### Tiếp tục làm tương tự và xóa theo thứ tự sau nhé:
- VPN Site to Site connection.
- Virtual Private Gateway.
- Customer Gateway.
- VPC ASG VPN
- VPC ASG.