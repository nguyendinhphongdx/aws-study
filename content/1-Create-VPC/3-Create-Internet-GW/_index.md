+++
title = "Create Internet Gateway"
date = 2020
weight = 3
chapter = false
pre = "<b>1.3. </b>"
+++

An Internet Gateway (IGW) is a horizontally scaled, redundant, and highly available Amazon VPC component that allows communication between instances in your Amazon VPC and the Internet. An IGW provides a target in your Amazon VPC route tables for Internet-routable traffic, and it performs network address translation for instances that have been assigned public IP addresses.  
Amazon EC2 instances within an Amazon VPC are only aware of their private IP addresses. When traffic is sent from the instance to the Internet, the IGW translates the reply address to the instance’s public IP address (or EIP address, covered later) and maintains the one-to-one map of the instance private IP address and public IP address. When an instance receives traffic from the Internet, the IGW translates the destination address (public IP address) to the instance’s private IP address and forwards the traffic to the Amazon VPC.

To add an internet gateway to your VPC, follow steps below:
- [Creating a subnet](#creating-a-subnet)
- [Creating and attaching an internet gateway](#creating-and-attaching-an-internet-gateway)
- [Creating a custom route table](#creating-a-custom-route-table)
- [Creating a security group for internet access](#creating-a-security-group-for-internet-access)
- [Adding Elastic IP addresses](#adding-elastic-ip-addresses)


#### Creating a subnet

* Open the Amazon VPC console at https://console.aws.amazon.com/vpc/
* In the navigation pane, choose **Subnets**, and then choose **Create Subnet**.
* In the **Create Subnet** dialog box, select the VPC, select the Availability Zone, and specify the IPv4 CIDR block for the subnet.
* (Optional, IPv6 only) For **IPv6 CIDR block**, choose **Specify a custom IPv6 CIDR**.
* Choose **Yes, Create**.

#### Creating and attaching an internet gateway

After you create an internet gateway, attach it to your VPC.

* Open the Amazon VPC console at https://console.aws.amazon.com/vpc/
* In the navigation pane, choose **Internet Gateways**, and then choose **Create internet gateway**.
* Optionally name your internet gateway, and then choose **Create**.
* Select the internet gateway that you just created, and then choose **Actions, Attach to VPC**.
* Select your VPC from the list, and then choose **Attach**.

#### Creating a custom route table

When you create a subnet, it automatically associate it with the main route table for the VPC. By default, the main route table doesn't contain a route to an internet gateway. The following procedure creates a custom route table with a route that sends traffic destined outside the VPC to the internet gateway, and then associates it with your subnet.

* Open the Amazon VPC console at https://console.aws.amazon.com/vpc/
* In the navigation pane, choose **Route Tables**, and then choose **Create Route Table**.
* In the **Create Route Table** dialog box, optionally name your route table, then select your VPC, and then choose **Yes, Create**.
* Select the custom route table that you just created. The details pane displays tabs for working with its routes, associations, and route propagation.
* On the **Routes** tab, choose **Edit, Add another route**, and add the following routes as necessary. Choose **Save** when you're done.
    * For IPv4 traffic, specify `0.0.0.0/0` in the **Destination** box, and select the internet gateway ID in the Target list.
    * For IPv6 traffic, specify `::/0` in the **Destination** box, and select the internet gateway ID in the Target list.
* On the **Subnet Associations** tab, choose **Edit**, select the **Associate** check box for the subnet, and then choose Save.

#### Creating a security group for internet access

By default, a VPC security group allows all outbound traffic. You can create a new security group and add rules that allow inbound traffic from the internet. You can then associate the security group with instances in the public subnet.

* Open the Amazon VPC console at https://console.aws.amazon.com/vpc/
* In the navigation pane, choose **Security Groups**, and then choose **Create Security Group**.
* In the **Create Security Group** dialog box, specify a name for the security group and a description. Select the ID of your VPC from the VPC list, and then choose **Yes, Create**.
* Select the security group. The details pane displays the details for the security group, plus tabs for working with its inbound rules and outbound rules.
  * On the **Inbound Rules** tab, choose **Edit**.
  * Choose **Add Rule**, and complete the required information. For example, select **HTTP** or **HTTPS** from the **Type** list, and enter the **Source** as `0.0.0.0/0` for IPv4 traffic, or `::/0` for IPv6 traffic.
  * Choose **Save** when you're done.
* Open the Amazon EC2 console at https://console.aws.amazon.com/ec2/
* In the navigation pane, choose **Instances**.
* Select the instance, choose **Actions**, then **Networking**, and then select **Change Security Groups**.
* In the **Change Security Groups** dialog box, clear the check box for the currently selected security group, and select the new one. Choose **Assign Security Groups**.

#### Adding Elastic IP addresses

After you've launched an instance into the subnet, you must assign it an Elastic IP address if you want it to be reachable from the internet over IPv4.

* Open the Amazon VPC console at https://console.aws.amazon.com/vpc/
* In the navigation pane, choose **Elastic IPs**.
* Choose **Allocate new address**.
* Choose **Allocate**.
* Select the Elastic IP address from the list, choose **Actions**, and then choose **Associate address**.
* Choose **Instance** or **Network interface**, and then select either the instance or network interface ID. Select the private IP address with which to associate the Elastic IP address, and then choose **Associate**.
