+++
title = "Setup Site to Site VPN"
date = 2020
weight = 3
chapter = false
pre = "<b>2.3. </b>"
+++

You can connect an existing data center to Amazon VPC using either hardware or software VPN connections, which will make Amazon VPC an extension of the data center. Amazon VPC offers two ways to connect a corporate network to a VPC: **VPG** and **CGW**.
* A Virtual Private Gateway (**VPG**) is the virtual private network (VPN) concentrator on the AWS side of the VPN connection between the two networks. 
* A Customer Gateway (**CGW**) represents a physical device or a software application on the customer’s side of the VPN connection.

The **VPN tunnel** is established after traffic is generated from the customer’s side of the VPN connection.  
You must specify the type of routing that you plan to use when you create a VPN connection. If the CGW supports Border Gateway Protocol (BGP), then configure the VPN connection for dynamic routing. Otherwise, configure the connections for static routing. If you will be using static routing, you must enter the routes for your network that should be communicated to the VPG. Routes will be `propagated` to the Amazon VPC to allow your resources to route network traffic back to the corporate network through the VGW and across the VPN tunnel.

Amazon VPC also supports multiple CGWs, each having a VPN connection to a single VPG (many-to-one design). In order to support this topology, the CGW IP addresses must be unique within the region. Amazon VPC will provide the information needed by the network administrator to configure the CGW and establish the VPN connection with the VPG. The VPN connection consists of two Internet Protocol Security (IPSec) tunnels for higher availability to the Amazon VPC.  
Following are the important points to understand about **VPG**, **CGW**, and **VPN**:
* The VPG is the AWS end of the VPN tunnel. 
* The CGW is a hardware or software application on the customer’s side of the VPN tunnel.
* You must initiate the VPN tunnel from the CGW to the VPG.
* VPGs support both dynamic routing with BGP and static routing.
* The VPN connection consists of two tunnels for higher availability to the VPC.

Let’s build a lab together then we have a better understanding about VPN Site-to-site.  
In this lab, assume that we have Main office and Branch office lies on 2 VPCs belonging to 2 different AZs to make network difference from 2 sites. On each VPC, we create 2 EC2 to allow SSH from outside, but not able to connect and ping each other using the Private IP address of each EC2. What we need to do is configure the VPN so that Private IP addresses can ping each other using Site-to-Site VPN.

![Lab Diagram](/images/3/0.png)

**Contents:**
- [1. Deploy Infrastructure](1-deploy-infra/)
- [2. Configure Site to Site VPN](2-config-vpn/)
- [3. Testing](3-testing/)
