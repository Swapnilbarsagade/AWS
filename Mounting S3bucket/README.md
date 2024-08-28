##############Mounting S3 bucket on AWS ubuntu instance######################################

Mounting an S3 bucket to an EC2 instance, you are essentially making the S3 bucket available as a file system on the EC2 instance. This allows you to easily store and access data from the S3 bucket on the EC2 instance.

Step 1 : Creating S3 bucket

    1. Log in to your AWS console.

    2. Navigate to the S3 service.

    3. Click on the “Create bucket” button.

    4. Give your bucket a name and select the region in which you want to create it.

    5. Choose the desired settings for your bucket (e.g., versioning, encryption, object lock, ACL).

    6. Click on the “Create” button to create your bucket.

Step 2 : Install and configure s3fs

    1. SSH into your EC2 instance.

---

    2. Install s3fs.

    s3fs is a FUSE (Filesystem in Userspace) based file system that allows you to mount an S3 bucket to your EC2 instance.

    sudo apt-get update && sudo apt-get install -y s3fs

---

    3. Configure s3fs

    create a file named .passwd-s3fs in the home directory of the user who will be mounting the S3 bucket.  In this file, enter your AWS access key ID and secret access key in the following format:

    accessKeyId:secretAccessKey

---

    4. change the permissions of the .passwd-s3fs file:

    chmod 600 ~/.passwd-s3fs

Step 3: Mount the S3 bucket to your EC2 instance.

    1. Create a directory where you want to mount the S3 bucket

    sudo mkdir /mnt/mybucket

---

    2.  Run the following command to mount the S3 bucket and replace “mybucket” with the name of your S3 bucket.

    sudo s3fs mybucket /mnt/mybucket -o passwd_file=~/.passwd-s3fs

Step 4: Verify the mount.

1. Navigate to the directory where you mounted

   sudo su
   cd /mnt/mybucket

---

2. Create a test file in the mounted directory:

   echo "This is a test file" > /mnt/mybucket/test.txt

Step 5 : Verify the created file.

    1.  navigate to your S3 bucket through the AWS console and check for file.

---

    2. Using aws cli:

    1. Install awscli using following command:

    snap install aws-cli --classic   or
    snap install aws-cli    or
    apt install awscli

    2. access aws using secret access key of user:

    aws configure

    3. Check for objects:

    aws s3 ls s3://mybucket/
