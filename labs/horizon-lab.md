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

<!-- Comment for Elena: this is step 1. This stays.-->w

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

## Cretea a Logical Network

For this Horizon Lab we have prepared several virtual machines, like Active Directory (AD), Hoirzon Connections Server, Unified Access Gateway (UAG) and the Goldenmaster Image.
The AD, Horizon Connection Server, UAG and Goldenmaster Image will be deployed in a 192.168.20.0/24 subnet. Therefore we need to create this network first.

## Create a Logical Network

The next step we need to complete is to create a logical network. For our Horizon Lab we are going to need several virtual machines. These are: Active Directory, Hoirzon Connections Server, Unified Access Gateway (UAG) and the Golden Master Image.

The AD, Horizon Connection Server, UAG and Golden Master Image will be deployed in a 192.168.xxx.xxx/24 subnet. Therefore we need to create this network first.

Navigate to **Networking & Security**, then **Segments** and **Add a segment**

Depending on weather you are working in SDDC **Student-Workshop-X.1** or **Student-Workshop-X.2** environmentm, you will need to create different networks.

1. **Name**
- **Student-Workshop-X.1** use **Horizon100** 
- **Student-Workshop-X.2** use **Horizon200**
2. **Type** - Routed
3. **Gateway / Prefix Length**
- **Student-Workshop-X.1** use 192.168.100.1/24
- **Student-Workshop-X.2** use 192.168.200.1/24
4. **DHCP** - Disabled

## Creating a VPN Connection

The next step will be to create the VPN connection between Student-Workshop-X.1 and Student-Workshop- X.2. To do that, follow the steps below:


1. Go to **Network & Security**
2. **VPN**
3. **Route Based**
4. **Add VPN**

- **Note** make a note of your Public IP adress.
- **Note** To complete the **Remote Public IP** field,  work with your workshop partner to obtain their Public IP address.
- **Note** Student-Workshop SDDC 1 needs to change the **LOCAL ASN** . Next to Add VPN , click on **EDIT LOCAL ASN** to 65001

![](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/VPN1.png)

![](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/VPN2.png)

| **Name** | **Local IP** | **Remote Public IP** | **BGP Local IP/Prefix Length** | **BGP Remote IP** | **BGP Remote ASN** | **Preshared Key** |
| Horizon-Student-Workshop-X.1 | Public | *Public IP Student X.2* | 169.254.111.1/30 | 169.254.111.2 | 65000| VMware1! |
| Horizon-Student-Workshop-X.2 | Public | *Public IP Student X.1* | 169.254.111.2/30 | 169.254.111.1 | 65001| VMware1! |


**Note** Click the refresh button as described in the screenshot to see that the tunnel is up and change to **green**

![](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/VPN3.png)

We have now completed network set up part of the lab. The next step will be to deploy the virtual machines we are going to need for the lab. We are goign to deploy these from templates, located in Content Libraries.


The next step will be to log onto vCenter and deploy VMs.

## Log into vCenter

To open vCenter, navigate to **OPEN VCENTER** in the top right hand corner of the screen. A pop up window will open, where you can vue vCenter login credentials.

Then go to **Show vCenter Credentials**

![](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/Show+vCdenter+Credentials.jpg)

Click on the clipboard icon, next to Password, to copy the password for accessing vCenter.

Click on the eye icon to make the password visible. **Make a note of this password, as it will be used later on in the lab**.

![](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/Open+vCenter.jpg)

Then click on **Open vCenter**. A new browser window will open, where you can log into vCenter.

**Username:** cloudadmin@vmc.local

**Password:** *Paste Password*

## Horizon Deployment

As part of the lab, we have already subscribed your SDDC to Content Libraries. These are located in an Amazon S3 bucket, where we have the VM templates stored and ready to use.

In the subscribed content library you will find the Active Directory VM, Golden Master Image VM, Unified Access Gateway (UAG) and the Connection Server VM templates, that you need to use for the deploymend of new desktops with Horizon.

1. Click on **Menu**
2. Click on **Content Libraries**
3. Click on **horizon-content-library**

The first VM we need to deploy is Active Directory VM.

## Create Active Directory VM

1. Locage and right Click on **AD-100** or **AD-200**, depending on your student workshop number.
2. Click on **New VM from This Template**
3. **Virtual Machine Name** - AD-100 or AD-200, depending on your student workshop number.
4. Under select a location for Virtual Machine, click on **SDDC-Datacenter**, then click on **Workloads**

![](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/create+AD+100-+1.png)

5. Click **Next**
6. Click **Cluster-1**, then click **Compute-ResourcePool**, then click **Next**
7. Review Details. Click **Next**
8. Select Storage. Select **WorkloadDatastore** and click **Next**
9. Select Networks - Select the network you created in the previous lab **Horizon100** or **Horizon200**, depending on your student workshop number.
10. Click **Next** and **Finish**

## Create Horizon Connection Server VM

1. Locate and right Click on the **CS-100** or **CS-200**, depending on your student workshop number. 
2. Click on **New VM from This Template**
3. **Virtual Machine Name** - CS-100 or CS-200, depending on your student workshop number.
4. Under select a location for Virtual Machine, click on **SDDC-Datacenter**, then click on **Workloads**
5. Click **Next**
6. Click **Cluster-1**, then click **Compute-ResourcePool**, then click **Next**
7. Review Details. Click **Next**
8. Select Storage. Select **WorkloadDatastore** and click **Next**
9. Select Networks - Select the network you created in the previous lab **Horizon100** or **Horizon200**, depending on your student workshop number.
10. Click **Next** and **Finish**

## Create your Golden Master Image

This Golden Master Image will be used to deploy desktops, using Instant Clone technology.

1. Locate and right Click on the **GM-W10-WS-1** or **GM-W10-WS-1**, depending on your student workshop number.
2. Click on **New VM from This Template**
3. **Virtual Machine Name** -  GM-W10
4. Under select a location for Virtual Machine, click on **SDDC-Datacenter**, then click on **Templates**
5. Click **Next**
6. Click **Cluster-1**, then click **Compute-ResourcePool**, then click **Next**
7. Review Details. Click **Next**
8. Select Storage. Select **WorkloadDatastore** and click **Next**
9. Select Networks - Select the network you created in the previous lab **Horizon100** or **Horizon200**, depending on your student workshop number.
10. Click **Next** and **Finish**

## Create your Unified Access Gateway (UAG) VM

1. Go to **Menu**, **VM and Templates**, 
2. right click on **Workloads**, select **deploy OVF tempate**, type URL: 
- **https://s3-us-west-2.amazonaws.com/horizon-200/UAG-200/euc-unified-access-gateway-3.4.0.0-11037344_OVF10.ova**
- Click **yes**
3. **Virtual Machine Name** - UAG-100** or UAG-200, depending on your student workshop number.
4. Under select a location for Virtual Machine, click on **SDDC-Datacenter**, then click on **Workloads**
5. Click **Next**
6. Click **Cluster-1**, then click **Compute-ResourcePool**, then click **Next**
7. Review Details. Click **Next**
8. Configuration - chose **Single NIC**, then click **Next**
9. Select Storage. Select **WorkloadDatastore** and click **Next**
10. Select Networks - chose **Horizon100** or **Horizon200** for all three networks. 
11. Customize template - Complete the following:
    - **IPMode for NIC 1 (eth0)** type **STATICV4** 

    ![](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/UAG+1.jpg)

    - **NIC 1 (eth0) IPv4 address** type **192.168.100.12** or **192.168.200.12**, depending on your student workshop number.

    ![](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/UAG+2.jpg)

    - **DNS server address** type **192.168.100.10** or **192.168.200.10**, depending on your student workshop number.
    - **NIC 1 (eth0) netmask** type **255.255.255.0**
    - **IPv4 Default Gateway** type **192.168.100.1** or **192.168.200.1**, depending on your student workshop number.
    - **Unified Gateway Appliance Nmae** type **UAG100** or **UAG200**, depending on your student workshop number.

    ![](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/UAG+3.jpg)

    - **Join CEIP** Untick the checkbox, under text.
    - **Password for the root user and the Admin User** type **VMware1!**

    ![](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/UAG+4.jpg)


## Power on Active Directory

1. Go to **Menu**
2. Go to **VMs and Templates**
3. Power on the VM **AD-100** or **AD-200**, depending on your student workshop number.
4. Launch the Web Console
5. Sign in with  **VDIONVMC\Administrator** and password **VMware1!**

## Power on Unified Access Gateway

1. Power on the VM **UAG-100** or **UAG-200**, depending on your student workshop number.

## Power on Horizon Connection Server

1. Power on the VM **CS-100** or **CS-200**, depending on your student workshop number.
2. Launch the Web Console
3. Sign in with  **vdionvmc\Administrator** and password **VMware1!**

## Configure Horizon Environment

In order to create desktops in your SDDC, first we need to implement your Student SDDC vCenter into the existing Horizon infrastructure.

Once logged into the **Horizon Connection Server**, locate Horizon 7 Administrator Console shortcut, located on the Desktop page of the Horizon Connection Server. Or open a browser and browse to **https://192.168.100.11/admin** or **https://192.168.200.11/admin** depending on your workshop sddc number.

![](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/CS+1.jpg)

Double click to open the Administrator Console. That will launcde in a browser window. You will see the below certificate error waring. Proceed to localhost.

Log into Horizon 7 Administrator Console.

**User Name:** Administrator
**Password** VMware1!

![](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/CS+2.jpg)

The next step of the Horizon configuration will be to add the vCenter server of your SDDC. To do that we need to make a note of the vCenter server address, user name and password. To obtain these, do the following.

1. Go back to your VMware Cloud on AWS Console.
2. Click on your SDDC
3. Click on the **Support** tab and make a note of the **vCenter Public IP address**.

![](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/CS+6.jpg)

Go back to the Web Console of the Horizon Connection Server. On the left hand side, go to **View Configuration**, then **Servers**.

![](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/CS+3.jpg)

Under vCenter Server tab, click **Add…**.

![](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/CS+4.jpg)

In the popup window fill in the following:

1. **Server Address** - fill in the Public vCenter IP address, which you noted down in the previous step.
2. **User Name** - **cloudadmin@vmc.local**
3. **Password** - fill in the Public vCenter IP address, which you noted down in previous section of the lab.
4. Make sure that the **VMware Cloud on AWS** tick box is selected
5. Click **Next**.

![](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/CS+5.jpg)

6. An **Invalid Certificate Detected** popup box will open. Click on **View Certificate**. Then click on **Accept**.
7. Click **Next**.
8. **VERY IMPORTANT** Make sure the **Reclaim VM disk space** tick box is **unchecked**. Click **Next** 
9. Click **Finish**

You have successfully added your vCenter Server.

The next step is to add **Instant Clone Domain Admins**
1. Go to **Instant Clone Domain Admins** on the right hand side.
2. Click **Add**
3. Fill in **User Name** and **Password** filed
    **User Name** - administrator
    **Password** - VMware1!
4. Click OK

![](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/CS+7.jpg)

## Deploy Desktop Pool

Instant Clones require a shapshot of the Golden Master Image. Therefore we need to create a snapshot of the Golden Master Image VM.

1. Go back to the vSphere Client. 
2. Click on **Hosts and Clusters**
3. Locate your Golden Master Image VM **GM-W10**
4. Right click, go to **Snapshots**, then **Take Snapshot**
5. Name **1.0**
6. Click **OK**

Go back to the Horizon Connection Server Web Console. Your Horizon Connection Server Console should be still open. If not please open it either via the shortcut on the desktop or go to https://192.169.100.11/admin or https://192.168.200.11/admin, depending on your workshop sddc number.

1. On the Horizon Connection Server admin console on the left site you can click on **Catalog** and then **Desktop Pools**
2. Click **Add** Select **Automated Desktop Pool** click **Next**
3. Select **Floating** click **next**
4. Select **Instant Clones** click **next**
5. On **ID** - type  **Pool1** or **Pool2**, depending on your Stundent workshop ID
6. On **Display name** - **Pool1** or **Pool2**, depending on your Stundent workshop ID
7. Click **next**
8. Activate **HTML Access**, click **next**
    ![](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/Desktops-pool1.png)
9. Under **Naming Pattern:** type -
- **desktops1{n:fixed=3}** or
- **desktops2{n:fixed=3}** depending on your Student Workshop ID
10. change **Max number of machines** - 2
11. click **next**
12. Click **Use VMware Virtual SAN**, click **next**
13. **Parent VM in vCenter:** click **Browse**  and select your **GM-W10** image , click **OK**
14. **Snapshot:** click **Browse** and select the previous created Snapshot, click **OK**
15. **VM folder location:** click **Browse**  - select **Workloads**, click **OK**
16. **Cluster:** click **Browse**  - select **Cluster-1** , click **OK**
17. **Ressource Pool** click **Browse** - Select **Horizon-RessourcePool** , click **OK**
18. **Datastore:** click **Browse** - select **WorkloadDatastore** and click **OK**
19. Do not change **Networks**. Click **Next**
20. Do not change **Guest customization**. Click **Next**
21. Click **Finish**

The deployment of the desktops should start now.
The deployment will **fail** the first time. Because this is a Single Node SDDC, you need to change the newly created Horizon vSAN policys to **no rendundancy**

For that please switch to your **vSphere Web Client Console**

1. Click **Menu**
2. Click **Policies and Profiles**
3. left side click **VM Storage Policies**
4. You will find **4** new Storage Policies that will have a **random number** at the end. All 4 needs to be changed: **search** for them in the storage profiles

- **OS_DISK_FLOATING_.............**
- **PERSISTENT_DISK_..............**
- **REPLICA_DISK_.................**
- **VM_HOME_......................**

in the screenshot you see 3 but you need to change all **4**
![](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/Desktops-pool2.png)

5. Cick on each of the above and click **Edit Settings**
6. click **NEXT**
7. Click **Next**
8. **Storage Policy:** click on **Failues to tolerate** and click on **No data redundancy** , click **next**, click **next** and **finish**
![](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/Desktops-pool5.png)
9. repeat this step for the **4** of the above mentionted storage policies!!!

Go back to your Horizon Connection Server Web Console:

1. Click on **Desktops Pools**
2. double Click on **your new created desktop pool**
3. Click **Status**
4. Click **Enable Provisioning**
![](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/Desktops-pool4.png)
5. Click **OK**

Entitle Users to the Pools to access it later with those users:

1. Click on **Entitlements**
2. Click **add entitlments**
3. click **add**
4. Search in **Name/user name** for **Workshop** you will find two users. 
5. Select both users and click **OK**
![](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/Desktops-pool6.png)
4. click **finish**

Go back to your **vSphere Web Client** and watch the the provisioning in the task list: It will take around 5-10 min to have the desktops available.


## External Access

Now we will make your Horizon environment available from external. 

1. Go to your VMC console.
2. Go to your SDDC
3. Go to **Networking and Security** tab
4. On the left site go to **Public IPs**
5. Click on **REQUEST NEW IP**
6. **Notes** - type **Horizon**
7. Click **Save**
![](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/External1.png)

Now that we have a public IP we need to create a NAT rule to your UAG

1. Go to your VMC console.
2. Go to your SDDC
3. Go to **Networking and Security** tab
4. On the left site go to **NAT**
5. Click **ADD RULE**
- **Name** - Horizon
- **Service** - delete **All Traffic** -  type **HTPPS / 443**
- **internal IP** - type ip of your UAG **192.168.100.12** or **192.168.200.12**  depending on your student workshop ID
7. Click **Save**
![](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/External2.png)

**Note** make a note of your Public IP cause we will need this later for configuring UAG and Horizon Connection server.

1. Go to **Horizon Connection Server Web Console**
2. Open *File Explorer** in your Horizon Server
3. Open the **locked.properties** file under c:\Program Files\VMware\VMmware View\Server\sslgateway\conf\ with your **notepad**
![](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/External3.png)

4. enter your **Public IP** into **balancedHost=**
![](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/External4.png)
5. **Save** the file
6. Close notepad
7. **restart** your horizon connection server - right click on **Start** -> **Studown or sign out** -> **restart**
8. wait for the server to come back.


You need to login back into the **Horizon Connection Server** and open a **browser**

1. Login to your horizon connection Server as **Administrator** and  **VMware1!**
2. Click on the shortcut icon for the Horizon Connection server or navigate to **https://localhost/admin**

- **Note** it can take up to 5 min to have all services started to be able to browse to the webpage of the horizon connection server

3. Login to the horizon 7 conneciton server administation console with **Administrator** and **VMware1!**
4. On the let site go to **View Configuration**
- **Servers**
- **connection server** tab
- select your **connection server**
- click **EDIT**
5. **disable** the tunneling
![](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/External6.png)
6. Click **OK**


NOW you are going to configure your UAG. Go to your Horizon Connection Server.

1. Open a **browser**
2. navigate to **https://192.168.100.12:9443** or **https://192.168.200.12:9443** , depending on your student workshop ID
3. login with **admin** and **VMware1!**
4. Click 

