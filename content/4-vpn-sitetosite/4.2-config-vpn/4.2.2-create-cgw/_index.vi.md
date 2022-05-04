+++
title = "Tạo Customer GW"
date = 2022
weight = 2
chapter = false
pre = "<b>4.2.2 </b>"
+++


1. Truy cập trang **Amazon VPC console** tại địa chỉ https://console.aws.amazon.com/vpc/.
  + Kéo xuống phía dưới thanh điều hướng bên phải.
  + Click **Customer Gateways**.
  + Click **Create Customer Gateway**.
![Create CGW](/images/vpn/create-cgw.png?width=90pc)

2. Tại trang **Create Customer Gateway**:
  + Điền thông tin **Name :** ```Customer Gateway```
  + Tại mục **Routing** bảo đảm cấu hình đang để mặc định là **static**.
  + Tại mục **IP Address** điền public IP address của máy chủ EC2 **Customer Gateway**.
  + Click **Create Customer Gateway**.
![Create EC2](/images/vpn/create-vpnec24.png?width=90pc)
![Create CGW](/images/vpn/create-cgw2.png?width=90pc)
  + Click **Close**.

{{%notice tip%}}
Lưu ý: theo như mô hình kiến trúc, Customer Gateway sẽ nằm ở VPC trên môi trường onpremise. Hiện tại chúng ta đang làm là khai báo với AWS rằng chúng ta sẽ có 1 Customer Gateway với địa chỉ IP public là địa chỉ public của EC2 instance : **Customer Gateway** nằm trong **ASG VPN** VPC.
{{%/notice%}}



