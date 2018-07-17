---
layout: single
title: "Working with your SDDC Lab Manual"
date: 2018-06-01
tags: workshop
toc: true
classes: wide
author_profile: false
---
# Introduction

In this lab we are going to start with looking at the basic tasks which you will perform in the VMware Cloud on AWS user interface when you are administering the platform.

## Add a Host to your SDDC

We will start by adding a host to the VMware Cloud on AWS platform. For the purposes of this lab, we have already precreated the VMware Cloud on AWS SDDC environments for you, in order to save time.

In your chrome browser you will see a bookmark on the bokkmarks bar named **VMware Cloud Services**. Please click this bookmark.

You will be directed to a login page for Cloud Services from VMware. You will need to login with the email address which you signed up to the VMware Cloud on AWS Experience day with. You will also need to ensure that this email address is associated with a "MyVMware Account" in order for the login to VMware Cloud Services to work correctly.

You will now be logged into your organisation hosting your VMware Cloud on AWS SDDC cluster.

1. On your "Student Workshop #" SDDC, click on the "View Details" button.
    ![view details](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/Screenshot+at+Jul+17+15-32-13.png)

    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-11-Image-6.png)
2. Click on the "Actions" button
3. Click on "Add Hosts"
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-12-Image-7.png)
4. As you will only be adding only one host, review the field highlighted
5. Click the "Add Hosts" button

Congratulations! You have completed this step. The adding of an additional host to an existing SDDC should take approximately 10 minutes to complete.

----------------

## Configuring SDDC Firewall Rules

### Management Gateway Firewall Rules

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-13-Image-8.png)

By default, the firewall for the management gateway is set to deny all inbound and outbound traffic. In this exercise, you will add a firewall rule to allow vCenter traffic. In order to access vCenter Server in your SDDC, you must set a firewall rule to allow traffic to the vCenter Server.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-13-Image-9.png)

1. Under **Management Gateway** click the arrow to expand **Firewall Rules**
2. Click **Add Rule**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-13-Image-10.png)
3. Enter a name for your rule under **Rule Name**
4. Type **Any** for Source
5. Make sure **vCenter** is selected as Destination
6. Select **HTTPS (TCP 443)** from the drop down box for Service
7. Click the **SAVE** button, your rule should look like the below image

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-14-Image-11.png)

### Compute Gateway Firewall Rules

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-14-Image-12.png)

By default, the Compute NSX Edge Services Gateway is also set to deny all inbound and outbound traffic. You need to add additional firewall rules to allow traffic as needed.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-14-Image-13.png)

#### Create Firewall Rule under Compute Gateway for Inbound Native AWS Services access

1. Under **Network** tab, navigate to **Compute Gateway**
2. Expand **Firewall Rules**
3. Click **ADD RULE**

#### AWS Inbound Firewall Rule

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-15-Image-14.png)

1. **Name** - AWS Inbound
2. **Action** - Allow
3. **Source** - All connected Amazon VPC
4. **Destination** - 192.168.#.0/24 (Where # is your student number)
5. **Service** - ANY
6. Click **SAVE** button.

#### Create AWS Outbound Firewall Rule

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-15-Image-15.png)

Follow the same process as in the previous step and create AWS Outbound Firewall Rule following these instructions:

1. **Name** - AWS Outbound
2. **Action** - Allow
3. **Source** - 192.168.#.0/24 (Where # is your student number)
4. **Destination** - All connected Amazon VPC
5. **Service** - ANY
6. Click **SAVE** button.

----------------

## Log into VMware Cloud on AWS vCenter

Connection Info Tab

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-16-Image-16.png)

Login to your Student # SDDC and click on the **Connection Info** tab. This displays different connection information for your VMware Cloud on AWS environment like:

- URL to vSphere Client HTML5 client for your vCenter server
- URL to vCenter Server API Explorer
- Local Username for access to vCenter Server on VMware Cloud on AWS **cloudadmin@vmc.local**
- Ability to copy Username to your computer's clipboard
- Password to utilize for the cloudadmin user to access vCenter
- Ability to view cloudadmin's password
- Ability to copy cloudadmin's password to your computer's clipboard
- PowerCLI Connect string to be used if you desire to use PowerCLI to connect to your VMware Cloud on AWS vCenter Server
- Ability to copy PowerCLI string to your computer's clipboard

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-17-Image-17.png)

Click on the vSphere Client's HTML5 URL, and login with **cloudadmin@vmc.local** User Name and copy the password to your computer's clipboard and paste it in the Password Field.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-17-Image-18.png)

You are now logged in to your VMware Cloud on AWS vCenter Server as **cloudadmin@vmc.local** user.

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

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-19-Image-19.png)

1. Click on **Menu**
2. Click on **Content Libraries**

### Subscribe to an existing Content Library

You may already have a Content Library in your on-premises data center, you can use the Content Library to import content into your SDDC.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-20-Image-20.png)

1. In your Content Library window, click the **+** sign to add a new Content Library.
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-20-Image-21.png)
2. Name your Content Library **Student#** where **#** is the number assigned to you
3. (Optional) Enter some notes for your Content Library
4. Click **Next** button
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-21-Image-22.png)
5. Select **Subscribed content library**
6. Under **Subscription URL** enter the following: <https://vcenter.sddc-34-216-241-49.vmc.vmware.com/cls/vcsp/lib/8d658764-2e89-44ff-https://vcenter.sddc-34-216-241-49.vmc.vmware.com/cls/vcsp/lib/8d658764-2e89-44ff-a7c1-ee777f0dfc8f/lib.jsona7c1-ee777f0dfc8f/lib.json>

    PLEASE NOTE THAT THERE MAY BE AN ISSUE WITH DROPPING/ADDITION OF CHARACTERS FOR THE URL WHEN COPYING AND PASTING FROM THE MANUAL. THE ACTUAL URL IS ALSO AVAILABLE IN YOUR STUDENT DESKTOP ON THE Z:\ DRIVE IN A TEXT FILE, OPEN THIS TEXT FILE AND COPY THE URL FROM THERE. ASK YOUR INSTRUCTOR IN THE EVENT YOU CANNOT LOCATE IT.
7. Click the checkbox for **Enable Authentication**
8. For **Password** enter: **VMware1!**
9. Ensure Download content is set to **Immediately**
10. Click **Next**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-22-Image-23.png)
11. Highlight the **WorkloadDatastore** as the storage location
12. Click **Next**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-22-Image-24.png)
13. Click **Finish**. Your content library should take about ~20 minutes to complete syncing.

### Create a Local Content Library

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-23-Image-25.png)

1. Click the **+** sign to create a new Content Library
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-23-Image-26.png)
2. Name your new Content Library: **LocalContentLibrary#** (where # is your student #)
3. (Optional) Enter some notes about your Content Library
4. Click **Next** button
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-24-Image-27.png)
5. Make sure **Local content library** is selected
6. Click **Next**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-24-Image-28.png)
7. Highlight the **WorkloadDatastore** as the storage location
8. Click **Next**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-25-Image-29.png)
9. Review your information and click **Finish**

Congratulations, you have created your Local Content Library.

## Create Logical Network

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-26-Image-30.png)

1. Once you are logged in to your vCenter Server Click on **Menu**
2. Select **Global Inventory Lists** from the drop down menu
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-26-Image-31.png)
3. Click on **Logical Networks** in the left pane
4. Click on the **Add** button
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-27-Image-32.png)
5. Name your New Logical Network **Student#-LN** (where # is your student number)
6. Make sure to select **Routed Network**
7. For CIDR Block enter **192.168.###.0/24** (where # is your student #)
    If your designated student number is between 1 and 9, your CIDR block should look like this: **192.168.1.0/24** - This example represents student number 1. For students 10 thru 20 it should look like this: **192.168.10.0/24** - This example represents student number 10
8. Enter **192.168.###.1** for the Default Gateway IP - Example: 192.168.1.1
9. Make sure DHCP is Enabled by clicking on the **checkbox**
10. Enter **192.168.###.100-192.168.###.200** for IP Range
11. Type **corp.local** as your DNS Domain Name
12. Click **OK** to create your new logical network

## Create Linux Customization Spec

When you clone a virtual machine or deploy a virtual machine from a template, you can customize the guest operating system of the virtual machine to change properties such as the computer name, network settings, and license settings.

Customizing guest operating systems can help prevent conflicts that can result if virtual machines with identical settings are deployed, such as conflicts due to duplicate computer names.

You can specify the customization settings by launching the Guest Customization wizard during the cloning or deployment process. Alternatively, you can create customization specifications, which are customization settings stored in the vCenter Server database. During the cloning or deployment process, you can select a customization specification to apply to the new virtual machine.

Use the Customization Specification Manager to manage customization specifications you create with the Guest Customization wizard.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-28-Image-33.png)

1. Click **Menu**
2. Click **Policies and Profiles**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-29-Image-34.png)
3. Click on **+ New** to add a new Linux Customization Specification
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-29-Image-35.png)
4. Give your VM Customization Spec a Name
5. Enter a description for it (Optional)
6. Make sure to select **Linux**
7. Click **Next**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-30-Image-36.png)
8. Click on the **Enter a name** button
9. Enter a name for your linux VMs
10. Click on the **Append a numeric value** checkbox
11. Enter **corp.local** for the Domain Name
12. Click the **Next** button
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-31-Image-37.png)
13. Select **US** for Area
14. Select **Eastern** for Location
15. Select **Local time**
16. Click **Next**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-31-Image-38.png)
17. Leave the defaults on the **Network** screen and click **Next**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-32-Image-39.png)
18. Under Primary DNS Server enter **10.46.159.10**
19. Type **corp.local** for DNS Search Paths
20. Click **Next**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-32-Image-40.png)
21. Review your entries and click **Finish**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-33-Image-41.png)

Congratulations! You have successfully created your VM Customization Spec for your Linux VMs. You can also export (Duplicate), Edit, Import, and Export a VM Customization Spec.

## Deploy Virtual Machine

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-34-Image-42.png)

1. On your Content Libraries (Menu -> Content Libraries)**, select **Student#** and select the **Templates** tab.
2. Right click on the **centos01-web** template and select **New VM from This Template**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-34-Image-43.png)
3. Name your Virtual Machine **StudentVM#** (where # is your student number)
4. Expand the location area until you see **Workloads** and highlight it
5. Click **Next**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-35-Image-44.png)
6. Expand the destination compute resources until you find **Compute-ResourcePool**, select it
7. Click **Next** button
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-35-Image-45.png)
8. Click **Next** button on the Review details screen
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-36-Image-46.png)
9. In the **Select storage** step, highlight **WorkloadDatastore**
10. Click **Next**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-36-Image-47.png)
11. In the **Select networks** step, click the drop down box to select the Destination Network (you may need to click Browse to see other networks and select your "Student#-LNStudent#-LN" network you created previously
12. Click **Next**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-37-Image-48.png)
13. In the **Ready to complete** section, review to ensure all your selections are correct and click **Finish**

## Convert Virtual Machine to Template

In this step you will be cloning your newly created Virtual Machine into a Template for later use in vRealize Automation section.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-38-Image-49.png)

1. Ensure your VM deployment completed from your previous step
2. Click on **Menu**
3. Select **VMs and Templates**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-39-Image-50.png)
4. Select your newly created VM **Student#** (where # is your student number)
5. Click on **Template**
6. Select **Convert to Template**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-39-Image-51.png)
7. Click **Yes**  in the Convert to Template prompt

You have completed this step.