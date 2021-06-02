+++
title = "Tạo kết nối VPN"
date = 2021
weight = 3
chapter = false
pre = "<b>4.2.3 </b>"
+++


1. Truy cập trang **Amazon VPC console** tại địa chỉ https://console.aws.amazon.com/vpc/
  + Kéo thanh điều hướng xuống dưới.
  + Click **Site-to-Site VPN Connections**.
  + Click **Create VPN Connection**.
![Create CGW](/images/vpn/create-vpn1.png?width=90pc)

2. Tại trang **Create VPN Connection** điền thông tin:
  + **Name tag:** ```VPN Connection```
  + **Target Gateway Type:** Chọn **Virtual Private Gateway**
  + **Virtual Private Gateway:** Chọn **VPN Gateway**
  + **Customer Gateway:** Existing
  + **Customer Gateway ID:**	Chọn **Customer Gateway**
  + **Routing Options:** Static
  + **Static IP Prefixes:** ```10.11.0.0/16```. Đây là giải địa chỉ IP ở môi trường Onpremise giả lập.
  + Các cấu hình khác giữ nguyên mặc định.
![Create CGW](/images/vpn/create-vpn2.png?width=90pc)
  + Kéo màn hình xuống dưới cùng và click **Create VPN Connection**.
  + Click **Close** sau khi VPN Connection đã được tạo thành công.

3. Cấu hình propagation cho các route table
  + Truy cập trang **Amazon VPC console** tại địa chỉ https://console.aws.amazon.com/vpc/
  + Tại menu bên trái, click **Route Tables**.
  + Click chọn route table **Route table - Public**.
  + Click tab **Route Propagation**.
  + Click **Edit route propagation**.
![Create CGW](/images/vpn/create-vpn3.png?width=90pc)
  + Click vào ô **Enable**.
  + Click **Save** để lưu Route table.
![Create CGW](/images/vpn/create-vpn4.png?width=90pc)
4. Làm tương tự bước 3 cho **Route table - Private**. Kiểm tra trạng thái Propagation là **Yes** như hình dưới.
![Create CGW](/images/vpn/create-vpn5.png?width=90pc)

5. Tiếp theo chúng ta sẽ tiến hành cài đặt và cấu hình Customer Gateway software sử dụng Open Swan.