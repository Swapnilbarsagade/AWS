###############Statichosting S3 bucket with cloudfront, Route53, SSL certificate##########################

Step 1 : Assigning NameServer of AWS on Hostinger

    -> It will take about 4-5 hrs.

Step 2 : Creating S3 bucket.

    -> Create bucket with name as given below as sudomain path in your bucket route.
             harryporter.swapnilbdevops.online  ---- example

    -> Grant Public Access

    -> enable pubic access in ACL and enable ACL

Step 3 : Assigning subdomain using Route53.

![alt text](S3record.png)

    -> Check if subdomain hits and give result.

![alt text](bucketdomain.png)

    -> Given subdomain hits but is not secure.

Step 4 : Creating Cloudfront Distribution.

    -> Select Origin name

![alt text](originname.png)

    -> Do not enable WAF service.

    -> Give Alternate domain name (CNAME)

![alt text](ADN.jpeg)

    -> Request certificate as given in step 5. and refresh and add newly created certificate.

![alt text](addcert.jpeg)

    -> Give default root object

![alt text](rootoject.jpeg)

    -> Lastly  Create the distribution (it will take time since send request to every edge location server to store cache about target site)

Step 5 : Request SSL Certificate. (region will be on N. Virginia)

    -> request public certificate and give subdomain(buckets name) to certificate.

![alt text](certificatedname.jpeg)

    -> Create Record in Route 53 to issue certificate.

![alt text](dnsrecordinr53.png)

    -> After following steps certicate is issued.

![alt text](issuecertificate.jpeg)

Step 6 : Edit S3 bucket record to add Cloudfront Distribution as below:

![alt text](edittocloudfrontdistro.png)

Step 7: Hit the subdomain and now since certificate is added, so it will open securely.

If it is unsecure add http:// in front of it.

![alt text](finalresult.png)
