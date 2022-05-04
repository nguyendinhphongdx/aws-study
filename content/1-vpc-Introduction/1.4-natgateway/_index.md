+++
title = "NAT Gateway"
date = 2022
weight = 4
chapter = false
pre = "<b>1.4 </b>"
+++

#### NAT Gateway

- By default, any EC2 running inside a Private subnet will not be able to communicate with the Internet through the IGW. From there the problem arises when that EC2 needs to go outside of the Internet to apply security updates, download patches, or update application software.
Understanding that need, AWS provides 2 methods that allow EC2s inside Private subnet to have access to the Internet, namely NAT Instance and NAT Gateway. For common cases, we should use NAT Gateway instead of NAT Instance. NAT Gateway ensures higher availability and bandwidth and requires less administrative effort than NAT Instance.

- To create a NAT gateway, you must specify a subnet (public) and an Elastic IP address. Make sure the Elastic IP address is not associated with any other Instance or Network interface.

- In case we want to migrate from a NAT instance to a NAT gateway, we can reuse the Elastic IP address of the NAT instance. But first, we need to separate the IP address from the NAT Instance.

![NAT Gateway](/images/architecture/natgw.png?width=70pc)

{{%notice tip%}}
Neither the NAT Gateway nor the NAT instance supports direct inbound traffic from the internet.
{{%/notice%}}