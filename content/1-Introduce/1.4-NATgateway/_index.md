---
title : "NAT Gateway"
date :  "`r Sys.Date()`" 
weight : 4 
chapter : false
pre : " <b> 1.4 </b> "
---

## NAT Gateway

- By default, any EC2 running inside a Private subnet will not be able to communicate with the Internet through the IGW. This situation becomes problematic when the EC2 needs to access the Internet for security updates, patch downloads, or application software updates.
  
   Recognizing this need, AWS offers two methods for granting EC2s in a Private subnet access to the Internet: **NAT Instance** and **NAT Gateway**. In most scenarios, it is advisable to opt for NAT Gateway over NAT Instance due to its enhanced availability, bandwidth, and reduced administrative overhead.

- To set up a NAT gateway, you are required to specify a public subnet and an Elastic IP address. Ensure that the chosen Elastic IP address is not associated with any other instances or network interfaces.
  
- In situations where transitioning from a NAT instance to a NAT gateway is desired, it is possible to reuse the Elastic IP address assigned to the NAT instance. However, prior to doing so, it is essential to disassociate the IP address from the NAT Instance.

![NAT Gateway](/images/1-Introduce/natgw.png?featherlight=false&width=70pc)

> **Tip:** Neither the NAT Gateway nor the NAT instance supports direct inbound traffic from the internet.
