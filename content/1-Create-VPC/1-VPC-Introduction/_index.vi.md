+++
title = "Giới thiệu VPC"
date = 2020
weight = 1
chapter = false
pre = "<b>1.1. </b>"
+++

Amazon Virtual Private Cloud (Amazon VPC) dịch theo nghĩa đen là “Đám mây Riêng tư Ảo” là một mạng ảo tùy chỉnh nằm bên trong AWS Cloud và tách biệt (hoặc cô lập) mạng Ảo của riêng bạn với toàn bộ thế giới bên ngoài. Khái niệm này tương tự như việc thiết kế và triển khai một mạng độc lập riêng biệt mà hoạt động trong một trung tâm dữ liệu on-premise, loại hình vẫn còn rất phổ biến hiện nay tại Việt Nam. 

Bên trong VPC tùy chỉnh đó, bạn có toàn quyền kiểm kiểm soát môi trường mạng ảo của mình, nghĩa là vừa có khả năng khởi tạo và chạy các tài nguyên AWS, vừa có thể lựa chọn phạm vi địa chỉ IP, tạo các mạng con và cấu hình các bảng định tuyến và cổng kết nối mạng. Bạn có thể sử dụng cả IPv4 và IPv6 để truy cập an toàn và dễ dàng vào tài nguyên và ứng dụng trong VPC. 

Region là khái niệm mô tả một trung tâm dữ liệu cực lớn của AWS đặt tại một vùng lãnh thổ nhất định. Trong một region, ta có thể tạo ra nhiều VPC và mỗi VPC được phân biệt nhau bởi những dải không gian địa chỉ IP khác nhau. Ta chỉ định phạm vi địa chỉ IPv4 bằng cách lựa chọn a Classless Inter-Domain Routing (CIDR), chẳng hạn như 10.0.0.0/16. Phạm vi địa chỉ của Amazon VPC không thể thay đổi sau khi nó đã được tạo. Phạm vi địa chỉ Amazon VPC có thể lớn bằng /16 (tức 65536 địa chỉ khả dụng) hoặc nhỏ bằng /28 (tức 16 địa chỉ khả dụng) và chúng không được phép trùng với bất kỳ mạng nào khác mà chúng sẽ được kết nối tới.

Dịch vụ Amazon VPC được ra mắt sau dịch vụ Amazon EC2, vì vậy mà có thời điểm AWS cung cấp hai nền tảng mạng khác nhau đó là EC2-Classic và EC2-VPC. EC2-Classic là nền tảng mạng đầu tiên, trong đó tất cả Amazon EC2 được tạo ra đều nằm trong một mạng phẳng duy nhất, chia sẻ kết nối giữa các khách hàng của AWS. Cho tới tháng 12 năm 2013, AWS chỉ còn hỗ trợ EC2-VPC với VPC mặc định được tạo ra ở mỗi Region cùng một subnet mặc định với CIDR block có giá trị là 172.31.0.0/16.

**Một Amazon VPC bao gồm các thành phần sau**
* Subnets
* Route tables
* Dynamic Host Configuration Protocol (DHCP) option sets
* Security groups
* Network Access Control Lists (ACLs)
  
**Bên cạnh đó là các thành phần bổ sung như:**
* Internet Gateways (IGWs)
* Elastic IP (EIP) addresses
* Elastic Network Interfaces (ENIs)
* Endpoints
* Peering
* Network Address Translation (NATs) instances and NAT gatewaysVirtual Private Gateway (VPG), Customer Gateways (CGWs), and Virtual Private Networks (VPNs)

**Nội dung:**
- [Khởi tạo VPC thông qua Bảng điều khiển](#khởi-tạo-vpc-thông-qua-bảng-điều-khiển)

#### Khởi tạo VPC thông qua Bảng điều khiển

1. Truy cập trang Amazon VPC tại https://console.aws.amazon.com/vpc/
2. Trên thanh điều hướng, chọn **Your VPCs**, **Create VPC**.
3. Khai báo thông tin khởi tạo VPC sau đó chọn **Create**.
   * **Name tag**: thông tin Name cho VPC. Đây là thông tin tùy chọn. 
     * **IPv4 CIDR block**: khai báo dải địa chỉ IPv4 CIDR cho VPC. Tốt nhất ta nên chọn IPv4 CIDR nằm trong dải Private đã được quy định trong RFC 1918; ví dụ, `10.0.0.0/16`, hoặc `192.168.0.0/16`.
     * **IPv6 CIDR block**: đây là thông tin tùy chọn và có thể lấy từ các nguồn sau:
        * **Amazon-provided IPv6 CIDR block**: Lấy dải IPv6 CIDR từ Amazon's pool.
        * **IPv6 CIDR owned by me**: (BYOIP) sử dụng dải địa chỉ IPv6 sở hữu.
   * **Tenancy**: lựa chọn Dedicated tenancy mang ý nghĩa Intance sẽ được chạy trên phần cứng single-tenant duy nhất.