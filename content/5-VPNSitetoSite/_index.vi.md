---
title : "Cấu hình Site to Site VPN"
date :  "`r Sys.Date()`" 
weight : 5 
chapter : false
pre : " <b> 5. </b> "
---

Chúng ta có thể kết nối từ Trung tâm dữ liệu tại chỗ  (On-premise) đến Amazon VPC sử dụng VPN cứng hoặc mềm tùy thuộc mục đích và nhu cầu sử dụng thực tế.  
Để thiết lập kết nối VPN Site to Site, chúng ta sẽ cần tạo và cấu hình Virtual Private Gateway (**VPG**) và Customer Gateway (**CGW**).  

* Virtual Private Gateway (**VPG**) là trung tâm điều khiển kết nối VPN (virtual private network, tạm dịch: mạng riêng ảo) được cài đặt ở phía AWS.  
* Customer Gateway (**CGW**) là thành phần đại diện cho thiết bị VPN cứng hoặc mềm được cài đặt ở phía Khách hàng.

**VPN tunnel**  sẽ được thiết lập ngay sau khi lưu lượng dữ liệu được truyền tải giữa AWS và hệ thống mạng của khách hàng. Trong kết nối đó, ta phải chỉ rõ loại định tuyến sẽ được sử dụng để đảm bảo an toàn cũng như chất lượng về mặt truyền tải dữ liệu.  
Nếu CGW ở phía khách hàng có hỗ trợ Border Gateway Protocol (BGP), thì trong cấu hình VPN connection ta bắt buộc phải đặt định tuyến là dynamic routing.  
Ngược lại, ta phải cấu hình định tuyến kết nối là static routing. Trường hợp sử dụng static routing, ta phải nhập chính xác những định tuyến cần thiết cho việc kết nối từ phía Khách hàng tới VPG được thiết lập ở phía AWS. Đồng thời, định tuyến cho VPC cũng phải được cấu hình propagated để cho phép các tài nguyên có thể trao đổi dữ liệu đi vào và đi ra trong kết nối VPN tunnel giữa AWS với hệ thống mạng của Khách hàng.

Amazon VPC cung cấp nhiều loại CGWs, và mỗi CGW được gán với một VPG nhưng một VPG có thể kết hợp với nhiều CGW (many-to-one design). Để hỗ trợ mô hình này thì địa chỉ IP của CGW phải là duy nhất trong một region.  
Amazon VPC cũng cung cấp các thông tin cần thiết cho Nhân viên quản trị mạng (Network Administrators) có thể cấu hình CGW và thiết lập kết nối VPN tới VPG trên AWS. Kết nối VPN luôn bao gồm hai Internet Protocol Security (IPSec) tunnels để đảm bảo tính sẵn sàng cao của kết nối.  
Bên dưới là những đặc điểm quan trọng mà ta cần nắm về **VPG**, **CGW** và **VPN**:
* VPG là thành phần đầu cuối của VPN tunnel nằm trên AWS.
* CGW có thể là thiết bị phần cứng hoặc ứng dụng phần mềm nằm ở phía Khách hàng trong kết nối VPN tunnel.
* Bạn phải khởi tạo kết nối VPN tunnel từ CGW tới VPG.
* VPG hỗ trợ cả dynamic routing (BGP) và static routing.
* Kết nối VPN luôn có hai tunnels nhằm đảm bảo tính sẵn sàng cao cho kết nối với VPC từ phía Khách hàng.

Bài lab giúp chúng ta học được cách thiết lập một kết nối Site to Site VPN trong AWS. Trong thực tế, giải pháp này khá được ưa chuộng do ưu điểm giá thành rẻ, đồng thời rất dễ cấu hình do AWS cung cấp hướng dẫn cho từng loại thiết bị phía Khách hàng. Việc duy nhất  Khách hàng bận tâm đó là chuẩn bị đường internet để từ đó tạo đường hầm an toàn bí mật (sử dụng IPSec) kết nối tới AWS thông qua AWS VPN tunnel.  
Trong phạm vi bài lab, giả lập rằng chúng ta có Main office (VPC **ASG**) và Branch office (VPC **ASG VPN**) đặt tại hai VPC thuộc hai AZ khác nhau để có sự khác biệt về mặt network. Trên mỗi VPC thực hiện tạo EC2 cho phép SSH từ bên ngoài, nhưng không có khả năng kết nối và ping lẫn nhau sử dụng địa chỉ Private IP của mỗi EC2. Việc ta cần làm là cấu hình VPN để các địa chỉ Private IP có thể ping được lẫn nhau sử dụng Site-to-Site VPN.

![VPN](/images/6-VPNSitetoSite/vpn.png?featherlight=false&width=90pc)


**Nội dung:**
1. [Tạo **ASG VPN** VPC và subnet](5.1-createvpnenv/)
2. [Cấu hình Site to Site VPN và kiểm tra kết nối bằng private IP ](5.2-vpnsitetosite/)
