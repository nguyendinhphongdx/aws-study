+++
title = "Build Simple Lab"
date = 2020
weight = 10
chapter = false
pre = "<b>1.5. </b>"
+++

Through out this lab, we will build a simple VPC follow the diagram below:

![Lab Diagram](/images/2-vpc/Lab-Diagram.PNG?width=80pc)

**Contents:**
- [Step 1. Create an Amazon Virtual Private Cloud (VPC)](#step-1-create-an-amazon-virtual-private-cloud-vpc)
- [Step 2. Create a Private subnet](#step-2-create-a-private-subnet)
- [Step 3. Create Public subnets](#step-3-create-public-subnets)
- [Step 4. Create an Internet gateway](#step-4-create-an-internet-gateway)
- [Step 5. Create a Route Table and added a route to the Internet](#step-5-create-a-route-table-and-added-a-route-to-the-internet)
- [Step 6. Create Security group for VM on Public & Private subnet](#step-6-create-security-group-for-vm-on-public--private-subnet)
- [Step 7. Create EC2 on Public subnet](#step-7-create-ec2-on-public-subnet)
- [Step 8. Create EC2 on Private subnet](#step-8-create-ec2-on-private-subnet)
- [Step 9. Test ping and SSH PUB-LINUX from Internet](#step-9-test-ping-and-ssh-pub-linux-from-internet)
- [Step 10. Test ping and SSH PRI-LINUX from PUB-LINUX](#step-10-test-ping-and-ssh-pri-linux-from-pub-linux)
- [Step 11. Create an Elastic IP address](#step-11-create-an-elastic-ip-address)
- [Step 12. Create an NAT Gateway](#step-12-create-an-nat-gateway)
- [Step 13. Test ping to the internet from PRI-LINUX](#step-13-test-ping-to-the-internet-from-pri-linux)

#### Step 1. Create an Amazon Virtual Private Cloud (VPC)

* Open the Amazon VPC console at https://console.aws.amazon.com/vpc/
* In the navigation pane, choose **Your VPCs**, **Create VPC**.
  * **Name tag**: `VPC-MAIN-ASG`
  * **IPv4 CIDR block**: `10.1.0.0/16`

#### Step 2. Create a Private subnet

* Open the Amazon VPC console at https://console.aws.amazon.com/vpc/
* In the left navigation pane, choose **Subnets**.
* Choose **Create Subnet**.
  * **Name tag**: `PRISUB-MAIN-ASG`
  * **VPC**: `VPC-MAIN-ASG`
  * **Availability Zone**: `us-west-1b`
  * **IPv4 CIDR block**: `10.1.3.0/24`
  * Choose **Yes**, **Create**

#### Step 3. Create Public subnets

* Open the Amazon VPC console at https://console.aws.amazon.com/vpc/
* In the left navigation pane, choose **Subnets**.
* Choose **Create Subnet**.
  * **Name tag**: `PUBSUB1-MAIN-ASG`
  * **VPC**: `VPC-MAIN-ASG`
  * **Availability Zone**: `us-west-1a`
  * **IPv4 CIDR block**: `10.1.1.0/24`
  * Choose **Yes**, **Create**

Then go to create the second Public subnet for NAT
* Choose **Create Subnet**.
  * **Name tag**: `PUBSUB2-MAIN-ASG`
  * **VPC**: `VPC-MAIN-ASG`
  * **Availability Zone**: `us-west-1a`
  * **IPv4 CIDR block**: `10.1.2.0/24`
  * Choose **Yes**, **Create**

On the VPC consle page
* In turn select **PUBSUB1-MAIN-ASG** and **PUBSUB2-MAIN-ASG** 
* Choose **Action**, **Modify auto-assign IP settings**
* Select **Enable auto-assign public IPv4 address** and choose **Save, Close**.

#### Step 4. Create an Internet gateway

* Open the Amazon VPC console at https://console.aws.amazon.com/vpc/
* In the left navigation pane, choose **Internet Gateways**, **Create Internet Gateways**
  * **Name tag**: `IGW-MAIN-ASG`  
  * Choose **Create, Close**
* Select **IGW-MAIN-ASG**, choose **Action** then **Attach to VPC**
  * **VPC**: `VPC-MAIN-ASG`

#### Step 5. Create a Route Table and added a route to the Internet

* Open the Amazon VPC console at https://console.aws.amazon.com/vpc/
* In the left navigation pane, choose **Route Tables**, **Create Route Table**
  * **Name tag**: `RT-MAIN-ASG`
  * **VPC**: `VPC-MAIN-ASG`
  * Choose **Create, Close**
* Select **RT-MAIN-ASG**, choose **Action** then **Edit Route**
* Choose **Add Route** 
  * **Destination**: `0.0.0.0/0`
  * **Target**: `IGW-MAIN-ASG`
  * Choose **Save Route**
* Continute to click on **Action**, then **Edit Subnet Association**
* Select **PUBSUB1-MAIN-ASG** and **PUBSUB2-MAIN-ASG**, then click on **Save**

#### Step 6. Create Security group for VM on Public & Private subnet

First, we go to create Security group for Public Subnet
* Open the Amazon VPC console at https://console.aws.amazon.com/vpc/
* In the left navigation pane, choose **Security Group**, **Create Security Group**
  * **Security group name**: `SG-PUB-MAIN-ASG`
  * **Description Info**: `security for public subnet`
  * **VPC**: `VPC-MAIN-ASG`
* In the **Inbound rules** area, click **Add rules**
  * **Type**: `SSH`, **Source**: `My IP`
  * **Type**: `Custom ICMP v4`, **Source**: `Anywhere` 
  * **Type**: `Custom ICMP 64`, **Source**: `Anywhere` 
  * Then click **Create Security Group**

Next, we go to create Security group for Private subnet
* In the left navigation pane, choose **Security Group**, **Create Security Group**
  * **Security group name**: `SG-PRI-MAIN-ASG`
  * **Description Info**: `security for private subnet`
  * **VPC**: `VPC-MAIN-ASG`
* In the **Inbound rules** area, click **Add rules**
  * **Type**: `SSH`, **Source**: `SG-PUB-MAIN-ASG`
  * **Type**: `Custom ICMP v4`, **Source**: `Anywhere` 
  * **Type**: `Custom ICMP 64`, **Source**: `Anywhere` 
  * Then click **Create Security Group**

#### Step 7. Create EC2 on Public subnet

* Open the Amazon EC2 console at https://console.aws.amazon.com/ec2/
* In the left navigation pane, choose **Intances**, **Launch Intance**
* **Step 1: Choose an Amazon Machine Image (AMI)**: `Amazon Linux 2 AMI (HVM), SSD Volume Type`
* **Step 2: Choose an Instance Type**: `General purpose t2.nano`, then **Next: Configure Instance Details**
* **Step 3: Configure Instance Details**
	* **Network**: `VPC-MAIN-ASG`
	* **Subnet**: `PUBSUB1-MAIN-ASG`
	* **Auto-assign Public IP**: `Enable`
* Then click **Next: Add Storage**, **Next: Add Tags**
* **Step 5: Add Tags**, choose **Add Tag**
	* **Key**: `Name`
	* **Value**: `PUB-LINUX`
* Then click **Next: Configure Security Group**
* **Step 6: Configure Security Group**
	* **Assign a security group**: `Select an existing security group`
	* **Name**: `SG-PUB-MAIN-ASG`
* Then click **Review and Launch**, **Launch**
* A **Select an existing key pair or Create new key pair** prompt appears, choose **Create a new key pair**
	* **Key pair name**: `awskey`
* Choose **View Instance** and wait for a while to EC2 initialize

#### Step 8. Create EC2 on Private subnet

* Open the Amazon EC2 console at https://console.aws.amazon.com/ec2/
* In the left navigation pane, choose **Intances**, **Launch Intance**
* **Step 1: Choose an Amazon Machine Image (AMI)**: `Amazon Linux 2 AMI (HVM), SSD Volume Type`
* **Step 2: Choose an Instance Type**: `General purpose t2.nano`, then **Next: Configure Instance Details**
* **Step 3: Configure Instance Details**
	* **Network**: `VPC-MAIN-ASG`
	* **Subnet**: `PRISUB-MAIN-ASG`
	* **Auto-assign Public IP**: `Disable`
* Then click **Next: Add Storage**, **Next: Add Tags**
* **Step 5: Add Tags**, choose **Add Tag**
	* **Key**: `Name`
	* **Value**: `PRI-LINUX`
* Then click **Next: Configure Security Group**
* **Step 6: Configure Security Group**
	* **Assign a security group**: `Select an existing security group`
	* **Name**: `SG-PRI-MAIN-ASG`
* Then click **Review and Launch**, **Launch**
* A **Select an existing key pair or Create new key pair** prompt appears, choose **Choose an existing key pair**
	* **Key pair name**: `awskey`
* Choose **View Instance** and wait for a while to EC2 initialize

#### Step 9. Test ping and SSH PUB-LINUX from Internet

![Testing Ping from Internet](/images/2-vpc/Test-ping-ssh-PUBLINUX-from-Internet.png?width=90pc)

#### Step 10. Test ping and SSH PRI-LINUX from PUB-LINUX

![Testing Ping between Subnets](/images/2-vpc/Test-ping-ssh-PRILINUX-from-PUBLINUX.png?width=90pc)

At the moment, PRI-LINUX can not ping to the internet cause we've not configure NAT Gateway

#### Step 11. Create an Elastic IP address

* Open the Amazon VPC console at https://console.aws.amazon.com/vpc/
* In the left navigation pane, choose **Elastic IPs**
* Choose **Allocate Elastic IP address** 
	* **Elastic IP address settings**: `Amazon's pool of IPv4 addresses`
	* Choose **Allocate**
Then we got a Public IP Address:`3.101.0.64`

#### Step 12. Create an NAT Gateway

* Open the Amazon VPC console at https://console.aws.amazon.com/vpc/
* In the left navigation pane, choose **NAT Gateways**.
* Choose **Create NAT Gateway**.
  * **Subnet**: `PUBSUB2-MAIN-ASG`
  * **Elastic IP Allocation ID**: `3.101.0.64`
* Choose **Add Tag** 
	* **Key**: `Name`
	* **Value**: `NAT-MAIN-ASG`
* Choose **Create a NAT Gateway**
* Choose **Edit Route Table**, **Create Route Table**
	* **Name tag**: `RT-NAT-ASG`
	* **VPC**: `VPC-MAIN-ASG`
	* Choose **Create, Close**
* Select **RT-NAT-ASG**, choose **Action** then **Edit Route**
* Choose **Add Route** 
  * **Destination**: `0.0.0.0/0`
  * **Target**: `NAT-MAIN-ASG`
  * Choose **Save Route**
* Continute to click on **Action**, then **Edit Subnet Association**
* Select **PUBSUB1-MAIN-ASG** and **PRISUB-MAIN-ASG**, then click on **Save**

#### Step 13. Test ping to the internet from PRI-LINUX

Our Final result is:

![Testing Internet Connection](/images/2-vpc/Test-ping-to-Internet-from-PRILINUX.png?width=90pc)
