+++
title = "Create IGW and Route table"
date = 2021
weight = 2
chapter = false
pre = "<b>3.2 </b>"
+++

After creating the VPC and subnets, the next step is to create the Internet Gateway, and create and configure the Route table. After completing the steps below, we will have the following architecture:
![Lab Model](/images/architecture/lab-3.2.png?width=40pc)

#### Create an Internet Gateway

* Access Amazon VPC console.
* On the left navigation bar, select **Internet Gateways**, **Create Internet Gateways**
![Internet Gateway](/images/vpc/create-igw.png?width=90pc)

* Name the Internet Gateway as **Internet Gateway**.
* Click **Create Internet Gateway**.
![Internet Gateway](/images/vpc/create-igw2.png?width=90pc)
* Click **Action**.
* Click **Attach to VPC**.
![Internet Gateway](/images/vpc/create-igw3.png?width=90pc)
* Click on VPC **ASG** , VPC ID will be automatically filled in.
* Click **Attach Internet Gateway**.
![Internet Gateway](/images/vpc/create-igw4.png?width=90pc)

#### Create Route Table to route outbound internet through Internet Gateway.

* Access Amazon VPC console.
* On the left navigation bar, select **Route Tables**, **Create Route Table**

![Internet Gateway](/images/vpc/create-route.png?width=90pc)

* Enter the Route Table name in the **Name tag** section: `Route table - Public`.
* Select **VPC** named **ASG** , VPC id will be automatically filled in.
* Click **Create**.
![Internet Gateway](/images/vpc/create-route2.png?width=90pc)
* Click **Close**.

* Select **Route table - Public **, click **Action** , click **Edit routes**.
![Internet Gateway](/images/vpc/create-route3.png?width=90pc)

* Click **Add route**.
* Fill in **Destination** CIDR : `0.0.0.0/0` represents the Internet.
* In the **Target** section, click **Internet Gateway**, then select the Internet Gateway we created. The Internet Gateway ID will be automatically filled in.
* Select **Save routes**
![Internet Gateway](/images/vpc/create-route4.png?width=90pc)

* Make sure **Route table - Public** is selected.
* Click on the **Subnet Associations** tab in the menu below.
* Click **Edit subnet associations**.
![Internet Gateway](/images/vpc/create-route5.png?width=90pc)

* Expand the **Subnet ID** column by dragging the pane to the right.
* Choose the correct 2 public subnets we have created.
* Click **Save**.
![Internet Gateway](/images/vpc/create-route6.png?width=90pc)

* So we have done:
  + Create Internet Gateway named **Internet Gateway**.
  + Create a Route table named **Route table - Public**
  + Edit route in route table **Route table - Public**.
    + Add route entry Destination: 0.0.0.0/0 Target: Internet Gateway.
  + Associate **Route table - Public** to 2 subnets **Public subnet 1** and **Public subnet 2**.