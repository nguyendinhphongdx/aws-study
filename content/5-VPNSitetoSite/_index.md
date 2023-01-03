---
title : "Configuring Site to Site VPN"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 5. </b> "
---

We can connect the On-premise data center to Amazon VPC using a hard or soft VPN depending on the purpose and actual use needs.
To establish a Site-to-Site VPN connection, we will need to create and configure a Virtual Private Gateway **VPG** and Customer Gateway **CGW**.

* Virtual Private Gateway (**VPG**) is a control center connecting the virtual private network (VPN) installed on top of AWS.
* A Customer Gateway (**CGW**) is the component representing the hard or soft VPN device installed at the Client end.

**VPN tunnel** will be established as soon as data traffic is transferred between AWS and the customer's network. In that connection, you must specify the type of routing that will be used to ensure the safety and quality of data transmission.
If the CGW on the client side supports Border Gateway Protocol (BGP), then in the VPN connection configuration we are required to set the routing to dynamic routing.
Otherwise, we must configure the connection routing as static routing. In the case of using static routing, you must enter the exact routes required for the connection from the Client side to the VPG set up at the AWS end. Also, routing for the VPC must be configured to propagate to allow resources to exchange data in and out of the VPN tunnel connection between AWS and the Customer's network.

Amazon VPC offers many types of CGWs, and each CGW is assigned to a VPG, but 1 VPG can be associated with multiple CGWs (many-to-one design). To support this model, the CGW's IP address must be unique within a region.
Amazon VPC also provides the necessary information for Network Administrators to be able to configure the CGW and establish a VPN connection to the VPG on AWS. VPN connection always includes 2 Internet Protocol Security (IPSec) tunnels to ensure high availability of the connection.
Below are the important features that we need to know about **VPG**, **CGW**, and **VPN**:
* VPG is the terminal component of the VPN tunnel located on AWS.
* CGW can be a hardware device or a software application located at the Client end in the VPN tunnel connection.
* You must initiate a VPN tunnel connection from CGW to VPG.
* VPG supports both dynamic routing (BGP) and static routing.
* VPN connection always has 2 tunnels to ensure high availability for connection to VPC from the Client site.

The lab helps us learn how to set up a Site-to-Site VPN connection in AWS. This solution is quite popular due to the advantages of low cost, and ease of configure because AWS provides instructions for each type of device at the Customer end. The customer's only concern is to prepare the internet from which to create a secret secure tunnel (using IPSec) connecting to AWS through the AWS VPN tunnel.
In the scope of the lab, assume that we have the Main office ( VPC **ASG** ) and Branch office ( VPC **ASG VPN** ) located at 2 VPCs belonging to 2 different AZs to have a difference in terms of the network. On each VPC doing EC2 creation allows external SSH but without the ability to connect and ping each other using each EC2's Private IP address. All we need to do is configure the VPN so that the Private IP addresses can ping each other using the Site-to-Site VPN.

![VPN](/images/6-VPNSitetoSite/vpn.png?featherlight=false&width=90pc)


**Content:**
1. [Create **ASG VPN** VPC and subnet](5.1-createvpnenv/)
2. [Configure Site to Site VPN and test connection with private IP ](5.2-vpnsitetosite/)