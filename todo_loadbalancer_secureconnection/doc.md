##############Creating load balancer endpoint for todo microservice using secure connection###############

Step 1 : Create EC2 instance & install httpd server in it.

    -> Script for launching httpd server

        #!/bin/bash
        yum update -y
        yum install httpd -y
        systemctl start httpd
        systemctl enable httpd

    -> upload file(zip) from local using moboXterm. and add static files to httpd dir.

    -> Add todo directory to httpd directory i.e.
        /var/www/html

    -> Copy ec2 public ip and check if todo app is working.
        65.0.31.142/todo/

![alt text](publiciptodo.png)

Step 2 : Create Load balancer and add listener & target groups for ec2 instance.

    -> Creating application load balancer
      - Load balancer name = todolb
      - select subnets.
      - Listener and routing is done below.


    -> create target group
      - Give Name = todotg
      - type = instance
      - port = http(80)
      - Health check path = /todo/index.html
      - register target i.e. include require instance

![alt text](registertarget.jpeg)

    -> Add Listener to load balancer for http (unsecure connection).
      - Select Protocol and port, here it is http 80
      - Add created target group. as shown in below.

![alt text](http_listener.jpeg)

    -> hit the dns of load balancer as below

![alt text](lbdns_unsecure.png)

Step 3 : Create https listener & create add ssl certificate for secure connection.

    -> Add Listener to load balancer for https (secure connection).
      - Select Protocol and port, here it is https 443
      - Select same target group as for http listener.

![alt text](https_listener.jpeg)

    -> Create domain for load balancer using Route 53

![alt text](lbdomain_route.jpeg)

    -> Create SSL certificate using ACM(AWS Certificate Manager) and add to listener.

![alt text](ssl_certifacte.jpeg)

    -> Create listener and hit lb_domain to see if connection is secure.
       if not add https:// in front of lb_domain.

![alt text](securelb_domain.png)

Since We don't want unsecure connection, we'll redirect http 80 listener to https as shown below.

Step 4 : Redirecting http listener.

    -> Edit listener http 80 as shown.

![alt text](redirecting_http.jpeg)

    -> copy lbdomain add http:// in front of it and hit it and see the result.
