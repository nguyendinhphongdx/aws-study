+++
title = "Network ACLs"
date = 2020
weight = 2
chapter = false
pre = "<b>2.1.2. </b>"
+++

**Nội dung:**
- [Cơ bản về Network ACL](#cơ-bản-về-network-acl)
- [Network ACL rules](#network-acl-rules)
- [Khởi tạo một network ACL](#khởi-tạo-một-network-acl)
- [Bổ sung rules vào một network ACL](#bổ-sung-rules-vào-một-network-acl)

#### Cơ bản về Network ACL

* VPC sau khi khởi tạo sẽ có sẵn một network ACL mặc định và có thể được sửa đổi. Mặc định, nó cấp phép truy cập cho tất cả lưu lượng truy cập IPv4 hoặc IPv6 có thể đi ra hoặc đi vào VPC. 
* Có thể tạo một network ACL tùy chỉnh và liên kết nó với một subnet. Mặc định các network ACL tùy chỉnh từ chối tất cả lưu lượng truy cập đi vào và đi ra, cho đến khi ta bổ sung rule cấp phép truy cập.
* Từng subnet trong VPC phải được liên kết với một network ACL. Nếu subnet không được liên kết với một network ACL cụ thể thì subnet sẽ được tự động liên kết với network ACL mặc định.
* Một network ACL có thể liên kết với nhiều subnet. Tuy nhiên, một subnet chỉ có thể liên kết với một network ACL tại một thời điểm. Khi liên kết network ACL mới với subnet,thì liên kết trước đó sẽ bị xóa.
* Một network ACL chứa một danh sách các rule được đánh số khác nhau. AWS đánh giá các rule dựa trên số thứ tự được gán, bắt đầu với rule được đánh số thấp nhất, để xác định xem lưu lượng có được phép đi vào hay đi ra khỏi bất kỳ subnet nào được liên kết với network ACL hay không. 
Số thứ tự lớn nhất có thể được gán cho rule là 32766 (tương đương số lượng rule lớn nhất của một network ACL là 32766).
* Network ACL có các rule cấp phép đi vào hoặc đi ra tách biệt và rule có thể là cho phép hoặc từ chối lưu lượng.
* Network ACL là dịch vụ Stateless, nghĩa là phản hồi đối với lưu lượng truy cập được phép đi vào, phải tuân theo rule đối với lưu lượng truy cập đi ra (và ngược lại).

#### Network ACL rules

Có thể thêm hoặc xóa một rule khỏi network ACL mặc định hoặc tạo network ACL mới cho VPC. Khi thêm hoặc xóa một rule khỏi network ACL, các thay đổi sẽ tự động được áp dụng cho các subnet được liên kết với nó.

Các thành phần của một network ACL rule:
* **Rule number**. Rule bắt đầu được đánh giá bắt đầu với rule có số thứ tự thấp nhất. 
Ngay khi rule đó match với lưu lượng truy cập, nó sẽ ngay lập tức được áp dụng cho dù nó mâu thuẫn với một rule nào đó được đánh số lớn hơn trong danh sách.
* **Type**.loại lưu lượng, ví dụ SSH. Có thể chỉ định tất cả các loại lưu lượng truy cập hoặc phạm vi tùy chỉnh.
* **Protocol**.  chỉ định giao thức cùng số giao thức chuẩn.
* **Port range**. port hoặc dải port lắng nghe của lưu lượng truy cập. Ví dụ: HTTP là 80.
* **Source**. [chỉ đối với Inbound rule] Xuất phát của lưu lượng truy cập (giá trị là dải CIDR).
* **Destination**. [chỉ đối với Outbound rule] Điểm đến của lưu lượng truy cập (giá trị là dải CIDR).
* **Allow/Deny**.  chỉ rõ là Cho phép hoặc Từ chối lưu lượng truy cập.

#### Khởi tạo một network ACL

* Truy cập Amazon VPC console tại địa chỉ https://console.aws.amazon.com/vpc/
* Trên thanh điều hướng bên trái, chọn **Network ACLs**.
* Chọn **Create Network ACL**.
* Trong hộp thoại **Create Network ACL**,
  * Điền tên network ACL (tùy chọn)
  * Chọn ID của VPC từ danh sách VPC. 
* Sau đó bấm **Yes, Create**.

#### Bổ sung rules vào một network ACL

* Truy cập Amazon VPC console tại địa chỉ https://console.aws.amazon.com/vpc/
* Trên thanh điều hướng bên trái, chọn **Network ACLs**.
* Trong phần nội dung chi tiết, chọn **Inbound Rules** hoặc **Outbound Rules** tab, tìm loại rule mà bạn muốn thêm, sau đó bấm **Edit**.
* Trong phần **Rule #**, nhập số thứ tự của rule. Số thứ tự của rule phải chưa được khai báo trong network ACL. AWS sẽ tiến hành thực thi các rule theo thứ tự từ số thấp nhất tới số lớn nhất.
* Chọn rule từ danh sách **Type** của rule.
* Trong phần **Source** hoặc **Destination**, nhập dải CIDR muốn được áp dụng rule.
* Từ danh sách **Allow/Deny**, chọn **ALLOW** để Cho phép hoặc **DENY** để Từ chối (không cho phép) lưu lượng truy cập.
* Chọn **Save**.
