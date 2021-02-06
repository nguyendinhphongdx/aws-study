+++
title = "Publish to CW Logs"
date = 2020
weight = 1
chapter = false
pre = "<b>2.2.1. </b>"
+++

**Contents:**
- [Create an IAM role for flow logs](#create-an-iam-role-for-flow-logs)
- [Creating a flow log that publishes to CloudWatch Logs](#creating-a-flow-log-that-publishes-to-cloudwatch-logs)

#### Create an IAM role for flow logs

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


#### Creating a flow log that publishes to CloudWatch Logs

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
