---
title : "VPC Resource Map"
date :  "`r Sys.Date()`" 
weight : 3
chapter : false
pre : " <b> 2.3 </b> "
---

#### VPC Resource Map

To get started, choose an existing VPC in the VPC console. In the details section, select the Resource map tab. Here, you can see the resources in your VPC and the relationships between those resources.

![Create VPC](/images/17/0001.jpg?featherlight=false&width=90pc)

As you hover over a resource, you can see the related resources and the connected lines highlighted. If you click to select the resource, you can see a few lines of details and a link to see the details of the selected resource.

#### Getting Started with VPC Creation Experience
I want to explain how to use the VPC creation experience to improve your workflow to create a new VPC to make a high-availability three-tier VPC easily.

Choose **Create VPC** and select **VPC and more** in the VPC console. You can preview the VPC resources that you are about to create all on the same page.

![Create VPC](/images/17/0002.jpg?featherlight=false&width=90pc)

In Name tag auto-generation, you can specify a prefix value for Name tags. This value is used to generate Name tags for all VPC resources in the preview. If I change the default value, which is project to channy, the Name tag in the preview changes to channy- something, such as channy-vpc. You can customize a Name tag per resource in the preview by clicking each resource and making changes.

You can easily change the default CIDR value (10.0.0.0/16) when you click the IPv4 CIDR block field to reveal the CIDR joystick. Use the left or right arrow to move to the previous (9.255.0.0/16) or next (10.0.1.0/16) CIDR block within the /16 network mask. You can also change the subnet mask to /17 by using the down arrow, or go back to /16 using the up arrow.

Choose the number of Availability Zones (AZs) up to 3. The number of public and private subnet types changes based on the number of AZs and shows the total number of each subnet type it will create.


![Create VPC](/images/17/0003.jpg?featherlight=false&width=90pc)

I want a high-availability VPC in three AZs and select 6 for the number of private subnets. In the preview panel, you can see that there are 9 subnets. When I hover over channy-rtb-public, I can visually confirm that this route table is connected to three public subnets and also routed to the internet gateway (channy-igw). The dotted lines indicate routes to network node, and the solid lines indicate relationships such as implicit or explicit associations.

Adding NAT gateways and VPC endpoints is easy. You can simply change the number of NAT gateways in or per Availability Zone (AZ). Note that there is a charge for each NAT gateway. We always recommend having one NAT gateway per AZ and route traffic from subnets in an AZ to the NAT gateway in the same AZ for high availability and to avoid inter-AZ data charges.

To route traffic to Amazon Simple Storage Service (Amazon S3) buckets more securely, you can choose the S3 Gateway endpoint by default. The S3 Gateway endpoint is free of charge and does not use NAT gateways when moving data from private subnets.

You can create additional tags and assign them to all resources in the VPC in no time. I select Add new tag and enter environment for the Key and test for the Value. This key-value pair will be added to every resource here.


![Create VPC](/images/17/0004.jpg?featherlight=false&width=90pc)

Choose Create VPC at the bottom of the page and see the resources and the IDs of those resources that are being created. Before creating, please validate resources from the preview.


![Create VPC](/images/17/0005.jpg?featherlight=false&width=90pc)

Once all the resources are created, choose View VPC at the bottom. The button takes you directly to the VPC resource map, where you can see a visual representation of what you created.


![Create VPC](/images/17/0006.jpg?featherlight=false&width=90pc)