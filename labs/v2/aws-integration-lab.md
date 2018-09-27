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
# Introduction

One of the most compelling reasons to adopt VMware Cloud on AWS is to integrate your existing systems which sit in your VMware cloud environment, with application platforms which reside in your AWS Virtual Private Cloud (VPC) environment. The intergration which VMware and AWS have created allows for these services to communicate, for free, across a private network address space for services such as EC2 instances, which connect into subnets within a native AWS VPC, or with platform services which have the ability to connect to a VPC Endpoint, such as S3 Storage.

In this lab we will work through some common basic integrations which you can utilise in your VMware Cloud on AWS platform.

Note: There is a requirement in this lab to have completed the steps in the [Working with your SDDC Lab](https://vmc-field-team.github.io/labs/working-with-sddc-lab/) concerning Content Library creation and Network creation and firewall rule creation.

## Integration with AWS Simple Storage Service (S3)

### AWS S3 Backed vSphere Content Library

vSphere Content Libraries enable customers to take advantage of different types of storage backing than just vSphere Datastores. The primary requirement is that the content endpoint is accessible over HTTP(s), which means that a number of solutions can be used from a simple web server like Nginx to an advanced distributed object store like AWS's Simple Storage Service (S3).

The work ow to create a 3rd Party vSphere Content Library on S3 is as follows:

1. Upload and organize the content on S3
2. Run a python script to index and generate the Content Library metadata
3. With the ability to remotely index and generate the Content Library metadata files, you do not have to keep a local copy of all your content. All changes can be made directly on the S3 bucket and then simply re-run the script to generate the updated metadata which can even be scheduled as a simple cron job.

![S3-1](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/S3-1.jpg)

The python script referenced which can be downloaded [here](https://code.vmware.com/samples/4388/automating-the-creation-of-3rd-party-content-library-directly-on-amazon-s3) can now index content both locally as well as a remote S3 bucket.

In case you did not know, S3 usage (ingress/egress) from a customers SDDC is 100% free for VMware Cloud on AWS customers by simply using a linked S3 Endpoint. This means you can take advantage of S3 to store your templates ISOs and other static files, which can also be shared by other SDDCs. This means you are not consuming any of your primary storage for static content and can be used for what it was meant, for your workloads.

For the purpose of this exercise, and in the interest of time, the contents of this exercise have been uploaded to an existing S3 bucket in AWS for you. Let's create the S3 backed Content Library!

#### Add S3 Backed Content Library

![S3-2](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/S3-2.jpg)

1. On your VMware Cloud on AWS vCenter window click on **Menu**
2. Click on **Content Libraries**

    ![S3-3](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/S3-3.jpg)
3. In your Content Library window, click the **+** sign to add a new Content Library.

    ![S3-4](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/S3-4.jpg)
4. Name your Content Library *S3 Content Library* or any other name of your choosing
5. (Optional) Enter some notes for your Content Library
6. Click **NEXT** button

    ![S3-5](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/S3-5.jpg)
7. Select **Subscribed content library**
8. Under *Subscription URL* enter the following:
    ```link
    http://vmc-elw-vms.s3-accelerate.amazonaws.com/lib.json
    ```
9. Make sure Download content is set to *immediately*
10. Click **NEXT**

    ![S3-6](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/S3-6.jpg)
11. On the *Unable to verify identity of the subscription host* notification click *YES*

    ![S3-7](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/S3-7.jpg)
12. Highlight the **WorkloadDatastore** as the storage location
13. Click **Next** button

    ![S3-8](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/S3-8.jpg)
14. Click *Finish* button. Your content library should take about 20+ minutes to complete syncing.

It may take a few minutes for your Content to sync, periodically check your content library to ensure you see the content.

Congratulations, you have completed this exercise.

## AWS Relational Database Service (RDS) Integration

### Deploy Photo VM

As a first step in setting up our integration between the VMware vSphere platform in VMware Cloud on AWS and native AWS services, we are going deploy a VM which we will use for this demo. This VM will be referred to as "Photo VM". Please follow the instructions below.

![RDS1](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS1.jpg)

1. If not already opened, open your VMware Cloud on AWS vCenter and click on the *Menu* drop down
2. Select *Content Libraries*

    ![RDS2](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS2.jpg)
3. Click on your previously created Content Library named *Student#* (where # is your student number)

    ![RDS3](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS3.jpg)
4. Make sure you click on the **Template** tab
5. Right-click on the **photo app** Template
6. Select *New VM from This Template*

    ![RDS4](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS4.jpg)
    **IIMPORTANT - Please ensure that you select Customize the operating system checkbox and apply the customization specification which you created earlier**
7. Name the virtual machine **PhotoApp#** (where # is your student #)
8. Expand the location and select **Workloads**
9. Click **Next**

    ![RDS5](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS5.jpg)
10. Expand the destination to select **Compute-ResourcePool** as the compute resource
11. Click **Next**

    ![RDS6](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS6.jpg)
12. In the **Review details** step click *Next*

    ![RDS7](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS7.jpg)
13. In the **Select storage** step, highlight the *WorkloadDatastore*
14. Click **Next**

    ![RDS8](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS8.jpg)
15. Select the network you created in a previous step for your VM
16. Click **Next**

    ![RDS9](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS9.jpg)
17. Click **Finish**

    ![RDS10](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS10.jpg)
18. Monitor the deployment of your VM until it's completed

    ![RDS11](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS11.jpg)
19. Check for completion of the deployment of your VM
20. Click *Menu*
21. Select *VMs and Templates*

    ![RDS12](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS12.jpg)
22. Check to make sure your VM is powered on. If not, right-click on your VM
23. Select *Power -> **Power On*

    ![RDS13](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS13.jpg)
24. Make sure your VM is assigned an IP addresses (may need to a few minutes for this information to be populated). Make a note of this IP address for a future step.

    ![RDS14](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS14.jpg)
25. Go back your VMware Cloud on AWS portal and click on the *Network* tab in order to request a Public IP address
26. Under the *Compute Gateway* click and expand *Public IPs*
27. Click on *REQUEST PUBLIC IP**

    ![RDS15](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS15.jpg)
28. (Optional) Enter Notes for this public IP, such as the name of the VM we are linking this too
29. Click on *Request*

    ![RDS16](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS16.jpg)
30. You should see a similar notification as the one above

    ![RDS17](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS17.jpg)
31. Take note of your newly acquired Public IP address

    ![RDS18](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS18.jpg)
32. Next you will create a NAT rule from the newly acquired Public IP address you noted in your last step to the internal IP address of the VM you created. Click on *NAT* under the *Compute Gateway* section to expand the NAT Rules
33. Click *ADD NAT RULE*

    ![RDS19](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS19.jpg)
34. Give your NAT rule a name
35. Your new Public IP address should be pre-filled for you, if not, select it now
36. Under *Service* select *Any (All Traffic)*
37. Type your VM's internal IP address
38. Click the *SAVE* button

    ![RDS20](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS20.jpg)
39. You should get a *NAT rule successfully created* notification

    ![RDS20](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS21.jpg)

40. Expand *Firewall Rules* under the Compute Gateway section
41. Click *ADD RULE*

    ![RDS22](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS22.jpg)
42. Give your Firewall Rule a name
43. Select *All Internet and VPN* for Source
44. Type the Public IP Address you noted under *Destination*
45. Select *Any (All Traffic)* for *Service*
46. Click **SAVE** button

    ![RDS23](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS23.jpg)
47. You should get a *Firewall rule successfully created* notification

    ![RDS24](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS24.jpg)

    On your browser, open a new tab and go to: <https://vmcworkshop.signin.aws.amazon.com/console>
48. For Account ID or alias ensure *vmcworkshop* is specified
49. IAM user name - **Student#** (where # is the number assigned to you)
50. Password - **VMCworkshop1211**
51. Click *Sign In*

    ![RDS25](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS25.jpg)
52. You are now signed in to the AWS console. Make sure the region selected is *Oregon* in the top right hand corner of the AWS Console
53. Click on the *RDS* service under the "Database" section

    ![RDS26](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS26.jpg)
54. In the left pane click on *Instances*
55. Click on the RDS instance that corresponds to your Student number

    ![RDS27](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS27.jpg)
56. Scroll down to the *Details* area and under *Security and network* notice that the RDS instance is not publicly accessible, meaning this instance can only be accessed from within AWS

    ![RDS28](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS28.jpg)
57. Go back to the main *Services* page in the AWS console by clicking the *Services* link in the top left corner of the console
58. Scroll down to *Networking & Content Delivery* and click *VPC*

    ![RDS29](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS29.jpg)
59. Click on **Security Groups** in the left pane under the "Security" section
60. Choose the **rds-launch-wizard-#** RDS Security group corresponding to your student number (where # is the number assigned to you)
61. After highlighting the appropriate security group click on the *Inbound Rules* tab below.

    VMware Cloud on AWS establishes routing in the default VPC Security Group, only RDS can leverage this or create its own.
62. Notice that the CIDR block range of your Student#-LN Logical Network you created in VMware Cloud on AWS is authorized for MySQL on port 3306. This was done for you ahead of time

    ![RDS30](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS30.jpg)

    AWS Relational Database Service (RDS), also creates its own Elastic Network Interface (ENI) for access which is separate from the ENI created by VMware Cloud on AWS.
63. Click on *Services* in the top left of the console to go back to the Main Console
64. Click on *EC2* under the Compute section

    ![RDS31](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS31.jpg)
65. In the EC2 Dashboard click *Network Interfaces* in the left pane under the *Network Interfaces* section
66. All Student environments belong to the same AWS account, therefore, hundreds of ENI's may exist. In order to minimize the view, type *RDS* in the filter area and press *Enter* to add a filter
67. Highlight your **rds-launch-wizard-#** corresponding to your student number (where # is the number assigned to you)
68. Make note of the *Primary private IPv4* IP address for the next step

    ![RDS32](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS32.jpg)
69. Open an additional browser tab and type your public IP address you requested in the VMware Cloud on AWS portal in the browser address bar followed by /Lychee (case sensitive) ie: x.x.x.x/Lychee
70. Enter the database connection information below (case sensitive), using the IP address you noted in the previous step from the RDS ENI:
    Database Host: **x.x.x.x:3306**
    Database Username: **student#** (where # is the number assigned to you)
    Database Password: **VMware1!**
71. Click *Connect*

You have now successfully created a Hybrid Application utilises components across both your VMware on AWS SDDC environment and your AWS services.

This functionality provides customers now with choices around how applications are migrated to the cloud. You can now split your application across platforms and consume services in vSphere and AWS as it makes sense. This level of choice can really help those who are looking to migrate to the cloud, accelerate that process by not getting "bogged down" in refactoring parts of an application which are difficult to move to hyperscale cloud.

## AWS Elastic File System (EFS) Integration

### EFS VM Creation

In our next section on integrating AWS services with VMware Cloud on AWS. We will utilise the AWS Elastic File System (EFS) which we can use to mount NFS shares to our VMware Cloud on AWS hosted virtual machines.

![EFS1](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS1.jpg)

1. Navigate to your Content Library, click *Menu* on your VMware Cloud on AWS vCenter Server
2. Select *Content Libraries*

    ![EFS2](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS2.jpg)
3. Click on your *Student#* Content Library

    ![EFS3](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS3.jpg)
4. Make sure the *Templates* tab is selected
5. Right-click on the *efs* template
6. Select *New VM from This Template*

    ![EFS4](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS4.jpg)
    **IIMPORTANT - Please ensure that you select Customize the operating system checkbox and apply the customization specification which you created earlier**
7. Name your VM **EFSVM#** (where # is your student number)
8. Select *Workloads* for the location of your VM
9. Click **Next**

    ![EFS5](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS5.jpg)
10. Select *Compute-ResourcePool* as the destination for your VM
11. Click *Next*

    ![EFS6](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS6.jpg)
12. Click **Next**

    ![EFS7](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS7.jpg)
13. Select **WorkloadDatastore** for storage
14. Click **Next**

    ![EFS8](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS8.jpg)
15. Select your Destination Network.  This should be *Student#-LN*
16. Click **Next**

    ![EFS9](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS9.jpg)

17. Click **Finish**

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