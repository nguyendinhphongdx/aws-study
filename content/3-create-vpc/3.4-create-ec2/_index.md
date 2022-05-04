+++
title = "Create EC2 Server"
date = 2021
weight = 4
chapter = false
pre = "<b>3.4 </b>"
+++

In this step we will create 2 EC2 servers ( EC2 instances ) like the architecture below.

![Lab Model](/images/architecture/lab-3.4.png?width=50pc)


#### Create EC2 in Public subnet

1. Access the Amazon EC2 console at https://console.aws.amazon.com/ec2/.
2. On the left navigation bar, select **Intances**, **Launch Intance**.
![Create EC2](/images/vpc/create-ec2.png?width=90pc)

3. Select **Select** to select Amazon Machine Image (AMI) **Amazon Linux 2 AMI (HVM), SSD Volume Type**.
![Create EC2](/images/vpc/create-ec2-2.png?width=90pc)

4. Leave the **General purpose t2.micro** selected, then click **Next: Configure Instance Details**.
![Create EC2](/images/vpc/create-ec2-3.png?width=90pc)

5. At **Configure Instance Details** page
  + **Network** select VPC **ASG**.
  + **Subnet** select subnet **Public subnet 1**.
  + **Auto-assign Public IP** check whether **Enable** or not. If not Enable, you need to check the configuration for automatically allocating public IP for the subnet in Section 3.1.
  + Then click **Next: Add Storage**.
![Create EC2](/images/vpc/create-ec2-4.png?width=90pc)

6. At the **Add Storage** page. Keep the default configuration and Click **Next: Add Tags**
7. On the **Add Tags** page, select **Add Tag**
  + **Key** enter `Name`.
  + **Value** enter `EC2 Public`.
  + Then click **Next: Configure Security Group**.
![Create EC2](/images/vpc/create-ec2-5.png?width=90pc)

8. At the **Configure Security Group** page
  + **Assign a security group** click on **Select an existing security group**.
  + **Name** selects the security group for EC2s in the public cubnet named **Public subnet -SG**.
  + Then click **Review and Launch**.
![Create EC2](/images/vpc/create-ec2-6.png?width=90pc)
9. Check the configuration information again and Click **Launch**.
  + A prompt **Choose an existing key pair or Create new key pair** appears.
  + Select **Create a new key pair**.
  + **Key pair name**: **asg-keypair**.
  + Click **Download Key Pair**.
  + Choose the path to save the key pair file. You will need the key pair file to be able to ssh to the **EC2 Public** instance you are about to create.
  + Click **Save** to save the key pair file.
  + Click **Launch**.
![Create EC2](/images/vpc/create-ec2-7.png?width=90pc)
  + Click **View Instances** and wait until EC2 finishes initializing.

#### Create EC2 in Private subnet

1. Access the Amazon EC2 console at https://console.aws.amazon.com/ec2/.
2. On the left navigation bar, select **Intances**, **Launch Intance**.
3. Select **Select** to select Amazon Machine Image (AMI) **Amazon Linux 2 AMI (HVM), SSD Volume Type**.
![Create EC2](/images/vpc/create-ec2-2.png?width=90pc)
4. Leave the **General purpose t2.micro** selected, then click **Next: Configure Instance Details**.
![Create EC2](/images/vpc/create-ec2-3.png?width=90pc)
5. At **Configure Instance Details** page
  + **Network** choose VPC **ASG**.
  + **Subnet** select subnet **Private subnet 2**.
  + **Auto-assign Public IP** check if it is **Disable** or not. If not Disable, you need to check the configuration of automatically allocating public IP for the subnet in Section 3.1.
  + Then click **Next: Add Storage**.
![Create EC2](/images/vpc/create-ec2-8.png?width=90pc)

6. At the **Add Storage** page. Keep the default configuration and Click **Next: Add Tags**
7. On the **Add Tags** page, select **Add Tag**
  + **Key** enter `Name`.
  + **Value** enter `EC2 Private`.
  + Then click **Next: Configure Security Group**.
![Create EC2](/images/vpc/create-ec2-9.png?width=90pc)

8. At the **Configure Security Group** page
  + **Assign a security group** click on **Select an existing security group**.
  + **Name** selects the security group for EC2s in the public cubnet named **Private subnet -SG**.
  + Then click **Review and Launch**.
![Create EC2](/images/vpc/create-ec2-10.png?width=90pc)

9. Check the configuration information again and Click **Launch**.
  + A prompt **Choose an existing key pair or Create new key pair** appears.
  + Select **Choose an existing keypair**.
  + Select **Key pair name**: **asg-keypair**.
  + Click **I acknowledge that I have access to the selected private key file (asg-keypair.pem), and that without this file, I won't be able to log into my instance.** to confirm that you have can access the Key pair you have selected.
  + Click **Launch**.
![Create EC2](/images/vpc/create-ec2-11.png?width=90pc)
  + Click **View Instances** and wait until EC2 finishes initializing.

10. So we have created 2 EC2 servers according to the architecture model below. The next step we will try to access these 2 servers.

![Lab Model](/images/architecture/lab-3.4.png?width=50pc)