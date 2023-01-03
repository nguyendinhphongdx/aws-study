---
title : "Test Connection"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 4.2 </b> "
---
<!---
Dùng Mac nên anh không kiểm tra chính xác bước 4.2 được
--->

#### Checking connection

{{% notice note %}}
There are many ways to connect to EC2, you can refer to [connect EC2 with **PuTTY**](https://000004.awsstudygroup.com/en/4-launchlinuxinstance/4.2-connectlinuxinstance/). In the lab, we use [MobaXterm](https://mobaxterm.mobatek.net/) to connect EC2
{{% /notice %}}

1. Download [MobaXterm](https://mobaxterm.mobatek.net/download.html)

![Create VPC](/images/4-CreateEc2Server/4.2-ec2connect/00019-ec2connect.png?featherlight=false&width=90pc)

2. Go to **EC2** page

- Select **Instances**
- Select **EC2 Public**
- Select **Details**
- View **Public IPv4 address**

![Create VPC](/images/4-CreateEc2Server/4.2-ec2connect/0001-ec2connect.png?featherlight=false&width=90pc)

3. After downloading **MobaXterm**, extract and open

- Select **Session**

![Create VPC](/images/4-CreateEc2Server/4.2-ec2connect/0002-ec2connect.png?featherlight=false&width=90pc)

4. In the **Session settings** interface

- Select **SSH**

![Create VPC](/images/4-CreateEc2Server/4.2-ec2connect/0003-ec2connect.png?featherlight=false&width=90pc)

5. In the **Session settings** interface

- **Remote host**, enter **```Public IPv4 address```**
- **Specify username**, enter **```ec2-user```**
- **Use private key**, choose the path of **aws-keypair.pem** created and download it at EC2 creation.

![Create VPC](/images/4-CreateEc2Server/4.2-ec2connect/0004-ec2connect.png?featherlight=false&width=90pc)

6. Connection successful.

![Create VPC](/images/4-CreateEc2Server/4.2-ec2connect/0005-ec2connect.png?featherlight=false&width=90pc)

7. Check the connection to the internet of EC2 Public, we execute the command:

```
ping amazon.com -c5
```

![Create VPC](/images/4-CreateEc2Server/4.2-ec2connect/0006-ec2connect.png?featherlight=false&width=90pc)

#### Connect to EC2 Private server and check internet connection.
8. Access to **EC2**

- Select **Instances**
- Select **EC2 Private**
- Select **Details**
- Select **Private IPv4 addresses**

![Create VPC](/images/4-CreateEc2Server/4.2-ec2connect/0007-ec2connect.png?featherlight=false&width=90pc)

9. Do a ping **<EC2 Private's private IP address>** to test the connection from the **EC2 Public** server to the **EC2 Private** server. We test the connection between two EC2 instances with the command:

```
ping 10.10.4.105 -c5
```

![Create VPC](/images/4-CreateEc2Server/4.2-ec2connect/0008-ec2connect.png?featherlight=false&width=90pc)

10. **EC2 Private** will not have a **public IP address** because we are not assigning this server a public IP. To be able to ssh into **EC2 Private**, we will make an ssh connection from EC2 Public through EC2 Private **private IP address**

- Download the **pscp** tool to the same folder containing the **aws-keypair.ppk** file to copy the **aws-keypair.pem** file from our machine to **EC2 Public**.


{{% notice info %}}
You download [a RSA and DSA key generation utility](https://the.earth.li/~sgtatham/putty/latest/w64/puttygen.exe) as **puttygen.exe**
{{% /notice %}}

{{% notice info %}}
You download [an SCP client, i.e. command-line secure file copy](https://the.earth.li/~sgtatham/putty/latest/w64/pscp.exe) is **pscp.exe**
{{% /notice %}}

11. We use **puttygen.exe** to generate key

- Select **Load**

![Create VPC](/images/4-CreateEc2Server/4.2-ec2connect/0009-ec2connect.png?featherlight=false&width=90pc)

12. Select **aws-keypair.pem**

- Select **OK**
- Select **Save private key** with the name **aws-keypair.ppk**

![Create VPC](/images/4-CreateEc2Server/4.2-ec2connect/00010-ec2connect.png?featherlight=false&width=90pc)

13. Complete the generation key

![Create VPC](/images/4-CreateEc2Server/4.2-ec2connect/00011-ec2connect.png?featherlight=false&width=90pc)

14. Launch **Command Prompt**. Change the path to the folder you just downloaded **pscp**. Run the command below to upload the **aws-keypair.pem** file to the **/home/ec2-user/** directory of the EC2 Public server.

- You will need to replace the **public IP address of the EC2 Public** parameter before running the command.

```
pscp -i aws-keypair.ppk aws-keypair.pem ec2-user@<EC2 PUBLIC public IP address>:/home/ec2-user/
```

![Create VPC](/images/4-CreateEc2Server/4.2-ec2connect/00012-ec2connect.png?featherlight=false&width=90pc)

15. Access to **EC2**

- Select **Instances**
- Select **EC2 Public**
- Select **Details**
- View **Public IPv4 address**

![Create VPC](/images/4-CreateEc2Server/4.2-ec2connect/0001-ec2connect.png?featherlight=false&width=90pc)

16. Return to the EC2 connection interface. To make sure you copy the **aws-keypair.pem** file to the **EC2 Public** server, we execute the command

```
ls
```

![Create VPC](/images/4-CreateEc2Server/4.2-ec2connect/00014-ec2connect.png?featherlight=false&width=90pc)

17. Update the permissions for the **aws-keypair.pem** file by running the **chmod 400 aws-keypair.pem** command. AWS requires a key pair file that needs to be restricted before it can be used to connect to the EC2 server.

```
chmod 400 aws-keypair.pem
```

![Create VPC](/images/4-CreateEc2Server/4.2-ec2connect/00015-ec2connect.png?featherlight=false&width=90pc)

18. **SSH** to **EC2 Private** server

```
ssh -i aws-keypair.pem ec2-user@<EC2 Private server's private IP address>
```

![Create VPC](/images/4-CreateEc2Server/4.2-ec2connect/00016-ec2connect.png?featherlight=false&width=90pc)

19. Perform **ping test to amazon.com**. As you can see, we are not able to connect **internet from EC2 Private**. In the next step, we will create **NAT Gateway** to allow the **EC2 Private** server to connect to the internet in the outbound direction. Keep the connection to **EC2 Private** so we can check the connection to **internet** after completing the creation and configuration of **NAT Gateway**.

```
ping amazon.com
```

![Create VPC](/images/4-CreateEc2Server/4.2-ec2connect/00017-ec2connect.png?featherlight=false&width=90pc)