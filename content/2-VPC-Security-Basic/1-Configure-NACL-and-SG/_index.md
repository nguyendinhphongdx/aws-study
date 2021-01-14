+++
title = "Configure NACL & SG"
date = 2020
weight = 1
chapter = false
pre = "<b>2.1. </b>"
+++

**Contents:**
- [Security groups for your VPC](#security-groups-for-your-vpc)
- [Network ACLs for your VPC](#network-acls-for-your-vpc)

#### Security groups for your VPC

A security group acts as a virtual firewall for your instance to control inbound and outbound traffic. When you launch an instance in a VPC, you can assign up to 5 security groups to the instance. 

{{% notice note %}}
Security groups act at the Instance level, not the Subnet level. Therefore, each instance in a subnet in your VPC can be assigned to a different set of security groups. 
{{% /notice %}}

**Security group basics**

There are some basic characteristics of Security groups for your VPC:
* You can specify allow rules, but not deny rules.
* You can specify separate rules for inbound and outbound traffic.
* When you create a security group, it has no inbound rules. Therefore, no inbound traffic originating from another host to your instance is allowed until you add inbound rules to the security group.
* By default, a security group includes an outbound rule that allows all outbound traffic. You can remove the rule and add outbound rules that allow specific outbound traffic only. If your security group has no outbound rules, no outbound traffic originating from your instance is allowed.
* Security groups are stateful — if you send a request from your instance, the response traffic for that request is allowed to flow in regardless of inbound security group rules. Responses to allowed inbound traffic are allowed to flow out, regardless of outbound rules.
* Instances associated with a security group can't talk to each other unless you add rules allowing the traffic (exception: the default security group has these rules by default).
* Security groups are associated with network interfaces. After you launch an instance, you can change the security groups that are associated with the instance, which changes the security groups associated with the primary network interface (eth0).

**Security group rules**

A rule applies either to inbound traffic (ingress) or outbound traffic (egress. You can grant access to a specific CIDR range, or to another security group in your VPC or in a peer VPC (requires a VPC peering connection).

There are few basic parts of a security group rule in a VPC:
* (Inbound rules only) The source of the traffic and the destination port or port range. The source can be another security group, an IPv4 or IPv6 CIDR block, or a single IPv4 or IPv6 address.
* (Outbound rules only) The destination for the traffic and the destination port or port range. The destination can be another security group, an IPv4 or IPv6 CIDR block, a single IPv4 or IPv6 address, or a prefix list ID (a service is identified by a prefix list—the name and ID of a service for a Region).
* Any protocol that has a standard protocol number. If you specify ICMP as the protocol, you can specify any or all of the ICMP types and codes.

**Create a security group** 

* Open the Amazon VPC console at https://console.aws.amazon.com/vpc/
* In the navigation pane, choose **Security Groups**.
* Choose **Create Security Group**.
* Enter a name for the security group (for example, `my-security-group`) and provide a description. Select the ID of your VPC from the **VPC** menu and choose **Yes, Create**.

**Adding a rules**

* Open the Amazon VPC console at https://console.aws.amazon.com/vpc/
* In the navigation pane, choose **Security Groups**.
* Select the security group to update.
* Choose **Actions, Edit inbound rules** or **Actions, Edit outbound rules**.
* For **Type**, select the traffic type, and then fill in the required information. 
* You can also allow communication between all instances that are associated with this security group. Create an inbound rule with the following options:
	* **Type**: All Traffic
	* **Source**: Enter the ID of the security group.
*  Choose **Save rules**.

#### Network ACLs for your VPC

A network access control list (ACL) is an optional layer of security for your VPC that acts as a firewall for controlling traffic in and out of one or more subnets. You might set up network ACLs with rules similar to your security groups in order to add an additional layer of security to your VPC. 

**Network ACL basics**

There are some basic things about network ACLs:
* Your VPC automatically comes with a modifiable default network ACL. By default, it allows all inbound and outbound IPv4 traffic and, if applicable, IPv6 traffic.
* You can create a custom network ACL and associate it with a subnet. By default, each custom network ACL denies all inbound and outbound traffic until you add rules.
* Each subnet in your VPC must be associated with a network ACL. If you don't explicitly associate a subnet with a network ACL, the subnet is automatically associated with the default network ACL.
* You can associate a network ACL with multiple subnets. However, a subnet can be associated with only one network ACL at a time. When you associate a network ACL with a subnet, the previous association is removed.
* A network ACL contains a numbered list of rules. AWS evaluate the rules in order, starting with the lowest numbered rule, to determine whether traffic is allowed in or out of any subnet associated with the network ACL. The highest number that you can use for a rule is 32766. 
* A network ACL has separate inbound and outbound rules, and each rule can either allow or deny traffic.
* Network ACLs are stateless, which means that responses to allowed inbound traffic are subject to the rules for outbound traffic (and vice versa).

**Network ACL rules**

You can add or remove rules from the default network ACL, or create additional network ACLs for your VPC. When you add or remove rules from a network ACL, the changes are automatically applied to the subnets that it's associated with.

There are some parts of a network ACL rule:
* **Rule number**. Rules are evaluated starting with the lowest numbered rule. As soon as a rule matches traffic, it's applied regardless of any higher-numbered rule that might contradict it.
* **Type**. The type of traffic; for example, SSH. You can also specify all traffic or a custom range.
* **Protocol**. You can specify any protocol that has a standard protocol number.
* **Port range**. The listening port or port range for the traffic. For example, 80 for HTTP traffic.
* **Source**. [Inbound rules only] The source of the traffic (CIDR range).
* **Destination**. [Outbound rules only] The destination for the traffic (CIDR range).
* **Allow/Deny**. Whether to allow or deny the specified traffic.

**Creating a network ACL**

* Open the Amazon VPC console at https://console.aws.amazon.com/vpc/
* In the navigation pane, choose **Network ACLs**.
* Choose **Create Network ACL**.
* In the **Create Network ACL** dialog box, optionally name your network ACL, and select the ID of your VPC from the VPC list. Then choose **Yes, Create**.

**Add rules to a network ACL**

* Open the Amazon VPC console at https://console.aws.amazon.com/vpc/
* In the navigation pane, choose **Network ACLs**.
* In the details pane, choose either the **Inbound Rules** or **Outbound Rules** tab, depending on the type of rule that you need to add, and then choose **Edit**.
* In **Rule #**, enter a rule number. The rule number must not already be in use in the network ACL. AWS process the rules in order, starting with the lowest number.
* Select a rule from the **Type** list.
* In the **Source** or **Destination** field, enter the CIDR range that the rule applies to.
* From the **Allow/Deny** list, select **ALLOW** to allow the specified traffic or **DENY** to deny the specified traffic.
* Choose **Save**.
