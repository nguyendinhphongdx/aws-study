---
title : "Kiểm tra kết nối"
date :  "`r Sys.Date()`" 
weight : 2 
chapter : false
pre : " <b> 4.2 </b> "
---

#### Kiểm tra kết nối

{{% notice note %}}
Có nhiều cách kết nối EC2, các bạn có thể tham khảo [kết nối EC2 bằng **PuTTY**](https://000004.awsstudygroup.com/vi/4-launchlinuxinstance/4.2-connectlinuxinstance/). Trong bài lab, chúng ta sử dụng [MobaXterm](https://mobaxterm.mobatek.net/) để kết nối EC2
{{% /notice %}}

1. Thực hiện tải [MobaXterm](https://mobaxterm.mobatek.net/download.html) 

![Create VPC](/images/4-CreateEc2Server/4.2-ec2connect/00019-ec2connect.png?featherlight=false&width=90pc)

2. Truy cập vào trang **EC2**

- Chọn **Instances**
- Chọn **EC2 Public**
- Chọn **Details**
- Xem **Public IPv4 address**

![Create VPC](/images/4-CreateEc2Server/4.2-ec2connect/0001-ec2connect.png?featherlight=false&width=90pc)

3. Sau khi tải **MobaXterm**, tiến hành giải nén và mở 

- Chọn **Session**

![Create VPC](/images/4-CreateEc2Server/4.2-ec2connect/0002-ec2connect.png?featherlight=false&width=90pc)

4. Trong giao diện **Session settings**

- Chọn **SSH**

![Create VPC](/images/4-CreateEc2Server/4.2-ec2connect/0003-ec2connect.png?featherlight=false&width=90pc)

5. Trong giao diện **Session settings**

- **Remote host**, nhập **```Public IPv4 address```**
- **Specify username**, nhập **```ec2-user```**
- **Use private key**, chọn đường dẫn của **aws-keypair.pem** đã tạo và tải về máy lúc tạo EC2.

![Create VPC](/images/4-CreateEc2Server/4.2-ec2connect/0004-ec2connect.png?featherlight=false&width=90pc)

6. Kết nối thành công.

![Create VPC](/images/4-CreateEc2Server/4.2-ec2connect/0005-ec2connect.png?featherlight=false&width=90pc)

7. Kiểm tra kết nối tới internet của EC2 Public, ta thực hiện lệnh:

```
ping amazon.com -c5
```

![Create VPC](/images/4-CreateEc2Server/4.2-ec2connect/0006-ec2connect.png?featherlight=false&width=90pc)

#### Kết nối vào máy chủ EC2 Private và kiểm tra kết nối internet.
8. Truy cập vào **EC2**

- Chọn **Instances**
- Chọn **EC2 Private**
- Chọn **Details**
- Chọn **Private IPv4 addresses**

![Create VPC](/images/4-CreateEc2Server/4.2-ec2connect/0007-ec2connect.png?featherlight=false&width=90pc)

9. Thực hiện lệnh ping **<địa chỉ IP private của EC2 Private>** để kiểm tra kết nối từ máy chủ **EC2 Public** sang máy chủ **EC2 Private**. Chúng ta kiểm tra kết nối giữa 2 EC2 instance bằng câu lệnh:

```
ping 10.10.4.105 -c5
```

![Create VPC](/images/4-CreateEc2Server/4.2-ec2connect/0008-ec2connect.png?featherlight=false&width=90pc)

10. **EC2 Private** sẽ không có **public IP address** vì chúng ta không gán cho máy chủ này public IP. Để có thể ssh vào **EC2 Private**, chúng ta sẽ thực hiện kết nối ssh từ EC2 Public thông qua địa chỉ **private IP address của EC2 Private**

- Download công cụ **pscp** vào cùng thư mục chứa file **aws-keypair.ppk** để thực hiện copy file **aws-keypair.pem** từ máy của chúng ta vào **EC2 Public**.


{{% notice info %}}
Bạn tải [a RSA and DSA key generation utility](https://the.earth.li/~sgtatham/putty/latest/w64/puttygen.exe) là dạng **puttygen.exe**
{{% /notice %}}

{{% notice info %}}
Bạn tải [an SCP client, i.e. command-line secure file copy](https://the.earth.li/~sgtatham/putty/latest/w64/pscp.exe) là **dạng pscp.exe**
{{% /notice %}}

11. Chúng ta sử dụng **puttygen.exe** để generation key

- Chọn **Load**

![Create VPC](/images/4-CreateEc2Server/4.2-ec2connect/0009-ec2connect.png?featherlight=false&width=90pc)

12. Chọn **aws-keypair.pem**

- Chọn **OK**
- Chọn **Save private key** với tên **aws-keypair.ppk**

![Create VPC](/images/4-CreateEc2Server/4.2-ec2connect/00010-ec2connect.png?featherlight=false&width=90pc)

13. Hoàn thành generation key

![Create VPC](/images/4-CreateEc2Server/4.2-ec2connect/00011-ec2connect.png?featherlight=false&width=90pc)

14. Khởi chạy **Command Prompt**. Chuyển đường dẫn tới thư mục bạn vừa download **pscp**. Chạy lệnh dưới đây để upload file **aws-keypair.pem** lên thư mục **/home/ec2-user/** của máy chủ EC2 Public.

- Bạn sẽ cần thay thế thông số **public IP address của EC2 Public** trước khi chạy câu lệnh.

```
pscp -i aws-keypair.ppk aws-keypair.pem ec2-user@<EC2 PUBLIC public IP address>:/home/ec2-user/
```

![Create VPC](/images/4-CreateEc2Server/4.2-ec2connect/00012-ec2connect.png?featherlight=false&width=90pc)

15. Truy cập vào **EC2**

- Chọn **Instances**
- Chọn **EC2 Public**
- Chọn **Details**
- Xem **Public IPv4 address**

![Create VPC](/images/4-CreateEc2Server/4.2-ec2connect/0001-ec2connect.png?featherlight=false&width=90pc)

16. Quay trở lại giao diện kết nối EC2. Để đảm bảo bạn copy file **aws-keypair.pem** lên máy chủ **EC2 Public** ta thực hiện lệnh

```
ls
```

![Create VPC](/images/4-CreateEc2Server/4.2-ec2connect/00014-ec2connect.png?featherlight=false&width=90pc)

17. Cập nhật quyền cho file **aws-keypair.pem** bằng cách chạy lệnh **chmod 400 aws-keypair.pem**. AWS yêu cầu file key pair cần được hạn chế quyền truy cập trước khi được sử dụng để kết nối tới máy chủ EC2.

```
chmod 400 aws-keypair.pem
```

![Create VPC](/images/4-CreateEc2Server/4.2-ec2connect/00015-ec2connect.png?featherlight=false&width=90pc)

18. **SSH** tới máy chủ **EC2 Private**

```
ssh -i aws-keypair.pem ec2-user@<EC2 Private server's private IP address>
```

![Create VPC](/images/4-CreateEc2Server/4.2-ec2connect/00016-ec2connect.png?featherlight=false&width=90pc)

19. Thực hiện **ping test tới amazon.com**. Bạn có thể thấy, chúng ta không thể kết nối **internet từ EC2 Private**. Trong bước tiếp theo chúng ta sẽ tạo **NAT Gateway** để cho phép máy chủ **EC2 Private** kết nối internet theo chiều từ nội bộ đi ra. Giữ nguyên kết nối tới **EC2 Private** để chúng ta có thể kiểm tra kết nối tới **internet** sau khi hoàn tất tạo và cấu hình **NAT Gateway** nhé.

```
ping amazon.com
```

![Create VPC](/images/4-CreateEc2Server/4.2-ec2connect/00017-ec2connect.png?featherlight=false&width=90pc)



