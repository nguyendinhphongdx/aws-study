+++
title = "Cấu hình NACL & SG"
date = 2020
weight = 1
chapter = false
pre = "<b>2.1. </b>"
+++

**Nội dung:**
- [Security groups](#security-groups)
- [Network ACLs cho VPC](#network-acls-cho-vpc)

#### Security groups

Một security group hoạt động như một tường lửa ảo cho Instance, giúp kiểm soát lưu lượng truy cập. 
Một Instance trong VPC có thể được gán tối đa 5 Security group do SG chỉ hoạt động ở tầng Instance mà không họat động ở tầng Subnet. 

{{% notice note %}}
Security groups hoạt động ở mức máy ảo, không phải ở mức subnet. Tuy nhiên, mỗi máy ảo trong một subnet có thể được gán với nhiều bộ Security group khác nhau,
{{% /notice %}}

**Cơ bản về Security group**

Một số đặc điểm cơ bản của Security group:

* Chỉ có thể thêm vào các Allow rule, mà không thể bổ sung Deny rule.
* Có thể chỉ định các rule riêng biệt cho lưu lượng truy cập đi ra hoặc đi vào.
* Một Security group mới được tạo ra không có sẵn Inbound rules. 
  Do đó, tại thời điểm ban đầu Instance sẽ không cho phép bất cứ lưu lượng truy cập nào được phép đi vào, đòi hỏi ta phải bổ sung Inbound rule để cấp phép truy cập.
* Mặc định, Security group có sẵn Outbound rule cho phép mọi lưu lượng được phép đi ra khỏi Instance. 
Rule này có thể được chỉnh sửa (xóa) và bổ sung các Outbound rule cụ thể, chỉ rõ lưu lượng nào xuất phát từ Instance được phép đi ra ngoài.
Nếu SG không có Outbound rule thì không một lưu lượng nào được phép đi ra khởi Instance. 
* Security groups là một dịch vụ Stateful - nghĩa là nếu lưu lượng đi vào Instance đã được cấp phép thì lưu lượng cũng có thể đi ra ngoài Instance, và ngược lại, bất kể Outbound rule như thế nào.
* Các Instance chỉ có thể giao tiếp được với nhau khi và chỉ khi chúng được liên kết với Security group cho phép kết nối, hoặc Security group mà Instance có liên kết chứa Rule cho phép lưu lượng try cập (ngoại trừ Security group mặc định với có các rule  mặc định cho phép toàn bộ lưu lượng được truy cập).
* Security group được liên kết với các network interface. 
Sau khi Instance đã được khởi tạo, ta vẫn có thể thay đổi Security group đã được gán với Instance, điều này cũng thay đổi security group đang được gán với primary network interface tương ứng. 

**Security group Rule**

Rule được sinh ra để cấp quyền truy cập cho lưu lượng đi vào hoặc đi ra khỏi Instance. Quyền truy cập này có thể được áp dụng cho một CIDR cụ thể hoặc cho một Security group nằm trong cùng một VPC hoặc nằm trong một VPC khác nhưng có kết nối peering đã được khởi tạo. 

Các thành phần cơ bản của Security group rule:
* (Chỉ đối với Inbound rules) gồm điểm xuất phát (nguồn) của lưu lượng truy cập và port đích hoặc dải port.
Nguồn có thể là một security group khác, là một dải IPv4 hoặc IPv6 CIDR hoặc đơn thuẩn là một địa chỉ IPv4 hoặc IPv6.
* (Chỉ đối với Outbound rules) gồm đích đến của lưu lượng và port đích hay dải port đích.
Đích đến có thể là một security group khác, là một dải IPv4 hoặc IPv6 CIDR hoặc đơn thuẩn là một địa chỉ IPv4 hoặc IPv6 hoặc là một dịch vụ bắt đầu bằng một tiền tố (ví dụ: igw_xxx) nằm trong danh sách prefix ID(một dịch vụ được xác định bởi prefix ID - tên và ID của dịch vụ khả dụng trong region).
* Mọi giao thức đều có một số giao thức chuẩn. Ví dụ: SSH là 22,..

**Tạo một security group**

* Truy cập Amazon VPC console tại địa chỉhttps://console.aws.amazon.com/vpc/
* Trên thanh điều hướng bên trái, chọn **Security Groups**.
* Chọn **Create Security Group**.
	* Nhập tên Security group (ví dụ, `my-security-group`) và nhập mô tả (nếu cần). 
	* Chọn ID của VPC mà ta muốn
	* Chọn **Yes, Create**.

**Thêm rule**

* Truy cập Amazon VPC console tại địa chỉhttps://console.aws.amazon.com/vpc/
* Trên thanh điều hướng bên trái, chọn **Security Groups**.
* Chọn security group mà ta muốn cập nhật.
* Chọn **Actions, Edit inbound rules** hoặc **Actions, Edit outbound rules**.
* Phần **Type**, chọn loại lưu lượng, sau đó điền các thông tin cần thiết. 
* Ta cũng có thể cấp phép kết nối giữa các Instance thuộc các security group khác nhau. Ví dụ để tạo inbound rule ta cần thực hiện như sau:
	* **Type**: `All Traffic`
	* **Source**: nhập ID của security group mà ta muốn cấp phép
*  Chọn **Save rules**.

#### Network ACLs cho VPC

Danh sách kiểm soát truy cập mạng (ACL) là lớp bảo mật tùy chọn cho VPC, nó hoạt động như một tường lửa để kiểm soát lưu lượng ra và vào cho một hoặc nhiều subnet. 
Ta có thể thiết lập network ACL với các rule tương tự như security groups, nhằm bổ sung thêm một lớp bảo mật nữa cho VPC.

**Cơ bản về Network ACL**

* VPC sau khi khởi tạo sẽ có sẵn một network ACL mặc định và có thể được sửa đổi. Mặc định, nó cấp phép truy cập cho tất cả lưu lượng truy cập IPv4 hoặc IPv6 có thể đi ra hoặc đi vào VPC. 
* Có thể tạo một network ACL tùy chỉnh và liên kết nó với một subnet. Mặc định các network ACL tùy chỉnh từ chối tất cả lưu lượng truy cập đi vào và đi ra, cho đến khi ta bổ sung rule cấp phép truy cập.
* Từng subnet trong VPC phải được liên kết với một network ACL. Nếu subnet không được liên kết với một network ACL cụ thể thì subnet sẽ được tự động liên kết với network ACL mặc định.
* Một network ACL có thể liên kết với nhiều subnet. Tuy nhiên, một subnet chỉ có thể liên kết với một network ACL tại một thời điểm. Khi liên kết network ACL mới với subnet,thì liên kết trước đó sẽ bị xóa.
* Một network ACL chứa một danh sách các rule được đánh số khác nhau. AWS đánh giá các rule dựa trên số thứ tự được gán, bắt đầu với rule được đánh số thấp nhất, để xác định xem lưu lượng có được phép đi vào hay đi ra khỏi bất kỳ subnet nào được liên kết với network ACL hay không. 
Số thứ tự lớn nhất có thể được gán cho rule là 32766 (tương đương số lượng rule lớn nhất của một network ACL là 32766).
* Network ACL có các rule cấp phép đi vào hoặc đi ra tách biệt và rule có thể là cho phép hoặc từ chối lưu lượng.
* Network ACL là dịch vụ Stateless, nghĩa là phản hồi đối với lưu lượng truy cập được phép đi vào, phải tuân theo rule đối với lưu lượng truy cập đi ra (và ngược lại).

**Network ACL rules**

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

**Khởi tạo một network ACL**

* Truy cập Amazon VPC console tại địa chỉ https://console.aws.amazon.com/vpc/
* Trên thanh điều hướng bên trái, chọn **Network ACLs**.
* Chọn **Create Network ACL**.
* Trong hộp thoại **Create Network ACL**, điền tên network ACL (tùy chọn), rồi lựa chọn ID của VPC từ danh sách VPC. Sau đó bấm **Yes, Create**.

**Bổ sung rules vào một network ACL**

* Truy cập Amazon VPC console tại địa chỉ https://console.aws.amazon.com/vpc/
* Trên thanh điều hướng bên trái, chọn **Network ACLs**.
* Trong phần nội dung chi tiết, chọn **Inbound Rules** hoặc **Outbound Rules** tab, tìm loại rule mà bạn muốn thêm, sau đó bấm **Edit**.
* Trong phần **Rule #**, nhập số thứ tự của rule. Số thứ tự của rule phải chưa được khai báo trong network ACL. AWS sẽ tiến hành thực thi các rule theo thứ tự từ số thấp nhất tới số lớn nhất.
* Chọn rule từ danh sách **Type** của rule.
* Trong phần **Source** hoặc **Destination**, nhập dải CIDR muốn được áp dụng rule.
* Từ danh sách **Allow/Deny**, chọn **ALLOW** để Cho phép hoặc **DENY** để Từ chối (không cho phép) lưu lượng truy cập.
* Chọn **Save**.
