+++
title = "Deploy Infrastructure"
date = 2020
weight = 1
chapter = false
pre = "<b>2.3.1. </b>"
+++

![Lab Diagram](/images/3/0.png)

**Contents:**
- [1. Configure Network](#1-configure-network)
- [2. Launch EC2 instance](#2-launch-ec2-instance)

#### 1. Configure Network

Firstly, we will create an Elastic IP prepare for NAT Gateway:
1. Browse to **VPC Console**, select **Elastic IPs** and click **Allocate Elastic IP address**.
2. Click **Allocate** to allocate new Elastic IP.

Now, go to **VPC Dashboard** to create a new VPC:
1. Click on **Launch VPC Wizard**

![Network](/images/3/1.png?width=90pc)

2. In **Step 1: Select a VPC Configuration** page, select **VPC with Public and Private Subnets**
3. Click **Select**
4. In **Step 2: VPC with Public and Private Subnets** page, input information:
   - **IPv4 CIDR block:** ```10.10.0.0/16```
   - **VPC name:**  ```LabVPC```
   - **Public subnet's IPv4 CIDR**: ```10.10.0.0/24```
   - **Public subnet name:**  ```Public subnet```
   - **Private subnet's IPv4 CIDR:** ```10.10.3.0/24```
   - **Private subnet name:** ```Private subnet```
   - **Elastic IP Allocation ID:**  Select the EIP previously created.
   - Let others setting as default.

![Network](/images/3/2.png?width=90pc)

5. Click **Create VPC** to create a new VPC.

#### 2. Launch EC2 instance

**Create Security Group for EC2**

1. Go to **EC2 Console**, click **Create security group**.
2. In **Create security group** page, input the Security Group information:
   - **Security group name:** ```Private-EC2-SG```
   - **Description:** ```Private-EC2-SG```
   - **VPC:** Select previous created VPC
   - In **Inbound rules** section, click **Add rule**
     - Type: **SSH**; Source: **My IP**
     - Type: **All Traffic**; Source: Custom, ```10.28.0.0/16```	
3. Click **Create security group**.

![Network](/images/3/3.png?width=90pc)

**Launch EC2 in LabVPC**

1. Go to **EC2 Console**, click **Instances** then click **Launch instances**.

![Network](/images/3/4.png?width=90pc)

2. Follow the step and choose below configuration for you EC2 instance.
   * **AMI:** Amazon Linux 2 AMI (HVM), SSD Volume Type
   * **Instance type:** t2.micro
   * **Network:** LabVPC
   * **Subnet:** Private subnet
   * **Auto-assign Public IP:** Disable
   * **Network interfaces** > **Primary IP:** ```10.10.3.100```
   * **Security group:** Private-EC2-SG
   * Leave other settings as default.
3. Click **Review and Launch**
4. Review EC2 instance information then click **Launch**.

![Network](/images/3/6.png?width=90pc)

5. In **Select an existing key pair or create a new key pair** dialog, create a new keypair:
   - Select **Create a new key pair**
   - **Key pair name:** ```ec2-keypair```

![Network](/images/3/5.png?width=90pc)

6. Click **Launch Instances**

![Network](/images/3/7.png?width=90pc)
