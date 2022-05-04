+++
title = "Configure Customer GW"
date = 2022
weight = 4
chapter = false
pre = "<b>4.2.4 </b>"
+++


1. To download the configuration file for VPN Connection on the On-premise side. You can download the Template from AWS provided:
  + Visit **Amazon VPC console** page at https://console.aws.amazon.com/vpc/
  + Drag the left navigation bar down, click **Site-to-Site VPN Connection**.
  + Select the created VPN Connection, then select Download Configuration.
![Configure VPN](/images/vpn/create-vpn6.png?width=90pc)

2. In the Download Configuration dialog box, select the appliance that's right for you: In this exercise, we'll be using OpenSwan.
   + **Vendor:** Select **OpenSwan**
   + **Platform:** Select **OpenSwan**
   + **Software:** Select **OpenSwan 2.6.38+**
   + Click **Download**.

![Configure VPN](/images/vpn/create-vpn8.png?width=90pc)
   + Save the image file information to the folder we use to store the key pair and tools for the lab.
  
3. Then, based on the configuration provided, you change the appropriate information and configuration for your device.
4. Connect ssh to EC2 **Customer Gateway**.
![Configure VPN](/images/vpn/configure-cgw.png?width=90pc)
  + Install **OpenSwan**
```bash
sudo su
yum install openswan -y
```
![Configure VPN](/images/vpn/configure-cgw2.png?width=90pc)


5. Check the configuration file **/etc/ipsec.conf**
 + Run the command **vi /etc/ipsec.conf**
 + Check the configuration is as shown below.
![Configure VPN](/images/vpn/configure-cgw3.png?width=90pc)
 + Press the ESC key and the combination :q! to exit the **vi** editor.

6. Configuration file **/etc/sysctl.conf**
 + Run the command **vi /etc/sysctl.conf**
 + Move down to the last position in the configuration file. Press the *i* key to proceed with editing the file.
 Add the following configuration at the end of the configuration file.
```bash
net.ipv4.ip_forward = 1
net.ipv4.conf.all.accept_redirects = 0
net.ipv4.conf.all.send_redirects = 0
```
 + Press the ESC key and the combination :wq! to save the configuration file.
![Configure VPN](/images/vpn/configure-cgw4.png?width=90pc)

7. Then to apply this configuration, run the command:
```bash
sysctl -p
```
8. Next we will configure the file **/etc/ipsec.d/aws.conf**
 + Run the command **vi /etc/ipsec.d/aws.conf**
 + Press the **i** key to edit the file.
 Add the following configuration to the configuration file. We will create 2 Tunnels with information taken from the VPN Connection configuration file you downloaded and saved together in the folder containing the key pair before.

```bash
conn Tunnel1
authby=secret
auto=start
left=%defaultroute
leftid=13.212.233.95
right=52.220.15.74
type=tunnel
ikelifetime=8h
keylife=1h
phase2alg=aes128-sha1;modp1024
ike=aes128-sha1;modp1024
keyingtries=%forever
keyexchange=ike
leftsubnet=10.11.0.0/16
rightsubnet=10.10.0.0/16
dpddelay=10
dpdtimeout=30
dpdaction=restart_by_peer
overlapip=yes

conn Tunnel2
authby=secret
auto=start
left=%defaultroute
leftid=13.212.233.95
right=54.255.91.142
type=tunnel
ikelifetime=8h
keylife=1h
phase2alg=aes128-sha1;modp1024
ike=aes128-sha1;modp1024
keyingtries=%forever
keyexchange=ike
leftsubnet=10.11.0.0/16
rightsubnet=10.10.0.0/16
dpddelay=10
dpdtimeout=30
dpdaction=restart_by_peer
overlapip=yes
```
 + Press the ESC key and the combination :wq! to save the configuration file.

{{%notice tip%}}
Make sure you edit the IP address and network class accordingly before copying the above configuration.\
For Amazon Linux, we will remove the **auth=esp** line in the original configuration file.\
Since we only have 1 public IP addres for **Customer Gateway**, we will need to configure **overlapip=yes** .\
{{%/notice%}}

{{%notice note%}}
**leftid**: IP Public Address on the Onprem side. (Here is the public IP of EC2 **Customer Gateway** in **ASG VPN** VPC) .\
**right**: IP Public Address on AWS VPN Tunnel side. .\
**leftsubnet**: CIDR of Local Side Network (If there are multiple network layers, you can leave it as 0.0.0.0/0). .\
**rightsubnet**: CIDR of Private Subnet on AWS.
{{%/notice%}}

![Configure VPN](/images/vpn/configure-cgw5.png?width=90pc)

9. Check out the next step in the configuration file we downloaded.
![Configure VPN](/images/vpn/configure-cgw6.png?width=90pc)

10. Create and configure the file **etc/ipsec.d/aws.secrets**
Create a new file with the following configuration to set up the authentication for the 2 Tunnels.
 + Run the command **touch /etc/ipsec.d/aws.secrets** to create the file.
 + Run the command **vi /etc/ipsec.d/aws.secrets**
 + Press the **i** key to edit the file.
 Add the following configuration at the end of the configuration file.
```bash
13,212.233.95 52.220.15.74: PSK "2ned6kIANuA8nPtYnwvQVwak_fJcmSP5"
13.212.233.95 54.255.91.142: PSK "jdkPQG140TeUacw3rnKlyavxnrNaAkZ7"
```
 + Press the ESC key and the combination :wq! to save the configuration file.
 + Run the command **cat /etc/ipsec.d/aws.secrets** to check the configuration file content.
![Configure VPN](/images/vpn/configure-cgw7.png?width=90pc)


11. Restart Network service & IPSEC service
```bash
service network restart
chkconfig ipsec on
service ipsec start
service ipsec status
```

![Configure VPN](/images/vpn/configure-cgw8.png?width=90pc)

{{%notice tip%}}
If the status tunnel is still not running properly, after checking and updating the configuration you will need to run the command to restart the service network and IPsec : .\
**sudo service network restart** .\
**sudo service ipsec restart**
{{%/notice%}}


12. After completing the configuration. Try to ping from the **Customer Gateway** server-side to the **EC2 Private** server. If the VPN configuration is successful, you will get the following results.

![Configure VPN](/images/vpn/testping.png?width=90pc)
