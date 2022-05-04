+++
title = "NAT Gateway"
date = 2021
weight = 6
chapter = false
pre = "<b>3.6 </b>"
+++

In this step we will create a NAT Gateway to allow the **EC2 Private** server to go out to the internet according to the below architecture.

![Lab Model](/images/architecture/vpc.png?width=75pc)

#### Generate an Elastic IP address

1. Access the Amazon VPC console.
2. On the left navigation bar, select **Elastic IPs**.
3. Click **Allocate Elastic IP address** .
![Create NATGW](/images/vpc/create-natgw.png?width=90pc)
4. Click **Allocate**.
5. We have successfully created a Public IP Address: **54.251.117.148**
![Create NATGW](/images/vpc/create-natgw2.png?width=90pc)

#### Create a NAT Gateway

1. Access the Amazon VPC console.
2. On the left navigation bar, select **NAT Gateways**.
3. Select **Create NAT Gateway**.
![Create NATGW](/images/vpc/create-natgw3.png?width=90pc)
4. At the **Create NAT Gateway** page:
  + In the **Name** field, enter **NAT Gateway**.
  + In the **subnet** section, select **Public subnet 2**.
  + In the **Elastic IP Allocation ID** section, select the Elastic IP address we just created.
  + Click **Create NAT Gateway**
![Create NATGW](/images/vpc/create-natgw4.png?width=90pc)

#### Create **Route table - Private** and assign to private subnets.

1. Access the Amazon VPC console.
2. On the left navigation bar, select **Route Tables**.
3. Click **Create route table**
![Create NATGW](/images/vpc/create-natgw5.png?width=90pc)
4. On the Create route table page:
  + In the **Name tag** section, fill in **Route table - Private**.
+ In the VPC section, select VPC **ASG**.
+ Click **Create**
![Create NATGW](/images/vpc/create-natgw6.png?width=90pc)
  + After creating the route table successfully, click **Close**.
5. Select **Route table - Private**.
  + Click the **Subnet Associations** tab.
  + Click **Edit subnet associations**.
![Create NATGW](/images/vpc/create-natgw7.png?width=90pc)
  + Select 2 private subnets and click **Save**.

6. Select **Route table - Private**.
  + Click the **Route** tab.
  + Click **Edit route**.
![Create NATGW](/images/vpc/create-natgw8.png?width=90pc)

  + Select **Add Route**
  + **Destination**: `0.0.0.0/0`
  + **Target**: **NAT Gateway**
  + Select **Save routes**
![Create NATGW](/images/vpc/create-natgw9.png?width=90pc)
  + Click **Close** after successfully adding route entry.

7. Test ping amazon.com successfully from **EC2 Private**.
![Create NATGW](/images/vpc/create-natgw10.png?width=90pc)

8. Congratulations, we have completed the lab model as below. In the next section, we will initialize and configure the Site to Site VPN connection to connect the onpremise environment to the AWS Cloud.
![Lab Model](/images/architecture/vpc.png?width=75pc)