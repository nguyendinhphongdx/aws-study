+++
title = "Security Group"
date = 2020
weight = 1
chapter = false
pre = "<b>2.1.1. </b>"
+++

**Contents:**
- [Security group basics](#security-group-basics)
- [Security group rules](#security-group-rules)
- [Create a Security Group](#create-a-security-group)
- [Adding a rules](#adding-a-rules)

#### Security group basics

There are some basic characteristics of Security groups for your VPC:
* You can specify allow rules, but not deny rules.
* You can specify separate rules for inbound and outbound traffic.
* When you create a security group, it has no inbound rules. Therefore, no inbound traffic originating from another host to your instance is allowed until you add inbound rules to the security group.
* By default, a security group includes an outbound rule that allows all outbound traffic. You can remove the rule and add outbound rules that allow specific outbound traffic only. If your security group has no outbound rules, no outbound traffic originating from your instance is allowed.
* Security groups are stateful — if you send a request from your instance, the response traffic for that request is allowed to flow in regardless of inbound security group rules. Responses to allowed inbound traffic are allowed to flow out, regardless of outbound rules.
* Instances associated with a security group can't talk to each other unless you add rules allowing the traffic (exception: the default security group has these rules by default).
* Security groups are associated with network interfaces. After you launch an instance, you can change the security groups that are associated with the instance, which changes the security groups associated with the primary network interface (eth0).

#### Security group rules

A rule applies either to inbound traffic (ingress) or outbound traffic (egress. You can grant access to a specific CIDR range, or to another security group in your VPC or in a peer VPC (requires a VPC peering connection).

There are few basic parts of a security group rule in a VPC:
* (Inbound rules only) The source of the traffic and the destination port or port range. The source can be another security group, an IPv4 or IPv6 CIDR block, or a single IPv4 or IPv6 address.
* (Outbound rules only) The destination for the traffic and the destination port or port range. The destination can be another security group, an IPv4 or IPv6 CIDR block, a single IPv4 or IPv6 address, or a prefix list ID (a service is identified by a prefix list—the name and ID of a service for a Region).
* Any protocol that has a standard protocol number. If you specify ICMP as the protocol, you can specify any or all of the ICMP types and codes.

#### Create a Security Group

* Open the Amazon VPC console at https://console.aws.amazon.com/vpc/
* In the navigation pane, choose **Security Groups**.
* Choose **Create Security Group**.
* Enter a name for the security group (for example, `my-security-group`) and provide a description. Select the ID of your VPC from the **VPC** menu and choose **Yes, Create**.

![security group](/images/2/1.png)

#### Adding a rules

* Open the Amazon VPC console at https://console.aws.amazon.com/vpc/
* In the navigation pane, choose **Security Groups**.
* Select the security group to update.
* Choose **Actions, Edit inbound rules** or **Actions, Edit outbound rules**.
* For **Type**, select the traffic type, and then fill in the required information. 
* You can also allow communication between all instances that are associated with this security group. Create an inbound rule with the following options:
	* **Type**: All Traffic
	* **Source**: Enter the ID of the security group.
*  Choose **Save rules**.

![security group](/images/2/2.png)
