+++
title = "Đẩy Flow Log lên S3"
date = 2020
weight = 2
chapter = false
pre = "<b>2.2.2. </b>"
+++

**Nội dung:**
- [IAM policy đối với IAM principals](#iam-policy-đối-với-iam-principals)
- [Phân quyền đối với bucket của Amazon S3](#phân-quyền-đối-với-bucket-của-amazon-s3)
- [Tạo luồng dữ liệu nhật ký đẩy lên Amazon S3](#tạo-luồng-dữ-liệu-nhật-ký-đẩy-lên-amazon-s3)
- [Xem nội dung Luồng dữ liệu nhật ký](#xem-nội-dung-luồng-dữ-liệu-nhật-ký)

#### IAM policy đối với IAM principals

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

#### Phân quyền đối với bucket của Amazon S3

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

#### Tạo luồng dữ liệu nhật ký đẩy lên Amazon S3

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

#### Xem nội dung Luồng dữ liệu nhật ký

* Truy cập trang Amazon VPC console tại địa chỉ https://console.aws.amazon.com/ec2/
* Trên thanh điều hướng bên trái, chọn **Network Interfaces** or **Your VPCs** or **Subnets**.
* Chọn một network interface, rồi chọn **Flow Logs**. Thông tin về Luồng dữ liệu nhật ký sẽ được hiển thị trong tab này.Cột **Destination type** chỉ rõ destination của luồng dữ liệu nhật ký được đưa lên.