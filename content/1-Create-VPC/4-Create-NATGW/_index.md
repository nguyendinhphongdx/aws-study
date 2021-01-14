+++
title = "Create NAT Gateway"
date = 2020
weight = 4
chapter = false
pre = "<b>1.4. </b>"
+++

By default, any instance that you launch into a private subnet in an Amazon VPC is not able to communicate with the Internet through the IGW. This is problematic if the instances within private subnets need direct access to the Internet from the Amazon VPC in order to apply security updates, download patches, or update application software.  
AWS provides NAT instances and NAT gateways to allow instances deployed in private subnets to gain Internet access. For common use cases, we recommend that you use a NAT gateway instead of a NAT instance. The NAT gateway provides better availability and higher bandwidth, and requires less administrative effort than NAT instances.

To create a NAT gateway, you must specify a subnet and an Elastic IP address. Ensure that the Elastic IP address is currently not associated with an instance or a network interface.  

If you are migrating from a NAT instance to a NAT gateway and you want to reuse the NAT instance's Elastic IP address, you must first disassociate the address from your NAT instance.

**Contents:**
- [Create NAT Gateway](#create-nat-gateway)
- [Add route to Route Table](#add-route-to-route-table)

#### Create NAT Gateway

* Open the Amazon VPC console at https://console.aws.amazon.com/vpc/
* In the navigation pane, choose **NAT Gateways**, **Create NAT Gateway**.
* Specify the subnet in which to create the NAT gateway, and select the allocation ID of an Elastic IP address to associate with the NAT gateway.
* [Add a tag] Choose **Add tag** and do the following:
    * For **Key**, enter the key name.
    * For **Value**, enter the key value.
* Choose **Create a NAT Gateway**.
* The NAT gateway displays in the console. After a few moments, its status changes to Available, after which it's ready for you to use.

#### Add route to Route Table

After you've created your NAT gateway, you must update your route tables for your private subnets to point internet traffic to the NAT gateway. We use the most specific route that matches the traffic to determine how to route the traffic (longest prefix match).

* Open the Amazon VPC console at https://console.aws.amazon.com/vpc/
* In the navigation pane, choose **Route Tables**.
* Select the route table associated with your private subnet and choose **Routes, Edit**.
* Choose **Add another route**. For **Destination**, enter `0.0.0.0/0`. For **Target**, select the ID of your NAT gateway.
* Choose **Save**.

To ensure that your NAT gateway can access the internet, the route table associated with the subnet in which your NAT gateway resides must include a route that points internet traffic to an internet gateway. 
