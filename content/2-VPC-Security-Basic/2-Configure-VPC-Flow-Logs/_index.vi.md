+++
title = "Cấu hình VPC Flow Logs"
date = 2020
weight = 1
chapter = false
pre = "<b>2.2. </b>"
+++

Chúng ta có thể tạo Flow log cho VPC, subnets hoặc network interfaces. Flow log có thể đẩy dữ liệu lên CloudWatch Log hoặc Amazon S3.

**Nội dung:**
- [1. Đẩy Luồng dữ liệu nhật ký lên CloudWatch Logs](#1-đẩy-luồng-dữ-liệu-nhật-ký-lên-cloudwatch-logs)
- [2. Đẩy luồng dữ liệu nhật ký lên Amazon S3](#2-đẩy-luồng-dữ-liệu-nhật-ký-lên-amazon-s3)
- [3. Xem nội dung Luồng dữ liệu nhật ký](#3-xem-nội-dung-luồng-dữ-liệu-nhật-ký)

#### 1. Đẩy Luồng dữ liệu nhật ký lên CloudWatch Logs

Khi đẩy dữ liệu lên CloudWatch Logs, luồng dữ liệu nhật ký sẽ được đưa vào log group, và mỗi network interface có một luồng dữ liệu nhật ký riêng biệt trong log group. 
Luồng nhật ký dữ liệu chứa các bản ghi dữ liệu nhật ký lưu lượng.
Bạn có thể tạo nhiều luồng dữ liệu nhật ký đẩy vào trong cùng một log group.
Nếu một network interface hiển thị trong một hoặc nhiều luồng dữ liệu nhật ký trong cùng một log group thì nó sẽ tạo ra một luồng dữ liệu nhật ký kết hợp.
Nếu đã có một luồng dữ liệu nhật ký chỉ bắt lưu lượng bị từ chối và luồng dữ liệu nhật ký khác chỉ bắt lưu lượng được chấp nhận, thì luồng dữ liệu nhật ký kết hợp sẽ bắt dữ liệu của cả 2 luồng lưu lượng trên.

**Tạo IAM role cho flow logs**

* Truy cập trang IAM tại địa chỉ https://console.aws.amazon.com/iam/
* Trên thanh điều hướng bên trái, chọn **Roles, Create role**.
* Trong phần **Type of trusted entity** chọn **AWS service: EC2, Lambda and others **. Đối với **Use case**, chọn **EC2**. Tiếp tục **Next: Permissions**.
* Tại trang **Attach permissions policies**, chọn **Next: Tags** rồi nhập thông tin tag (nếu cần). Chọn tiếp **Next: Review**.
* Nhập tên Role (ví dụ Flow-Logs-Role), nếu cần thiết có thể bổ sung thêm mô tả. Chọn **Create role**.
* Chọn tên Role. Trong phần **Permissions**, chọn **Add inline policy, JSON**.
* Sao chép policy bên dưới rồi chọn Review policy.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
        "logs:CreateLogGroup",
        "logs:CreateLogStream",
        "logs:PutLogEvents",
        "logs:DescribeLogGroups",
        "logs:DescribeLogStreams"
      ],
      "Effect": "Allow",
      "Resource": "*"
    }
  ]
}   
```
* Nhâp tên policy, rồi chọn **Create policy**.
* Chọn tên của role. Trong phần **Trust relationships**, chọn **Edit trust relationship** sau đó thay đổi service từ `ec2.amazonaws.com` sang `vpc-flow-logs.amazonaws.com`. Rồi chọn **Update Trust Policy**.
* Tại trang **Summary**, lưu lại thông tin ARN role bởi nó sẽ được sử dụng cho việc tạo flow log sau này.


**Tạo Luồng dữ liệu nhật kí hiển thị trên CloudWatch Logs**

* Truy cập trang Amazon VPC console tại địa chỉ https://console.aws.amazon.com/ec2/
* Trên thanh điều hướng bên trái, chọn **Network Interfaces** or **Your VPCs** or **Subnets**.
* Chọn một hoặc nhiều network interfaces rồi chọn **Actions, Create flow log**.
* Trong phần **Filter**, chọn loại dữ liệu sẽ được ghi nật ký. Sau đó chọn **All** để ghi nhận dữ liệu của mọi traffic được cho phép hoặc bị từ chối. Cụ thể **Rejected** là lựa chọn để ghi lại lưu lượng dữ liệu bị từ chối, hoặc **Accepted** để ghi lại lưu lượng dữ liệu được cho phép. Phần *Maximum aggregation interval**, chỉ định khoảng thời gian mà dữ liệu nhật ký được ghi lại và tổng hợp thành bản ghi dữ liệu nhật ký.
* Mục **Destination**, chọn **Send to CloudWatch Logs**.
* Mục **Destination log group**, nhập tên của log group - nơi chưa dữ liệu lưu lượng nhật ký. Nếu như chưa tồn tại log group, AWS sẽ tự động tạo một log group làm destination.
* Mục **IAM role**, chỉ rõ tên của rile mà có quyền đẩy dữ liệu nhật ký vào CloudWatch Logs.
* Mục **Format**, chỉ rõ định dạng của bản ghi dữ liệu nhật ký.
	* Nếu muốn sử dụng định dạng mặc định thì chọn **AWS default format**.
	* Trường hợp muốn sử dụng định dạng mới chọn **Custom format**. Phần **Log format**, nhập định dạng bản ghi dữ liệu nhật ký bạn mong muốn. 
* (Tùy chọn) Bấm **Add Tag** để thêm tag cho luồng dữ liệu nhật ký.
* Bấm **Create**

#### 2. Đẩy luồng dữ liệu nhật ký lên Amazon S3

Trường hợp bạn muốn đẩy dữ liệu nhất ký lên Amazon S3, thì cẩn đảm bảo bucket đã được tạo sẵn trước khi sử dụng.
Các bản ghi dữ liệu nhật ký liên quan tới network interfaces sẽ được lưu trữ thành một loạt các object trên bucket. 
Trường hợp là dữ liệu nhật ký hoạt động của VPC, luồng dữ liệu nhật kỹ sẽ đẩy dữ liệu của toàn bộ the network interfaces trong VPC đó lên bucket.

**IAM policy đối với IAM principals**

Một IAM principal ví dụ như IAM user phải được gán quyền hạn để đẩy dữ liệu nhật ký lên bucket. 
Cụ thể là quyền tạo và đẩy luồng dữ liệu nhật ký lên destination mong muốn. Ví dụ bên dưới là đoạn json mô tả những quyền cần có đối với một IAM principal.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "logs:CreateLogDelivery",
        "logs:DeleteLogDelivery"
        ],
      "Resource": "*"
    }
  ]
}
```

**Phân quyền đối với bucket của Amazon S3**

Mặc định bucket và các object nằm bên trong nó đều ở chế độ private và chỉ có bucket owner mới được quyền truy cập. Tuy nhiên thì bucket owner cũng có thể gán quyền cho phép những resource khác hoặc user khác bằng cách chỉnh sửa nội dung chính sách truy cập.  
Trường hợp user tạo luồng dữ liệu nhật ký chính là owner của bucket thì AWS sẽ tự động gán những chính sách bên dưới đây và bucket để cấp quyền đẩy dữ liệu nhật ký lên trên S3. 

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AWSLogDeliveryWrite",
            "Effect": "Allow",
            "Principal": {"Service": "delivery.logs.amazonaws.com"},
            "Action": "s3:PutObject",
            "Resource": "arn:aws:s3:::bucket_name/optional_folder/AWSLogs/account_id/*",
            "Condition": {"StringEquals": {"s3:x-amz-acl": "bucket-owner-full-control"}}
        },
        {
            "Sid": "AWSLogDeliveryAclCheck",
            "Effect": "Allow",
            "Principal": {"Service": "delivery.logs.amazonaws.com"},
            "Action": "s3:GetBucketAcl",
            "Resource": "arn:aws:s3:::bucket_name"
        }
    ]
}
```

**Tạo luồng dữ liệu nhật ký đẩy lên Amazon S3**

* Truy cập trang Amazon VPC console tại địa chỉ https://console.aws.amazon.com/ec2/
* Trên thanh điều hướng bên trái, chọn **Network Interfaces** or **Your VPCs** or **Subnets**.
* Chọn một hoặc nhiều network interfaces rồi chọn **Actions, Create flow log**.
* Trong phần **Filter**, chọn loại dữ liệu sẽ được ghi nật ký. Sau đó chọn **All** để ghi nhận dữ liệu của mọi traffic được cho phép hoặc bị từ chối. Cụ thể **Rejected** là lựa chọn để ghi lại lưu lượng dữ liệu bị từ chối, hoặc **Accepted** để ghi lại lưu lượng dữ liệu được cho phép. Phần *Maximum aggregation interval**, chỉ định khoảng thời gian mà dữ liệu nhật ký được ghi lại và tổng hợp thành bản ghi dữ liệu nhật ký.
* Mục **Destination**, chọn **Send to an Amazon S3 bucket**.  
* Phần **S3 bucket ARN**, điền vào thông tin Amazon Resource Name (ARN) của bucket. Ta cũng có thể thêm vào đường dẫn thư mục con nằm bên trong bucket ARN. 
Một lưu ý đó à bucket không thể sử dụng AWSLogs làm tên thư mục con do thông tin này đã được reserved cho mục đích sử dụng khác. 
* Mục **Format**, chỉ rõ định dạng của bản ghi dữ liệu nhật ký.
	* Nếu muốn sử dụng định dạng mặc định thì chọn **AWS default format**.
	* Trường hợp muốn sử dụng định dạng mới chọn **Custom format**. Phần **Log format**, nhập định dạng bản ghi dữ liệu nhật ký bạn mong muốn. 
* (Tùy chọn) Bấm **Add Tag** để thêm tag cho luồng dữ liệu nhật ký.
* Bấm **Create**

#### 3. Xem nội dung Luồng dữ liệu nhật ký

* Truy cập trang Amazon VPC console tại địa chỉ https://console.aws.amazon.com/ec2/
* Trên thanh điều hướng bên trái, chọn **Network Interfaces** or **Your VPCs** or **Subnets**.
* Chọn một network interface, rồi chọn **Flow Logs**. Thông tin về Luồng dữ liệu nhật ký sẽ được hiển thị trong tab này.Cột **Destination type** chỉ rõ destination của luồng dữ liệu nhật ký được đưa lên.
