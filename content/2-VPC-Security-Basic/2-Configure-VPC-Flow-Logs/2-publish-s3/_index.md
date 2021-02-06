+++
title = "Publish to S3"
date = 2020
weight = 2
chapter = false
pre = "<b>2.2.2. </b>"
+++

**Contents:**
- [IAM policy for IAM principals](#iam-policy-for-iam-principals)
- [Amazon S3 bucket permissions](#amazon-s3-bucket-permissions)
- [Creating a flow log that publishes to Amazon S3](#creating-a-flow-log-that-publishes-to-amazon-s3)

#### IAM policy for IAM principals

An IAM principal in your account, such as an IAM user, must have sufficient permissions to publish flow logs to the Amazon S3 bucket. This includes permissions to work with specific logs: actions to create and publish the flow logs. The IAM policy must include the following permissions. 

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

#### Amazon S3 bucket permissions

By default, Amazon S3 buckets and the objects they contain are private. Only the bucket owner can access the bucket and the objects stored in it. However, the bucket owner can grant access to other resources and users by writing an access policy.  
If the user creating the flow log owns the bucket, AWS automatically attach the following policy to the bucket to give the flow log permission to publish logs to it. 

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

#### Creating a flow log that publishes to Amazon S3

* Open the Amazon EC2 console at https://console.aws.amazon.com/ec2/
* In the navigation pane, choose **Network Interfaces** or **Your VPCs** or **Subnets**.
* Select one or more network interfaces and choose **Actions, Create flow log**.
* For **Filter**, specify the type of IP traffic data to log. Choose **All** to log accepted and rejected traffic, **Rejected** to record only rejected traffic, or **Accepted** to record only accepted traffic.
* For **Maximum aggregation interval**, choose the maximum period of time during which a flow is captured and aggregated into one flow log record.
* For **Destination**, choose **Send to an Amazon S3 bucket**. 
* For **S3 bucket ARN**, specify the Amazon Resource Name (ARN) of an existing Amazon S3 bucket. You can include a subfolder in the bucket ARN. The bucket cannot use AWSLogs as a subfolder name, as this is a reserved term. 
* For **Format**, specify the format for the flow log record.
	* To use the default flow log record format, choose **AWS default format**.
	* To create a custom format, choose **Custom format**. For **Log format**, choose the fields to include in the flow log record. 
* (Optional) Choose **Add Tag** to apply tags to the flow log.
* Choose **Create**
