+++
title = "Create Security Group"
date = 2021
weight = 3
chapter = false
pre = "<b>3.3 </b>"
+++

In this step we will create 2 security groups that allow ping and ssh to be used for 2 EC2 servers located in public subnet and private subnet.

![Lab Model](/images/architecture/lab-3.2.png?width=40pc)

#### Create Security Group for servers located in Public subnet

* Access Amazon VPC console.
* On the left navigation bar, select **Security Group**, click **Create Security Group**.
![Security Group](/images/vpc/create-sg.png?width=90pc)

* In the **Security group name** field enter **Public subnet - SG**
* In the **Description ** section enter **Allow SSH and Ping for servers in public subnet**.
* Click **VPC**, select VPC named **ASG**.
![Security Group](/images/vpc/create-sg2.png?width=90pc)

* In **Inbound rules**, click **Add rule**.
* Select **Type**: `SSH` and **Source**: `My IP`. My IP represents a public IPv4 address you are using.
* Click **Add rule** to add a new rule.
* Select **Type**: `Custom ICMP v4` and **Source**: `Anywhere`. Allow ping from any IP address.
![Security Group](/images/vpc/create-sg3.png?width=90pc)
* Drag the screen down and Click **Create Security Group**.

#### Create Security Group for servers located in Private subnet
* Access Amazon VPC console.
* On the left navigation bar, select **Security Group**, click **Create Security Group**.
![Security Group](/images/vpc/create-sg4.png?width=90pc)

* In the **Security group name** field enter **Private subnet - SG**
* In the **Description** section enter **Allow SSH and Ping for servers in private subnet**.
* Click **VPC**, select VPC named **ASG**.
![Security Group](/images/vpc/create-sg5.png?width=90pc)

* In **Inbound rules**, click **Add rule**.
* Select **Type**: `SSH` and leave **Source**: `Custom` as it is. Click on the search box and select **Public subnet SG**. This option allows all servers assigned **Public subnet SG** to be SSHed into servers assigned **Private subnet SG**.
![Security Group](/images/vpc/create-sg6.png?width=90pc)

* Click **Add rule** to add a new rule.
* Select **Type**: `Custom ICMP v4` and **Source**: `Anywhere`. Allow ping from any IP address.
![Security Group](/images/vpc/create-sg7.png?width=90pc)
* Drag the screen down and Click **Create Security Group**.

* So we have created 2 Security Groups for servers located in public subnet and private subnet.
* Next we will proceed to create 2 EC2 servers.