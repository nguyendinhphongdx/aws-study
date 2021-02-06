+++
title = "Configure VPC Flow Logs"
date = 2020
weight = 2
chapter = false
pre = "<b>2.2. </b>"
+++

You can create flow logs for your VPCs, subnets, or network interfaces. Flow logs can publish data to CloudWatch Logs or Amazon S3. 

**Contents:**
- [1. Publishing flow logs to CloudWatch Logs](#1-publishing-flow-logs-to-cloudwatch-logs)
- [2. Publishing flow logs to Amazon S3](#2-publishing-flow-logs-to-amazon-s3)

#### 1. Publishing flow logs to CloudWatch Logs

When publishing to CloudWatch Logs, flow log data is published to a log group, and each network interface has a unique log stream in the log group. Log streams contain flow log records. You can create multiple flow logs that publish data to the same log group. If the same network interface is present in one or more flow logs in the same log group, it has one combined log stream. If you've specified that one flow log should capture rejected traffic, and the other flow log should capture accepted traffic, then the combined log stream captures all traffic.

> [**Publishing flow logs to CloudWatch Logs**](1-publish-cw-logs/)

#### 2. Publishing flow logs to Amazon S3

When publishing to Amazon S3, flow log data is published to an existing Amazon S3 bucket that you specify. Flow log records for all of the monitored network interfaces are published to a series of log file objects that are stored in the bucket. If the flow log captures data for a VPC, the flow log publishes flow log records for all of the network interfaces in the selected VPC.

> [**Publishing flow logs to Amazon S3**](2-publish-s3/)
