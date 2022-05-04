+++
title = "Clean up resources"
date = 2022
weight = 5
chapter = false
pre = "<b>5. </b>"
+++

We will proceed to delete the resources in the following order

1. Terminate the EC2 Instance.
  + Access the Amazon EC2 console at https://console.aws.amazon.com/ec2/.
  + On the left navigation bar, select **Intances**
  Select all EC2 Instances related to the lab.
  + Click **Actions**.
  + Click **Manage Instance State**.
  + Select **Terminate**.
  + Click **Change State**
 ![Configure VPN](/images/vpn/clean2.png?width=90pc)

2. Remove NAT Gateway and Elastic IP Address. AWS charges for wasted EIPs, so you need to double-check to avoid unintended charges.
  + Visit **Amazon VPC console** page at https://console.aws.amazon.com/vpc/
  + On the left navigation bar, click **NAT Gateway**.
  + Select **NAT Gateway**.
  + Click **Action**.
  + Click **Delete NAT Gateway**.
  + Type **delete**.
  + Click **Delete** to confirm deletion of **NAT Gateway**.
![Configure VPN](/images/vpn/clean3.png?width=90pc)

3. Continue to delete Elastic IP Address.
  + Visit **Amazon VPC console** page at https://console.aws.amazon.com/vpc/
  + On the left navigation bar, click **Elastic IP**.
  + Select the Elastic IP Address we created.
  + Click **Action**.
  + Click **Release Elastic IP Address**.
![Configure VPN](/images/vpn/clean4.png?width=90pc)
  + Click **Release**.

4. Continue to do the same and delete in the following order:
 + VPN Site to Site connection.
 + Virtual Private Gateway.
 + Customer Gateway.
 + VPC **ASG VPN**.
 ![Configure VPN](/images/vpn/clean5.png?width=90pc)
 + VPC **ASG**.