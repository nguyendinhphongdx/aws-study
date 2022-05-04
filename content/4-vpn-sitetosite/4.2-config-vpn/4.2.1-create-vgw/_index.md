+++
title = "Create Virtual Private GW"
date = 2022
weight = 1
chapter = false
pre = "<b>4.2.1 </b>"
+++


1. Go to the **Amazon VPC console** page at https://console.aws.amazon.com/vpc/.
  + Drag down the right navigation bar.
  + Click **Virtual Private Gateway**.
  + Click **Create Virtual Private Gateway**.
![Create VGW](/images/vpn/create-vgw.png?width=90pc)

2. At the **Create Virtual Private Gateway** page:
  + Fill in information **Name tag:** ```VPN Gateway```
  + Click **Create Virtual Private Gateway**
![Create VGW](/images/vpn/create-vgw2.png?width=90pc)
  + Click **Close**.

3. In the list of **Virtual Private Gateway**, select the **VPN Gateway** just created.
  + Select **Actions** > **Attach to VPC**.
  + Select VPC **ASG**.
  + Click **Yes, Attach**.
![Create VGW](/images/vpn/create-vgw3.png?width=90pc)

{{%notice tip%}}
Note: according to the architectural model, the Virtual Private Gateway will reside in the VPC on the AWS environment. Currently, we are using VPC **ASG** to act as the AWS environment and VPC **ASG VPN** to emulate the on-premise environment. (Traditional Data Center)
{{%/notice%}}