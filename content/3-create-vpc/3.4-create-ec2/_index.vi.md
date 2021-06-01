+++
title = "Tạo máy chủ EC2"
date = 2021
weight = 4
chapter = false
pre = "<b>3.4 </b>"
+++

Ở bước này chúng ta sẽ tạo 2 máy chủ EC2 ( EC2 instance ) như kiến trúc dưới đây.

![Mô hình Lab](/images/architecture/lab-3.4.png?width=50pc)


#### Tạo EC2 nằm trong Public subnet

1. Truy cập Amazon EC2 console tại địa chỉ https://console.aws.amazon.com/ec2/.
2. Trên thanh điều hướng bên trái, chọn **Intances**, **Launch Intance**.
![Create EC2](/images/vpc/create-ec2.png?width=90pc)

3. Chọn **Select** để chọn Amazon Machine Image (AMI) **Amazon Linux 2 AMI (HVM), SSD Volume Type**.
![Create EC2](/images/vpc/create-ec2-2.png?width=90pc)

4. Giữ nguyên lựa chọn **General purpose t2.micro**, sau đó click **Next: Configure Instance Details**.
![Create EC2](/images/vpc/create-ec2-3.png?width=90pc)

5. Tại trang **Configure Instance Details**
  + **Network** chọn VPC **ASG**.
  + **Subnet** chọn subnet **Public subnet 1**.
  + **Auto-assign Public IP** kiểm tra xem có đang **Enable** hay không. Nếu không Enable, bạn cần kiểm tra lại phần cấu hình tự động cấp phát public IP cho subnet tại mục 3.1.
  + Sau đó click **Next: Add Storage**.
![Create EC2](/images/vpc/create-ec2-4.png?width=90pc)

6. Tại trang **Add Storage**. Giữ nguyên cấu hình mặc định và Click **Next: Add Tags**
7. Tại trang **Add Tags**, Chọn **Add Tag**
  + **Key** điền `Name`.
  + **Value** điền `EC2 Public`.
  + Sau đó bấm **Next: Configure Security Group**.
![Create EC2](/images/vpc/create-ec2-5.png?width=90pc)

8. Tại trang **Configure Security Group**
  + **Assign a security group** click chọn **Select an existing security group**.
  + **Name** chọn security group cho các EC2 trong public cubnet có tên **Public subnet -SG**.
  + Sau đó bấm **Review và Launch**.
![Create EC2](/images/vpc/create-ec2-6.png?width=90pc)
9. Kiểm tra lại thông tin cấu hình và Click **Launch**.
  + Một prompt **Choose an existing key pair or Create new key pair** xuất hiện.
  + Chọn **Create a new key pair**.
  + **Key pair name**: **asg-keypair**.
  + Click **Download Key Pair**.
  + Chọn đường dẫn để lưu file key pair. Bạn sẽ cần file key pair để có thể kết nối ssh tới **EC2 Public** instance bạn sắp tạo.
  + Click **Save** để lưu file key pair.
  + Click **Launch**.
![Create EC2](/images/vpc/create-ec2-7.png?width=90pc)
  + Click **View Instances** và chờ cho tới khi EC2 khởi tạo xong.

#### Tạo EC2 nằm trong Private subnet

1. Truy cập Amazon EC2 console tại địa chỉ https://console.aws.amazon.com/ec2/.
2. Trên thanh điều hướng bên trái, chọn **Intances**, **Launch Intance**.
3. Chọn **Select** để chọn Amazon Machine Image (AMI) **Amazon Linux 2 AMI (HVM), SSD Volume Type**.
![Create EC2](/images/vpc/create-ec2-2.png?width=90pc)
4. Giữ nguyên lựa chọn **General purpose t2.micro**, sau đó click **Next: Configure Instance Details**.
![Create EC2](/images/vpc/create-ec2-3.png?width=90pc)
5. Tại trang **Configure Instance Details**
  + **Network** chọn VPC **ASG**.
  + **Subnet** chọn subnet **Private subnet 2**.
  + **Auto-assign Public IP** kiểm tra xem có đang **Disable** hay không. Nếu không Disable, bạn cần kiểm tra lại phần cấu hình tự động cấp phát public IP cho subnet tại mục 3.1.
  + Sau đó click **Next: Add Storage**.
![Create EC2](/images/vpc/create-ec2-8.png?width=90pc)

6. Tại trang **Add Storage**. Giữ nguyên cấu hình mặc định và Click **Next: Add Tags**
7. Tại trang **Add Tags**, Chọn **Add Tag**
  + **Key** điền `Name`.
  + **Value** điền `EC2 Private`.
  + Sau đó bấm **Next: Configure Security Group**.
![Create EC2](/images/vpc/create-ec2-9.png?width=90pc)

8. Tại trang **Configure Security Group**
  + **Assign a security group** click chọn **Select an existing security group**.
  + **Name** chọn security group cho các EC2 trong public cubnet có tên **Private subnet -SG**.
  + Sau đó bấm **Review và Launch**.
![Create EC2](/images/vpc/create-ec2-10.png?width=90pc)

9. Kiểm tra lại thông tin cấu hình và Click **Launch**.
  + Một prompt **Choose an existing key pair or Create new key pair** xuất hiện.
  + Chọn **Choose an existing keypairr**.
  + Chọn **Key pair name**: **asg-keypair**.
  + Click **I acknowledge that I have access to the selected private key file (asg-keypair.pem), and that without this file, I won't be able to log into my instance.** để xác nhận rằng mình có thể truy cập tới Key pair bạn đã chọn.
  + Click **Launch**.
![Create EC2](/images/vpc/create-ec2-11.png?width=90pc)
  + Click **View Instances** và chờ cho tới khi EC2 khởi tạo xong.

10. Như vậy chúng ta đã tạo được 2 máy chủ EC2 theo mô hình kiến trúc bên dưới. Bước tiếp theo chúng ta sẽ thử truy cập vào 2 máy chủ này.

![Mô hình Lab](/images/architecture/lab-3.4.png?width=50pc)





