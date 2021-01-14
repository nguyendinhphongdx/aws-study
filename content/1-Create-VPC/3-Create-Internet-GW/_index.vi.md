+++
title = "Tạo Internet Gateway"
date = 2020
weight = 3
chapter = false
pre = "<b>1.3. </b>"
+++

Internet Gateway (IGW) là một thành phần Amazon VPC giúp các tài nguyên bên trong VPC, cụ thể là EC2, có khả năng giao tiếp với Internet. IGW có khả năng co giãn mạnh theo chiều ngang, đồng thời tính dự phòng và sẵn sàng cao. Nó hoạt động như một target trong bảng Địnhtuyến của Amazon VPC, giúp lưu lượng truy cập được định tuyến ra ngoài Internet bằng cách biên dịch địa chỉ mạng của EC2 thành địa chỉ Public IP đã được gán cho nó.  
Cụ thể hơn, các EC2 Instance bên trong VPC chỉ biết các địa chỉ Private IP được gán cho nó, nhưng khi có lưu lượng được gửi từ EC2 ra ngoài Internet, IGW sẽ biên dịch địa chỉ Private IP đó thành địa chỉ Public IP (hoặc địa chỉ EIP, sẽ thảo luận sau) mà gán với EC2, và duy trì mapping 1-1 cho tới khi địa chỉ Public IP bị release. Khi EC2 nhận được lưu lượng truy cập từ bên ngoài Internet, IGW sẽ thực hiện dịch địa chỉ Target (địa chỉ Public IP) thành địa chỉ Private IP của EC2 Instance và chuyển tiếp lưu lượng truy cập đến Amazon VPC.

Để tạo Internet gateway ta thực hiện các bước sau đây:
- [Tạo một Subnet](#tạo-một-subnet)
- [Tạo Internet gateway và attach vào VPC](#tạo-internet-gateway-và-attach-vào-vpc)
- [Tạo một custom route table](#tạo-một-custom-route-table)
- [Tạo Security group cho phép truy cập Internet](#tạo-security-group-cho-phép-truy-cập-internet)
- [Thêm địa chỉ Elastic IP](#thêm-địa-chỉ-elastic-ip)

#### Tạo một Subnet

* Truy cập trang Amazon VPC console tại địa chỉ https://console.aws.amazon.com/vpc/
* Trên thanh điều hướng bên trái, chọn **Subnets**.
* Chọn **Create Subnet**.
* **Name tag**, nhập tên cho subne, như là **Private subnet**.
* **VPC**, chọn VPC mà bạn đã tạo từ trước.
* **Availability Zone**, lựa chọn một Availability Zone khác với các subnet đã khai báo trong VPC.
* **IPv4 CIDR block**, nhập dải IPv4 khả dụng. 
* Chọn **Yes**, **Create**

#### Tạo Internet gateway và attach vào VPC

Sau khi tạo thành công Internet gateway, attach IGW vào VPC.

* Truy cập trang Amazon VPC console tại địa chỉ https://console.aws.amazon.com/vpc/
* Trên thanh điều hướng bên trái, chọn **Internet Gateways**, sau đó **Create internet gateway**.
* Nhập tên Internet gateway (tùy chọn), rồi bấm **Create**.
* Lựa chọn Internet gateway mà bạn vừa tạo, sau đó bấm chọn **Actions, Attach to VPC**.
* Lựa VPC từ danh sách rồi bấm **Attach**.

#### Tạo một custom route table

Khi một subnet được tạo ra, nó sẽ tự động được liên kết với main route table của VPC. Mặc định, main route table sẽ không chứa bản ghi điều hướng tới Internet gateway.  
Bên dưới là mô tả cách tạo một custom route table với định tuyến hướng lưu lượng ra ngoài VPC phải đi qua Internet gateway, và cách để liên kết nó với một subnet.

* Truy cập Amazon VPC console tại địa chỉ https://console.aws.amazon.com/vpc/
* Tại hộp thoại **Create Route Table**, nhập tên Route table(tùy chọn), sau đó lựa chọn VPC, rồi bấm **Yes, Create**.
* Bấm chọn the custom route table mà bạn vừa tạo. Thông tin chi tiết của Route table hiển thị bên dưới
* Tại **Routes** tab, chọn **Edit, Add another route**, sau đó bổ sung các route cần thiết. Chọn **Save**.
    * Với IPv4 traffic, điền `0.0.0.0/0` vào ô **Destination**, và chọn Internet gateway ID làm **Target**.
    * Với IPv6 traffic, điền `::/0` vào ô **Destination**, và chọn Internet gateway ID làm **Target**.
* Tại **Subnet Associations** tab, bấm **Edit**, chọn **Associate** với subnet mong muốn, rồi bấm Save.

#### Tạo Security group cho phép truy cập Internet

VPC Security group mặc định cho phép mọi lưu lượng Ra Vào VPC. Ta có thể tạo một Security group mới và bổ sung rule cho phép lưu lượng Ra Vào Internet. Bạn có thể liên kết Security group với Instance nằm trong Public subnet.

* Truy cập Amazon VPC console tại địa chỉ https://console.aws.amazon.com/vpc/
* Trên thanh điều hướng bên trái, chọn **Security Groups**, rồi chọn **Create Security Group**.
* Tại hộp thoại **Create Security Group**, nhập Tên của Security group và điền thêm mô tả. Chọn VPC ID mong muống từ danh sách VPC, sau đó chọn **Yes, Create**.
* Chọn tên của Security group vừa tạo ở trên. Thông tin chi tiết của Route table hiển thị như bên dưới:
	* Tại **Inbound Rules** tab, bấm **Edit**. 
	* Chọn **Add Rule**, và điền thông tin theo nhu cầu. Ví dụ, chọn **HTTP** hoặc **HTTPS** từ **Type** list, rồi nhập **Source** với giá trị `0.0.0.0/0` cho lưu lượng IPv4, hoặc `::/0`cho lưu lượng IPv6. 
	* Bấm **Save** sau khi hoàn thành.
* Truy cập Amazon EC2 console tại địa chỉ https://console.aws.amazon.com/ec2/
* Trên thanh điều hướng bên trái, chọn **Instances**.
* Lựa chọn Instance theo tên, bấm chọn **Actions**, rồi **Networking**, bấm **Change Security Groups**.
* Tại hộp thoại **Change Security Groups**, lựa chọn Security group mới được tạo. Rồi bấm **Assign Security Groups**.

#### Thêm địa chỉ Elastic IP

Sau khi Instance đã được launch, ta thực hiện gán địa chỉ Elastic IP với Instance để cung cấp khả nwang truy cập Internet qua IPv4.

* Truy cập Amazon VPC console tại địa chỉ https://console.aws.amazon.com/vpc/
* Trên thanh điều hướng bên trái, chọn **Elastic IPs**.
* Bấm **Allocate new address**, **Allocate**.
* Chọn địa chỉ Elastic IP đã tạo rồi bấm **Actions**, sau đó bấm **Associate address**.
* Chọn **Instance** hoặc **Network interface**, rồi chọn ID của Instance hoặc Network interface. Chọn địa chỉ Private IP cần được liên kết với địa chỉ Elastic IP, rồi chọn **Associate**.