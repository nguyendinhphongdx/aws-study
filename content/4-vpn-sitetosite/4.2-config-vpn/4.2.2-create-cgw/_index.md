+++
title = "Create Customer GW"
date = 2021
weight = 2
chapter = false
pre = "<b>4.2.2 </b>"
+++


1. Go to **Amazon VPC console** page at https://console.aws.amazon.com/vpc/.
  + Drag down the right navigation bar.
  + Click **Customer Gateways**.
  + Click **Create Customer Gateway**.
![Create CGW](/images/vpn/create-cgw.png?width=90pc)

2. At the **Create Customer Gateway** page:
  + Fill in information **Name :** ```Customer Gateway```
  + In the **Routing** section, make sure the configuration is set to **static** by default.
  + In the **IP Address** field, enter the public IP address of the EC2 server **Customer Gateway**.
  + Click **Create Customer Gateway**.
![Create EC2](/images/vpn/create-vpnec24.png?width=90pc)
![Create CGW](/images/vpn/create-cgw2.png?width=90pc)
  + Click **Close**.

{{%notice tip%}}
Note: according to the architectural model, the Customer Gateway will reside in the VPC on the onpremise environment. What we are currently doing is declaring to AWS that we will have a Customer Gateway with a public IP address that is the public address of the EC2 instance: **Customer Gateway** located in the **ASG VPN** VPC.
{{%/notice%}}