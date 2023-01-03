---
title : "Network ACLs"
date :  "`r Sys.Date()`" 
weight : 2
chapter : false
pre : " <b> 2.2 </b> "
---

#### Network ACLs

* VPC after initialization will have a default network ACL available and can be modified. By default, it grants access to all IPv4 or IPv6 traffic that may go in or out of the VPC.
* Can create a custom network ACL and associate it with a subnet. By default, custom network ACLs deny all incoming and outgoing traffic, until we add an access permission rule.
* Each subnet in the VPC must be associated with a network ACL. If the subnet is not associated with a specific network ACL, the subnet is automatically associated with the default network ACL.
* A network ACL can be associated with multiple subnets. However, a subnet can only be associated with one network ACL at a time. When a new network ACL is associated with a subnet, the previous association is deleted.
* A network ACL contains a list of differently numbered rules. AWS evaluates rules based on their assigned sequence number, starting with the lowest numbered rule, to determine if traffic is allowed in or out of any subnets associated with the network ACLs. .
The largest sequence number that can be assigned to a rule is 32766 (equivalent to the maximum number of rules for a network ACL is 32766).
* The Network ACL has separate entry and exit permission rules and the rule can be to allow or deny traffic.
* Network ACL is a Stateless service, which means that responses to traffic that are allowed in must follow the rules for outbound traffic (and vice versa).

#### Network ACL rules

You can add or remove a rule from the default network ACL or create a new network ACL for the VPC. When adding or removing a rule from the network ACL, the changes are automatically applied to the subnets associated with it.

Components of a network ACL rule:
* **Rule number**:  The starting rule is evaluated starting with the rule with the lowest sequence number.
As soon as that rule matches traffic, it will immediately be applied even if it conflicts with a higher-numbered rule in the list.
* **Type**: traffic type, eg SSH. All traffic types or custom ranges can be specified.
* **Protocol**:  specify the protocol with the standard protocol number.
* **Port range**: port or port range listening for traffic. For example, HTTP is 80.
* **Source**: [Inbound rule only] Origin of traffic (value is CIDR range).
* **Destination**: [Outbound rule only] Destination of traffic (value is CIDR range).
* **Allow/Deny**: specify Allow or Deny traffic.