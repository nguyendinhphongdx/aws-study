---
title : "Create NAT Gateway"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 4.3 </b> "
---

#### Create NAT Gateway

#### Generate an Elastic IP address

1. Access **EC2**

- Select **Elastic IPs**
- Select **Allocate Elastic IP address**

![Create NAT Gateway](/images/4-CreateEc2Server/4.3-natgateway/0001-natgaw.png?featherlight=false&width=90pc)

2. In the **Allocate Elastic IP address** interface

- **Public IPv4 address pool**: select **Amazon's pool of IPv4 addresses**
- Select **Allocate**

![Create NAT Gateway](/images/4-CreateEc2Server/4.3-natgateway/0002-natgaw.png?featherlight=false&width=90pc)

3. We have successfully created a **Public IP Address**: **13.213.151.199**

![Create NAT Gateway](/images/4-CreateEc2Server/4.3-natgateway/0003-natgaw.png?featherlight=false&width=90pc)

4. Access to **VPC**

- Select **NAT Gateways**
- Select **Create NAT gateway**

![Create NAT Gateway](/images/4-CreateEc2Server/4.3-natgateway/0004-natgaw.png?featherlight=false&width=90pc)

5. In **NAT gateway** interface

- **Name**: enter `NAT gateway`
- **Subnet**: select **Public subnet 2**
- **Connectivity type**: select **Public**
- **Elastic IP allocation ID**: select **Elastic IP** just created.

![Create NAT Gateway](/images/4-CreateEc2Server/4.3-natgateway/0005-natgaw.png?featherlight=false&width=90pc)

6. Select **Create NAT gateway**

![Create NAT Gateway](/images/4-CreateEc2Server/4.3-natgateway/0006-natgaw.png?featherlight=false&width=90pc)

7. Successfully created **NAT gateway**

![Create NAT Gateway](/images/4-CreateEc2Server/4.3-natgateway/0007-natgaw.png?featherlight=false&width=90pc)

#### Create Route table - Private and assign to private subnets.

8. In the **VPC** interface

- Select **Route Tables**
- Select **Create route table**

![Create NAT Gateway](/images/4-CreateEc2Server/4.3-natgateway/0008-natgaw.png?featherlight=false&width=90pc)

9. In the **Route table** interface

- **Name**, enter **```Route table - Private```**
- **VPC**, select **ASG** vpc
- Select **Cretae route table**

![Create NAT Gateway](/images/4-CreateEc2Server/4.3-natgateway/0009-natgaw.png?featherlight=false&width=90pc)

10. Finish creating **Route table - Private**

![Create NAT Gateway](/images/4-CreateEc2Server/4.3-natgateway/00010-natgaw.png?featherlight=false&width=90pc)

11. In the **Route table - Private** interface

- Select **Subnet Associations** tab
- Select **Edit subnet associations**

![Create NAT Gateway](/images/4-CreateEc2Server/4.3-natgateway/00011-natgaw.png?featherlight=false&width=90pc)

12. In the **Edit subnet associations** interface

- Choose **Private subnet 1** and **Private subnet 2**
- Select **Save associations**

![Create NAT Gateway](/images/4-CreateEc2Server/4.3-natgateway/00012-natgaw.png?featherlight=false&width=90pc)

13. In the **Route table - Private** interface

- Select **Routes** tab
- Select **Edit routes**

![Create NAT Gateway](/images/4-CreateEc2Server/4.3-natgateway/00013-natgaw.png?featherlight=false&width=90pc)

14. In the **Edit routes** interface

- Select **Add route**
- Select **Destination**: **0.0.0.0/0**
- **Target**: **NAT Gateway**
- Select **Save changes**

![Create NAT Gateway](/images/4-CreateEc2Server/4.3-natgateway/00014-natgaw.png?featherlight=false&width=90pc)

15. Double check **Routes**

![Create NAT Gateway](/images/4-CreateEc2Server/4.3-natgateway/00015-natgaw.png?featherlight=false&width=90pc)

16. Test ping amazon.com successfully from EC2 Private.

```
ping amazon.com -c5
```

![Create VPC](/images/4-CreateEc2Server/4.2-ec2connect/00018-ec2connect.png?featherlight=false&width=90pc)