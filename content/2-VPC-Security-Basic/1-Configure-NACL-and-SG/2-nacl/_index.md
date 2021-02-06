+++
title = "Network ACLs"
date = 2020
weight = 2
chapter = false
pre = "<b>2.1.2. </b>"
+++

**Contents:**
- [Network ACL basics](#network-acl-basics)
- [Network ACL rules](#network-acl-rules)
- [Creating a network ACL](#creating-a-network-acl)
- [Add rules to a network ACL](#add-rules-to-a-network-acl)

#### Network ACL basics

There are some basic things about network ACLs:
* Your VPC automatically comes with a modifiable default network ACL. By default, it allows all inbound and outbound IPv4 traffic and, if applicable, IPv6 traffic.
* You can create a custom network ACL and associate it with a subnet. By default, each custom network ACL denies all inbound and outbound traffic until you add rules.
* Each subnet in your VPC must be associated with a network ACL. If you don't explicitly associate a subnet with a network ACL, the subnet is automatically associated with the default network ACL.
* You can associate a network ACL with multiple subnets. However, a subnet can be associated with only one network ACL at a time. When you associate a network ACL with a subnet, the previous association is removed.
* A network ACL contains a numbered list of rules. AWS evaluate the rules in order, starting with the lowest numbered rule, to determine whether traffic is allowed in or out of any subnet associated with the network ACL. The highest number that you can use for a rule is 32766. 
* A network ACL has separate inbound and outbound rules, and each rule can either allow or deny traffic.
* Network ACLs are stateless, which means that responses to allowed inbound traffic are subject to the rules for outbound traffic (and vice versa).

#### Network ACL rules

You can add or remove rules from the default network ACL, or create additional network ACLs for your VPC. When you add or remove rules from a network ACL, the changes are automatically applied to the subnets that it's associated with.

There are some parts of a network ACL rule:
* **Rule number**. Rules are evaluated starting with the lowest numbered rule. As soon as a rule matches traffic, it's applied regardless of any higher-numbered rule that might contradict it.
* **Type**. The type of traffic; for example, SSH. You can also specify all traffic or a custom range.
* **Protocol**. You can specify any protocol that has a standard protocol number.
* **Port range**. The listening port or port range for the traffic. For example, 80 for HTTP traffic.
* **Source**. [Inbound rules only] The source of the traffic (CIDR range).
* **Destination**. [Outbound rules only] The destination for the traffic (CIDR range).
* **Allow/Deny**. Whether to allow or deny the specified traffic.

#### Creating a network ACL

* Open the Amazon VPC console at https://console.aws.amazon.com/vpc/
* In the navigation pane, choose **Network ACLs**.
* Choose **Create Network ACL**.
* In the **Create Network ACL** dialog box,
  * Name your Network ACL
  * Select the ID of your VPC from the VPC list. 
* Then choose **Yes, Create**.

#### Add rules to a network ACL

* Open the Amazon VPC console at https://console.aws.amazon.com/vpc/
* In the navigation pane, choose **Network ACLs**.
* In the details pane, choose either the **Inbound Rules** or **Outbound Rules** tab, depending on the type of rule that you need to add, and then choose **Edit**.
* In **Rule #**, enter a rule number. The rule number must not already be in use in the network ACL. AWS process the rules in order, starting with the lowest number.
* Select a rule from the **Type** list.
* In the **Source** or **Destination** field, enter the CIDR range that the rule applies to.
* From the **Allow/Deny** list, select **ALLOW** to allow the specified traffic or **DENY** to deny the specified traffic.
* Choose **Save**.
