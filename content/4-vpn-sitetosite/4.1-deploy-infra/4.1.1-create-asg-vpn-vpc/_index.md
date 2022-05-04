+++
title = "Create ASG VPN VPC"
date = 2021
weight = 1
chapter = false
pre = "<b>4.1.1 </b>"
+++

 
#### Create ASG VPN VPC

1. Access the Amazon VPC console at https://console.aws.amazon.com/vpc/
2. In the navigation pane, Select **Your VPCs**, **Create VPC**.
  + **Name tag** enter the VPC name: `ASG VPN`
  + **IPv4 CIDR block** fill in the IP block range of the VPC: `10.11.0.0/16`
  + Click **Create VPC**.
![Create VPN VPC](/images/vpn/create-asgvpn.png?width=90pc)

{{%notice warning%}}
The Tennacy configuration section we will leave the default mechanism. If we switch to **Dedicated** there will be some EC2 Instance type mismatch and will not be created in VPC with tennacy mode of **Dedicated**
{{%/notice%}}

3. On the left navigation bar, select **Subnets**.
4. Select **Create Subnet**.
  + Select the VPC named **ASG VPN** we just created.
  + **Name tag** enter subnet name: `VPN Public`
  + Select **Availability Zone**: `ap-southeast-1a`
  + Select **IPv4 CIDR block** as `10.11.1.0/24` according to the described architecture.
![Create VPN VPC](/images/vpn/create-asgvpn2.png?width=90pc)
  + Scroll down, click **Create subnet** to proceed with creating **VPN Public** with CIDR of **10.11.1.0/24** located in Availability Zone **ap-southeast-1a**.

5. In the subnet management interface.
  + Click on **VPN Public**.
  + Click action.
  + Click **Modify auto-assign IP settings**.
6. Click **Enable auto-assign public IPv4 address** and Click **Save**.

#### Create an Internet Gateway for ASG VPC VPN

1. Access the Amazon VPC console.
2. On the left navigation bar, select **Internet Gateways**, **Create Internet Gateways**
  + Name the Internet Gateway as **Internet Gateway**.
  + Click **Create Internet Gateway**.
3. Next we need to Attach Internet Gateway to VPC **ASG VPN**.
  + Click **Action**.
  + Click **Attach to VPC**.
  + Click on VPC **ASG VPN** , VPC ID will be automatically filled in.
  + Click **Attach Internet Gateway**.
![Create VPN VPC](/images/vpn/create-asgvpn4.png?width=90pc)

4. Next we need to create a Route Table that routes out to the internet through the Internet Gateway.
  + On the left navigation bar, select **Route Tables**, **Create Route Table**

5. Enter the Route Table name in the **Name tag** section: `Route table VPN - Public`.
  + Select **VPC** named **ASG VPN** , VPC id will be automatically filled in.
  + Click **Create**.
![Create VPN VPC](/images/vpn/create-asgvpn5.png?width=90pc)
  + Click **Close**.

6. Select **Route table VPN - Public**, click **Action** , click **Edit routes**.

  + Click **Add route**.
  + Fill in the **Destination** CIDR : `0.0.0.0/0` representing the Internet.
  + In the **Target** section, click **Internet Gateway**, then select the Internet Gateway we created. The Internet Gateway ID will be automatically filled in.
  + Select **Save routes**

7. Make sure **Route table - Public** is selected.
  + Click on the **Subnet Associations** tab in the menu below.
  + Click **Edit subnet associations**.
  + Expand the **Subnet ID** column by dragging the pane to the right.
  + Select subnet **VPN Public**.
  + Click **Save**.
![Create VPN VPC](/images/vpn/create-asgvpn6.png?width=90pc)

* So we have done:
  + Create Internet Gateway named **Internet Gateway**.
  + Create a Route table named **Route table VPN - Public**.
  + Edit route in route table **Route table VPN - Public**.
    + Add route entry Destination: 0.0.0.0/0 Target: Internet Gateway.
  + Associate **Route table VPN - Public** to subnet **VPN Public**.