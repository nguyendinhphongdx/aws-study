+++
title = "Kiểm tra kết nối"
date = 2021
weight = 5
chapter = false
pre = "<b>3.5 </b>"
+++

Ở bước này chúng ta sẽ kết nối ssh tới 2 máy chủ EC2 ( EC2 instance ) chúng ta đã tạo ở bước 3.4. Sau đó thực hiện ping test để kiểm tra khả năng kết nối internet.

![Mô hình Lab](/images/architecture/lab-3.4.png?width=50pc)


#### Kết nối vào máy chủ EC2 Public và kiểm tra truy cập Internet.

1. Truy cập Amazon EC2 console tại địa chỉ https://console.aws.amazon.com/ec2/.
2. Kiểm tra và lấy thông tin public IP của máy chủ **EC2 Public**.
![Connect EC2](/images/vpc/connect-ec2.png?width=90pc)

3. Download putty và puttygen để kết nối ssh tới **EC2 Public**.
  + {{%attachments title="Putty and Puttygen" pattern=".*(exe)"/%}}

4. Trước tiên mở puttygen bạn đã download về để thực hiện convert **asg-keypair.pem** sang **asg-keypair.ppk**.
  + Click **Load**.
  + Chuyển thư mục tới đường dẫn mà bạn đã download **asg-keypair.pem**.
  + Chọn hiển thị **All Files**.
  + Chọn **asg-keypair.pem**.
  + Click **Open**.
![Connect EC2](/images/vpc/connect-ec2-2.png?width=60pc)
  + Click **Ok**.
  + Click **Save Private Key**.
  + Click **Yes**.
  + Điền tên **asg-keypair**.
  + Click **Save**.
![Connect EC2](/images/vpc/connect-ec2-3.png?width=60pc)

5. Mở công cụ putty mà bạn đã tải về, điền thông tin public IP vào mục **Host name**.
 + Điền **EC Public** vào mục **Saved sessions**.
 + Click **Save**.
![Connect EC2](/images/vpc/connect-ec2-4.png?width=90pc)

6. Tiếp theo chúng ta sẽ sử dụng key pair để có thể kết nối tới **EC2 public**.
  + Click **SSH**.
  + Click **Auth**.
  + Click **Browse**.
  + Chuyển đường dẫn thư mục tới thư mục chứa file **asg-keypair.ppk**.
  + Chuyển định dạng file sang *.ppk.
  + Chọn file **asg-keypair.ppk**.
  + Click **Open**.
  + Click **Open** để thực hiện kết nối tới **EC2 Public**.
![Connect EC2](/images/vpc/connect-ec2-5.png?width=90pc)

7. Nhập **ec2-user**. **ec2-user** là user mặc định của Amazon Linux AMI.
  + Gõ lệnh ping amazon.com để kiểm tra kết nối tới internet của **EC2 Public**.
  ```
  ping amazon.com
  ```
![Connect EC2](/images/vpc/connect-ec2-6.png?width=90pc)

#### Kết nối vào máy chủ EC2 Private và kiểm tra kết nối internet.


1. Truy cập Amazon EC2 console.
2. Kiểm tra và lấy thông tin private IP của máy chủ **EC2 Private**.
  + Thực hiện lệnh ping <địa chỉ IP private của EC2 Private> để kiểm tra kết nối từ máy chủ **EC2 Public** sang máy chủ **EC2 Private**.
![Connect EC2](/images/vpc/connect-ec2-7.png?width=90pc)

3. **EC2 Private** sẽ không có public IP address vì chúng ta không gán cho máy chủ này public IP. Để có thể ssh vào **EC2 Private**, chúng ta sẽ thực hiện kết nối ssh từ **EC2 Public** thông qua địa chỉ private IP address của EC2 **Private**
 + Download công cụ **pscp** vào cùng thư mục chứa file **asg-keypair.ppk** để thực hiện copy file **asg-keypair.pem* từ máy của chúng ta vào **EC2 Public**.
    * {{%attachments title="pscp tool" pattern="pscp.*(exe)"/%}}

 + Khởi chạy **Command Prompt**.Chuyển đường dẫn tới thư mục bạn vừa download **pscsp**. Chạy lệnh dưới đây để upload file **asg-keypair.pem** lên thư mục **/home/ec2-user/** của máy chủ **EC2 Public**.
 + Bạn sẽ cần thay thế thông số public IP address của EC2 Public trước khi chạy câu lệnh.

```
pscp -i asg-keypair.ppk asg-keypair.pem ec2-user@<EC2 PUBLIC public IP address>:/home/ec2-user/
```
![Connect EC2](/images/vpc/connect-ec2-8.png?width=60pc)

{{%notice tip%}}
Đảm bảo bạn copy file asg-keypair.pem lên máy chủ EC2 Public.
{{%/notice%}}

4. Cập nhật quyền cho file **asg-keypair.pem** bằng cách chạy lệnh **chmod 400 asg-keypair.pem**. AWS yêu cầu file key pair cần được hạn chế quyền truy cập trước khi được sử dụng để kết nối tới máy chủ EC2.
```
chmod 400 asg-keypair.pem
```
5. ssh tới máy chủ EC2 Private và thực hiện ping test tới **amazon.com**.
```
ssh -i asg-keypair.pem ec2-user@<EC2 Private server's private IP address>
ping amazon.com
```
![Connect EC2](/images/vpc/connect-ec2-10.png?width=90pc)
6. Bạn có thể thấy, chúng ta không thể kết nối internet từ EC2 Private. Trong bước tiếp theo chúng ta sẽ tạo NAT Gateway để cho phép máy chủ **EC2 Private** kết nối internet theo chiều từ nội bộ đi ra.

