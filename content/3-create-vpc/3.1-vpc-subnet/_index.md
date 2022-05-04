+++
title = "Create VPC and Subnet"
date = 2022
weight = 1
chapter = false
pre = "<b>3.1 </b>"
+++

First we will proceed to create a VPC and 4 subnets, like the architecture below.
![Lab Model](/images/architecture/lab-3.1.png?width=40pc)

#### Create an Amazon Virtual Private Cloud (VPC)

* Access the Amazon VPC console at https://console.aws.amazon.com/vpc/
* In the navigation pane, Select **Your VPCs**, **Create VPC**.

![Create VPC](/images/vpc/create-vpc.png?width=90pc)

* **Name tag** enter VPC name: `ASG`
* **IPv4 CIDR block** Enter the IP block range of the VPC : `10.10.0.0/16`
* Click **Create VPC**.

![Create VPC](/images/vpc/name-ip-create.png?width=90pc)
{{%notice warning%}}
The Tennacy configuration section we will leave the default mechanism. If we switch to **Dedicated** there will be some EC2 Instance type mismatch and will not be created in VPC with tennacy mode of **Dedicated**
{{%/notice%}}

#### Create subnets

* Access Amazon VPC console.
* On the left navigation bar, select **Subnets**.
* Select **Create Subnet**.
![Create VPC](/images/vpc/create-subnet.png?width=90pc)

* Select the VPC named **ASG** we just created.
* **Name tag** enter the name of the subnet: `Public Subnet 1`
* Select **Availability Zone**: `ap-southeast-1a`
* Select **IPv4 CIDR block** as `10.10.1.0/24` according to the architecture description.
![Create VPC](/images/vpc/name-ip-subnet.png?width=90pc)
* Drag the screen down, click **Create subnet** to proceed to create **Public subnet 1** with CIDR of **10.10.1.0/24** located in Availability Zone **ap-southeast-1a** .

* Do the same to create 3 more subnets including:
  + **Public subnet 2** with CIDR of **10.10.2.0/24** located in Availability Zone **ap-southeast-1b**.
  + **Private subnet 1** with CIDR of **10.10.3.0/24** located in Availability Zone **ap-southeast-1a**.
  + **Private subnet 2** with CIDR of **10.10.4.0/24** located in Availability Zone **ap-southeast-1b**.
![Create VPC](/images/vpc/done-subnet.png?width=90pc)


{{%notice tip%}}
You can see there are 2 columns, Availability Zone and Availability Zone ID. To avoid the uneven use of EC2 resources, (we tend to use AZ a to run primary and AZ b to stand by for example) AWS will randomly assign Availability Zone to Availability Zone ID. We can understand that Availability Zone is a form of alias, and Availability Zone ID is the identifier.
For example in the image above Availability Zone **ap-southeast-1a** is assigned Availability Zone ID as **apse1-az2**. In another AWS account, the Availability Zone **ap-southeast-1a** may have an Availability Zone ID of **apse1-az1**.
{{%/notice%}}


{{%notice tip%}}
Another point worth noting is that the subnets are basically the same, through configuring the route table and allocating the public IP address that we have categorized them into Public and Private Subnet.
{{%/notice%}}

#### Allows automatic allocation of public ip addresses for 2 public subnets.

* Click on **Public subnet 1**.
* Click action.
* Click **Modify auto-assign IP settings**.
![Create VPC](/images/vpc/modify-public-ip.png?width=90pc)

* Click **Enable auto-assign public IPv4 address** and Click **Save**.
![Create VPC](/images/vpc/modify-public-ip2.png?width=90pc)

* Do the same for **Public subnet 2**.