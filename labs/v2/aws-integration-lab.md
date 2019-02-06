---
layout: single
title: "AWS Integration Lab Manual"
categories: labs
date: 2018-07-20
tags: workshop
toc: true
classes: wide
author_profile: false
comments: true
categories: labs
---
# Understanding Integrations with AWS Services

As the above diagram illustrates, the VMware stack not only sits next to the AWS services, but is tightly integrated with these services. This introduces a new way of thinking about how to design and leverage AWS services with your VMware SDDC. Some integrations our customers are using include:

* VMware front-end and RDS backend
* VMware back-end and EC2 front-end
* AWS Application Load Balancer (ELBv2) with VMware front-end (pointing to private IPs)
* Lambda, Simple Queueing Service (SQS), Simple Notification Service (SNS), S3, Route53, and Cognito
* AWS Lex, and Alexa with the VMware Cloud APIs

These are only a few of the integrations we've seen.There are many different services that can be integrated into your environment.
In this exercise we'll be exploring integrations with both AWS Simple Storage Service (S3) and AWS Relational Database Service (RDS).

Note: There is a requirement in this lab to have completed the steps in the [Working with your SDDC Lab](https://vmc-field-team.github.io/labs/v2/working-with-sddc-lab/) concerning Content Library creation and Network creation and firewall rule creation.

## How are these integrations possible?

![aws01](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/aws01.jpg)

In addition to sitting within the AWS Infrastructure, there is an Elastic Network Interface (ENI) connecting VMware Cloud on AWS and the customer's Virtual Private Cloud (VPC), providing a high-bandwidth, low latency connection between the VPC and the SDDC. This is where the traffic flows between the two technologies (VMware and AWS). There are no EGRESS charges across the ENI within the same Availability Zone and there are firewalls on both ends of this
connection for security purposes.

## How is traffic secured across the ENI?

![aws02](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/aws02.jpg)

From the VMware side (see image below), the ENI comes into the SDDC at the Compute Gateway (NSX Edge). This means, on this end of the technology we allow and disallow traffic from the ENI with NSX Firewall rules. By default, no ENI traffic can enter the SDDC. Think of this as a security gate blocking traffic to and from AWS Services on the ENI until the rules are modified.

![aws03](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/aws03.jpg)

On the AWS Services side (see image below), Security Groups are utilized. For those who are not familiar with Security Groups, they act as a virtual firewall for different services (VPCs, Databases, EC2 Instances, etc). This should be configured to deny traffic to and from the VMware SDDC unless otherwise configured.

![aws04](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/aws04.jpg)

In this exercise, everything has been configured on the AWS side for you. You will however walk through how to open AWS traffic to come in and out of your VMware Cloud on AWS SDDC.

## AWS S3 Backed vSphere Content Library

vSphere Content Libraries enable customers to take advantage of different types of storage backing than just vSphere Datastores. The primary requirement is that the content endpoint is accessible over HTTP(s), which means that a number of solutions can be used from a simple web server like Nginx to an advanced distributed object store like AWS's Simple Storage Service (S3).

The work ow to create a 3rd Party vSphere Content Library on S3 is as follows:

1. Upload and organize the content on S3
2. Run a python script to index and generate the Content Library metadata
3. With the ability to remotely index and generate the Content Library metadata files, you do not have to keep a local copy of all your content. All changes can be made directly on the S3 bucket and then simply re-run the script to generate the updated metadata which can even be scheduled as a simple cron job.

![S3-1](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/S3-1.jpg)

The python script referenced which can be downloaded [here](https://code.vmware.com/samples/4388/automating-the-creation-of-3rd-party-content-library-directly-on-amazon-s3) can now index content both locally as well as a remote S3 bucket.

In case you did not know, S3 usage (ingress/egress) from a customers SDDC is 100% free for VMware Cloud on AWS customers by simply using a linked S3 Endpoint. This means you can take advantage of S3 to store your templates ISOs and other static files, which can also be shared by other SDDCs. This means you are not consuming any of your primary storage for static content and can be used for what it was meant, for your workloads.

For the purpose of this exercise, and in the interest of time, the contents of this exercise have been uploaded to an existing S3 bucket in AWS for you. Let's create the S3 backed Content Library!

### Add S3 Backed Content Library

![S3-2](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/S3-2.jpg)

1. On your VMware Cloud on AWS vCenter window click on **Menu**
2. Click on **Content Libraries**

    ![S3-3](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/S3-3.jpg)
3. In your Content Library window, click the **+** sign to add a new Content Library.

    ![S3-4](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/S3-4.jpg)
4. Name your Content Library **S3 Content Library** or any other name of your choosing
5. (Optional) Enter some notes for your Content Library
6. Click **NEXT** button

    ![S3-5](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/S3-5.jpg)
7. Select **Subscribed content library**
8. Under **Subscription URL** enter the following:
    ```link
    http://vmc-elw-vms.s3-accelerate.amazonaws.com/lib.json
    ```
9. Make sure Download content is set to *immediately*
10. Click **NEXT**

    ![S3-6](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/S3-6.jpg)
11. On the *Unable to verify identity of the subscription host* notification click **YES**

    ![S3-7](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/S3-7.jpg)
12. Highlight the **WorkloadDatastore** as the storage location
13. Click **Next** button

    ![S3-8](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/S3-8.jpg)
14. Click **Finish** button. Your content library should take about 20+ minutes to complete syncing.

It may take a few minutes for your Content to sync, periodically check your content library to ensure you see the content.

Congratulations, you have completed this exercise.

## AWS Relational Database Service (RDS) Integration

Amazon RDS makes it easy to set up, operate, and scale a relational database in the cloud. It provides cost- efficient and resizable capacity while automating time-consuming administration tasks such as hardware provisioning,database setup, patching and backups. It frees you to focus on your applications so you can give them the fast performance, high availability, security and compatibility they need.

In this exercise, you will be able to integrate a VMware Cloud on AWS virtual machine to work in conjunction with a relational database running in Amazon Web Services (AWS) that has been previously setup on your behalf.

### Make Note of Webserver01 IP Address
Insert pic here

You will be using the VM created in the previous module in order to complete this exercise.
1. In your vCenter interface for VMware Cloud on AWS, find your "Webserver01" VM you deployed, and ensure it has been assigned an IP address as shown in the graphic.


### Assign Public IP

Insert pic here

1. Go back your VMware Cloud on AWS portal and click on the **Networking & Security** tab in order to request a Public IP address.
2. Click **Public IPs** in the left pane.
3. Click on **REQUEST NEW IP**.
4. In the notes area type **PhotoApp IP**.
5. Click **SAVE**.

### Note New Public IP

insert pic here

Take note of your newly created Public IP.

### Create a NAT Rule

1. Click **NAT** in the left pane.
2. Click **ADD NAT RULE**.
3. Type **PhotoApp NAT **for Name.
4. Ensure the Public IP you requested in the previous step appears under Public IP.
5. Leave **All Traffic** (no change).
6. Type the IP address of your "Webserver01" VM you noted at the beginning of this exercise.
7. Click **SAVE**.

### AWS Relational Database Service (RDS) Integration

insert pic here

1. On your browser, open a new tab and go to:
    ```link
    https://vmcworkshop.signin.aws.amazon.com/console
    ```
2.  Account ID or alias - **vmcworkshop**.
3.  IAM user name - **Student#** (where # is the number assigned to you).
4.  Password - **VMCworkshop1211**.
5.  Click **Sign In**.

### RDS Information

insert pic here

1. You are now signed in to the AWS console. Make sure the region selected is **N. Virginia** or
**Oregon**
2. Click on the **RDS** service (You may need to expand **All services**

### RDS Instance

insert pic here

1. In the left pane click on **Instances**.
2. Click on the RDS instance that corresponds to designated student number.

### Navigate to Security Groups

insert pic here

1. Scroll down to the **Details** area and under **Security and network** notice that the RDS instance is not publicly accessible, meaning this instance can only be accessed from within AWS.
2. Click in the blue hyperlink under **Security groups**.

### Security Groups

insert pic here

1. Choose the **rds-launch-wizard-#** RDS Security group corresponding to you (may not match your student number).
2. After highlighting the appropriate security group click on the **Inbound** tab below VMware Cloud on AWS establishes routing in the default VPC Security Group, only RDS can leverage this or create its own.

### Outbound Traffic

insert pic here

1. Click **Outbound** tab.
2. You can see All traffic (internal to AWS) allowed, this includes your VMware Cloud on AWS SDDC logical networks.

### Elastic Network Interface (ENI)

insert pic here

AWS Relational Database Service (RDS), also creates its own Elastic Network Interface (ENI) for
access which is separate from the ENI created by VMware Cloud on AWS.

1. Click on **Services** to go back to the Main Console.
2. Click on **EC2**.

### ENI (Continued)

insert pic here

1. In the EC2 Dashboard click **Network Interfaces** in the left pane.
2. All Student environments belong to the same AWS account, therefore, hundreds of ENI's may exist. In order to minimize the view, type **RDS** in the search area and press Enter to add a filter.
3. Highlight your rds-launch-wizard-# corresponding to your student number based on the second octect of the CIDR block in the last column.
 **Notes:  In this example the CIDR block is 172.6.8.187, this would correspond to student "6"**
4. Make note of the **Primary private IPv4 IP** address for the next step.

### Photo App

insert pic here

1. On your smart phone (tablet or personal computer), open up a browser and type your public IP address you requested in the VMware Cloud on AWS portal in the browser address bar followed by /Lychee (case sensitive) ie: 1.2.3.4/Lychee
2. Enter the database connection information below (case sensitive), using the IP address you noted in the previous step from the RDS ENI:
Database Host: **x.x.x.x:3306**
Database Username: **student#** (where # is the number assigned to you)
Database Password: **VMware1!**
3. Click **Connect**.

### Login to Photo App

insert pic here

1. In the upper left part of the browser window click the little arrow to log in.

### Enter Login Information

insert pic here

1. Type "vmworld" for both user name and password (not sure if this is correct).
2. Click "Sign In".
   
### Photo Albums

insert pic here
Congratulations, you have successfully logged in to the photo app!

OPTIONAL: Feel free to take a picture of the room with your smart phone and upload it to the Public folder.

In summary, the front end (web server) is running in VMware Cloud on AWS as a VM, the back end which is a MySQL database is running in AWS Relational Database Service (RDS) and communicating through the Elastic Network Interface (ENI) that gets established upon the creation of the SDDC.

## AWS Elastic File System (EFS) Integration

### EFS VM Creation

In our next section on integrating AWS services with VMware Cloud on AWS. We will utilize the AWS Elastic File System (EFS) which we can use to mount NFS shares to our VMware Cloud on AWS hosted virtual machines.

![EFS1](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS1.jpg)

1. Navigate to your Content Library, click *Menu* on your VMware Cloud on AWS vCenter Server.
2. Select **Content Libraries**.

    ![EFS2](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS2.jpg)
3. Click on your *Student#* Content Library

    ![EFS3](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS3.jpg)
4. Make sure the **Templates** tab is selected.
5. Right-click on the **efs** template.
6. Select **New VM from This Template**.

    ![EFS4](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS4.jpg)
    **IMPORTANT - Please ensure that you select Customize the operating system checkbox and apply the customization specification which you created earlier**.
7. Name your VM **EFSVM#** (where # is your student number).
8. Select **Workloads** for the location of your VM.
9. Click **Next**.

    ![EFS5](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS5.jpg)
10. Select *Compute-ResourcePool* as the destination for your VM
11. Click **Next**.

    ![EFS6](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS6.jpg)
12. Click **Next**.

    ![EFS7](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS7.jpg)
13. Select **WorkloadDatastore** for storage
14. Click **Next**.

    ![EFS8](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS8.jpg)
15. Select your Destination Network.  This should be *Student#-LN*
16. Click **Next**.

    ![EFS9](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS9.jpg)

17. Click **Finish**.

    ![EFS10](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS10.jpg)
18. Make sure to Power on your VM and ensure it has an assigned private IP address

    ![EFS11](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS11.jpg)

    On your browser, open a new tab and go to:

    ```link
    https://vmcworkshop.signin.aws.amazon.com/console
    ```
19. Account ID or alias - **vmcworkshop**
20. IAM user name - **Student#** (where # is the number assigned to you)
21. Password - **VMCworkshop1211**
22. Click **Sign In**

    ![EFS12](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS12.jpg)
23. You are now signed in to the AWS console. Make sure the region selected is *Oregon*
24. Click on the *EFS* service under the storage section

    ![EFS13](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS13.jpg)
25. Select your Student# NFS (where # is the number assigned to you)
26. Note the IP address

    ![EFS14](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS14.jpg)
27. Back on your vCenter Server tab, click on *Launch Web Console* for your EFS VM (Might need to allow pop ups in browser). Log in using the following credentials:
  User: **root**
  Password: **VMware1!**

Enter the following commands at the prompt:

```bash
cd /
mkdir efs
sudo mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2
MOUNT_TARGET_IP:/ efs
```

Where MOUNT_TARGET_IP is the IP you noted for your EFS

```bash
cd efs
touch hello.world
ls
```

You have now integrated AWS EFS with a VM running in VMware Cloud on AWS.