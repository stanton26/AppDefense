---
layout: single
title: "Working with your SDDC Lab Manual"
date: 2018-09-24
tags: workshop
toc: true
classes: wide
author_profile: false
categories: labs
comments: true
---
# Introduction

In this lab we are going to start with looking at the basic tasks which you will perform in the VMware Cloud on AWS user interface when you are administering the platform.

## Viewing your SDDC

![SDDC01](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc01.jpg)

After you login, you should see a single SDDC in the user interface following the naming format Student-Workshop-#.#. An SDDC is a fully deployed environment including vSphere, NSX, vSAN and vCenter Server. Deployment of a fully configured SDDC takes about two hours so for the purposes of this lab, we have already deployed it for you. This SDDC is in the same state it would be if you have deployed it. Let's take a look at the SDDC properties.

1. First click on View Details to open the SDDC properties.

![SDDC02](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc02.jpg)

You will start with the Summary of the SDDC. There are a number of other tabs available as follows:
1. Support - You can contact Support with your SDDC ID, Org ID, vCenter Private and Public IPs and the date of your SDDC Deployment.
2. Settings: Gives you access to your vSphere Client (HTML5), vCenter Server API, PowerCLI Connect, vCenter Server and reviews your Authentication information.
3. Troubleshooting: Allows you to run network connectivity tests to ensure all necessary access is available to perform select use cases.
4. Add Ons: Here you will find Add On services for your VMware Cloud on AWS environment like Hybrid Cloud Extension and VMware Site Recovery.
5. Networking & Security: Provides a full diagram of the Management and Compute Gateways.  This is where you can configuration locgical networks, VPN's and firewall rules. We will cover this in more detail later. Click on Networking & Security to proceed to the next article to learn more about VMware Cloud on AWS Network and Security Configuration.

## Create a Logical Network

![SDDC03](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc03.jpg)

From the previous article, you should see the Network & Security information for the SDDC.
VMware Cloud on AWS allows you to quickly and easily create new logical network segments on
demand. Let's create a new network segment in the SDDC.

1. Click the **Networking & Security** tab, then click on **Segments** to show all of the existing network segments.
2. Click on **Add Segments** to create a new network segment.
3. Enter **Demo-Net** for the Name of the new network segment.
4. For the Gateway/Prefifix Length enter 10.10.xx.1/24 (xx depicts your student number). This represents the default gateway
of the network and the prefix length of the network. For more details on IP addressing see below.
5. For **DHCP**, click the down arrow and select Enabled to enable DHCP on the network.
6. Enter **10.10.xx.10-10.10.xx.200** for the **DHCP IP Range**. This is the range of IP addresses the DHCP server will grant to workloads attached to the network.
7. Click **Save** to save the logical network.

**Note: Make sure you leave the default of Routed for Type and do not enter anything for the DNS suffix.**

<aside class="notice">
<font color="dodgerblue">
<img src="https://s3-us-west-2.amazonaws.com/vmc-workshops-images/info.jpeg" width="25" height="25"> CIDR notation is a compact representation of an IP address and its associated routing prefix. The notation is constructed from an IP address, a slash('/') character, and a decimal number. The number is the count of leading bits in the routing mask, traditionally called the network mask.  The IP address is expressed according to the standards of IPv4 or IPv6.
<br/>
<br/>
The address may denote a single, distinct interface address or the beginning address of an entire network. The maximum size of the network is given by the number of addresses that are possible with the remaining, least-significant bits below the prefix.  The aggregation of these bits is often called the host identifier.
<br/>
<br/>
For example:
<br/>
<br/>
• 192.168.100.14/24 represents the IPV4 address 192.168.100.14 and its associated routing prefix 192.168.100.0, or equivalently, its subnet mask 255.255.255.0, which has 24 leading 1-bits.
<br/>
<br/>
• The IPV4 block 192.168.100.0/22 represents the 1024 IPV4 addresses from 192.168.100.0 to 192.168.103.255.
</font>
</aside>

## Verify Network Segment Configuration

![SDDC04](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc04.jpg)

1. Verify the network segment was added correctly.  Your information should match the highlighted area above.


## Configure Firewall Rule for vCenter Access

![SDDC05](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc05.jpg)

By default, all inbound firewall rules are set to Deny in VMware Cloud on AWS. In order to access vCenter server, we will need to configure a firewall rule allowing inbound access.

**Note: In most enterprise environments, you would create VPN or Direct Connect VIF to allow limited access firewall rules to vCenter. In this environment, we will open it to any IP address on the internet which is not recommended.**

1. Click on **Gateway Firewall** on the lefthand side of the screen.
2. If it is not already selected, click on **Management Gateway** to create a firewall rules that allow access to management components in the SDDC.
3. Click **Add New Rule** to add a new rule to the edge gateway.
4. For the **Name** enter **vCenter Inbound Rule**.
5. Click **Set Source** to define the source for the firewall rule.

### Select the Firewall Rule Source

![SDDC06](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc06.jpg)

1. Click the **Radio Button next** to **Any**.
2. Click **Save** to save the source information in the rule.

### Configure Firewall Rule for vCenter Access (Continued)

![SDDC07](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc07.jpg)

1. Click **Set Destination** to launch a new window to set the destination for the rule.

### Select the Firewall Rule Destination

![SDDC08](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc08.jpg)

1. Click the **Radio Button** next to **System Defined Groups**.
2. Select the **Checkbox** next to **vCenter**.
3. Click **Save** to save the destination information in the rule.

### Configure Firewall rule for vCenter Access (Contined)

![SDDC09](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc09.jpg)

Continue configuring the vCenter Inbound Rule:

1. Click box below **Services** and select **HTTPS (TCP 443)** to allow SSL access to the vCenter server.
2. Publish the rules by clicking **Publish** button to activate the firewall rule.

vCenter should now be accessible from anywhere in the internet.  in the next section, we will access vCenter HTML5 client to being configuring virtual machines.



### Compute Gateway Firewall Rules

![SDDC8](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC8.jpg)

Like the Management NSX Edge Services Gateway. By default, the Compute NSX Edge Services Gateway is also set to deny all inbound and outbound traffic. You need to add additional firewall rules to allow access to your workload VMs which you provision in the VMware Cloud on AWS platform.

![SDDC9](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC9.jpg)

Create Firewall Rule under Compute Gateway for Inbound Native AWS Services access

1. Under **Network** tab, navigate to **Compute Gateway**
2. Expand **Firewall Rules**
3. Click **ADD RULE**

    ![SDDC10](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC10.jpg)
4. Name - **AWS Inbound**
5. Action - **Allow**
6. Source - **All connected Amazon VPC**
7. Destination - **192.168.#.0/24** (Where # is your student number)
8. Service - **ANY**
9. Click *SAVE* button.

![SDDC11](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC11.jpg)

Follow the same process as in the previous step and create AWS Outbound Firewall Rule following these instructions:

1. Name - **AWS Outbound**
2. Action - **Allow**
3. Source - **192.168.#.0/24** (Where # is your student number)
4. Destination - **All connected Amazon VPC**
5. Service - **ANY**
6. Click **SAVE** button.

We have now successfully created rules which will allow us to access our vCenter server over port 443 from any location, in a real world deployment we would change this to only allow communication from a specific IP address range over a private link, once we have a VPN in place. We have also configured our Compute workloads to be able to communicate with any services in our native AWS VPC which our VMware Cloud on AWS environment is connected too.

## Log into VMware Cloud on AWS vCenter

### Settings Tab

We will now login to our VMware Cloud on AWS vCenter instance, now that the required ports have been open on our Management NSX Edge Services Gateway.

![SDDC12](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC12.jpg)

1. Login to your Student # SDDC and click on the *Settings* tab. This displays different connection information for your VMware Cloud on AWS environment like:
2. Default account information for your vCenter server
3. URL to vCenter Server
4. URL to vCenter API Explorer
5. PowerCLI Connect string to be used if you desire to use PowerCLI to connect to your VMware Cloud on AWS vCenter Server
6. vCenter FQDN
7. vCenter Public IP information
8. Actual Public IP Address assigned to vCenter
9. vCenter Private IP

![SDDC13](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC13.jpg)

Click on the vSphere Client's HTML5 URL copy button (\#3 above), and login with cloudadmin@vmc.local User Name and copy the password to your computer's clipboard and paste it in the Password Field.

![SDDC14](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC14.jpg)

You have successfully logged in to the vCenter Server in your VMware Cloud on AWS environments as local user cloudadming@vmc.local.

## Create Content Library

Content libraries are container objects for VM templates, vApp templates, and other types of files like ISO images.

You can create a content library in the vSphere Web Client, and populate it with templates, which you can use to deploy virtual machines or vApps in your VMware Cloud on AWS environment, or if you already have a Content Library in your on-premises data center, you can use the Content Library to import content into your SDDC.

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

![SDDC15](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC15.jpg)

1. In the vSphere HTML5 client, click on **Menu**
2. Click on **Content Libraries**

### Subscribe to an existing Content Library

You may already have a Content Library in your on-premises data center, you can use the Content Library to import content into your SDDC.

![SDDC16](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC16.jpg)

1. In your Content Library window, click the *+* sign to add a new Content Library.

    ![SDDC17](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC17.jpg)
2. Name your Content Library *Student#* (where # is the number assigned to you)
3. (Optional) Enter some notes for your Content Library
4. Click *Next* button

    ![SDDC18](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC18.jpg)
5. Select *Subscribed content library*
6. Under *Subscription URL* enter the following:

    ``` link
    https://vcenter.sddc-34-216-241-49.vmc.vmware.com:443/cls/vcsp/lib/4aa185b4-3d6e-45b4-90ca-cd3a845d4502/lib.json
    ```
7. Click the checkbox for **Enable Authentication**
8. For the *Password* enter: **VMware1!**
9. Ensure Download content is set to *Immediately*
10. Click *Next*

    ![SDDC19](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC19.jpg)
11. Highlight the *WorkloadDatastore* as the storage location
12. Click *Next*

    ![SDDC20](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC20.jpg)
13. Click *Finish*. Your content library should take about ~20 minutes to complete syncing.

You have now successfully subscribed to a vSphere content library from your VMware Cloud on AWS vCenter instance.

### Create a Local Content Library

We will now create a local content library for this VMware Cloud on AWS vCenter instance.

![SDDC21](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC21.jpg)

1. Click the *+* sign to create a new Content Library

    ![SDDC22](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC22.jpg)
2. Name your new Content Library: **LocalContentLibrary#** (where # is your student #)
3. (Optional) Enter some notes about your Content Library
4. Click *Next* button

    ![SDDC23](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC23.jpg)
5. Make sure *Local content library* is selected
6. Click *Next*

    ![SDDC24](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC24.jpg)
7. Highlight the *WorkloadDatastore* as the storage location
8. Click *Next*

    ![SDDC25](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC25.jpg)
9. Review your information and click *Finish*

Congratulations, you have created your Local Content Library.

## Create a Logical Network

![SDDC26](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC26.jpg)

1. Once you are logged in to your vCenter Server Click on *Menu*
2. Select *Global Inventory Lists*

    ![SDDC27](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC27.jpg)
3. Click on *Logical Networks* in the left pane
4. Click on the *Add* button

    ![SDDC28](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC28.jpg)
5. Name your New Logical Network *Student#-LN* (where # is your student number)
6. Select the *Routed Network* radio button
7. For CIDR Block enter **192.168.#.0/24** (where # is your student #)

    • For Example. If your designated student number is between 1, then your CIDR block should look like this: **192.168.1.0/24** - This example represents student number 1

8. Enter **192.168.#.1** for the Default Gateway IP - Example: 192.168.1.1
9. Make sure DHCP is Enabled by clicking on the *checkbox*
10. Enter **192.168.#.100-192.168.#.200** for IP Range
11. Type **corp.local** as your DNS Domain Name
12. Click **OK** to create your new logical network

## Create Linux Customization Spec

When you clone a virtual machine or deploy a virtual machine from a template, you can customize the guest operating system of the virtual machine to change properties such as the computer name, network settings, and license settings.

Customizing guest operating systems can help prevent conflicts that can result if virtual machines with identical settings are deployed, such as conflicts due to duplicate computer names.

You can specify the customization settings by launching the Guest Customization wizard during the cloning or deployment process. Alternatively, you can create customization specifications, which are customization settings stored in the vCenter Server database. During the cloning or deployment process, you can select a customization specification to apply to the new virtual machine.

Use the Customization Specification Manager to manage customization specifications you create with the Guest Customization wizard.

![SDDC29](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC29.jpg)

1. In the vCenter HTML5 client click *Menu*
2. Click *Policies and Profiles*

    ![SDDC30](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC30.jpg)
3. Click on *+ New* to add a new Customization Specification

    ![SDDC31](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC31.jpg)
4. Give your VM Customization Spec a name, such as **Linux Spec**
5. Enter a description for it (Optional)
6. Make sure to select *Linux* in the Guest OS, Target guest OS section
7. Click *Next*

    ![SDDC32](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC32.jpg)
8. Click on the third option, **Enter a name** button
9. Enter a name for your linux VMs, such as *linux-vm*
10. Click on the **Append a numeric value** checkbox
11. Enter **corp.local** for the Domain Name
12. Click the **Next** button

    ![SDDC33](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC33.jpg)

13. Select **US** for Area
14. Select **Eastern** for Location
15. Select **Local time**
16. Click **Next**

    ![SDDC34](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC34.jpg)
17. Leave the defaults on the *Network* screen and click *Next*

    ![SDDC35](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC35.jpg)

18. Under Primary DNS Server enter **8.8.8.8** and leave the Secondary DNS Server and Tertiary DNS Server blank
19. Type **corp.local** for DNS Search Paths and click **Add**
20. Click *Next*

    ![SDDC36](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC36.jpg)
21. Review your entries and click **Finish**

![SDDC37](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC37.jpg)

Congratulations! You have successfully created your VM Customization Spec for your Linux VMs. You can also export (Duplicate), Edit, Import, and Export a VM Customization Spec.

## Deploy Virtual Machines

![SDDC38](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC38.jpg)

1. On your Content Libraries *(Menu -> Content Libraries)*, select *Student#* and select the *Templates* tab.
2. Right click on the **centos01-web** template and select **New VM from This Template**

    ![SDDC39](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC39.jpg)
3. Name your Virtual Machine **StudentVM#** (where # is your student number)
4. Expand the location area until you see **Workloads** and highlight it
5. Ensure that you tick the option to **Customize the Operating System**
6. Click **Next**
7. Choose the linux customization specification which we created in the previous step
8. Click **Next**
9. Expand the destination compute resources until you find *Compute-ResourcePool*, select it
10. Click the *Next* button

    ![SDDC41](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC41.jpg)
11. Click the *Next* button on the Review details screen

    ![SDDC42](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC42.jpg)
12. In the *Select storage* step, highlight *WorkloadDatastore*
13. Click **Next**
14. In the *Select networks* step, click the drop down box to select the Destination Network (you may need to click Browse to see other networks and select your *Student#-LN* network you created previously
15. Click **Next**

    ![SDDC44](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC44.jpg)
16. In the *Ready to complete* section, review and ensure all your selections are correct and click *Finish*

## Convert a Virtual Machine to a Template

In this step you will be cloning your newly created Virtual Machine into a Template for later use in vRealize Automation section.

![SDDC45](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC45.jpg)

1. Ensure your VM deployment completed from your previous step
2. Click on **Menu**
3. Select **VMs and Templates**

    ![SDDC46](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC46.jpg)
4. Select your newly created VM *Student#* (where # is your student number)
5. Click on **Template**
6. Select *C*onvert to Template**

    ![SDDC47](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC47.jpg)
7. Click *Yes*  in the Convert to Template prompt

You have completed this module.

Please add comments below if you would like to give feedback on this exercise.