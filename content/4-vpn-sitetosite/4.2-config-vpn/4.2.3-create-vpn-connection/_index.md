+++
title = "Create VPN Connection"
date = 2022
weight = 3
chapter = false
pre = "<b>4.2.3 </b>"
+++


1. Go to **Amazon VPC console** page at https://console.aws.amazon.com/vpc/
  + Drag the navigation bar down.
  + Click **Site-to-Site VPN Connections**.
  + Click **Create VPN Connection**.
![Create CGW](/images/vpn/create-vpn1.png?width=90pc)

2. At the **Create VPN Connection** page, fill in the information:
  + **Name tag:** ```VPN Connection```
  + **Target Gateway Type:** Select **Virtual Private Gateway**
  + **Virtual Private Gateway:** Select **VPN Gateway**
  + **Customer Gateway:** Existing
  + **Customer Gateway ID:** Select **Customer Gateway**
  + **Routing Options:** Static
  + **Static IP Prefixes:** ```10.11.0.0/16```. This is the IP address resolution in a simulated Onpremise environment.
  + The other configurations keep the default.
![Create CGW](/images/vpn/create-vpn2.png?width=90pc)
  + Drag the screen to the bottom and click **Create VPN Connection**.
  + Click **Close** after VPN Connection has been created successfully.

3. Configure propagation for route tables
  + Visit **Amazon VPC console** page at https://console.aws.amazon.com/vpc/
  + In the left menu, click **Route Tables**.
  + Click on route table **Route table - Public**.
  + Click the **Route Propagation** tab.
  + Click **Edit route propagation**.
![Create CGW](/images/vpn/create-vpn3.png?width=90pc)
  + Click on **Enable** box.
  + Click **Save** to save the Route table.
![Create CGW](/images/vpn/create-vpn4.png?width=90pc)
4. Do the same step 3 for **Route table - Private**. Check Propagation status is **Yes** as shown below.
![Create CGW](/images/vpn/create-vpn5.png?width=90pc)

5. Next we will install and configure Customer Gateway software using Open Swan.