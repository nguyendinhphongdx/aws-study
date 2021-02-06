+++
title = "Đẩy Flow Log lên CW Logs"
date = 2020
weight = 1
chapter = false
pre = "<b>2.2.1. </b>"
+++

**Nội dung:**
- [Tạo IAM role cho flow logs](#tạo-iam-role-cho-flow-logs)
- [Tạo Luồng dữ liệu nhật kí hiển thị trên CloudWatch Logs](#tạo-luồng-dữ-liệu-nhật-kí-hiển-thị-trên-cloudwatch-logs)
- [Xem nội dung Luồng dữ liệu nhật ký](#xem-nội-dung-luồng-dữ-liệu-nhật-ký)

#### Tạo IAM role cho flow logs

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


#### Tạo Luồng dữ liệu nhật kí hiển thị trên CloudWatch Logs

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

#### Xem nội dung Luồng dữ liệu nhật ký

* Truy cập trang Amazon VPC console tại địa chỉ https://console.aws.amazon.com/ec2/
* Trên thanh điều hướng bên trái, chọn **Network Interfaces** or **Your VPCs** or **Subnets**.
* Chọn một network interface, rồi chọn **Flow Logs**. Thông tin về Luồng dữ liệu nhật ký sẽ được hiển thị trong tab này.Cột **Destination type** chỉ rõ destination của luồng dữ liệu nhật ký được đưa lên.