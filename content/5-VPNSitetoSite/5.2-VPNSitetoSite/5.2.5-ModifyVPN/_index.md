---
title : "Modify AWS VPN Tunnel"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 5.2.5 </b> "
---

#### Modify AWS VPN Tunnel

1. Access to **VPC** interface

   - Select **Site-to-Site VPN connections**
   - Select **VPN** just created.
   - Select **Actions**
   - Select **Modify VPN tunnel options**

![Create VPC](/images/13/00023.png?featherlight=false&width=90pc)

2. Select **VPN Tunnel outside IP address**

![Create VPC](/images/13/00024.png?featherlight=false&width=90pc)

3. Select **Confirm UP tunnel modification** and the rest of the parameters are default.

![Create VPC](/images/13/00025.png?featherlight=false&width=90pc)

4. For **Tunnel activity log**, select **Enable**

   - Select **Amazon CloudWatch log group** (if not already you can create in CloudWatch)
   - For **Output format**, select **text**
   - Select **Save changes**

![Create VPC](/images/13/00026.png?featherlight=false&width=90pc)

5. Access to **CloudWatch**

   - Select **Log groups**
   - Select **Log streams**
   - Select a stream.

![Create VPC](/images/1300027.png?featherlight=false&width=90pc)

6. Go to **Log events**

![Create VPC](/images/13/00028.png?featherlight=false&width=90pc)

7. You do the same with the remaining tunnel.

![Create VPC](/images/13/00029.png?featherlight=false&width=90pc)

8. Make sure both tunnels are **UP**

![Create VPC](/images/13/00030.png?featherlight=false&width=90pc)