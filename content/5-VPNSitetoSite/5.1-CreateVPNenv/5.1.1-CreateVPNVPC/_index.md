---
title : "Create VPC for VPN"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 5.1.1 </b> "
---

#### Create a VPN environment

1. Access **VPC** interface

   - Select **Yours VPC**
   - Select **Create VPC**

![Create VPC](/images/9/0001.png?featherlight=false&width=90pc)

2. In the **Create VPC** interface

   - **Resource**, select **VPC only**
   - **Name**, enter **```ASG VPN```**
   - **IPv4 CIDR block**, enter **```10.11.0.0/16```**

![Create VPC](/images/9/0002.png?featherlight=false&width=90pc)

3. Select **Create VPC**

![Create VPC](/images/9/0003.png?featherlight=false&width=90pc)

4. Create a successful VPC

![Create VPC](/images/9/0004.png?featherlight=false&width=90pc)

5. Access **VPC** interface

   - Select **Subnets**
   - Select **Create subnet**

![Create VPC](/images/9/0005.png?featherlight=false&width=90pc)

6. In the **Create subnet** interface

   - Select **ASG VPN** vpc

![Create VPC](/images/9/0006.png?featherlight=false&width=90pc)


7. In the **Subnet settings** interface

   - **Subnet name**, enter **```VPN Public```**
   - Select **Availability Zone**: **ap-southeast-1a**
   - Select **IPv4 CIDR block** as **10.11.1.0/24** according to the architecture described

![Create VPC](/images/9/0007.png?featherlight=false&width=90pc)

8. Successfully Created **VPN Public**

![Create VPC](/images/9/0008.png?featherlight=false&width=90pc)

9. In the **VPC** interface

   - Select **Subnets**
   - Select **VPN Public**
   - Select **Actions**
   - Select **Edit subnet settings**

![Create VPC](/images/9/0009.png?featherlight=false&width=90pc)

10. Execute **Auto-assign IP settings**

    - Select **Enable auto-assign public IPv4 address**
    - Select **Save**

![Create VPC](/images/9/00010.png?featherlight=false&width=90pc)

11. Successful IP allocation

![Create VPC](/images/9/00011.png?featherlight=false&width=90pc)

12. In the **VPC** interface

    - Select **Internet Gateway**
    - Select **Create internet gateway**

![Create VPC](/images/9/00012.png?featherlight=false&width=90pc)

13. In the **Create internet gateway** interface

    - **Name tag**, enter **```Internet Gateway VPN```**
    - Select **Create internet gateway**

![Create VPC](/images/9/00013.png?featherlight=false&width=90pc)

14. After creating **Internet Gateway VPN** successfully and **State** is **Detached**. Next, we need to Attach the Internet Gateway to VPC ASG VPN.

    - Select **Actions**
    - Select **Attach to VPC**

![Create VPC](/images/9/00014.png?featherlight=false&width=90pc)

15. Select **VPC ASG VPN**, VPC ID will be automatically filled in.

    - Select **Attach Internet Gateway**

![Create VPC](/images/9/00015.png?featherlight=false&width=90pc)

16. **Attach** succeeds when **State** is **Attached**

![Create VPC](/images/9/00016.png?featherlight=false&width=90pc)

17. Next we need to create a Route Table that routes out to the internet through the Internet Gateway. In the **VPC** interface

    - Select **Route Tables**
    - Select **Create route table**

![Create VPC](/images/9/00017.png?featherlight=false&width=90pc)

18. In the **Create route table** interface

    - **Name**, enter **```Route table VPN - Public```**
    - Select **VPC** named **ASG VPN** , **VPC id** will be automatically filled in.
    - Select **Create route table**

![Create VPC](/images/9/00018.png?featherlight=false&width=90pc)

![Create VPC](/images/9/00019.png?featherlight=false&width=90pc)

19. Successfully created route table. In the **Route table VPN - Public** interface

    - Select **Route**
    - Select **Edit route**

![Create VPC](/images/9/00020.png?featherlight=false&width=90pc)

20. In the **Edit routes** interface

    - Select **Add route**
    - Fill in the **Destination CIDR** : **0.0.0.0/0** representing the Internet.
    - In the **Target** section select **Internet Gateway**, then select the **Internet Gateway VPN** we created. **Internet Gateway ID** will be automatically filled in.
    - Select **Save changes**

![Create VPC](/images/9/00021.png?featherlight=false&width=90pc)

![Create VPC](/images/9/00022.png?featherlight=false&width=90pc)

21. Complete and test the route

22. In the **Route table VPN - Public** interface

    - Select **Subnet associations**
    - Select **Edit subnet associations**

![Create VPC](/images/9/00023.png?featherlight=false&width=90pc)


23. In the **Edit subnet associations** interface

    - Expand the Subnet ID column by dragging the pane to the right.
    - Select **subnet VPN Public**.
    - Select **Save associations**

![Create VPC](/images/9/00024.png?featherlight=false&width=90pc)

24. Complete and recheck **Routes**

![Create VPC](/images/9/00025.png?featherlight=false&width=90pc)