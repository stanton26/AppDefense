---
layout: single
title: "VMware on AWS Workshop Manual"
date: 2018-03-22
tags: workshop
toc: true
author_profile: false
---

Release Date: December 2, 2017
Revision Date : February 11th, 2018

Authors :

- Chris Lennon <clennon@vmware.com>
- Eric Hardcastle <ehardcastle@vmware.com>
- Jamie Maillart <jmaillart@vmware.com>
- Randy Carson <rcarson@vmware.com>
- Roberto Canton <rcanton@vmware.com>

# Workshop Details

## Windows RDP Access

At the start of the workshop, your instructor will assign a Student Number for you, based on that number, please login via your preferred Remote Access Client to the appropriate desktop based on the instructor's slide.

### Location of Files

Any file(s) referenced in this manual are located in the Z: drive of the RDP desktop you are assigned

### Workshop URL's

VMware Cloud on AWS Login <https://vmc.vmware.com>

vRealize Automation <https://vraapp.corp.local/vcac>

vRealize Operations <https://vrops.corp.local>

Swagger API Interface <https://vmc.vmware.com/swagger/index.html>

AWS Console <https://vmcworkshop.signin.aws.amazon.com/console>

### Workshop Bookmarks

Import Bookmarks into Chrome by double clicking the Chrome Icon on your Desktop (Preferred browser for workshop)

1. Click on the three dots on the top right corner
2. Select **Bookmarks**
3. Select **Import bookmarks and settings**
4. Click on the down arrow
5. Select **Bookmarks HTML File**
6. Click **Choose File**
7. Click on **Desktop**
8. Select **bookmarks_vmc_workshopbookmarks_vmc_workshop**
9. Click **Open**

# Working with your SDDC

## Add a Host to your SDDC

1. On your "Student Workshop #" SDDC, click on the "View Details" button.
2. Click on the "ActionsActions" button
3. Click on "Add HostsAdd Hosts"
4. As you will only be adding only one host, review the field highlighted
5. Click the "Add HostsAdd Hosts" button

Congratulations! You have completed this step. The adding of an additional host to an existing SDDC should take approximately 10 minutes to complete.

## Configuring SDDC Firewall Rules

### Management Gateway Firewall Rules

By default, the firewall for the management gateway is set to deny all inbound and outbound traffic. In this exercise, you will add a firewall rule to allow vCenter traffic. In order to access vCenter Server in your SDDC, you must set a firewall rule to allow traffic to the vCenter Server.

1. Under **Management Gateway** click the arrow to expand **Firewall Rules**
2. Click **Add Rule**
3. Enter a name for your rule under **Rule Name**
4. Type **Any** for Source
5. Make sure **vCenter** is selected as Destination
6. Select **HTTPS (TCP 443)** from the drop down box for Service
7. Click the **SAVE** button, your rule should look like the below image

### Compute Gateway Firewall Rules

By default, the Compute NSX Edge Services Gateway is also set to deny all inbound and outbound traffic. You need to add additional firewall rules to allow traffic as needed.

#### Create Firewall Rule under Compute Gateway for Inbound Native AWS Services access.

1. Under **Network** tab, navigate to **Compute Gateway**
2. Expand **Firewall Rules**
3. Click **ADD RULE**

## AWS Inbound Firewall Rule

### Create AWS Inbound Firewall Rule

1. **Name** - AWS Inbound
2. **Action** - Allow
3. **Source** - All connected Amazon VPC
4. **Destination** - 192.168.#.0/24 (Where # is your student number)
5. **Service** - ANY
6. Click **SAVE** button.

### AWS Outbound Firewall Rule

Follow the same process as in the previous step and create AWS Outbound Firewall Rule following these instructions:

1. **Name** - AWS Outbound
2. **Action** - Allow
3. **Source** - 192.168.#.0/24 (Where # is your student number)
4. **Destination** - All connected Amazon VPC
5. **Service** - ANY
6. Click **SAVE** button.

#### Log into VMware Cloud on AWS vCenter

Connection Info Tab

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

Click on the vSphere Client's HTML5 URL, and login with **cloudadmin@vmc.local** User Name and copy the password to your computer's clipboard and paste it in the Password Field.

You are now logged in to your VMware Cloud on AWS vCenter Server as **cloudadmin@vmc.local** user.

### Create Content Library

Content libraries are container objects for VM templates, vApp templates, and other types of files like ISO images. You can create a content library in the vSphere Web Client, and populate it with templates, which you can use to deploy virtual machines or vApps in your VMware Cloud on AWS environment or if you already have a Content Library in your on-premises data center, you can use the Content Library to import content into your SDDC. You can create two types of libraries: local or subscribed libraries.

Local Libraries You use a local library to store items in a single vCenter Server instance. You can publish the local library so that users from other vCenter Server systems can subscribe to it. When you publish a content library externally, you can configure a password for authentication. VM templates and vApp templates are stored as an OVF file format in the content library. You can also upload other file types, such as ISO images, text files, and so on, in a content library.

### Subscribed Libraries

You subscribe to a published library by creating a subscribed library. You can create the subscribed library in the same vCenter Server instance where the published library is, or in a different vCenter Server system. In the Create Library wizard you have the option to download all the contents of the published library immediately after the subscribed library is created, or to download only metadata for the items from the published library and later to download the full content of only the items you intend to use. To ensure the contents of a subscribed library are up-to-date, the subscribed library automatically synchronizes to the source published library on regular intervals. You can also manually synchronize subscribed libraries. You can use the option to download content from the source published library immediately or only when needed to manage your storage space.

Synchronization of a subscribed library that is set with the option to download all the contents of the published library immediately, synchronizes both the item metadata and the item contents. During the synchronisation the library items that are new for the subscribed library are fully downloaded to the storage location of the subscribed library. Synchronization of a subscribed library that is set with the option to download contents only when needed synchronizes only the metadata for the library items from the published library, and does not download the contents of the items. This saves storage space. If you need to use a library item you need to synchronize that item. After you are done using the item, you can delete the item contents to free space on the storage. For subscribed libraries that are set with the option to download contents only when needed, synchronizing the subscribed library downloads only the metadata of all the items in the source published library, while synchronizing a library item downloads the full content of that item to your storage. If you use a subscribed library, you can only utilize the content, but cannot contribute with content. Only the administrator of the published library can manage the templates and files.

1. Click on **Menu**
2. Click on **Content Libraries**

### Subscribe to an existing Content Library

You may already have a Content Library in your on-premises data center, you can use the Content Library to import content into your SDDC.

1. In your Content Library window, click the **+** sign to add a new Content Library.
2. Name your Content Library **Student#** where **#** is the number assigned to you
3. (Optional) Enter some notes for your Content Library
4. Click **Next** button
5. Select **Subscribed content library**
6. Under **Subscription URL** enter the following: <https://vcenter.sddc-34-216-241-49.vmc.vmware.com/cls/vcsp/lib/8d658764-2e89-44ff-https://vcenter.sddc-34-216-241-49.vmc.vmware.com/cls/vcsp/lib/8d658764-2e89-44ff-a7c1-ee777f0dfc8f/lib.jsona7c1-ee777f0dfc8f/lib.json>

    PLEASE NOTE THAT THERE MAY BE AN ISSUE WITH DROPPING/ADDITION OF CHARACTERS FOR THE URL WHEN COPYING AND PASTING FROM THE MANUAL. THE ACTUAL URL IS ALSO AVAILABLE IN YOUR STUDENT DESKTOP ON THE Z:\ DRIVE IN A TEXT FILE, OPEN THIS TEXT FILE AND COPY THE URL FROM THERE. ASK YOUR INSTRUCTOR IN THE EVENT YOU CANNOT LOCATE IT.
7. Click the checkbox for **Enable Authentication**
8. For **Password** enter: VMware1!VMware1!
9. Ensure Download content is set to **Immediately**
10. Click **Next**
11. Highlight the **WorkloadDatastore** as the storage location
12. Click **Next**
13. Click **Finish**. Your content library should take about ~20 minutes to complete syncing.

### Create a Local Content Library

1. Click the **+** sign to create a new Content Library
2. Name your new Content Library: **LocalContentLibrary#** (where # is your student #)
3. (Optional) Enter some notes about your Content Library
4. Click **Next** button
5. Make sure **Local content library** is selected
6. Click **Next**
7. Highlight the **WorkloadDatastore** as the storage location
8. Click **Next**
9. Review your information and click **Finish**

Congratulations, you have created your Local Content Library.

### Create Logical Network

1. Once you are logged in to your vCenter Server Click on **Menu**
2. Select **Global Inventory Lists** from the drop down menu
3. Click on **Logical Networks** in the left pane
4. Click on the **+ Add** button
5. Name your New Logical Network **Student#-LN** (where # is your student number)
6. Make sure to select **Routed Network**
7. For CIDR Block enter **192.168.###.0/24** (where # is your student #)
    If your designated student number is between 1 and 9, your CIDR block should look like this: **192.168.1.0/24** - This example represents student number 1. For students 10 thru 20 it should look like this: **192.168.10.0/24** - This example represents student number 10
8. Enter **192.168.###.1** for the Default Gateway IP - Example: 192.168.1.1
9. Make sure DHCP is Enabled by clicking on the **checkbox**
10. Enter **192.168.###.100-192.168.###.200** for IP Range
11. Type **corp.local** as your DNS Domain Name
12. Click **OK** to create your new logical network

### Create Linux Customization Spec

When you clone a virtual machine or deploy a virtual machine from a template, you can customize the guest operating system of the virtual machine to change properties such as the computer name, network settings, and license settings. Customizing guest operating systems can help prevent conflicts that can result if virtual machines with identical settings are deployed, such as conflicts due to duplicate computer names. You can specify the customization settings by launching the Guest Customization wizard during the cloning or deployment process. Alternatively, you can create customization specifications, which are customization settings stored in the vCenter Server database. During the cloning or deployment process, you can select a customization specification to apply to the new virtual machine. Use the Customization Specification Manager to manage customization specifications you create with the Guest Customization wizard.

1. Click **Menu**
2. Click **Policies and Profiles**
3. Click on **+ New** to add a new Linux Customization Specification
4. Give your VM Customization Spec a Name
5. Enter a description for it (Optional)
6. Make sure to select **Linux**
7. Click **Next**
8. Click on the **Enter a name** button
9. Enter a name for your linux VMs
10. Click on the **Append a numeric value** checkbox
11. Enter **corp.local** for the Domain Name
12. Click the **Next** button
13. Select **US** for Area
14. Select **Eastern** for Location
15. Select **Local time**
16. Click **Next**
17. Leave the defaults on the **Network** screen and click **Next**
18. Under Primary DNS Server enter **10.46.159.10**
19. Type **corp.local** for DNS Search Paths
20. Click **Next**
21. Click **Finish**

Congratulations! You have successfully created your VM Customization Spec for your Linux VMs. You can also export (Duplicate), Edit, Import, and Export a VM Customization Spec.

### Deploy a Virtual Machine

1. On your Content Libraries **(Menu -> Content Libraries)**, select **Student#** and select the **Templates** tab.
2. Right click on the **centos01-web** template and select **New VM from This Template**
3. Name your Virtual Machine **StudentVM#** (where # is your student number)
4. Expand the location area until you see **Workloads** and highlight it
5. Click **Next**
6. Expand the destination compute resources until you find **Compute-ResourcePool**, select it
7. Click **Next** button
8. Click **Next** button on the Review details screen
9. In the **Select storage** step, highlight **WorkloadDatastore**
10. Click **Next**
11. In the **Select networks** step, click the drop down box to select the Destination Network (you may need to click Browse to see other networks and select your "Student#-LNStudent#-LN" network you created previously
12. Click **Next**
13. In the **Ready to complete** section, review to ensure all your selections are correct and click **Finish**

### Convert Virtual Machine to Template

In this step you will be cloning your newly created Virtual Machine into a Template for later use in vRealize Automation section.

1. Ensure your VM deployment completed from your previous step
2. Click on **Menu**
3. Select **VMs and Templates**
4. Select your newly created VM **Student#** (where # is your student number)
5. Click on **Template**
6. Select **Convert to Template**
7. Click **Yes**  in the Convert to Template prompt

You have completed this step.

## APIs

### Using PowerShell

Open the PowerShell CLI windows

1. Click on **Start**, and scroll down until you see the Windows PowerShell menu
2. Right click on the **PowerShell CLI** shortcut icon and select **Run as Administrator**
3. Click **Yes**

Install the VMware PowerCLI module if not loaded

``` powershell
install-Module VMware.PowerCLI
```

Note: You can use the tab complete feature to complete the command. ie...type install-mod and then press tab. There may be a slight delay the first time but the command **install-module** will complete. Note: You will be asked to install the NuGet provider, take the default or press Y and press enter, you will then be asked to trust an untrusted repository, DO NOT take the default but type **Y** and press Enter.

We now need to set the execution policy to Remote Signed.

``` powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned
```

Note: You will be asked to change the execution policy, type **Y** and press Enter

You now will need to set the PowerCLI Configuration to Ignore Invalid Certificates.

IMPORTANT STEP:

``` powershell
Set-PowerCLIConfiguration -InvalidCertificateAction Ignore
```

NOTE: Be sure the "i" in "Ignore" is capitalized

NOTE: You will be asked to Update PowerCLI Configuration, type Y and press Enter

We now need to install the VMware CLI commands

``` powershell
Install-Module -name VMware.VMC -scope AllUsers
```

Note: You will be asked to trust an untrusted repository, type Y and press Enter

Let's take a quick look at the VMware CLI commands.

``` powershell
Get-VMCCommand
```

We now need to get your Refresh Token from the VMC console. Switch back to or open the web browser and log into **vmc.vmware.com**

If you are not already logged in

1. open a new tab
2. click on the **VMware Cloud on AWS** bookmark shortcut
3. Fill in your email address
4. click on **Next**
5. Click on the drop down next to your Name/Org ID
6. Click on **OAuth Refresh Token**

Now we create a refresh token for your ID tied to this Org

1. Click on **Create a new token**
2. Click on **Copy to Clipboard**

Now let's attach to the VMC server

``` powershell
connect-vmc -refreshtoken "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
```

NOTE:Within the PowerShell window you can just right-click to paste the code, quotes are
optional

NOTE: Paste the refresh token you copied earlier in the exercise.

Now we can see what Orgs we have access to

``` powershell
Get-VMCorg
```

1. Note the Org Display_Name and ID

Now that we know the Org Display_Name we can find out information about the SDDC's inside our org.

```powershell
Get-VMCSDDC -Org VMC-WS#
```

NOTE:replace # with your workstation number

Another cool thing you can do is see the Default Credentials for your SDDC

```powershell
Get-VMCSDDCDefaultCredential -org VMC-WS#
```

NOTE:replace # with your workstation number

### REST API with Swagger

To start using the REST API commands we need to log into the Swagger interface that is build into **vmc.vmware.com**

If it is not already open, Double-click on the Chrome browser icon on the desktop.

If you are not already logged into your Org,

1. Open a new tab
2. Click on the VMware Cloud on AWS bookmark
3. Log into your Org

To get to the Swagger interface

1. Open a new tab
2. Click on the **Swagger** bookmark

The first thing we need to do is get "Authorized" to run these REST APIs

1. Click on the **refresh_token** link
2. Click on the **Generate** button to get your Access Key
3. Copy the **Access Key**
4. Close this tab
5. Click on the **Authorize** button on the Swagger tab
6. Paste your **Access Key** into the Value box.
7. Click on **Authorize**
8. Click on orgs: to show the REST API calls for the Organizations
9. Click on **GET /orgs**
10. Click on the **Try it out** button This section shows you how to run this REST API. The cool thing is they will give you examples when you when you click on Try It, you see:
- Response Code: 200 is the code that means success, other response codes are listed under Response Messages.
- Response Header: This gives you some metadate about the response
- Response Body: This is the details that you are looking for. Look through this and find the Display_NameDisplay_NameandIDIDfor your Org.
- If you wanted to run the command at a linux prompt using the "curl" command, you can cut and paste this into your linux terminal

NOTE: Copy the ID without the quotes.

Now scroll down to the SDDC: Operations on SDDC section

1. Click on the **SDDC**: to show the REST API calls for the SDDCs
2. Click on the **GET /org/{orgs}/sddcs**
3. Paste the Org ID you copied from the previous step in the org : Value box under the **Parameters** section
4. Click on the **Try it out** button

Note the Response Code of 200.

Look through the Response Body for details about the SDDC(s) in your organization.

### AWS Service Integration

AWS Relational Database Service (RDS)

Integration

#### Deploy Photo VM

1. If not already opened, open your VMware Cloud on AWS vCenter and click on the **Menu** drop down
2. Select **Content Libraries**
3. Click on your previously created Content Library named **Student#** (where # is your student number)
4. If not already there, make sure you click on the **Template** tab
5. Right-click on the **photo app** Template
6. Select **New VM from This Template**
7. Name the virtual machine **PhotoApp#** (where # is your student #)
8. Expand the location and select **Workloads**
9. Click **Next**
10. Expand the destination to select **Compute-ResourcePool** as the compute resource
11. Click **Next**
12. In the **Review details** step click **Next**
13. In the **Select storage** step, highlight the **WorkloadDatastore**
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
24. Make sure your VM is assigned an IP addresses (may need to wait a minute or 2). Make a note of this IP address for a future step.

### Firewall Rules for RDS Integration

1. Go back your VMware Cloud on AWS portal and click on the **Network** tab in order to request a **Public IP address**
2. Under the **Compute Gateway** click and expand **Public IPs**
3. Click on **REQUEST PUBLIC IP**
4. (Optional) Enter Notes for this public IP
5. Click on **Request**
6. You should see a similar notification as the one above
7. Take note of your newly acquired Public IP address
8. Next you will create a **NAT rule** from the newly acquired Public IP address you noted in your last step to the internal IP address of the VM you created. Click on **NAT** to expand
9. Click **ADD NAT RULE**
10. Give your **NAT rule** a name
11. Your new Public IP address should be pre-filled for you, if not, enter it
12. Under **Service** select **Any (All Traffic)**
13. Type your VM's internal IP address
14. Click the **SAVE** button
15. You should get a **NAT rule successfully created** notification
16. Expand **Firewall Rules**
17. Click **ADD RULE**
18. Give your Firewall Rule a name
19. Select **All Internet and VPN** for **Source**
20. Type the Public IP Address you noted under **Destination**
21. Select **Any (All Traffic)** for **Service**
22. Click **SAVE** button
23. You should get a **Firewall rule successfully created** notification

### AWS Relational Database Service (RDS) Integration

On your browser, open a new tab and go to: <https://vmcworkshop.signin.aws.amazon.com/console>

1. Account ID or alias -vmcworkshop
2. IAM user name -Student# (where # is the number assigned to you)
3. Password -VMCworkshop1211
4. Click **Sign In**
5. You are now signed in to the AWS console. Make sure the region selected is **Oregon**
6. Click on the **RDS** service
7. In the left pane click on **Instances**
8. Click on the RDS instance that corresponds to your Student number
9. Scroll down to the **Details** area and under **Security and network** notice that the RDS instance is not publicly accessible, meaning this instance can only be accessed from within AWS
10. Go back to the main Services page in the AWS console by clicking the **Services** link
11. Scroll down to **Networking & Content Delivery** and click **VPC**
12. Click on **Security Groups** in the left pane
13. Choose the r**ds-launch-wizard-#** RDS Security group corresponding to your student number
14. After highlighting the appropriate security group click on the **Inbound Rules** tab below VMware Cloud on AWS establishes routing in the default VPC Security Group, only RDS can leverage this or create its own
15. Notice that the CIDR block range of your Student#-LN Logical Network you created in VMware Cloud on AWS is authorized for MySQL on port 3306. This was done for you ahead of time

    AWS Relational Database Service (RDS), also creates its own Elastic Network Interface (ENI) for access which is separate from the ENI created by VMware Cloud on AWS.
16. Click on **Services** to go back to the Main Console
17. Click on **EC2**
18. In the EC2 Dashboard click **Network Interfaces** in the left pane
19. All Student environments belong to the same AWS account, therefore, hundreds of ENI's may exist. In order to minimize the view, type "RDS" in the search area and press **Enter** to add a filter
20. Highlight your **rds-launch-wizard-#** corresponding to your student number
21. Make note of the **Primary private IPv4** IP address for the next step
22. Open an additional browser tab and type your public IP address you requested in the VMware Cloud on AWS portal in the browser address bar followed by /Lychee (case sensitive) ie: x.x.x.x/Lychee
23. Enter the database connection information below (case sensitive), using the IP address you noted in the previous step from the RDS ENI:
    **Database Host**: x.x.x.x:3306
    **Database Username**: student#
    **Database Password**:VMware1!
24. Click **Connect**

### AWS Elastic File System (EFS) Integration

### EFS VM Creation

1. Navigate to your Content Library, click **Menu** on your VMware Cloud on AWS vCenter Server
2. Select **Content Libraries**
3. Click on your **Student#** Content Library
4. Make sure the **Templates** tab is selected
5. Right-click on the **efs** template
6. Select **New VM from This Template**
7. Name your VM **EFSVM#** (where # is your student number)
8. Select **Workloads** for the location of your VM
9. Click **Next**
10. Select **Compute-ResourcePool** as the destination for your VM
11. Click **Next**
12. Click **Next**
13. Select "WorkloadDatastoreWorkloadDatastore" for storage
14. Click **Next**
15. Select your Destination Network
16. Click **Next**
17. Click **Finish**
18. Make sure to Power on your VM and ensure it is assigned an IP address

### AWS Elastic File System (EFS)

On your browser, open a new tab and go to: <https://vmcworkshop.signin.aws.amazon.com/console>

1. Account ID or alias - vmcworkshop
2. IAM user name -Student# (where # is the number assigned to you)
3. Password - VMCworkshop1211
4. Click **Sign In**
5. You are now signed in to the AWS console. Make sure the region selected is **Oregon**
6. Click on the **EFS** service
7. Select your Student # NFS
8. Note the IP address
9. Back on your vCenter Server tab, click on **Launch Web Console**  for your EFS VM (Might need to allow pop ups in browser). Log in using the following credentials:
 a. **User**: root
 b. **Password**: VMware1!VMware1!

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

## Site Recovery

### Activate Site Recovery Add On

Important Instructions for Site Recovery ExercisesImportant Instructions for Site Recovery Exercises

PLEASE BE AWARE THAT THESE EXERCISES MUST BE PERFORMED FROM THE ASSIGNED RDP DESKTOP YOUR INSTRUCTORS ASSIGNED. IF YOU TRY TO PERFORM SOME OF THE EXERCISES OUTSIDE OF THE RDP SESSION YOU WILL EXPERIENCE SOME FAILURES.

1. In your SDDC, click on the **Add Ons** tab
2. Click the **Activate** button
3. Click the **Activate** button

Wait until the Site Recovery Add On has been activated.

What is VMware Site Recovery?

VMware Site Recovery brings VMware enterprise-class Software-Defined Data Center (SDDC) Disaster Recovery as a Service to the AWS Cloud. It enables customers to protect and recover applications without the requirement for a dedicated secondary site. It is delivered, sold, supported, maintained and managed by VMware as an on-demand service. IT teams manage their cloud-based resources with familiar VMware tools without the difficulties of learning new skills or utilizing new tools. VMware Site Recovery is an add-on feature to VMware Cloud on AWS, powered by VMware Cloud Foundation, VMware Cloud on AWS integrates VMware's flagship compute, storage, and network virtualization products: VMware vSphere, VMware vSAN, and VMware NSX along with VMware vCenter Server management. It optimizes them to run on elastic, bare-metal AWS infrastructure. With the same architecture and operational experience on-premises and in the cloud, IT teams can now get instant business value via the AWS and VMware hybrid cloud experience. The VMware Cloud on AWS solution enables customers to have the flexibility to treat their private cloud and public cloud as equal partners and to easily transfer workloads between them, for example, to move applications from DevTest to production or burst capacity. Users can leverage the global AWS footprint while getting the benefits of elastically scalable SDDC clusters, a single bill from VMware for its tightly integrated software plus AWS infrastructure, and on-demand or subscription services like VMware Site Recovery Service. VMware Site Recovery extends VMware Cloud on AWS to provide a managed disaster recovery, disaster avoidance and non-disruptive testing capabilities to VMware customers without the need for a secondary site, or complex configuration.

VMware Site Recovery works in conjunction with VMware Site Recovery Manager 8.0 and VMware vSphere Replication 8.0 to automate the process of recovering, testing, re-protecting, and failing-back virtual machine workloads. VMware Site Recovery utilizes VMware Site Recovery Manager servers to coordinate the operations of the VMware SDDC. This is so that as virtual machines at the protected site are shut down, copies of these virtual machines at the recovery site startup. By using the data replicated from the protected site these virtual machines assume responsibility for providing the same services.

VMware Site Recovery can be used between a customers datacenter and an SDDC deployed on VMware Cloud on AWS or it can be used between two SDDCs deployed to different AWS availability zones or regions. The second option allows VMware Site Recovery to provide a fully VMware managed and maintained Disaster Recovery solution. Migration of protected inventory and services from one site to the other is controlled by a recovery plan that specifies the order in which virtual machines are shut down and started up, the resource pools to which they are allocated, and the networks they can access. VMware Site Recovery enables the testing of recovery plans, using a temporary copy of the replicated data, and isolated networks in a way that does not disrupt ongoing operations at either site. Multiple recovery plans can be configured to migrate individual applications or entire sites providing finer control over what virtual machines are failed over and failed back. This also enables flexible testing schedules. VMware Site Recovery extends the feature set of the virtual infrastructure platform to provide for rapid business continuity through partial or complete site failures.

### Create a Cross SDDC VPN

We will be setting up a IPSEC VPN connection between your VPC and the VPC of the person you were paired with.

1. Go back to the **VMware Cloud on AWS** tab.
2. In the main SDDC windows, click on **View Details**
3. Then click on the **Network** menu In the Management Gateway box, make a note of the Public IP and the Infrastructure Subnet CIDR, Scroll down a little to get to the Management Gateway setting
4. Click the drop down arrow to open the **IPsec VPNs** section
5. Click on **ADD VPN**

Fill in the following information
6. Student ## MGMT GW : Put the student number of the person you were paired with.
7. The **Public IP address** of the person you were paired with
8. The **Infrastructure IP CIDR** of the person you were paired with
9. Pre-shared key is **VMware1!**
10. Click on **Save**.

When both you and the person you were paired with have completed these steps you should see the status turn to Connected

There will be a need to setup a second VPN to our Host infrastructure for this setup to work. This is not normally needed when setting up your on-premises environment but it's needed for the special setup done for this workshop.

1. Make sure the drop down is opened, if not click it under **Management Gateway**
2. Click on **Add VPN**

    Fill in the following information
3. Name this VPN **Student# to Host** (where # is your student number)
4. Enter **54.70.191.234** for the Remote Gateway Public IP
5. Enter **192.168.30.0/24** under Remote Networks
6. Pre-shared key is **VMware1!**
7. Click on **Save**.

#### Prepare and Pair Site Recovery

Firewall Rules for Site Recovery

If you still have your OAuth Token from the Powershell module, you are free to use it, otherwise, follow the following instructions to obtain a new one.

1. Click on the drop down next to your **Name/Org ID**
2. Click on **OAuth Refresh Token**

Now we create a refresh token for your ID tied to this Org

1. Click on **Create a new token**
2. Click on **Continue**
3. Click on **Copy to Clipboard**

Now let's attach to the VMC server

``` powershell
connect-vmc -refreshtoken "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"connect-vmc -refreshtoken "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
```

NOTE: Paste the refresh token you copied earlier in the exercise.

#### Import Firewall Rules for Site Recovery via PowerCLI

Open a Powershell window and enter the following commands:

``` powershell
Set-Location \\vmcwindc01\documents\import-dr-fw-rules.ps1 -refreshToken "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx" -orgName "VMC-WS#" -sddcName "Student Workshop #"
```

Where xxxx is the OAuth Token you generated in a previous step and # is your Student number.

Ensure the appropriate firewall rules were created by:

1. Click on the **Network** tab.
2. Expand **Firewall Rules** under **Management Gateway**

    Confirm the following Firewall Rules have been created:
3. SRM 9086
4. VR Replication
5. VR 8043
6. VR 443
7. VR Out
8. SRM Out

Please note that the rules may have been created in a different order than shown above.

### VMware Site Recovery - Site Pairing

**IMPORTANT NOTE**: Only one person can do the Site Pairing exercise. Please decide between you and your partner who performs this step.

1. On your VMware Cloud on AWS Portal click on the **Add Ons** tab
2. Click **Open Site Recovery**
3. Click on **New Site Pair** You will be pairing the partner site that was assigned to you by your instructor, note that this is not the information for your SDDC used up until now. This is the information your partner will need from you and you will need from your partner's site.
4. Click on the **Connection Info** tab
5. Copy or note the URL for the vCenter Server, drop the /ui at the end
6. Note the username
7. Note the password
8. Make sure your local vCenter is selected
9. Enter the information from your partner's SDDC:
    PSC host name
    User name
    Password
10. Make sure local vCenter server is selected
11. Select **all Services**
12. Click **Next**
13. Click **Finish** button

### Configure Network Mappings

1. Click **Network Mappings** in the left pane of the Site Recovery page
2. Click **+ New**
3. Select **Prepare mappings manually**
4. Click **Next**
5. Expand **SDDC Datacenter** on both sides
6. Expand **Management Networks** on both sides
7. Expand **vmc-dvs** on both sides
8. Select your **Student#-LN** network and your partner's **Student#-LN** (You may need to scroll down to find these networks)
9. Click the **Add Mappings** button
10. Click **Next**
11. DO NOT enter or select anything in Reverse Mappings, click **Next**
12. Leave defaults and click **Next**
13. Click **Finish**

### Folder mappings

1. Select **Folder Mappings** in the left pane
2. Click **+ New** to create a new folder mapping
3. Select **Prepare mappings manually**
4. Click **Next**
5. Expand **SDDC Datacenter** on both sides
6. Select **Workloads** on both sides
7. Click the **Add Mappings** button
8. Click **Next**
9. DO NOT select any Reverse mappings, click **Next**
10. Click **Finish**

### Resource Mappings

1. Click **Resource Mappings** in the left pane
2. Click **+ New**
3. Expand **SDDC Datacenter** on both sides
4. Expand **Cluster 1** on both sides
5. Select **Compute-ResourcePool** on both sides
6. Click **Add Mappings** button
7. Click **Next**
8. DO NOT select any reverse mappings, click **Next**
9. Click **Finish**

### Placeholder Datastores

1. Select **Placeholder Datastores** in the left pane
2. Click **+ New**
3. Select **WorkloadDatastore**
4. Click **Add**

### SRM - Protect a VM

1. Select a VM to replicate and right-click
2. Select **All Site Recovery actions**
3. Click **Configure Replication**

    NOTE: You may need to log in to the paired site (This is your partner's site), make sure you use **cloudadmin@vmc.local** and get your partner users password. After entering you may need to repeat this step.
4. Click **Next**
5. Select the Target Site
6. If not logged in you may need to login (Remember this is your partner's site not yours)
7. Enter your partners site credentials
8. Leave all defaults and click **Next**
9. Leave the default **Datastore Default** as the VM Storage Policy
10. Select **WorkloadDatastore**
11. Click **Next**
12. Leave the default 1 hour for Recovery Point Objective, RPO can be as low as 5 minutes, as high as 24 hour
13. Click **Next**
14. Select **Add to new protection group**
15. Name your Protection Group **PG#** (where # is your student number)
16. Click **Next**
17. Select **Add to new recovery plan**
18. Name your Recovery Plan **RP#** (where # is your student number)
19. Click **Next** button
20. Click **Finish**

### Perform a Recovery Test

1. In the VMware Cloud on AWS portal, click the **Add Ons** tab
2. Click on **Open Site Recovery** (You may need to allow Pop-ups in browser)
3. In the Site Recovery window, click **Open**
4. Click on **Recovery Plans**
5. Click on your protection group **PG#** (where # is your student number)
6. Click on **Recovery Plans**
7. Click on **RP#** which should be your Recovery Plan you created in a previous step
8. Click the **Test** button
9. Leave all defaults and click **Next** button
10. Click **Finish** button
11. Follow the progress in the Recent Tasks area at the bottom of your window
12. Notice the test has successfully completed
13. Click the **Cleanup** button to clean up the activity and return the environment to its normal state
14. Click **Next**
15. Click **Finish**, the environment will now be clean. Please note that during testing, your replications protecting your VM's is not interrupted

## Hybrid Cloud Extension (HCX)

What is Hybrid Cloud Extension (HCX)What is Hybrid Cloud Extension (HCX)

Hybrid Cloud Extension abstracts on-premises and cloud resources and presents them to the apps as one continuous hybrid cloud. On this, Hybrid Cloud Extension provides high-performance, secure and optimized multisite interconnects. The abstraction and interconnects create infrastructure hybridity. Over this hybridity, Hybrid Cloud Extension facilitates secure and seamless app mobility and disaster recovery across on-premises vSphere platforms and VMware Clouds. Hybrid Cloud Extension is a multi-site, multi cloud service, facilitating true hybrid cloud.

### Hybrid Cloud Extension Features

Any-to-Any vSphere Cloud App Mobility

Eliminate the need for cloud readiness and app dependency assessment

- Rapidly move existing workloads from a vSphere platform to the latest SDDC
- Reduce upfront planning time for cost and resource analysis
- Accelerate cloud adoption and avoid retrofitting on-premises environment Business Continuity with Lower TCOBusiness Continuity with Lower TCO
- IP and MAC address remapping is not required
- No need to retrofit existing VM environment
- Hybrid Cloud Extension provides warm and cold bulk migration, and bidirectional migration
- Hybrid Cloud Extension simplifies your operational model Architected for SecurityArchitected for Security
- Ensure highly secure tethering of private and public clouds
- Protect resources with resilient disaster recovery capabilities
- Hybrid Cloud Extension hybrid DMZ enables portability of enterprise network and security practices to the cloud
- Security policies migrate with applications High-Performance Infrastructure HybridityHigh-Performance Infrastructure Hybridity
- In-built WAN optimization is tuned for the needs of hybrid use cases
- Hybrid Cloud Extension provides agile, intelligent routing
- Traffic load balancing overlay is policy-enforced
- Multiple VM migration models (including vMotion, warm, cold) make it easy to migrate to and from the cloud without any changes

### HCX - vMotion Migration

1. Open your Chrome browser and click on the **HCX-vMotion** bookmark
2. Click the **X** on the right pane to enlarge the main screen The first tab in the browser demonstrate an on-premises vCenter server.

3. Click on the second tab, this represents a second data center (This can also represent a VMware Cloud on AWS vCenter)
4. Click the first tab in the browser to go back to the on-premises vCenter.
5. Click on the VM named **Mission Critical Workload 1**
6. Click on the **Console screen**

    A console window is now open for the **Mission Critical Workload 1** VM, it will try to ping IP Address 10.159.137.212 which corresponds to a VM in the second site.
7. Click in the tab corresponding to the second site
8. Click on the VM named **TargetSite-TestVM**
9. Note the IP address corresponding to this VM is the IP address that the **Mission Critical Workload 1** VM on the source site is trying to ping
10. Click on the **Mission Critical Workload 1** tab
    - Press enter after the ping command
    - Click **Control-C** to stop ping
    - Type ping 172.16..4.2 which is this VM's own IP address
11. Click the **X** on this tab to close this tab on your browser
12. Click on the first tab to go back to the on-premises vCenter
13. Click on the **Actions** button
14. Click on **Hybridity Actions**
15. Click on **Migrate to the Cloud**
16. Click on **(Specify Destination Container)**
17. Select **RedwoodCluster**
18. Click on **Select Destination** button
19. Click on **(Select Storage)** button
20. Select **cloudStorage**
21. Click **(Select Virtual Disk Format)**
22. Select **Same format as source**
23. Click on **(Select Migration Type)**
24. Select **vMotion**
25. Click **Next**
26. Look for the **Validation is Successful** message
27. Click **Finish** button
28. Click on the **Home** button
29. Click **HCX** in the left pane
30. Click the **Migration** tab
31. Notice the progress of the vMotion migration
32. Click on the **Refresh** tab to update the progress
33. Once progress has completed, click on the second tab to open the target site's vCenter
34. You now see that **Mission Critical Workload 1** has successfully migrated to the Target Site, click on its name
35. Click on the Console window
    - You can see that the ping you left running in a previous step never stopped, successfully vMotioning the VM with zero downtime
    - Press **Control-C** to stop the ping
36. Click on the **X** on the browser tab to close the console tab

### Reverse Migration

1. Click on the first tab in the browser
2. Click the **Migrate Virtual Machines** button
3. Click the **Reverse Migration** checkbox
4. Click the checkbox for **Mission Critical Workload 1**
5. Click on **(Specify Destination Container)**
6. Select **Tier 0 Workloads**
7. Click **Select Destination** button
8. Click on **(Select Storage)**
9. Select **onpremStorage**
10. Click **(Select Virtual Disk Format)**
11. Select **Same format as source**
12. Click on **(Select Migration Type)**
13. Select **vMotion**
14. Click **Next**
15. Once **Validation is Successful** message appears
16. Click **Finish** button
17. Check the progress of the migration
18. Click **Refresh** button
19. Click the **Home** button
20. Click **Hosts and Clusters** button
21. Click on the **Mission Critical Workload 1** VM, you can see that the Reverse migration to the on-premises vCenter was successful

### HCX - Bulk Migration

1. In your Chrome browser click the **HCX-Bulk** bookmark
2. Click on the **X** to enlarge the main screen
3. As with the vMotion module, in this example we also have a source site (on-premises) vCenter
4. Click on the second tab in the browser to view the Target site vCenter
5. Click on the tab for the on-premises vCenter
6. Click on the **Home** button
7. Click **HCX**
8. Click on the **Migration** tab
9. Click the **Migrate Virtual Machines** button
10. Select **Tier 1 Workloads** in the left pane
11. Click the **checkbox** to select all VM's
12. Click **(Specify Destination Container)**
13. Select **RedwoodCluster** as the Destination Container
14. Click the **Select Destination** button
15. Click **(Select Storage)**
16. Select **cloudStorage**
17. Click **(Select Virtual Disk Format)**
18. Select **Same format as source**
19. Click **(Select Migration Type)**
20. Select **Bulk Migration**
21. Click on **HelpDesk Workload 1** to see the options for this workload
22. Click on **HelpDesk Workload 2** to see the options for this workload
23. Click on the **sidebar** to view all the options
24. Click the **Next** button
25. Wait for **Validation is Successful** message
26. Click **Finish** button
27. Check Progress of the migrations
28. Click **Refresh** button
29. Once migrations complete, click the **Hosts and Clusters** button
30. Click on the second tab to view the cloud vCenter
31. You can see that both workloads have successfully migrated to the Target vCenter

## vRealize Automation

### What is vRealize Automation (vRA)

VMware vRealize Automation is the IT Automation tool of the modern Software-Define Data Center. vRealize Automation enables IT Automation through the creation and management of personalized infrastructure, application and custom IT services (XaaS). This IT Automation lets you deploy IT services rapidly across a multi-vendor, multi-cloud infrastructure.

- Agility Through IT Automation
- Accelerate the end-to-end delivery and management of infrastructure and applications.
- Choice Through Flexibility
- Provision and manage multi-vendor, multi-cloud infrastructure and applications by leveraging new and existing infrastructure, tools and processes.
- Control Through Governance PoliciesControl Through Governance Policies
- Ensure that users receive the right size resources, or applications, at the appropriate service level for the jobs they need to perform.
- Cost Saving Through EfficiencyCost Saving Through Efficiency Reduce operational cost by replacing time-consuming, manual processes and gain additional cost savings through automated reclamation of inactive resources.

vRealize Automation supports VMware Cloud on AWS in delivering a unified hybrid cloud management experience.

### Login to vRealize Automation

1. On your Student desktop, click on the Chrome browser.
2. On your bookmarks bar in Chrome click on the **VMware vRealize Automation Appliance** link
3. Click on the "Proceed to vraapp.corp.local (unsafe) link
4. Click on the **vRealize Automation Console** link
5. Click **Next**
6. Login with your **vmcws#** user name (where # is your student number)
7. Enter your password - VMware1!

You have successfully logged in to vRealize Automation!

### Add a vSphere Endpoint

An endpoint is anything that vRealize Automation uses to complete it's provisioning processes. This could be a public cloud resource such as Amazon Web Services EC2, VMware Cloud on AWS, an external orchestrator appliance, or a private cloud hosted by vSphere or other hypervisors.

1. Click on **Infrastructure** tab
2. Click on **Endpoints** on left pane
3. Click on **Endpoints** again on the left pane
4. Click on **New** button
5. Select **Virtual**
6. Select **vSphere (vCenter)**

    For these next few steps, make sure you are logged in to your VMware Cloud on AWS portal.
7. On your VMware Cloud on AWS portal, make sure you click on the **Connection Info** tab, you will be using some of this information to create the vRealize Automation Endpoint
8. Name your vRA Endpoint **ws#** (where # is your student number)
9. Under **Address** enter the information from the VMware Cloud on AWS portal under **vSphere Client (HTML5)** and replace the words 'ui' for 'sdk'
10. Example:
    From VMware Cloud on AWS portal: <https://vcenter.sddc-X-X-X-X.vmc.vmware.com/ui>
    What it should look like in vRealize Automation: <https://vcenter.sddc-X-X-X-X.vmc.vmware.com/sdk>
11. Enter the user name **cloudadmin@vmc.local**
12. Enter the password (Please refer to VMware Cloud on AWS portal for this information)
13. Click on the **Test Connection** button
14. If connection passes test, click on **Ok** button to create your vRealize Automation Endpoint

### Create a Fabric Group

An administrator can organize virtualization compute resources and cloud endpoints into fabric groupsby type and intent. Make sure for this step you've selected the Endpoint you created in the previous module.

1. Click on **Fabric Groups** on the left pane
2. Click on **+ New** to create a new Fabric Group
3. Name your Fabric Group **ws#FabricGroup** (where # is your student #)
4. Add the user assigned to your student number: **vmcws#@corp.local** and click on the magnifying glass icon to find and add your user
5. Select the vRealize Automation endpoint you created in the previous step
    NOTE: MAKE SURE YOU SELECT THE CORRECT COMPUTE RESOURCE AS ILLUSTRATED BY THE ARROWS. SHOULD BE THE "WS#" ENDPOINT YOU CREATED IN A PREVIOUS STEP.
6. Click **OK** button

## Reservations

When a user requests a machine, it can be provisioned on any reservation of the appropriate type that has sufficient capacity for the machine. You can apply a reservation policy to a blueprint to restrict the machines provisioned from a that blueprint to a subset of available

### Create Reservation Policy

For this step if your Reservation tab is missing you may need to hit the Refresh button on your browser.

1. Click on the **Infrastructure** button on the left pane
2. Click **Reservations** on the left pane
3. Click **Reservation Policies** on the left pane
4. Click **New** button
5. Name your Reservation Policy "Student#"
6. Click **OK** button

### Create a Reservation

A vRealize Automation reservation is a means to allocate resources in a fabric group (CPU, RAM, Storage, etc.) to a specific business group.

1. Click on the **nfrastructure** button on the left pane
2. Click on **Reservations** on the left pane
3. Click **Reservations** on the left pane
4. Click **+ New** to create a new Reservation
5. Select **vSphere (vCenter)**
6. Give your reservation a name: **Student#** (where # is your student number assigned to you)
7. Leave/select vsphere.local as the Tenant
8. Select **vmc-ws#** (where # is your student number)
9. For Reservation policy type or select **Student#** (where # is your student number assigned to you)
10. Priority should be set to **1**
11. Click on the **Resources** tab
12. On the **Resources** tab for **Compute Resource** select **Cluster-1 (WS#)** from the drop down box
13. Type **1024** in the **This Reservation** field
14. Select the checkbox for **WorkloadDatastore**
15. Type **10000** under the **This Reservation Reserved** field
16. Type **0** under the Priority field and hit the **OK** button
17. Select **Compute-ResourcePool** in the **Resource Pool** field
18. Click on the **Network** tab
19. Select the **sddc-cgw-network-1** Network Adapter by clicking the checkbox to the left of it and selecting **sddc-cgw-network-1** from the dropdown box on the right side
20. Click on the **OK** button

### Create a Blue  Print

A blueprint is a complete specification for a service. A blueprint determines the components of a service, which are the input parameters, submission and read-only forms, sequence of actions and provisioning. You can create service blueprints to provision custom resources that you previously created.

1. Click on the **Design** tab
2. On the left pane click **Blueprints**
3. Click the **+ New** button
4. Name your Blueprint **Student#** (where # is the Student number assigned to you)
5. Use the same name for ID
6. Enter **1** as the Deployment Limit
7. Enter **1** as the Minimum Lease days
8. Enter **1** as the Maximum Lease days
9. Enter **0** for Archive Days
10. Click the **OK** button, the Design Canvas below appears
11. Ensure **Machine Types** is selected
12. Click and drag onto the Canvas the **vSphere (vCenter) Machine**
13. Leave all defaults on the **General** tab and click on the **Build Information** tab
14. Make sure **Server** is selected as the Blueprint Type
15. Change Action to **Clone**
16. Click on the elipsis button and select **StudentVM#** that you created in your previous step and click **OK**
17. Click **Save**
18. Click **Finish**
19. Select your Blueprint by highlighting it
20. Click on the **Publish** button to publish your blueprint
21. Make sure you are in the **Administration** tab, if not, click on it and select **Catalog Management**
22. Select **Catalog Items** from the left pane and click your **Student#** to configure
23. Select **Desktops** from the drop down box next to **Service**
24. Click the **OK** button

### Entitle your Blueprint

1. Click the **Administration** tab
2. Select **Catalog Management** from the left pane
3. Click on **Entitlements** on the left pane
4. Click the **+ New** button in order to create a new Entitlement
5. Name the Entitlement **ws#** (where # is your Student #)
6. Change the Status to **Active**
7. Select **vmc-ws#** for the Business Group (where # is the number assigned to you)
8. Click **Next**
9. Click the **+** sign next to **Entitled Services**
10. Make sure to select the checkbox next to **Desktops**
11. Click **OK**
12. Click on the **+** sign next to **Entitle Actions**
13. Select the checkbox at the top to select all Actions
14. Click **OK** button
15. Click **Finish** button
16. Select the **Catalog** tab
17. Click on your newly created **Student#** blueprint and click on the **Request** button
18. Highlight the **vSphere_VCenter_Machine** under your Blueprint
19. Check all the options are correct and click the **Submit** button
20. Click on the **Requests** tab to check on the status of your Blueprint submission
21. Check the status to ensure the request completes successfully