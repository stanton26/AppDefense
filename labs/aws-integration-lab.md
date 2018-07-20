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
---
# Introduction

One of the most compelling reasons to adopt VMware Cloud on AWS is to integrate your existing systems which sit in your VMware cloud environment, with application platforms which reside in your AWS Virtual Private Cloud (VPC) environment. The intergration which VMware and AWS have created allows for these services to communicate, for free, across a private network address space for services such as EC2 instances, which connect into subnets within a native AWS VPC, or with platform services which have the ability to connect to a VPC Endpoint, such as S3 Storage.

In this lab we will work through some common basic integrations which you can utilise in your VMware Cloud on AWS platform.

Note: There is a requirement in this lab to have completed the steps in the [Working with your SDDC Lab](https://vmc-field-team.github.io/labs/working-with-sddc-lab/) concerning Content Library creation and Network creation.

## AWS Relational Database Service (RDS) Integration

### Deploy Photo VM

As a first step in setting up our integration between the VMware vSphere platform in VMware Cloud on AWS and native AWS services, we are going deploy a VM which we will use for this demo. This VM will be referred to as "Photo VM". Please follow the instructions below.

1. If not already opened, open your VMware Cloud on AWS vCenter and click on the **Menu** drop down
2. Select **Content Libraries**
3. Click on your previously created Content Library named **Student#** (where # is your student number)
4. Make sure you click on the **Template** tab
5. Right-click on the **photo app** Template
6. Select **New VM from This Template**
7. Name the virtual machine **PhotoApp#** (where # is your student #)
8. Expand the location and select **Workloads**
9. Click **Next**
10. Expand the destination to select **Compute-ResourcePool** as the compute resource
11. Click **Next**
12. In the "Review details" step click **Next**
13. In the **Select storage** step, highlight the "WorkloadDatastore"
14. Click **Next**
15. Select the network you created in a previous step for your VM
16. Click **Next**
17. Click **Finish**
18. Monitor the deployment of your VM until it's completed
19. Check for completion of the deployment of your VM
20. Click **Menu**
21. Select **VMs and Templates**
22. Check to make sure your VM is powered on. If not, right-click on your VM
23. Select **Power** -> **Power On**
24. Make sure your VM is assigned an IP addresses (may need to a few minutes for this information to be populated). Make a note of this IP address for a future step.

### Firewall Rules for RDS Integration

In this step we will ensure that we have the correct firewall rules in place in order for our Photo App VM in VMware Cloud on AWS to talk across to the RDS service in our AWS VPC.

1. Go back your VMware Cloud on AWS portal and click on the **Network** tab in order to request a **Public IP address**
2. Under the **Compute Gateway** click and expand **Public IPs**
3. Click on **REQUEST PUBLIC IP***
4. (Optional) Enter Notes for this public IP, such as the name of the VM we are linking this too.
5. Click on **Request**
6. Take note of your newly acquired Public IP address
7. Next you will create a **NAT rule** from the newly acquired Public IP address you noted in your last step to the internal IP address of the VM you created. Click on **NAT** under the "Compute Gateway" section to expand the NAT Rules
8. Click **ADD NAT RULE**
9. Give your **NAT rule** a name
10. Your new Public IP address should be pre-filled for you, if not, select it now
11. Under **Service** select **Any (All Traffic)**
12. Type your VM's internal IP address
13. Click the **SAVE** button
14. You should get a **NAT rule successfully created** notification
15. Expand **Firewall Rules** under the Compute Gateway section
16. Click **ADD RULE**
17. Give your Firewall Rule a name
18. Select **All Internet and VPN** for **Source**
19. Type the Public IP Address you noted under **Destination**
20. Select **Any (All Traffic)** for **Service**
21. Click **SAVE** button
22. You should get a **Firewall rule successfully created** notification

### AWS Relational Database Service (RDS Configuration)

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-86-Image-135.png)

On your browser, open a new tab and go to: <https://vmcworkshop.signin.aws.amazon.com/console>

1. Account ID or alias -vmcworkshop
2. IAM user name -Student# (where # is the number assigned to you)
3. Password -VMCworkshop1211
4. Click **Sign In**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-87-Image-136.png)
5. You are now signed in to the AWS console. Make sure the region selected is **Oregon**
6. Click on the **RDS** service
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-88-Image-137.png)

7. In the left pane click on **Instances**
8. Click on the RDS instance that corresponds to your Student number
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-88-Image-138.png)
9. Scroll down to the **Details** area and under **Security and network** notice that the RDS instance is not publicly accessible, meaning this instance can only be accessed from within AWS
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-89-Image-139.png)
10. Go back to the main Services page in the AWS console by clicking the **Services** link
11. Scroll down to **Networking & Content Delivery** and click **VPC**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-89-Image-140.png)
12. Click on **Security Groups** in the left pane
13. Choose the r**ds-launch-wizard-#** RDS Security group corresponding to your student number
14. After highlighting the appropriate security group click on the **Inbound Rules** tab below VMware Cloud on AWS establishes routing in the default VPC Security Group, only RDS can leverage this or create its own
15. Notice that the CIDR block range of your Student#-LN Logical Network you created in VMware Cloud on AWS is authorized for MySQL on port 3306. This was done for you ahead of time
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-90-Image-141.png)

    AWS Relational Database Service (RDS), also creates its own Elastic Network Interface (ENI) for access which is separate from the ENI created by VMware Cloud on AWS.
16. Click on **Services** to go back to the Main Console
17. Click on **EC2**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-91-Image-142.png)
18. In the EC2 Dashboard click **Network Interfaces** in the left pane
19. All Student environments belong to the same AWS account, therefore, hundreds of ENI's may exist. In order to minimize the view, type "RDS" in the search area and press **Enter** to add a filter
20. Highlight your **rds-launch-wizard-#** corresponding to your student number
21. Make note of the **Primary private IPv4** IP address for the next step
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-92-Image-143.png)
22. Open an additional browser tab and type your public IP address you requested in the VMware Cloud on AWS portal in the browser address bar followed by /Lychee (case sensitive) ie: x.x.x.x/Lychee
23. Enter the database connection information below (case sensitive), using the IP address you noted in the previous step from the RDS ENI:
    **Database Host**: x.x.x.x:3306
    **Database Username**: student#
    **Database Password**:VMware1!
24. Click **Connect**

## AWS Elastic File System (EFS) Integration

### EFS VM Creation

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-93-Image-144.png)

1. Navigate to your Content Library, click **Menu** on your VMware Cloud on AWS vCenter Server
2. Select **Content Libraries**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-94-Image-145.png)
3. Click on your **Student#** Content Library
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-94-Image-146.png)
4. Make sure the **Templates** tab is selected
5. Right-click on the **efs** template
6. Select **New VM from This Template**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-95-Image-147.png)
7. Name your VM **EFSVM#** (where # is your student number)
8. Select **Workloads** for the location of your VM
9. Click **Next**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-95-Image-148.png)
10. Select **Compute-ResourcePool** as the destination for your VM
11. Click **Next**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-96-Image-149.png)
12. Click **Next**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-96-Image-150.png)
13. Select **WorkloadDatastore** for storage
14. Click **Next**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-97-Image-151.png)
15. Select your Destination Network
16. Click **Next**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-97-Image-152.png)
17. Click **Finish**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-98-Image-153.png)
18. Make sure to Power on your VM and ensure it is assigned an IP address

### AWS Elastic File System (EFS)

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-98-Image-154.png)

On your browser, open a new tab and go to: <https://vmcworkshop.signin.aws.amazon.com/console>

1. Account ID or alias - vmcworkshop
2. IAM user name -Student# (where # is the number assigned to you)
3. Password - **VMCworkshop1211**
4. Click **Sign In**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-99-Image-155.png)
5. You are now signed in to the AWS console. Make sure the region selected is **Oregon**
6. Click on the **EFS** service
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-99-Image-156.png)
7. Select your Student # NFS
8. Note the IP address
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-100-Image-157.png)
9. Back on your vCenter Server tab, click on **Launch Web Console**  for your EFS VM (Might need to allow pop ups in browser). Log in using the following credentials:
 a. **User**: root
 b. **Password**: VMware1!

Enter the following commands at the prompt:

``` bash
cd /
mkdir efs
sudo mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2
MOUNT_TARGET_IP:/ efs Where MOUNT_TARGET_IP is the IP you noted for your EFS
cd efs
touch hello.world
ls
```
