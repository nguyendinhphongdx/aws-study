+++
title = "Test connection"
date = 2021
weight = 5
chapter = false
pre = "<b>3.5 </b>"
+++

In this step we will connect ssh to 2 EC2 servers ( EC2 instances ) we created in step 3.4. Then perform a ping test to check internet connectivity.

![Lab Model](/images/architecture/lab-3.4.png?width=50pc)


#### Connect to EC2 Public server and test Internet access.

1. Access the Amazon EC2 console at https://console.aws.amazon.com/ec2/.
2. Check and get public IP information of **EC2 Public** server.
![Connect EC2](/images/vpc/connect-ec2.png?width=90pc)

3. Download putty and puttygen to connect ssh to **EC2 Public**.
  
{{%attachments style="orange" title="Putty and Puttygen" pattern=".*(exe)"/%}}

4. First open puttygen you downloaded to convert **asg-keypair.pem** to **asg-keypair.ppk**.
  + Click **Load**.
  + Move the directory to the path you downloaded **asg-keypair.pem**.
  + Choose to display **All Files**.
  + Select **asg-keypair.pem**.
  + Click **Open**.
![Connect EC2](/images/vpc/connect-ec2-2.png?width=60pc)
  + Click **Ok**.
  + Click **Save Private Key**.
  + Click **Yes**.
  + Enter the name **asg-keypair**.
  + Click **Save**.
![Connect EC2](/images/vpc/connect-ec2-3.png?width=60pc)

5. Open the putty tool you downloaded, enter the public IP information in the **Host name** field.
 + Enter **EC Public** in the **Saved sessions** field.
 + Click **Save**.
![Connect EC2](/images/vpc/connect-ec2-4.png?width=90pc)

6. Next we will use key pair to be able to connect to **EC2 public**.
  + Click **SSH**.
  + Click **Auth**.
  + Click **Browse**.
  + Change the directory path to the directory containing the file **asg-keypair.ppk**.
  + Convert file format to *.ppk.
  + Select the file **asg-keypair.ppk**.
  + Click **Open**.
  + Click **Open** to make connection to **EC2 Public**.
![Connect EC2](/images/vpc/connect-ec2-5.png?width=90pc)

7. Enter **ec2-user**. **ec2-user** is the default user of the Amazon Linux AMI.
  + Type the command ping amazon.com to check the connection to the internet of **EC2 Public**.
  ```
  ping amazon.com
  ```
![Connect EC2](/images/vpc/connect-ec2-6.png?width=90pc)

#### Connect to EC2 Private server and check internet connection.


1. Access the Amazon EC2 console.
2. Check and get private IP information of **EC2 Private** server.
  + Perform a ping command <EC2 Private's private IP address> to test the connection from the **EC2 Public** server to the **EC2 Private** server.
![Connect EC2](/images/vpc/connect-ec2-7.png?width=90pc)

3. **EC2 Private** will not have a public IP address because we are not assigning this server a public IP. To be able to ssh into **EC2 Private**, we will make ssh connection from **EC2 Public** through EC2's private IP address **Private**
 + Download the **pscp** tool to the same folder containing the **asg-keypair.ppk** file to copy the **asg-keypair.pem* file from our computer to **EC2 Public**.
    
{{%attachments style="orange" title="pscp tool" pattern="pscp.*(exe)"/%}}

 + Launch **Command Prompt**.Go to the directory you just downloaded **pscsp**. Run the command below to upload the **asg-keypair.pem** file to the **/home/ec2-user/** directory of the **EC2 Public** server.
 + You will need to replace the EC2 Public's public IP address parameter before running the command.

```
pscp -i asg-keypair.ppk asg-keypair.pem ec2-user@<EC2 PUBLIC public IP address>:/home/ec2-user/
```
![Connect EC2](/images/vpc/connect-ec2-8.png?width=60pc)

{{%notice tip%}}
Make sure you copy the asg-keypair.pem file to the EC2 Public server.
{{%/notice%}}

1. Update the permissions for the **asg-keypair.pem** file by running the **chmod 400 asg-keypair.pem** command. AWS requires a key pair file that needs to be restricted before it can be used to connect to the EC2 server.
```
chmod 400 asg-keypair.pem
```
5. ssh to the EC2 Private server and do a ping test to **amazon.com**.
```
ssh -i asg-keypair.pem ec2-user@<EC2 Private server's private IP address>
ping amazon.com
```
![Connect EC2](/images/vpc/connect-ec2-10.png?width=90pc)
6. As you can see, we cannot connect to the internet from EC2 Private. In the next step we will create a NAT Gateway to allow the **EC2 Private** server to connect to the internet in the outbound direction.
Keep the connection to **EC2 Private** so that we can check the connection to the internet after finishing creating and configuring the NAT Gateway.