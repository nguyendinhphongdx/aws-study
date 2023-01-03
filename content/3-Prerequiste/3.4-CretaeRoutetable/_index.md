---
title : "Create Route Table"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 3.4 </b> "
---

#### Create Route Table to route outbound internet through Internet Gateway.


1. In the **VPC** interface

- Select **Route tables**
- Select **Create route table**

![Create VPC](/images/3-Prerequiste/3.2-igwandroutetable/0007-igwandroutetable.png?featherlight=false&width=90pc)

2. Configure **Route table**

- **Name**: enter `Route table-Public`
- **VPC**: select **ASG** VPC. VPC ID will be automatically filled in.
- Select **Create route table**

![Create VPC](/images/3-Prerequiste/3.2-igwandroutetable/0008-igwandroutetable.png?featherlight=false&width=90pc)

3. Finish creating **Route table**

![Create VPC](/images/3-Prerequiste/3.2-igwandroutetable/0009-igwandroutetable.png?featherlight=false&width=90pc)

4. Execute **Edit route**

- Select **Actions**
- Select **Edit routes**

![Create VPC](/images/3-Prerequiste/3.2-igwandroutetable/00010-igwandroutetable.png?featherlight=false&width=90pc)

5. In the **Edit routes** interface

- Select **Add route**
- Then fill in the **Destination CIDR** : `0.0.0.0/0` representing the Internet.
- In the **Target** section: select **Internet Gateway**, then select the **Internet Gateway** we created. **Internet Gateway ID** will be automatically filled in.
- Select **Save changes**

![Create VPC](/images/3-Prerequiste/3.2-igwandroutetable/00011-igwandroutetable.png?featherlight=false&width=90pc)

6. Complete and recheck **Routes**

![Create VPC](/images/3-Prerequiste/3.2-igwandroutetable/00012-igwandroutetable.png?featherlight=false&width=90pc)

7. Make sure **Route table - Public** is selected.

- Select **subnet associations** tab
- Select **Edit subnet associations**

![Create VPC](/images/3-Prerequiste/3.2-igwandroutetable/00013-igwandroutetable.png?featherlight=false&width=90pc)

8. In step **Edit subnet associations**

- Expand the **Subnet ID** column by dragging the pane to the right.
- Select **Public subnet 1** and **Public subnet 2** we have created.
- Select **Save associations**

![Create VPC](/images/3-Prerequiste/3.2-igwandroutetable/00014-igwandroutetable.png?featherlight=false&width=90pc)

9. Complete and recheck **Subnet associations**

![Create VPC](/images/3-Prerequiste/3.2-igwandroutetable/00015-igwandroutetable.png?featherlight=false&width=90pc)