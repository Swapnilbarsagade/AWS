## Site to Site VPN (connecting ON-premise network to aws cloud network)

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


Step 3: Create two instancea, one in  Seoul region and another in Osaka region with following configuration .

-> AMI image

![alt text](image-13.png)

-> Security Groups

  port          | source
  SSH (22)      |  0.0.0.0/0
  ALL TCP       |  10.0.0.0/8
  ALL ICMP IPv4 |  10.0.0.0/8 

![alt text](image-14.png)

-> launch instance

![alt text](image-15.png)

Step 4: Creating the site to site VPN connection.

-> Create Customer Gateway in aws-site i.e osaka region  using public ip of customerend server (seoul region)

![alt text](image-16.png)

-> Create Virtual Private Gateway in aws-site i.e osaka region and attach to VPC.

![alt text](image-17.png)

![alt text](image-18.png)

->  Go to route table and enable route propogation(Actions - edit route propogation - enable -save)

![alt text](image-19.png)


-> Create site to site vpn in aws-site i.e osaka region

    ➔Select Virtual Private Gateway
    ➔Select Customer Gateway
    ➔Routing Options
    ➔Static
    ➔Static IP prefix.
    ➔10.20.0.0/16
    ➔10.30.0.0/24
    ➔Local IPv4 network CIDR 
    ➔10.30.0.0/24 ……………. Use Seoul Subnet CIDR
    ➔ Remote IPv4 network CIDR
    ➔10.20.0.0/24 ……………… Use Osaka Subnet CIDR
    ➔Create VPN Connection

![alt text](image-21.png)

![alt text](image-22.png)

-> Download Configuration file of created VPN

![alt text](image-23.png)

Step 4:  Connecting to Customer Server(Seoul Server) through SSH

-> Run following commands to install openswan

```
sudo -i
yum install openswan -y

```

-> Follow all the steps from the downloaded openswan Configuration File for Tunnel 1

1) Open /etc/sysctl.conf and ensure that its values match the following:

```
net.ipv4.ip_forward = 1
net.ipv4.conf.default.rp_filter = 0
net.ipv4.conf.default.accept_source_route = 0
```

2) Apply the changes in step 1 by executing the command 

```
sysctl -p
```

3) Open /etc/ipsec.conf and look for the line below. Ensure that the # in front of the line has been removed, then save and exit the file.

```
#include /etc/ipsec.d/*.conf
```

4) Create a new file at /etc/ipsec.d/aws.conf if doesn't already exist, and then open it. Append the following configuration to the end in the file:
 #leftsubnet= is the local network behind your openswan server, and you will need to replace the <LOCAL NETWORK> below with this value (don't include the brackets). If you have multiple subnets, you can use 0.0.0.0/0 instead.
 #rightsubnet= is the remote network on the other side of your VPN tunnel that you wish to have connectivity with, and you will need to replace <REMOTE NETWORK> with this value (don't include brackets).

 Note :  Remove “auth=esp ”

```
conn Tunnel1
	authby=secret
	auto=start
	left=%defaultroute
	leftid=43.202.112.252
	right=13.208.91.62
	type=tunnel
	ikelifetime=8h
	keylife=1h
	phase2alg=aes128-sha1;modp1024
	ike=aes128-sha1;modp1024
	keyingtries=%forever
	keyexchange=ike
	leftsubnet=<LOCAL NETWORK>   ---> use cus-end (Seoul) VPC CIDR
	rightsubnet=<REMOTE NETWORK>  ---> use aws-site (Osaka) VPC CIDR
	dpddelay=10
	dpdtimeout=30
	dpdaction=restart_by_peer
```

5) Create a new file at /etc/ipsec.d/aws.secrets if it doesn't already exist, and append this line to the file (be mindful of the spacing!):

```
43.202.112.252 13.208.91.62: PSK "qG7Qqp.UyDm1Q3hbpHtFtbIdvcZMOWQd"
```

-> start the ipsec service

```
systemctl start ipsec.service
```

```
systemctl status ipsec.service
```

```
systemctl restart ipsec.service
```

![alt text](<Screenshot (448).png>)

![alt text](image-24.png)

Step 5: Go to customer end server (Seoul Server) and do the following settings.

 ➔Select Instance 

 ➔Actions

 ➔Networking

 ➔Change source / Destination Check

 ➔ ” Stop” And Save 

 ![alt text](image-25.png)

 Step 6: Add network interface route to cust-RT table in cus-end VPC i.e Seoul Region.

 ![alt text](image-26.png)

 Step 7: Testing the Connection of tunnel

 -> ping the private ip of osaka from seoul.

![alt text](<Screenshot (449).png>)

-> connecting to seoul instance with private  ip

![alt text](<Screenshot (450).png>)


###############################################