+++
title = "Dọn dẹp tài nguyên  "
date = 2021
weight = 5
chapter = false
pre = "<b>5. </b>"
+++

Chúng ta sẽ tiến hành xóa các tài nguyên theo thứ tự sau 

1. Terminate các EC2 Instance.
  + Truy cập Amazon EC2 console tại địa chỉ https://console.aws.amazon.com/ec2/.
  + Trên thanh điều hướng bên trái, chọn **Intances**
  + Chọn tất cả EC2 Instance liên quan tới bài lab.
  + Click **Actions**.
  + Click **Manage Instance State**.
  + Chọn **Terminate**.
  + Click **Change State**
 ![Configure VPN](/images/vpn/clean2.png?width=90pc)

2. Xóa NAT Gateway và Elastic IP Address. AWS sẽ thu tiền cho các EIP lãng phí nên bạn cần kiểm tra kỹ để tránh bị trừ chi phí ngoài ý muốn.
  + Truy cập trang **Amazon VPC console** tại địa chỉ https://console.aws.amazon.com/vpc/
  + Trên thanh điều hướng bên trái , click **NAT Gateway**.
  + Chọn **NAT Gateway**.
  + Click **Action**.
  + Click **Delete NAT Gateway**.
  + Gõ **delete**.
  + Click **Delete** để xác nhận xóa **NAT Gateway**.
![Configure VPN](/images/vpn/clean3.png?width=90pc)

3. Tiếp tục xóa Elastic IP Address.
  + Truy cập trang **Amazon VPC console** tại địa chỉ https://console.aws.amazon.com/vpc/
  + Trên thanh điều hướng bên trái , click **Elastic IP**.
  + Chọn Elastic IP Address chúng ta đã tạo.
  + Click **Action**.
  + Click **Release Elastic IP Address**.
![Configure VPN](/images/vpn/clean4.png?width=90pc)
  + Click **Release**.

4. Tiếp tục làm tương tự và xóa theo thứ tự sau nhé:
 + VPN Site to Site connection.
 + Virtual Private Gateway.
 + Customer Gateway.
 + VPC **ASG VPN**.
 ![Configure VPN](/images/vpn/clean5.png?width=90pc)
 + VPC **ASG**.