+++
title = "Giới thiệu Amazon VPC"
date = 2021
weight = 1
chapter = false
pre = "<b>1. </b>"
+++

Amazon Virtual Private Cloud (Amazon VPC) dịch theo nghĩa đen là “Đám mây Riêng tư Ảo” là một mạng ảo tùy chỉnh nằm bên trong AWS Cloud và tách biệt (hoặc cô lập) mạng Ảo của riêng bạn với toàn bộ thế giới bên ngoài. Khái niệm này tương tự như việc thiết kế và triển khai một mạng độc lập riêng biệt mà hoạt động trong một trung tâm dữ liệu on-premise, loại hình vẫn còn rất phổ biến hiện nay tại Việt Nam. 

Bên trong VPC tùy chỉnh đó, bạn có toàn quyền kiểm kiểm soát môi trường mạng ảo của mình, nghĩa là vừa có khả năng khởi tạo và chạy các tài nguyên AWS, vừa có thể lựa chọn phạm vi địa chỉ IP, tạo các mạng con và cấu hình các bảng định tuyến và cổng kết nối mạng. Bạn có thể sử dụng cả IPv4 và IPv6 để truy cập an toàn và dễ dàng vào tài nguyên và ứng dụng trong VPC. 

Region là khái niệm mô tả một trung tâm dữ liệu cực lớn của AWS đặt tại một vùng lãnh thổ nhất định. Trong một region, ta có thể tạo ra nhiều VPC và mỗi VPC được phân biệt nhau bởi những dải không gian địa chỉ IP khác nhau. Ta chỉ định phạm vi địa chỉ IPv4 bằng cách lựa chọn a Classless Inter-Domain Routing (CIDR), chẳng hạn như 10.0.0.0/16. Phạm vi địa chỉ của Amazon VPC không thể thay đổi sau khi nó đã được tạo. Phạm vi địa chỉ Amazon VPC có thể lớn bằng /16 (tức 65536 địa chỉ khả dụng) hoặc nhỏ bằng /28 (tức 16 địa chỉ khả dụng) và chúng không được phép trùng với bất kỳ mạng nào khác mà chúng sẽ được kết nối tới.

Dịch vụ Amazon VPC được ra mắt sau dịch vụ Amazon EC2, vì vậy mà có thời điểm AWS cung cấp hai nền tảng mạng khác nhau đó là EC2-Classic và EC2-VPC. EC2-Classic là nền tảng mạng đầu tiên, trong đó tất cả Amazon EC2 được tạo ra đều nằm trong một mạng phẳng duy nhất, chia sẻ kết nối giữa các khách hàng của AWS. Cho tới tháng 12 năm 2013, AWS chỉ còn hỗ trợ EC2-VPC với VPC mặc định được tạo ra ở mỗi Region cùng một subnet mặc định với CIDR block có giá trị là 172.31.0.0/16.

**Amazon VPC bao gồm các thành phần cơ bản:**
- [Subnets](#subnets)
- [Route Table](#route-table)
- [Internet Gateway](#internet-gateway)
- [NAT Gateway](#nat-gateway)

Bây giờ chúng ta sẽ cùng nhau đi qua các khái niệm cơ bản nhất của VPC nhé.

#### Subnets

Subnet là một phân đoạn của dải địa chỉ IP mà bạn sử dụng khi khởi tạo Amazon VPC, cung cấp trực tiếp dải mạng hoạt động cho các tài nguyên AWS có thể chạy bên trong nó như Amazon EC2, Amazon RDS (CSDL Quan hệ Amazon),... Các subnet cũng được xác định thông qua CIDR block (ví dụ: 10.0.1.0/24 và 192.168.0.0/24) và bắt buộc các CIDR của subnet phải nằm trong CIDR của VPC. Subnet nhỏ nhất có thể tạo được là /28 (16 địa chỉ IP). AWS lưu trữ 4 địa chỉ IP đầu tiên và 1 địa chỉ IP cuối cùng của mỗi subnet cho các mục đích kết nối mạng nội bộ. Ví dụ: subnet  /28 có 16 địa chỉ IP khả dụng, nhưng loại bỏ 5 reserved IP cho AWS, như vậy còn lại 11 địa chỉ IP có thể sử dụng cho các tài nguyên hoạt động bên trong subnet này.

Availability Zone hay được viết tắt thành AZ là một trung tâm dữ liệu con, nằm bên trong Region và được xác định dựa treo vị trí địa lý. Bên trong AZ có thể có một hoặc nhiều subnet, nhưng một subnet chỉ có thể nằm trong duy nhất một AZ mà không thể mở rộng sang AZ khác.  

Các subnet được chia thành các loại như **Public**, **Private**, hoặc **VPN-only**. 
+ **Public subnet** là subnet có route table (sẽ thảo luận sau) điều hướng lưu lượng truy cập bên trong subnet đi tới VPC IGW (cũng sẽ thảo luận sau) 
+ **Private subnet** thì ngược lại với Public subnet, nó không có route table điều hướng lưu lượng truy cập tới VPC IGW. 
+ **VPN-only subnet** là subnet mà có route table điều hướng lưu lượng truy cập tới VPG của Amazon VPC (sẽ thảo luận sau). 

Bất kể loại mạng con nào, dải địa chỉ IP nội bộ của subnet luôn là private (nghĩa là từ bên ngoài Internet không thể kết nối trực tiếp tới các địa chỉ thuộc dải này).


![Subnets](/images/architecture/subnet.png?width=40pc)

#### Route Table

Route Table hay còn gọi là bảng định tuyến, cung cấp hướng dẫn định tuyến và được gán vào các Subnets. 
Ví dụ khi bạn tạo VPC với lớp mạng 10.10.0.0/16, cùng 2 subnet 10.10.1.0/24,10.10.2.0/24 thì mỗi subnets mặc định sẽ được gán 1 default route table.\
Bên trong route table sẽ có route entry **destination**:10.10.0.0/16 **target**:local. Route entry này thể hiện các tài nguyên tạo ra trong cùng 1 VPC có thể kết nối với nhau.

![Route Table](/images/architecture/routetable.png?width=40pc)
#### Internet Gateway

Internet Gateway (IGW) là một thành phần Amazon VPC giúp các tài nguyên bên trong VPC, cụ thể là EC2, có khả năng giao tiếp với Internet. IGW có khả năng co giãn mạnh theo chiều ngang, đồng thời tính dự phòng và sẵn sàng cao. Nó hoạt động như một target trong bảng định tuyến của Amazon VPC, giúp lưu lượng truy cập được định tuyến ra ngoài Internet bằng cách biên dịch địa chỉ mạng của EC2 thành địa chỉ Public IP đã được gán cho nó.  
Cụ thể hơn, các EC2 Instance bên trong VPC chỉ biết các địa chỉ Private IP được gán cho nó, nhưng khi có lưu lượng được gửi từ EC2 ra ngoài Internet, IGW sẽ biên dịch địa chỉ Private IP đó thành địa chỉ Public IP (hoặc địa chỉ EIP, sẽ thảo luận sau) mà gán với EC2, và duy trì mapping 1-1 cho tới khi địa chỉ Public IP bị release. Khi EC2 nhận được lưu lượng truy cập từ bên ngoài Internet, IGW sẽ thực hiện dịch địa chỉ Target (địa chỉ Public IP) thành địa chỉ Private IP của EC2 Instance và chuyển tiếp lưu lượng truy cập đến Amazon VPC.

![Internet Gateway](/images/architecture/igw.png?width=55pc)
#### NAT Gateway

Mặc định, mọi EC2 chạy bên trong Private subnet thì sẽ không có khả năng giao tiếp với Internet thông qua IGW. Từ đó vấn đề phát sinh khi EC2 đó cần truy cập ra ngoài Internet để áp dụng các bản cập nhật bảo mật, tải xuống các bản vá hoặc cập nhật phần mềm ứng dụng.  
Nắm bắt được nhu cầu đó, AWS cung cấp 2 phương thức cho phép các EC2 bên trong Private subnet có quyền được truy cập Internet, đó là NAT Instance và NAT Gateway. Với các trường hợp thông thường, thì ta nên sử dụng NAT Gateway thay cho NAT Instance. NAT Gateway đảm bảo tính sẵn sàng và băng thông cao hơn, đồng thời đòi hỏi ít nỗ lực quản trị hơn so với NAT Instance.

Để tạo một một NAT gateway, ta phải chỉ định một mạng con (public) và một địa chỉ Elastic IP. Cần đảm bảo địa chỉ Elastic IP đang không được liên kết với bất cứ Instance hoặc một Network interface nào khác.

Trường hợp ta muốn migrate từ NAT instance sang NAT gateway, ta có thể sử dụng lại địa chỉ Elastic IP của NAT instance. Nhưng trước hết ta cần phải tách địa chỉ IP ra khỏi NAT Instance. 

![NAT Gateway](/images/architecture/natgw.png?width=70pc)

{{%notice tip%}}
NAT Gateway và NAT instance đều không hỗ trợ traffic chiều vào trực tiếp từ internet.
{{%/notice%}}