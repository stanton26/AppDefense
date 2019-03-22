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

In this Lab we are going to create and install a Horizon 7 environment, using Cloud Pod Architectire to connect two Horizon environemnts for Global Management and Entitlement Rights.

## Viewing your SDDC

![SDDC-Network-01](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc01.jpg)

After logging in, you should see a single SDDC in the user interface following the naming format Student-Workshop-#.#. 

An SDDC is a fully deployed environment including vSphere, NSX, vSAN and vCenter Server. Deployment of a fully configured SDDC takes about two hours, so for the purposes of this lab, we have the SDDC already deployed. This SDDC is in the same state as it would be if you have deployed it. Let's take a look at the SDDC properties.

1. First click on **View Details** to open the SDDC properties.

![SDDC-Network-02](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/sddc02.jpg)

Let's start with the Summary of the SDDC. There are a number of other tabs available, which are as follows:

1. **Support** - You can contact Support with your SDDC ID, Org ID, vCenter Private and Public IPs and the date of your SDDC Deployment.
2. **Settings** - Gives you access to your vSphere Client (HTML5), vCenter Server API, PowerCLI Connect, vCenter Server and reviews your Authentication information.
3. **Troubleshooting** - Allows you to run network connectivity tests to ensure all necessary access is available to perform select tasks.
4. **Add Ons** - Here you will find Add On services for your VMware Cloud on AWS environment like Hybrid Cloud Extension (HCX) and VMware Site Recovery.
5. **Networking & Security** - Provides a full diagram of the Management and Compute Gateways.  This is where you can configure logical networks, VPNs and firewall rules. We will cover this in more detail later. Click on Networking & Security to learn more about VMware Cloud on AWS Network and Security Configuration.

## What is Horizon on VMware Cloud on AWS

VMware Horizon® 7 for VMware Cloud™ on AWS delivers a seamlessly integrated hybrid cloud for virtual desktops and applications. It combines the enterprise capabilities of the VMware Software-Defined Data Center, delivered as a service
on AWS, with the market-leading capabilities of VMware Horizon for a simple, secure, and scalable solution. You can easily extend desktop services to address more use cases such as on-demand capacity, disaster recovery, and cloud co-location without buying additional data center resources.

![1](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/1.png)

### Simplify Public and Hybrid Cloud Management

For customers, who are already familiar with Horizon 7 or have Horizon 7 deployed on premises, running Horizon 7 on VMware Cloud on AWS lets you leverage an unified architecture and familiar tools. You can simplify management for Horizon 7 deployments using on-premises infrastructure and VMware Cloud on AWS with Cloud Pod Architecture (CPA) by linking Cloud deployments in different regions, or by linking on-premises deployments to VMware Cloud on AWS ones. This means that you use the same expertise and tools you know from VMware vSphere® and Horizon 7 for operational consistency, and leverage the rich feature set and flexibility you expect from Horizon 7.

![2](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Horizon-LAB/2.png)

Let's proceed to the Workshop excersise.

## Configuring SDDC Firewall Rules

<!-- Elena - the above sentence needs changing. there is no previous lab-->

### Compute Gateway Firewall Rules

By default, the Compute Gateway is set to deny all inbound and outbound traffic. You need to add additional firewall rules to allow access to your workload VMs which you provision in the VMware Cloud on AWS platform.

### Create Compute Gateway Firewall Rule

1. Under **Network & Security** tab, navigate to **Security**, then **Gateway Firewall**
2. On the right hand side go to **Compute Gateway**
3. Click **ADD NEW RULE**

Horizon requires a number of ports to be opened for communication and Inter-POD connectivity. For the purposes of the lab,and ease of management, we are going to allow communication cross everything. The first rule we are going to create is an Any - Any - Any - rule.

1. **Name** - Horizon
2. **Source** - Any
3. **Destination** - Any
4. **Service** - Any
5. **Action** - Allow
6. **Applied To** - All Uplinks

The next step will be to edit the **Default VTI Rule** in the Compute Gateway Firewall Rules

1. Click the three dots next to **Default VTI Rule**
2. **EDIT**
3. **Action** - Allow

### Cretea a Logical Network

For this Horizon Lab we have prepared several virtual machines, which are Active Directory (AD), Horizon Connection Server, Unified Access Gateway (UAG) and a Golden Master Image.

In the next step we are going to create a logical network for the virtual machines we will deploy in this lab.

The AD, Horizon Connection Server, UAG and Golden Master Image will be deployed in a 192.168.xxx.xxx/24 subnet. Therefore we need to create this network first.

Navigate to **Networking & Security**, then **Segments** and **Add a segment**

Depending on whether you are working in SDDC **Student-Workshop-X.1** or **Student-Workshop-X.2** environment, you will need to create different networks.

1. **Name**
    - **Student-Workshop-X.1** use **Horizon100**
    - **Student-Workshop-X.2** use **Horizon200**
2. **Type** - Routed
3. **Gateway/Prefix Length**
    - **Student-Workshop-X.1** use 192.168.100.1/24 - where X is your **Student ID**
    - **Student-Workshop-X.2** use 192.168.200.1/24 - where X is your **Student ID**
4. **DHCP** - Disabled

### Creating a VPN Connection

The next step will be to create the VPN connection between Student-Workshop-X.1 and Student-Workshop- X.2. To do that, follow the steps below:

1. Go to **Network & Security**
2. **VPN**
3. **Route Based**
4. **Add VPN**

- **Note** make a note of your Public IP adress.
- **Note** To complete the **Remote Public IP** field,  work with your workshop partner to obtain their Public IP address.
- **Note** Student-Workshop SDDC 1 needs to change the **LOCAL ASN**. Next to Add VPN,click on **EDIT LOCAL ASN** and type 65001.

![VPN1](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/VPN1.png)

![VPN2](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/VPN2.png)

| **Name** | **Local IP** | **Remote Public IP** | **BGP Local IP/Prefix Length** | **BGP Remote IP** | **BGP Remote ASN** | **Preshared Key** |
| Horizon-Student-Workshop-X.1 | Public | *Public IP Student X.2* | 169.254.111.1/30 | 169.254.111.2 | 65000| VMware1! |
| Horizon-Student-Workshop-X.2 | Public | *Public IP Student X.1* | 169.254.111.2/30 | 169.254.111.1 | 65001| VMware1! |

**Note** Click the refresh button, as shown in the screenshot below, to check if the tunnel is up and change to **green**.

![VPN3](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/VPN3.png)

We have now completed network setup part of the lab. The next step will be to deploy the virtual machines we need for the lab. We are going to deploy those VMs from templates, located in Content Libraries.

The next step will be to log into vCenter and deploy VMs.

## Log into vCenter

To open vCenter, navigate to **OPEN VCENTER** in the top right hand corner of the screen. A pop-up window will open, where you can view vCenter login credentials.

Then go to **Show vCenter Credentials**

![Show+vCdenter+Credentials](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/Show+vCdenter+Credentials.jpg)

Click on the clipboard icon, next to Password, to copy the password for accessing vCenter.

Click on the eye icon to make the password visible. **Make a note of this password, as it will be used later on in the lab**.

![Open+vCenter](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/Open+vCenter.jpg)

Then click on **Open vCenter**. A new browser window will open, where you can log into vCenter.

**Username:** cloudadmin@vmc.local

**Password:** *Paste Password*

## Horizon Deployment

As part of the lab, we have already subscribed your SDDC to Content Libraries. These are located in an Amazon S3 bucket, where we have the VM templates stored and ready to use.

In the subscribed content library you will find the Active Directory VM, Golden Master Image VM, Unified Access Gateway (UAG) and the Connection Server VM templates, that you need to use for the deployment of new desktops with Horizon.

1. Click on **Menu**
2. Click on **Content Libraries**
3. Click on **horizon-content-library**

The first VM we need to deploy is Active Directory VM.

### Create Active Directory VM

1. Locage and right Click on **AD-100** or **AD-200**, depending on your student workshop number
2. Click on **New VM from This Template**
3. **Virtual Machine Name** - AD-100 or AD-200, depending on your student workshop number
4. Under select a location for Virtual Machine, click on **SDDC-Datacenter**, then click on **Workloads**
    ![create+AD+100-+1](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/create+AD+100-+1.png)
5. Click **Next**
6. Click **Cluster-1**, then click **Compute-ResourcePool**, then click **Next**
7. Review Details. Click **Next**
8. Select Storage. Select **WorkloadDatastore** and click **Next**
9. Select Networks - Select the network you created in the previous lab **Horizon100** or **Horizon200**, depending on your student workshop number
10. Click **Next** and **Finish**

### Create Horizon Connection Server VM

1. Locate and right Click on the **CS-100** or **CS-200**, depending on your student workshop number
2. Click on **New VM from This Template**
3. **Virtual Machine Name** - CS-100 or CS-200, depending on your student workshop number
4. Under select a location for Virtual Machine, click on **SDDC-Datacenter**, then click on **Workloads**
5. Click **Next**
6. Click **Cluster-1**, then click **Compute-ResourcePool**, then click **Next**
7. Review Details. Click **Next**
8. Select Storage. Select **WorkloadDatastore** and click **Next**
9. Select Networks - Select the network you created in the previous lab **Horizon100** or **Horizon200**, depending on your student workshop number
10. Click **Next** and **Finish**

### Create your Golden Master Image

This Golden Master Image will be used to deploy desktops, using Instant Clone technology.

1. Locate and right Click on the **GM-W10-WS-1**
2. Click on **New VM from This Template**
3. **Virtual Machine Name** -  GM-W10
4. Under select a location for Virtual Machine, click on **SDDC-Datacenter**, then click on **Templates**
5. Click **Next**
6. Click **Cluster-1**, then click **Compute-ResourcePool**, then click **Next**
7. Review Details. Click **Next**
8. Select Storage. Select **WorkloadDatastore** and click **Next**
9. Select Networks - Select the network you created in the previous lab **Horizon100** or **Horizon200**, depending on your student workshop number
10. Click **Next** and **Finish**

### Create your Unified Access Gateway (UAG) VM

1. Go to **Menu**, **VM and Templates**
2. Right click on **Workloads**, select **deploy OVF template**, type URL:
    - **https://s3-us-west-2.amazonaws.com/horizon-200/UAG-200/euc-unified-access-gateway-3.4.0.0-11037344_OVF10.ova**
    - Click **Yes**
3. **Virtual Machine Name** - **UAG-100** or **UAG-200**, depending on your student workshop number
4. Under select a location for Virtual Machine, click on **SDDC-Datacenter**, then click on **Workloads**
5. Click **Next**
6. Click **Cluster-1**, then click **Compute-ResourcePool**, then click **Next**
7. Review Details. Click **Next**
8. Configuration - chose **Single NIC**, then click **Next**
9. Select Storage - select **WorkloadDatastore** and click **Next**
10. Select Networks - chose **Horizon100** or **Horizon200** for all three networks
11. Customize template - complete the following:
    - **IPMode for NIC 1 (eth0)** type **STATICV4**

    ![UAG+1](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/UAG+1.jpg)

    - **NIC 1 (eth0) IPv4 address** type **192.168.100.12** or **192.168.200.12**, depending on your student workshop number

    ![UAG+2](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/UAG+2.jpg)

    - **DNS server address** type **192.168.100.10** or **192.168.200.10**, depending on your student workshop number
    - **NIC 1 (eth0) netmask** type **255.255.255.0**
    - **IPv4 Default Gateway** type **192.168.100.1** or **192.168.200.1**, depending on your student workshop number
    - **Unified Gateway Appliance Nmae** type **UAG100** or **UAG200**, depending on your student workshop number

    ![UAG+3](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/UAG+3.jpg)

    - **Join CEIP** Untick the checkbox, under text
    - **Password for the root user and the Admin User** type **VMware1!**

    ![UAG+4](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/UAG+4.jpg)

### Power on Active Directory

1. Go to **Menu**
2. Go to **VMs and Templates**
3. Power on the VM **AD-100** or **AD-200**, depending on your student workshop number
4. Launch the Web Console
5. Sign in with  **VDIONVMC\Administrator** and password **VMware1!**

### Power on Unified Access Gateway

1. Power on the VM **UAG-100** or **UAG-200**, depending on your student workshop numbe

### Power on Horizon Connection Server

1. Power on the VM **CS-100** or **CS-200**, depending on your student workshop number
2. Launch the Web Console
3. Sign in with  **vdionvmc\Administrator** and password **VMware1!**

## Configure Horizon Environment

In order to create desktops in your SDDC, first we need to implement your Student SDDC vCenter into the existing Horizon infrastructure.

Once logged into the **Horizon Connection Server**, locate Horizon 7 Administrator Console shortcut, located on the Desktop screen of the Horizon Connection Server. Or open a browser and browse to **https://192.168.100.11/admin** or **https://192.168.200.11/admin** depending on your workshop sddc number.

![CS+1](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/CS+1.jpg)

Double click to open the Administrator Console. That will launch into a browser window. You will a certificate error warning. Proceed to localhost.

Log into Horizon 7 Administrator Console.

**User Name:** Administrator
**Password** VMware1!

![CS+2](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/CS+2.jpg)

The next step of the Horizon configuration will be to add the vCenter server of your SDDC. To do that we need to make a note of the vCenter server IP address, user name and password. To obtain these, do the following.

1. Go back to your VMware Cloud on AWS Console
2. Click on your SDDC
3. Click on the **Support** tab and make a note of the **vCenter Public IP address**

![CS+6](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/CS+6.jpg)

Go back to the Web Console of the Horizon Connection Server. On the left hand side, go to **View Configuration**, then **Servers**.

![CS+3](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/CS+3.jpg)

Under vCenter Server tab, click **Add…**.

![CS+4](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/CS+4.jpg)

In the popup window fill in the following:

1. **Server Address** - fill in the Public vCenter IP address, which you noted down in the previous step
2. **User Name** - **cloudadmin@vmc.local**
3. **Password** - fill in the vCenter Password, which you noted down in previous section of the lab
    **NOTE** type the password in the description field because you can see it in clear text!! Compare it with the one you noted.  We saw some Keyboard typos in privious LABS.
4. Make sure that the **VMware Cloud on AWS** tick box is selected
5. Click **Next**

    ![CS+5](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/CS+5.jpg)

6. An **Invalid Certificate Detected** popup box will open. Click on **View Certificate**. Then click **Accept**
7. Click **Next**
8. **VERY IMPORTANT** Make sure the **Reclaim VM disk space** tick box is **unchecked**. Click **Next**
    **Note**  If the checkbox stays grey, the password is wrong.
9. Click **Finish**

You have successfully added your vCenter Server.

The next step is to add **Instant Clone Domain Admins**

1. Go to **Instant Clone Domain Admins** on the left hand side
2. Click **Add**
3. Fill in **User Name** and **Password** field
    **User Name** - administrator
    **Password** - VMware1!
4. Click **OK**

![CS+7](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/CS+7.jpg)

## Deploy Desktop Pool

Instant Clones requires a snapshot of the Golden Master Image. Therefore we need to create a snapshot of the Golden Master Image VM.

1. Go back to the vSphere Client.
2. Click on **Hosts and Clusters**
3. Locate your Golden Master Image VM **GM-W10**
4. Right click, go to **Snapshots**, then **Take Snapshot**
5. Name **1.0**
6. Click **OK**

Go back to the Horizon Connection Server Web Console. Your Horizon Connection Server Console should be still opened. If not please open it either via the shortcut on the desktop, or go to https://192.169.100.11/admin or https://192.168.200.11/admin, depending on your workshop sddc number.

1. On the Horizon Connection Server admin console on the left side you can click on **Catalog** and then **Desktop Pools**
2. Click **Add**, select **Automated Desktop Pool**, click **Next**
3. Select **Floating** click **Next**
4. Select **Instant Clones** click **Next**
5. On **ID** - type  **Pool1** or **Pool2**, depending on your Student workshop ID
6. On **Display Name** - **Pool1** or **Pool2**, depending on your Student workshop ID
7. Click **Next**
8. Activate **HTML Access**, click **Next**

    ![Desktops-pool1](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/Desktops-pool1.png)

9. Under **Naming Pattern** type -
    - **desktops1{n:fixed=3}** or
    - **desktops2{n:fixed=3}** depending on your Student Workshop ID
10. Change **Max number of machines** - 2
11. click **Next**
12. Click **Use VMware Virtual SAN**, click **Next**
13. **Parent VM in vCenter:** click **Browse**  and select your **GM-W10** image, click **OK**
14. **Snapshot:** click **Browse** and select the previously created Snapshot, click **OK**
15. **VM folder location:** click **Browse**  - select **Workloads**, click **OK**
16. **Cluster:** click **Browse**  - select **Cluster-1**, click **OK**
17. **Resource Pool** click **Browse** - Select **Horizon-ResourcePool**, click **OK**
18. **Datastore:** click **Browse** - select **WorkloadDatastore**, click **OK** - **warning message appears** click **OK**
19. Do **not** change **Networks**, Click **Next**
20. Do **not** change **Guest customization**, Click **Next**
21. Click **Finish**

The deployment of the desktops should start now.
**NOTE** The deployment will **fail** the first time. Because this is a Single Node SDDC, you need to change the newly created Horizon vSAN policies to **no redundancy**

For that please switch to your **vSphere Web Client Console**

1. Click **Menu**
2. Click **Policies and Profiles**
3. Left side click **VM Storage Policies**
4. You will find **4** new Storage Policies that will have a **random number** at the end. All 4 needs to be changed: **search** for them in the storage profiles
    - **OS_DISK_FLOATING_.............**
    - **PERSISTENT_DISK_..............**
    - **REPLICA_DISK_.................**
    - **VM_HOME_......................**

    In the screenshot you see 3 but you need to change all **4**
    ![Desktops-pool2](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/Desktops-pool2.png)
5. Cick on each of the above and click **Edit Settings**
6. Click **Next**
7. Click **Next**
8. **Storage Policy:** click on **Failures to tolerate** and click on **No data redundancy** , click **Next**, click **Next** and **Finish**
    ![Desktops-pool5](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/Desktops-pool5.png)
9. Repeat these steps for **4** of the above mentionted storage policies!

Go back to your Horizon Connection Server Web Console:

1. Click on **Desktops Pools**
2. Double Click on **your new created desktop pool**
3. Click **Status**
4. Click **Enable Provisioning**
    ![Desktops-pool4](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/Desktops-pool4.png)
5. Click **OK**

Entitle Users to the Pools, in order to be able to access the desktops later.

1. Click on **Entitlements**
2. Click **Add Entitlments**
3. In **Name/User name** type **Workshop**, then **Find**. You will find two users
4. Select both users and click **OK**
    ![Desktops-pool6](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/Desktops-pool6.png)
5. Click **Finish**

Go back to your **vSphere Web Client** and watch the the provisioning in the task list: It will take around 5-10 min to have the desktops available.

## External Access

The next step will be to enable your Horizon environment to be accessed externally.

1. Go to your VMC console
2. Go to your SDDC
3. Go to **Networking and Security** tab
4. On the left site go to **Public IPs**
5. Click on **REQUEST NEW IP**
6. **Notes** - type **Horizon**
7. Click **Save**
![External1](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/External1.png)

Now that we have a public IP we need to create a NAT rule to your UAG.

1. Go to your VMC console
2. Go to your SDDC
3. Go to **Networking and Security** tab
4. On the left site go to **NAT**
5. Click **ADD RULE**
    - **Name** - Horizon
    - **Service** - delete **All Traffic** -  type **HTPPS / 443**
    - **Internal IP** - type ip of your UAG **192.168.100.12** or **192.168.200.12**  depending on your student workshop ID
6. Click **Save**

![External2](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/External2.png)

**Note** make a note of your Public IP Address, because it will be needed later on in the lab, for configuring the UAG and Horizon Connection server.

1. Go to **Horizon Connection Server Web Console**
2. Open **File Explorer** in your Horizon Server
3. Open the **locked.properties** file under c:\Program Files\VMware\VMmware View\Server\sslgateway\conf\ with your **Notepad**
    ![External3](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/External3.png)
4. Enter your **Public IP** into **balancedHost=**
    ![External4](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/External4.png)
5. **Save** the file
6. Close Notepad
7. **Restart** your horizon connection server - click on **Start** -> **Stutdown or sign out** -> **Restart**
8. Wait for the server to come back

You need to login back into the **Horizon Connection Server** and open a **browser**

1. Login to your Horizon Connection Server as **Administrator** and **VMware1!**
2. Click on the shortcut icon for the Horizon Connection server or navigate to **https://localhost/admin**

    - **Note** it can take up to 5 min to have all services started to be able to browse to the webpage of the Horizon Connection Server

3. Login to the Horizon 7 Conneciton Server Administation Console with **Administrator** and **VMware1!**
4. On the left side go to **View Configuration**
    - **Servers**
    - **Connection Server** tab
    - Select your **connection server**
    - Click **EDIT**
5. **Disable** the tunneling
     Uncheck **HTTP(S) Secure Tunnel**
     Click on **Do not use Blast Secure Gateway**
    ![External6](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/External6.png)
7. Click **OK**

The next step is to configure your UAG. Go to your Horizon Connection Server.

1. Open a **browser**
2. Navigate to **https://192.168.100.12:9443** or **https://192.168.200.12:9443** , depending on your student workshop ID
3. Click **Proceed** on the **security warning**
4. Click **Go Back**
5. Login with **admin** and **VMware1!** , Click **Login**
6. Click **Select** on the right side **Configure Manually**
7. Click **Edge Service Settings** **Show**
    - Click **Settings sign** next to horizon settings
    - Click **Enable Horizon**
    - Click **Enable Blast**
    - Click **Enable Tunnel**
8. For **Connection Server URL** type **https://hz-ws-cs100.vdionvmc.local:443** or **https://hz-ws-cs200.vdionvmc.local:443**, depending on your Studen ID
9. For **Connection Server URL Thumbprint** go back to the **connection server tab** / **View Administrator** and click on the certificate

    ![External](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/External7.png)

    ![External8](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/External8.png)

    - Go to **Details** Tab
    - Scroll down to **Thumbprint**
    - **Copy** the thumbprint you see
    - **Switch** to the **UAG Tab** / Console in the other **browser tab**
10. In **Connection Server URL Thumbprint** write **sha1=*your copy of the thumbprint***
11. **Blast External URL** type **https://your public IP:443**
12. **Tunnel External URL** type **https://your public IP:443**
13. Click **Save**
14. Verify your configuration
    Click the **Refresh Button**
    Click on the **Horizon Settings** arrow
    Check that **Tunnel**, **Blast**, **UDP Tunnel Server**, **Horizon Destination Server** is green
    ![External9](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/External9.png)

## Access your Desktop

1. Open a browser on your local Machine / Laptop
2. Navigate to **https://your-public-ip**
3. **Proceed on the security warnings**
4. Click **VMware Horizon HTML Access** on the right side.
5. Sign in with Username: **Workshop100** or **Workshop200** and Password: **VMware1!**
6. Click **Start**
7. **Restart your Desktop**

Congratulations! You succesfully accessed your desktop from the Internet!

## Expand Desktop Pool

The next step will be to expand the desktop pool we ceated previously in this lab.

1. Go to your **Horizon Connection Server**
2. Open the shortcut for the Horizon Connection Server configuration or go to **https://localhost/admin**
3. Login with **Administrator** and **VMware1!**
4. Go to **Catalog** -> **Desktop Pools**
5. Right click on your previously created Desktop Pool and click **Edit**
    - Go to **Provisioning Settings**
    - Change **Maximum Number of Machines** to **20**
    - Click **OK**

    ![DesktopPool](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/Desktop7.jpg)

6. Double click the Desktop Pool.
7. Go to **Inventory** tab. Monitor progress of desktop creation.

## Build Cloud Pod Architecture

The Cloud Pod Architecture feature links together multiple View Pods to provide a single large desktop brokering and management environment. When the Cloud Pod Architecture feature is enabled, you can join togehter multiple View Pods to form a single View implementation called a Pod Federation. A Pod Federation can span multiple sites and datacenters.

For configuring CPA you need to work **together** with your partner in this section because only **one** person can **initialize** the CPA and the other needs to **join** the CPA. Please discuss within your **team** who will initialize and who will join the initilized CPA.

![CPA1](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/CPA1.png)

When you decided who will initialize and who will be joining, proceed as follows:

To initilize the CPA:

1. Go to your **Horizon Connection Server**
2. Open the shortcut for the Horizon Connection Server configuration or go to **https://localhost/admin**
3. Login with **Administrator** and **VMware1!**
4. Go to **View Configruation** -> **Cloud Pod Architecture**
5. Click **Initialize the Cloud Pod Architecture feature**
6. Click **OK**
7. **Wait** for the task to complete
8. Click **OK**

To join the CPA:

1. Go to your **Horizon Connection Server**
2. Open the shortcut for the Horizon Connection Server configuration or go to **https://localhost/admin**
3. Login with **Administrator** and **VMware1!**
4. Go to **View Configruation** -> **Cloud Pod Architecture**
5. Click **Join the pod federation**
6. For **Connection Server** type the local IP-Address of your Partner, which is either **192.168.100.11** or **192.168.200.11**
7. **Username** - vdionvmc\Administrator
8. **Password** - VMware1!
9. Click **Save**
10. Wait to join

![CPA4](https://s3-us-west-2.amazonaws.com/horizon-workshop/Screenshots/CPA4.png)

You now have successfully configured a CPA between SDDC1 and SDDC2. This could also be a SDDC running in two different regions - Frankfurt and Oregon for example.

The next step will be to create Global Entitlement rights to show some of the benefits of CPA. You need to decide which student will create the Global Entitlements policy.

**This task can only be done by ONE person!**

1. Go to **Catalog** -> **Global Entitlements**
2. Click **Add**
3. **Desktop Entitlement**
4. Click **Next**
5. Name and Policy Section
    - Name **Global Policy**
    - Policies click **Floating**
    - Scope **All Sites**
    - Default Display Protocol **VMware Blast**
    - Allow users to choose protocol **No**
    - Enable **HTML Access**
    - Click **Next**
6. Users and Groups
    - Click **Add**
    - Name/User Name - type **workshop** -> click **Find**
    - Select both users. Click **OK**
7. Click **Next**
8. Click **Finish**

We have a Global Policy created.

Following Horizon best practices we cannot have users in both Local and Global entitlements. Users can only have one or the other. Therefore the next step will be to delete the Local Pool entitlement.

**The next steps need to be completed by BOTH students**

1. Go to **Catalog** -> **Desktop Pools**
2. Right Click on your desktop pool. **Remove Entitlement**
3. Select both users. Click **OK**
4. In the pop up message click **OK**

## Add the Desktop Pools to Global Entitlement

1. Go to **Catalog** -> **Global Entitlements**
2. Double click on your global entitlement.
3. Click on **Local Pools** tab
4. Click **Add**
5. Select your pool and click **Add**

Now that we have the Global Entitlements set up, we can simulate a desktop pool failure by disabling one of the desktop pools.

**ONLY ONE person can do this test at a time**

1. Open a browser on your local Machine / Laptop
2. Navigate to **https://your-public-ip**
3. **Proceed on the security warnings**
4. Click **VMware Horizon HTML Access** on the right site.
5. Sign in with Username: **Workshop100** **Workshop200** and Password: **VMware1!**
6. Open **Command Prompt** and type **hostname**
7. Make a note of your desktop name.
8. Go back to your Horizon Connection Server.
9. Go to **Catalog** -> **Desktop Pools**
10. Right Click on the desktop pool and **Disable Desktop Pool**
11. Click **OK**
12. Repeat steps 1 to 6.
13. The host name should be a desktop from your partners pool, that resides on another SDDC.
14. Repeat steps 9 and 10, instead of disabling, chose **Enable Desktop Pool**

Your Horizon Lab partner can repeat the same steps.

This is the end of the lab. Thank you for attending.