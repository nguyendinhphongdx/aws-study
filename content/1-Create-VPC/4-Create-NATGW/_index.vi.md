+++
title = "Tạo NAT Gateway"
date = 2020
weight = 4
chapter = false
pre = "<b>1.4. </b>"
+++

Mặc định, mọi EC2 chạy bên trong Private subnet thì sẽ không có khả năng giao tiếp với Internet thông qua IGW. Từ đó vấn đề phát sinh khi EC2 đó cần truy cập ra ngoài Internet để áp dụng các bản cập nhật bảo mật, tải xuống các bản vá hoặc cập nhật phần mềm ứng dụng.  
Nắm bắt được nhu cầu đó, AWS cung cấp 2 phương thức cho phép các EC2 bên trong Private subnet có quyền được truy cập Internet, đó là NAT Instance và NAT Gateway. Với các trường hợp thông thường, thì ta nên sử dụng NAT Gateway thay cho NAT Instance. NAT Gateway đảm bảo tính sẵn sàng và băng thông cao hơn, đồng thời đòi hỏi ít nỗ lực quản trị hơn so với NAT Instance.

Để tạo một một NAT gateway, ta phải chỉ định một mạng con (public) và một địa chỉ Elastic IP. Cần đảm bảo địa chỉ Elastic IP đang không được liên kết với bất cứ Instance hoặc một Network interface nào khác.

Trường hợp ta muốn migrate từ NAT instance sang NAT gateway, ta có thể sử dụng lại địa chỉ Elastic IP của NAT instance. Nhưng trước hết ta cần phải tách địa chỉ IP ra khỏi NAT Instance. 

**Nội dung:**
- [Tạo NAT Gateway](#tạo-nat-gateway)
- [Thêm các route cho bảng định tuyến](#thêm-các-route-cho-bảng-định-tuyến)

#### Tạo NAT Gateway

* Truy cập Amazon VPC console tại địa chỉ https://console.aws.amazon.com/vpc/
* Trên thanh điều hướng bên trái, chọn **NAT Gateways**, **Create NAT Gateway**.
* Chỉ rõ Public subnet đặt NAT gateway, rồi lựa chọn địa chỉ Elastic IP theo ID để liên kết với NAT gateway.
* Chọn **Add tag** và nhập giá trị:
  * **Key**, nhập tên khóa
  * **Value**: nhập giá trị của khóa
* Chọn **Create a NAT Gateway**.
* Sau đó, NAT gateway sẽ hiển thị trong bảng điều khiển. Sau một lúc, trạng thái của NAT gateway sẽ chuyển thành **Available** - nghĩa là thiết lập đã sẵn sàng để sử dụng.

#### Thêm các route cho bảng định tuyến 

Sau khi NAT gateway đã được tạo, thực hiện cập nhật bảng định tuyến cho Private subnet để định tuyến mọi lưu lượng từ subnet này đi ra ngoài internet đều phải đi qua NAT Gateway.

* Truy cập Amazon VPC console tại địa chỉ https://console.aws.amazon.com/vpc/
* Trên thanh điều hướng bên trái, chọn **Route Tables**.
* Chọn bảng định tuyến liên kết với Private subnet rồi Chọn **Routes, Edit**.
* Chọn **Add another route**. Đối với **Destination**, nhập `0.0.0.0/0`. Đối với **Target**, nhập ID của NAT gateway.
* Chọn chọn **Save**.
