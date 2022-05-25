---
title : "Clean up resources"
date : "`r Sys.Date()`"
weight : 6
chapter : false
pre : " <b> 6. </b> "
---
#### Clean up resources

We will proceed to delete the resources in the following order

### Terminate EC2 Instances.

- Access the Amazon EC2 console at https://console.aws.amazon.com/ec2/.
- On the left navigation bar, select Instances
- Select all EC2 Instances related to the lab.
- Click Actions.
- Click Manage Instance State.
- Select Terminate.
- Click Change State

![Create VPC](/images/7-cleanup/0001-cleanup.png?featherlight=false&width=90pc)

#### Remove NAT Gateway and Elastic IP Address

- Remove NAT Gateway and Elastic IP Address. AWS charges for wasted EIPs, so you need to double-check to avoid unintended charges.
- Visit the Amazon VPC console page at https://console.aws.amazon.com/vpc/
- On the left navigation bar, click NAT Gateway.
- Select NAT Gateway.
- Click Action.
- Click Delete NAT Gateway.
- Type delete.
- Click Delete to confirm deletion of NAT Gateway


![Create VPC](/images/7-cleanup/0002-cleanup.png?featherlight=false&width=90pc)

#### Delete delete Elastic IP Address.
- Continue to delete Elastic IP Address.
- Visit the Amazon VPC console page at https://console.aws.amazon.com/vpc/
- On the left navigation bar, click Elastic IP.
- Select the Elastic IP Address we created.
- Click Action.
- Click Release Elastic IP Address
- Click Release.

![Create VPC](/images/7-cleanup/0003-cleanup.png?featherlight=false&width=90pc)

#### Continue to do the same and delete in the following order:
- VPN Site to Site connection.
- Virtual Private Gateway.
- Customer Gateway.
- VPC ASG VPN
- VPC ASG.