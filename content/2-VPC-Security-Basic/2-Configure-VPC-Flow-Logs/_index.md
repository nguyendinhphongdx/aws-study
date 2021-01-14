+++
title = "Configure VPC Flow Logs"
date = 2020
weight = 1
chapter = false
pre = "<b>2.2. </b>"
+++

You can create flow logs for your VPCs, subnets, or network interfaces. Flow logs can publish data to CloudWatch Logs or Amazon S3. 

**Contents:**
- [1. Publishing flow logs to CloudWatch Logs](#1-publishing-flow-logs-to-cloudwatch-logs)
- [2. Publishing flow logs to Amazon S3](#2-publishing-flow-logs-to-amazon-s3)
- [3. Viewing flow logs](#3-viewing-flow-logs)

#### 1. Publishing flow logs to CloudWatch Logs

When publishing to CloudWatch Logs, flow log data is published to a log group, and each network interface has a unique log stream in the log group. Log streams contain flow log records. You can create multiple flow logs that publish data to the same log group. If the same network interface is present in one or more flow logs in the same log group, it has one combined log stream. If you've specified that one flow log should capture rejected traffic, and the other flow log should capture accepted traffic, then the combined log stream captures all traffic.

**Create an IAM role for flow logs**

* Open the IAM console at https://console.aws.amazon.com/iam/
* In the navigation pane, choose **Roles, Create role**.
* Choose **EC2** as the service to use this role. For **Use case**, choose **EC2**. Choose **Next: Permissions**.
* On the **Attach permissions policies** page, choose **Next: Tags** and optionally add tags. Choose **Next: Review**.
* Enter a name for your role (for example, Flow-Logs-Role) and optionally provide a description. Choose **Create role**.
* Select the name of your role. For **Permissions**, choose **Add inline policy, JSON**.
* Copy the below policy and paste it in the window. Choose Review policy.

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
* Enter a name for your policy, and choose **Create policy**.
* Select the name of your role. For **Trust relationships**, choose **Edit trust relationship**. In the existing policy document, change the service from `ec2.amazonaws.com` to `vpc-flow-logs.amazonaws.com`. Choose **Update Trust Policy**.
* On the **Summary** page, note the ARN for your role. You need this ARN when you create your flow log.


**Creating a flow log that publishes to CloudWatch Logs**

* Open the Amazon EC2 console at https://console.aws.amazon.com/ec2/
* In the navigation pane, choose **Network Interfaces** or **Your VPCs** or **Subnets**.
* Select one or more network interfaces and choose **Actions, Create flow log**.
* For **Filter**, specify the type of IP traffic data to log. Choose **All** to log accepted and rejected traffic, **Rejected** to record only rejected traffic, or **Accepted** to record only accepted traffic.
* For **Maximum aggregation interval**, choose the maximum period of time during which a flow is captured and aggregated into one flow log record.
* For **Destination**, choose **Send to CloudWatch Logs**.
* For **Destination log group**, enter the name of a log group in CloudWatch Logs to which the flow logs are to be published. If you specify the name of a log group that does not exist, AWS will create the log group for you.
* For **IAM role**, specify the name of the role that has permissions to publish logs to CloudWatch Logs.
* For **Format**, specify the format for the flow log record.
	* To use the default flow log record format, choose **AWS default format**.
	* To create a custom format, choose **Custom format**. For **Log format**, choose the fields to include in the flow log record. 
* (Optional) Choose **Add Tag** to apply tags to the flow log.
* Choose **Create**

#### 2. Publishing flow logs to Amazon S3

When publishing to Amazon S3, flow log data is published to an existing Amazon S3 bucket that you specify. Flow log records for all of the monitored network interfaces are published to a series of log file objects that are stored in the bucket. If the flow log captures data for a VPC, the flow log publishes flow log records for all of the network interfaces in the selected VPC.

**IAM policy for IAM principals**

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

**Amazon S3 bucket permissions**

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

**Creating a flow log that publishes to Amazon S3**

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

#### 3. Viewing flow logs

* Open the Amazon EC2 console at https://console.aws.amazon.com/ec2/
* In the navigation pane, choose **Network Interfaces** or **Your VPCs** or **Subnets**.
* Select a network interface, and choose **Flow Logs**. Information about the flow logs is displayed on the tab. The **Destination type** column indicates the destination to which the flow logs are published.
