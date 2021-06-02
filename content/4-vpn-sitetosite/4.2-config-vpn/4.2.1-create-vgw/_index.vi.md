+++
title = "Tao Virtual Private GW"
date = 2021
weight = 1
chapter = false
pre = "<b>4.2.1 </b>"
+++


1. Truy cập trang **Amazon VPC console** tại địa chỉ https://console.aws.amazon.com/vpc/.
  + Kéo xuống phía dưới thanh điều hướng bên phải.
  + Click **Virtual Private Gateway**.
  + Click **Create Virtual Private Gateway**.
![Create VGW](/images/vpn/create-vgw.png?width=90pc)

2. Tại trang **Create Virtual Private Gateway**:
  + Điền thông tin **Name tag:** ```VPN Gateway```
  + Click **Create Virtual Private Gateway**
![Create VGW](/images/vpn/create-vgw2.png?width=90pc)
  + Click **Close**.

3. Trong danh sách các **Virtual Private Gateway**, chọn vào **VPN Gateway** vừa tạo.
  + Chọn **Actions** > **Attach to VPC**.
  + Chọn VPC **ASG**.
  + Click **Yes, Attach**.
![Create VGW](/images/vpn/create-vgw3.png?width=90pc)

{{%notice tip%}}
Lưu ý: theo như mô hình kiến trúc, Virtual Private Gateway sẽ nằm ở VPC trên môi trường AWS. Hiện tại chúng ta đang dùng VPC **ASG** để đóng vai trò môi trường AWS và VPC **ASG VPN** để giả lập môi trường on-premise.(Trung tâm dữ liệu truyền thống )
{{%/notice%}}



