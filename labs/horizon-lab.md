---
layout: single
title: "Horizon Lab Manual"
categories: labs
date: 2018-07-10
tags: workshop
toc: true
classes: wide
author_profile: false
comments: true
---

# Introduction

We do have a working Horizon environment. You are using it to jump on the Workshop SDDC. This Horizon environement is running on our BU SDDC.
In this Lab we will conect you Student SDDC vCenter to this existing Horizon environment to rollout Desktops. You can then see the new created pool.
Hold in mind. Only one of the Students per SDDC can do this task.

## What is Horizon on VMware Cloud on AWS

VMware Horizon® 7 for VMware Cloud™ on AWS delivers a seamlessly integrated hybrid cloud for virtual desktops and applications. It combines the enterprise capabilities of the VMware Software-Defined Data Center, delivered as a service
on AWS, with the market-leading capabilities of VMware Horizon for a simple, secure, and scalable solution. You can easily extend desktop services to address more use cases such as on-demand capacity, disaster recovery, and cloud co-location without buying additional data center resources.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/1.png)

### Simplify Public and Hybrid Cloud Management

For customers who are already familiar with Horizon 7 or have Horizon 7 deployed
on premises, running Horizon 7 on VMware Cloud lets you leverage a unified
architecture and familiar tools. You can simplify management for Horizon 7
deployments using on-premises infrastructure and VMware Cloud on AWS with
Cloud Pod Architecture (CPA) by linking cloud deployments in different regions,
or by linking on-premises deployments to VMware Cloud on AWS deployments.
This means that you use the same expertise and tools you know from VMware
vSphere® and Horizon 7 for operational consistency, and leverage the rich feature
set and flexibility you expect from Horizon 7


![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/2.png) 


## Create a Cross SDDC VPN

We will be setting up a IPSEC VPN connection between your VPC and the VPC where Horizon Connection Server is already installed.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/Cross-SDDC-VPN-HZ.png)

1. Go back to the **VMware Cloud on AWS** tab.
2. In the main SDDC windows, click on **View Details**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-106-Image-164.png)
3. Then click on the **Network** menu

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-107-Image-165.png) 

In the Management Gateway box, make a note of the Public IP and the Infrastructure Subnet CIDR.
Make also a note of the Compute Gatway box. Public IP and the Subnet you created in the previous LAB.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-107-Image-166.png)![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/Cross-SDDC-VPN-HZ2.png)

Scroll down a little to get to the Management Gateway setting

1. Click the drop down arrow to open the **IPsec VPNs** section
2. Click on **ADD VPN**

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-102-Image-158.png)

Fill in the following information

1. Make sure the drop down is opened, if not click it under **Management Gateway**
2. Click on **Add VPN**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-108-Image-161.png)

    Fill in the following information
3. Name this VPN **Student MGW# to Host CGW** (where # is your student number)
4. Enter **54.70.191.234** for the Remote Gateway Public IP
5. Enter  **192.168.20.0/24** for SDDC under remote network
6. Pre-shared key is **VMware1!**
7. Click on **Save**.
8. Do the same for the same task for establishing connection from Studen SDDC compute gateway and Host coumpute gateway.

Fill in the following information

1. Make sure the drop down is opened, if not click it under **Compute Gateway**
2. Click on **Add VPN**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-108-Image-161.png)

    Fill in the following information
3. Name this VPN **Student CGW# to Host CGW** (where # is your student number)
4. Enter **54.70.191.234** for the Remote Gateway Public IP
5. Enter  **192.168.20.0/24** for SDDC under remote network
6. Choose your created student logical network that you created in previous LAB
7. Pre-shared key is **VMware1!**
8.  Click on **Save**.


<!--  
## Create Content Library

Content libraries are container objects for VM templates, vApp templates, and other types of files like ISO images.

You can create a content library in the vSphere Web Client, and populate it with templates, which you can use to deploy virtual machines or vApps in your VMware Cloud on AWS environment or if you already have a Content Library in your on-premises data center, you can use the Content Library to import content into your SDDC.

You can create two types of libraries: local or subscribed libraries.

### Local Libraries

You use a local library to store items in a single vCenter Server instance. You can publish the local library so that users from other vCenter Server systems can subscribe to it. When you publish a content library externally, you can configure a password for authentication.

VM templates and vApp templates are stored as an OVF file format in the content library. You can also upload other file types, such as ISO images, text files, and so on, in a content library.

### Subscribed Libraries

You subscribe to a published library by creating a subscribed library. You can create the subscribed library in the same vCenter Server instance where the published library is, or in a different vCenter Server system. In the Create Library wizard you have the option to download all the contents of the published library immediately after the subscribed library is created, or to download only metadata for the items from the published library and later to download the full content of only the items you intend to use.

To ensure the contents of a subscribed library are up-to-date, the subscribed library automatically synchronizes to the source published library on regular intervals.

You can also manually synchronize subscribed libraries. You can use the option to download content from the source published library immediately or only when needed to manage your storage space.

Synchronization of a subscribed library that is set with the option to download all the contents of the published library immediately, synchronizes both the item metadata and the item contents. During the synchronisation the library items that are new for the subscribed library are fully downloaded to the storage location of the subscribed library.

Synchronization of a subscribed library that is set with the option to download contents only when needed synchronizes only the metadata for the library items from the published library, and does not download the contents of the items. This saves storage space. If you need to use a library item you need to synchronize that item. After you are done using the item, you can delete the item contents to free space on the storage. For subscribed libraries that are set with the option to download contents only when needed, synchronizing the subscribed library downloads only the metadata of all the items in the source published library, while synchronizing a library item downloads the full content of that item to your storage. If you use a subscribed library, you can only utilize the content, but cannot contribute with content. Only the administrator of the published library can manage the templates and files.

In the subscribed content library you will find the Golden Master Image that you need to use for the deploymend of new desktops with the help of horizon

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-19-Image-19.png)

1. Click on **Menu**
2. Click on **Content Libraries**

### Subscribe to an existing Content Library

You may already have a Content Library in your on-premises data center, you can use the Content Library to import content into your SDDC.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-20-Image-20.png)

1. In your Content Library window, click the **+** sign to add a new Content Library.
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-20-Image-21.png)
2. Name your Content Library **Student#-HorizonGM** where **#** is the number assigned to you
3. (Optional) Enter some notes for your Content Library
4. Click **Next** button

5. Select **Subscribed content library**
6. Under **Subscription URL** enter the following: <https://vcenter.sddc-34-216-241-49.vmc.vmware.com:443/cls/vcsp/lib/ddfe9c01-09ea-4fc2-a03b-91cc7ed5f4b1/lib.json>

    PLEASE NOTE THAT THERE MAY BE AN ISSUE WITH DROPPING/ADDITION OF CHARACTERS FOR THE URL WHEN COPYING AND PASTING FROM THE MANUAL.ASK YOUR INSTRUCTOR IN THE EVENT YOU CANNOT LOCATE IT.

7. Ensure Download content is set to **Immediately**
8. Click **Next**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-22-Image-23.png)
9. Highlight the **WorkloadDatastore** as the storage location
10. Click **Next**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-22-Image-24.png)
11. Click **Finish**. Your content library should take about ~20 minutes to complete syncing.
-->

## Create your Golden Master Image

1.  Click on **Menu**
2.  Click on **Content Library**
3.  Click on the content library you subscribed to in the previus lab
4.  Click on **Templates**
![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/GM-W10-1.png)

5.  Right Click on the W10-LTBS-1607-WS Template and choose **New VM from this Template....**
6.  Give it the same the name **W10-LTBS-#** where # is put your student ID in
7.  As location click on **Templates**
8.  Click on **Next**
9.  Select **Compute-ResourcePool** and click **Next**
10. Click **next**
11. Select **WorkloadDatastore** and click **next**
12. Select the network you created in privious LAB **Student#-LN**
13. Click **next** and **finish**


## Import a Windows Customization Spec
As we support Full Clones at the moment we need to create a windows customization spec that we will use in Horizon for creating a bunch of VM's and this will be directly placed in the Active Directoy.


1.  Click on **Menu**
2.  Click **Policies and Profiles**
3.  Click on **import**
![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/customization1.png)

4.  Click on **Browse**
5.  got to "Z:\Horizon\W10-NEW.xml"
6.  enter a name **W10-Customization**
7.  Click **OK**


### We now do have a customization spec imported. We need to open it and run through all the settings again to double check everything is configurated right

8.  Click on your new imported Custom Spec click **edit**
9.  Check Guest OS "Generate a new security identity (SID)" is selected
10. Click on the left site on **Administrator password**
11. Set Administrator password to **VMware1!**
12. Click on the left site on **Workgroup or domain**
13. type in " windows server domain" corp.local
14. username : your studen username / password ... /  Check this point with adam
15. click **OK**

## Create your SDDC vcenter as an enpoint in the existing Horizon environment
That you can create desktops in your SDDC we need to implement your Student SDDC vCenter into the existing Horizon infrastructure.

1.  Please open a Browser and navigate to : "https://192.168.20.70/admin"
![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-server1.png)
2.  enter student username and passworsd
3.  click **Log In**

You now can see the Dashboard / manin page of the Horizon Connection Server. This is the place where we will be working the next hour
![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-server2.png)

1.  Click on the left site on Servers:
![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-server3.png)

2.  Click on vCenter Servers **Add**
3.  type in server adress "this is the ip adress of your student vcenter"
4.  type in username and password / "cloudadmin@vmc.local and the password from cloudadmin of your student vcenter"
5.  click **next**


## Deploy Desktop Pool

Now as we have the vCenter as an Endpoint in Horizon we can deploy Desktops.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-server4.png)
![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-server5.png)
![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-server6.png)
![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-server7.png)
![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-server8.png)
![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-server9.png)
![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-pool1.png)
![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-pool2.png)
![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-pool3.png)
![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-pool4.png)
![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-pool5.png)
![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-pool6.png)
![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-pool7.png)
![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-pool8.png)
![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-pool9.png)

