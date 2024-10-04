##################Site to Site VPN (connecting ON-premise network to aws cloud network)##############

Step 1: Creating aws site VPC in region 1(osaka).

-> Create VPC with CIDR block 10.20.0.0/16

![alt text](image.png)

-> Add subnet with CIDR 10.20.0.0/24

![alt text](image-1.png)

-> Create Internet Gateway and attach to VPC.

![alt text](image-2.png)

![alt text](image-3.png)

-> Create route table name aws-RT and associated respective subnet to it.

![alt text](image-4.png)

![alt text](image-5.png)

-> add IG route to aws-RT table

![alt text](image-6.png)


Step 2: Creating customer end (cus-end) VPC in region 2(Seoul).

-> Create VPC with CIDR block 10.30.0.0/16

![alt text](image-7.png)

-> Add subnet with CIDR 10.30.0.0/24

![alt text](image-8.png)

-> Create route table name cus-RT and associated respective subnet to it.

![alt text](image-9.png)

![alt text](image-10.png)

-> Create Internet Gateway and attach to VPC.

![alt text](image-11.png)

-> -> add IG route to cus-RT table

![alt text](image-12.png)


Step 3: Create instance in Seoul region.

-> AMI image

![alt text](image-13.png)

-> Security Groups

![alt text](image-14.png)

-> launch instance

![alt text](image-15.png)

Step 4: Create VPN Connections.vpn 