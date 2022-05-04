+++
title = "Create EC2 Instance"
date = 2021
weight = 2
chapter = false
pre = "<b>4.1.2 </b>"
+++

 
#### Create EC2 as Customer Gateway

1. Create Security Group for **Customer Gateway**
  + Access Amazon VPC console.
  + On the left navigation bar, select **Security Group**, click **Create Security Group**.
2. In the **Security group name** field enter **VPN Public - SG**
  + In the **Description** section enter **Allow IPSec, SSH and Ping for servers in public subnet**.
  + Click **VPC**, select VPC named **ASG VPN**.
![Create EC2](/images/vpn/create-sg.png?width=90pc)

3. In **Inbound rules**, click **Add rule**.
  + Select **Type**: `SSH` and **Source**: `My IP`. My IP represents a public IPv4 address you are using.
  + Click **Add rule** to add a new rule.
  + Select **Type**: `Custom ICMP v4` and **Source**: `Anywhere`. Allow ping from any IP address.
  + Click **Add rule** to add a new rule.
  + Select **Type**: `Custom UDP` , **Port:400 and **Source** : `Anywhere`.
  + Click **Add rule** to add a new rule.
  + Select **Type**: `Custom UDP` , **Port:500 and **Source** : `Anywhere`.
![Create EC2](/images/vpn/create-sg2.png?width=90pc)
  + Drag the screen down and Click **Create Security Group**.

4. So we have created Security Group. Next we will proceed to create an EC2 server that plays the Customer Gateway role.

5. Access the Amazon EC2 console at https://console.aws.amazon.com/ec2/.
  + On the left navigation bar, select **Intances**, **Launch Intance**.
  + Select **Select** to select Amazon Machine Image (AMI) **Amazon Linux 2 AMI (HVM), SSD Volume Type**.
![Create EC2](/images/vpc/create-ec2-2.png?width=90pc)

6. Leave the **General purpose t2.micro** selected, then click **Next: Configure Instance Details**.
![Create EC2](/images/vpc/create-ec2-3.png?width=90pc)

7. At **Configure Instance Details** page
  + **Network** select VPC **ASG VPN**.
  + **Subnet** select subnet **VPN Public**.
  + **Auto-assign Public IP** check whether **Enable** or not. If not Enable, you need to check the configuration to automatically allocate public IP for the subnet.
  + Then click **Next: Add Storage**.
![Create EC2](/images/vpn/create-vpnec2.png?width=90pc)

8. At the **Add Storage** page. Keep the default configuration and Click **Next: Add Tags**
9. On the **Add Tags** page, select **Add Tag**
  + **Key** enter `Name`.
  + **Value** fill in `Customer Gateway`.
  + Then click **Next: Configure Security Group**.

10. At the **Configure Security Group** page
  + **Assign a security group** click on **Select an existing security group**.
  + **Name** selects the security group for EC2s in the public cubnet named **VPN Public - SG**.
  + Then click **Review and Launch**.
![Create EC2](/images/vpn/create-vpnec21.png?width=90pc)

11. Check the configuration information again and Click **Launch**.
  + A prompt **Choose an existing key pair or Create new key pair** appears.
  + Select **Choose an existing keypair**.
  + Select **Key pair name**: **asg-keypair**.
  + Click **I acknowledge that I have access to the selected private key file (asg-keypair.pem), and that without this file, I won't be able to log into my instance.** to confirm that you have can access the Key pair you have selected.
  + Click **Launch**.
![Create EC2](/images/vpc/create-ec2-11.png?width=90pc)
  + Click **View Instances** and wait until EC2 finishes initializing.

12. So we have created the EC2 server according to the required architectural model. The next step we will start to initialize and configure the Site to Site VPN connection.