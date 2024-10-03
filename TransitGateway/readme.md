##################Transit Gateway#################################

Step 1: Confiuring the VPC A.

-> Create Vpc-A in with CIDR 10.1.0.0/16 

![alt text](image.png)

-> Create public subnet A with CIDR 10.1.0.0/24 

![alt text](image-1.png)

-> Create Internet Gateway  and attach to VPC A.

![alt text](image-2.png)

![alt text](image-3.png)

-> Add subnet association to route table of VPC.

![alt text](image-4.png)

-> add route of internet gateway to route table

![alt text](image-5.png)

Step 2: Similarly create VPC B with CIDR 10.2.0.0/16 and VPC C with 10.3.0.0/16 with private subnets 10.2.0.0/24 and 10.3.0.0/24 respectively  and add subnet associations.

![alt text](image-6.png)

![alt text](image-7.png)

![alt text](image-8.png)


Step 3: Create instance in each VPC

![alt text](image-9.png)

-> public instance security groups:

SSH     --->       0.0.0.0/0

ALL Traffic  -->   10.0.0.0/8 

->  -> private instance security groups:

ALL Traffic  -->   10.0.0.0/8 


Step 4: Create Transit Gateway

![alt text](image-10.png)

![alt text](image-11.png)

Step 5: create a Transit Gateway attachment for each VPC

![alt text](image-12.png)

![alt text](image-13.png)

-> All the attachments:

![alt text](image-14.png)

Step 6: Adding routes for transit gateway to each route tables.

-> public route table A

![alt text](image-15.png)

-> private route table B

![alt text](image-16.png)

-> private route table C

![alt text](image-17.png)


Step 7: Connect to public instance A through SSH

![alt text](image-18.png)


Step 8: ping the private instance inside the public instance

-> ping private instance B with private ip 10.2.0.232

![alt text](image-19.png)

-> -> ping private instance B with private ip 10.3.0.130

![alt text](image-20.png)


###############THE END#########################