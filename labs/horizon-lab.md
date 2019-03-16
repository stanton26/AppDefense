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

In this Lab we are going to cfreate and install a Horizon 7 environment. Using Cloud Pod Architectire to connect two Horizon Environemnts for Global Management and Entitlement Rights.

## What is Horizon on VMware Cloud on AWS

VMware Horizon® 7 for VMware Cloud™ on AWS delivers a seamlessly integrated hybrid cloud for virtual desktops and applications. It combines the enterprise capabilities of the VMware Software-Defined Data Center, delivered as a service
on AWS, with the market-leading capabilities of VMware Horizon for a simple, secure, and scalable solution. You can easily extend desktop services to address more use cases such as on-demand capacity, disaster recovery, and cloud co-location without buying additional data center resources.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/1.png)

### Simplify Public and Hybrid Cloud Management

For customers, who are already familiar with Horizon 7 or have Horizon 7 deployed on premises, running Horizon 7 on VMware Cloud on AWS lets you leverage a unified architecture and familiar tools. You can simplify management for Horizon 7 deployments using on-premises infrastructure and VMware Cloud on AWS with Cloud Pod Architecture (CPA) by linking cloud deployments in different regions, or by linking on-premises deployments to VMware Cloud on AWS deployments. This means that you use the same expertise and tools you know from VMware vSphere® and Horizon 7 for operational consistency, and leverage the rich feature set and flexibility you expect from Horizon 7

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/2.png) 


## Configuring SDDC Firewall Rules

If not done already in the previous lab please also create the Firewall rule for the Management Gateway so you can access the vCenter.

<!-- Comment for Elena: this is step 1. This stays.-->

### Compute Gateway Firewall Rules

By default, the Compute Gateway is set to deny all inbound and outbound traffic. You need to add additional firewall rules to allow access to your workload VMs which you provision in the VMware Cloud on AWS platform.

### Create Compute Gateway Firewall Rule

1. Under **Network & Security** tab, navigate to **Security**, then **Gateway Firewall**
2. On the right hand side go to **Compute Gateway**
3. Click **ADD NEW RULE**

Horizon requires a number of ports to be opened for communcation and Inter-POD connectivity. For the purposes of the lab and ease of management we are going to allow communicaqtiona cross everythig. The first rule we are going to create is an Any Any Any rule.

1. **Name** - Horizon
2. **Source** - Any
3. **Destination** - Any
4. **Service** - Any
5. **Action** - Allow
6. **Applied To** - All Uplinks

The next step will be to edit the **Default VTI Rule** in the Compure Gatwway Firewall Rules
1. Click the three point next to **Default VTI Rule** 
2. **EDIT**
3. **Action** - Allow


<!-- Elena - include a screen shots -->

## Cretea a Logical Network

For this Horizon Lab we have prepared several virtual machines, like Active Directory (AD), Hoirzon Connections Server, Unified Access Gateway (UAG) and the Goldenmaster Image.
The AD, Horizon Connection Server, UAG and Goldenmaster Image will be deployed in a 192.168.20.0/24 subnet. Therefore we need to create this network first.

## Create a Logical Network

The next step we need to complete is to create a logical network. For our Horizon Lab we are going to need several virtual machines. These are: Active Directory, Hoirzon Connections Server, Unified Access Gateway (UAG) and the Golden Master Image.

The AD, Horizon Connection Server, UAG and Golden Master Image will be deployed in a 192.168.xxx.xxx/24 subnet. Therefore we need to create this network first.

Navigate to **Networking & Security**, then **Segments** and **Add a segment**

Depending on weather you are working in SDDC **Student-Workshop-X.1** or **Student-Workshop-X.2** environmentm, you will need to create different networks.

1. **Name**
**Student-Workshop-X.1** use **Horizon100** , or 
**Student-Workshop-X.2** use **Horizon200**
2. **Type** - Routed
3. **Tunnel ID** - Leave as is
4. **Gateway / Prefix Length**
**Student-Workshop-X.1** use 192.168.100.1/24
**Student-Workshop-X.2** use 192.168.200.1/24
5. **DHCP** - Disabled

## Creating a VPN Connection

The next step will be to create the VPN connection between Student-Workshop-X.1 and Student-Workshop- X.2. To do that, follow the steps below:

1. Go to **Network & Security**
2. **VPN**
3. **Route Based**
4. **Add VPN**

| **Name** | **Local IP** | **Remote Public IP** | **BGP Local IP/Prefix Length** | **BGP Remote IP** | **BGP Remote ASN** | **Preshared Key** |
| Horizon-Student-Workshop-X.1 | Public | *Public IP Student X.2* | 169.254.111.1/30 | 169.254.111.2 | 65000| VMware1! |
| Horizon-Student-Workshop-X.2 | Public | *Public IP Student X.1* | 169.254.111.2/30 | 169.254.111.1 | 65001| VMware1! |

**Note:** To complete the **Remote Public IP** field, please work with your workshop partner to obtain their Public IP address.

We have now completed network set up part of the lab. The next step will be to deploy the virtual machines we are going to need for the lab. We are goign to deploy these from templates, located in Content Libraries.

The next step will be to log onto vCenter and deploy VMs.

## Log into vCenter

To open vCenter, navigate to **OPEN VCENTER** in the top right hand corner of the screen. A pop up window will open, where you can vue vCenter login credentials.

Then go to **Show vCenter Credentials**

![](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/Show+vCdenter+Credentials.jpg)

Click on the clipboard icon, next to Password, to copy the password for accessing vCenter.

![](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/Open+vCenter.jpg)

Then click on **Open vCenter**. A new browser window will open, where you can log into vCenter.

**Username:** cloudadmin@vmc.local

**Password:** *Paste Password*

## Horizon Deployment

As part of the lab, we have already subscribed your SDDC to Content Libraries. These are located in an Amazon S3 bucket, where we have the VM templates stored and ready to use.

In the subscribed content library you will find the Golden Master Image that you need to use for the deploymend of new desktops with the help of horizon

1. Click on **Menu**
2. Click on **Content Libraries**







## Create your Active Directory VM

5.  Right Click on the **VMCWINDC01** and choose **New VM from this Template....**
6.  Give it the same the name **VMCWINDC01**
7.  As location click on **Workloads**
8.  Click on **Next**
9.  Select **Compute-ResourcePool** and click **Next**
10. Click **next**
11. Select **WorkloadDatastore** and click **next**
12. Select the network you created in privious LAB **Horizon#-LN**
13. Click **next** and **finish**

## Create Horizon Server VM

5.  Right Click on the **HZ-76-WS** and choose **New VM from this Template....**
6.  Give it the same the name **HZ-76-WS** where # is put your student ID in
7.  As location click on **Workloads**
8.  Click on **Next**
9.  Select **Compute-ResourcePool** and click **Next**
10. Click **next**
11. Select **WorkloadDatastore** and click **next**
12. Select the network you created in privious LAB **Horizon#-LN**
13. Click **next** and **finish**

## Create your Golden Master Image

With Horizon 7.6 we do have the option to also do Instant Clones. For this lab we prepared two Golden Master Images. The first one is for Instant Clones with the Name **W10-LTBS-1607-IC**, the second one is for Full Clones. You can decide to either go for Full clones or use Instant Clones. We suggest to do instant clones cause it is much faster to rollout this desktops.

5.  Right Click on the **W10-LTBS-1607-IC Template** and choose **New VM from this Template....**
6.  Give it the same the name **W10-LTBS-#** where # is put your student ID in
7.  As location click on **Templates**
8.  Click on **Next**
9.  Select **Compute-ResourcePool** and click **Next**
10. Click **next**
11. Select **WorkloadDatastore** and click **next**
12. Select the network you created in privious step for 192.168.20.0/24 **Horizon#-LN**
13. Click **next** and **finish**


##Power on the new created VM's
1.  Power on the VM **VMCWINDC01**
2.  Launch the Web Console
3.  Sign in with  **corp\vmcws1** and password **VMware1!**


##Power on the new created VM's

1.  Power on the VM **HZ-76-WS**
2.  Launch the Web Console
3.  Sign in with  **corp\vmcws1** and password **VMware1!**

Wait about 10 minutes until all services are runnig. In the meantime jump create the UAG VM

## Create UAG VM
1.  Go back to your vCenter web Client
2.  Right click on the compute ressource pool
![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/customization9.png)
3. click on deploy ovf template
4. ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/customization10.png)
5.  Select **Local Files**
6.  Click **Choose Files**
7.  Go to Z://Horizon/ and select euc-unified-access-gateway-3.3.0.0-8539
8.  ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/customization11.png)
9.  Click **Open**
10. Click **next**
11. Click **next**
12. Click **next**
13. Select **Single NIC**
14. Click **next**
15. Select Destination Network for all three networks **Horizon#-LN**
16. Following Settings need to be done:
![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/customization12.png)
![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/customization13.png)
![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/customization14.png)
![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/customization15.png)

17. Click **Finish**
18. Wait until the new created UAG VM is powered on.
19. Open you browser and go to : https://192.168.20.73:9443
![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/customization16.png)
20. Click on Import Settings
![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/customization17.png)
21. Brwose to ** z://horizon/...
![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/customization18.png)
22. Click **Import**
23. Login agian to https://192.168.20.72/9443 
24. choose the right side **configure manually** and click **select**
25. Click on Edge Services Settings **show**
![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/customization19.png)
26. Tunnel, BLAST,UDP Tunnel Server, HORIZON DESTINATION Server have to be **GREEN**


Now we will request a public IP adress. We will use this public IP adress to access the Horizon infrastructre afterwards. Please go back to VMC console in your browser. Go to the network tab.

1.  Scroll down to Compute Gateway and request a new public ip. 
2.  Please note this public IP.
![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/customization8.png)
3.  Click on NAT
4.  Click on **ADD NAT RULE**
5.  Under Description type **Horizon Desktop**
6.  Public IP **your requested IP**
7.  Service **HTTPS(TCP 443)
8.  Public Ports **443**
9.  Internal IP **192.168.20.73**
10. Click **SAVE**
11. Check the Firewall Rule you previous created. -> ANY ANY ANY

Now we have to create a Snapshot on the Golden Master Image. Cause Instant Clones are working with snapshots.

1.  Right click on **W10-LTBS-1607-IC**
2.  Click on **Snapshot**
3.  Take a Snapshot
3.  type a name **1.0** for example
4.  Click **OK**


If decision made to go for Full Clones in the previous step we need to create a windows customization spec for the Full Cones that we will use in Horizon for creating a bunch of VM's and those will be directly placed in the Active Directoy.

## Create a Windows Customization Spec
1.  Click on **Menu**
2.  Click **Policies and Profiles**
3.  Click on **create**
4.  Type a Name **Windows**
5.  Click **Next**
6.  Type Owner name **VMC** Owner organization **VMC** click **Next**
7.  Check that **Use the virtual machine name** is selected
8.  Do not use a product key **next**
9.  Type Password **VMware1!**
10. Click **next**
11. Click **next**
12. Click **next**
13. Select **Windows Server domain** and type **corp.local** under username **vmcws#** where # is please chose your ID and your studen password
14. Click **Finish**


## Create your SDDC vcenter as an enpoint in the existing Horizon environment
That you can create desktops in your SDDC we need to implement your Student SDDC vCenter into the existing Horizon infrastructure.

1.  Please open a Browser and navigate to : "https://192.168.20.70/admin"
![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-server1.png)
2.  enter student username and passworsd
3.  click **Log In**

You now can see the Dashboard / manin page of the Horizon Connection Server. This is the place where we will be working the next hour
![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-server2.png)

4.  Click on the left site on Servers:
![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-server3.png)

5.  Click on vCenter Servers **Add**
6.  type in server adress "this is the ip adress of your student vcenter" for example 54.72.217.99
7.  type in username and password / "cloudadmin@vmc.local and the password from cloudadmin of your student vcenter"
8.  you can find your cloudadmin password by going back to the VMC tab in your browser.
![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-server10.png)
8.  if you had filled in the fields: server adress, user name and password click **next**
9.  you will get a promt Invalid Certifcate Detected 
![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-server6.png)
10. Click on View Certficate and **Accept**
11. On View Composer settings check **Do not use View Composer** click **Next**
12. Please verfiy that **Enable View Storage Accelerator** is NOT selected
![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-server8.png)
10. Click **next**
11. Click **Finish**

## Deploy Desktop Pool

Now as we have the vCenter as an Endpoint in Horizon we can start deploying Desktops.
1.  On the Horizon Connection Server admin console on the left site you can click on **Desktop Pools**
![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-pool1.png)
2.  Select **Automated Desktop Pool** and click on **Next**
![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-pool2.png)
3.  Select **Dedicated** and click **next**
![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-pool3.png)
4.  you will get a promt "More Information" click **Ignore**
![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-pool4.png)
5. Select **Full virtual machines** and select **your Student vCenter** with your student vCenter IP
![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-pool5.png)
6. Type under ID: **Studen-#** Display name **Stunden-#** and select Access group **WS1** under Description  **Stundent #**  click **next**
    Note # is your student ID
![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-pool6.1.png)
7.  On Desktop Pool Settings under Remote Settings click **Remote machine Power Policy** and select **Always powered on** click **next**
![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-pool7.1.png)
8.  scroll down and select **HTML Access** click **next**
![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-pool8.png)
9.  Under " use a naming pattern" enter **studen-#** click **next**
![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-pool9.1.png)
10. Select **Use VMware Virtual SAN**
![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-pool11.png)

Now we need to select your GM Image which you converted to a Template
11. Click on **Browse** and select your Golden Master Image for example **W10-LTSB-1**  click **OK**

If you cant see any template here it might be that you forgot to convert the VM into a Template. As we are working with full clones we have to have a Template. This will change in the future when we will work with Instant Clones.
![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-pool12.png)
12. Click **browse** on VM folder location and select **Workloads** click **OK**
13. Click **browse** on host and cluster and select the cluster click **OK**
14. Click **browse** next to Ressource pool and select **Compute-ResourcePool**  click **OK**
15. Click **browse** next Storage and select **WorkloadDatastore** click **OK**
16. Verify all fields do have entries and click **next**
![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-pool13.png)
17. On Advanced Storage Options don't select anything just click **next**
18. Select **Use this customization specification:** and select your customization policy click **netx**
 ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/horizon-pool14.png)
19. Check all settings and click **Finish**


## Ckeck if the desktops get created
1. Go back to your vCenter and see if the cloning starts. This could take up to 5 minutes
