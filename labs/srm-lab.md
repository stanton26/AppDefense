---
layout: single
title: "Site Recovery Manager (SRM) Lab Manual"
date: 2018-07-20
tags: workshop
toc: true
classes: wide
author_profile: false
comments: true
categories: labs
---
# Introduction

In this lab you will pair up with another student group in order to simulate the setup and configuration tasks for VMware Site Recovery Manager

## Activate Site Recovery Add On

Important Instructions for Site Recovery Exercises

PLEASE BE AWARE THAT THESE EXERCISES MUST BE PERFORMED FROM THE ASSIGNED HORIZON DESKTOP YOUR INSTRUCTORS ASSIGNED. IF YOU TRY TO PERFORM SOME OF THE EXERCISES OUTSIDE OF THE HORIZON SESSION YOU WILL EXPERIENCE SOME FAILURES.

### Activate Site Recovery

1. In your VMware Cloud on AWS Organisation, click on the **View Details** for your SDDC deployment
2. Click on the **Add ons** tab
3. Under the Site Recovery Add On, Click the **Activate** button
4. In the pop up window Click **Activate** again

Wait until the Site Recovery Add On has been activated. This process should take ~10 minutes to complete.

## What is VMware Site Recovery

VMware Site Recovery brings VMware enterprise-class Software-Defined Data Center (SDDC) Disaster Recovery as a Service to the AWS Cloud. It enables customers to protect and recover applications without the requirement for a dedicated secondary site. It is delivered, sold, supported, maintained and managed by VMware as an on-demand service. IT teams manage their cloud-based resources with familiar VMware tools without the difficulties of learning new skills or utilizing new tools and processes.

VMware Site Recovery is an add-on feature to VMware Cloud on AWS. VMware Cloud on AWS integrates VMware's flagship compute, storage, and network virtualization products: VMware vSphere, VMware vSAN, and VMware NSX along with VMware vCenter Server management. It optimizes them to run on elastic, bare-metal AWS infrastructure. With the same architecture and operational experience on-premises and in the cloud, IT teams can now get instant business value via the AWS and VMware hybrid cloud experience.

The VMware Cloud on AWS solution enables customers to have the flexibility to treat their private cloud and public cloud as equal partners and to easily transfer workloads between them, for example, to move applications from DevTest to production or burst capacity. Users can leverage the global AWS footprint while getting the benefits of elastically scalable SDDC clusters, a single bill from VMware for its tightly integrated software plus AWS infrastructure, and on-demand or subscription services like VMware Site Recovery Service. VMware Site Recovery extends VMware Cloud on AWS to provide a managed disaster recovery, disaster avoidance and non-disruptive testing capabilities to VMware customers without the need for a secondary site, or complex configuration.

VMware Site Recovery works in conjunction with VMware Site Recovery Manager and VMware vSphere Replication to automate the process of recovering, testing, re-protecting, and failing-back virtual machine workloads. VMware Site Recovery utilizes VMware Site Recovery Manager servers to coordinate the operations of the VMware SDDC. This is so that, as virtual machines at the protected site are shut down, copies of these virtual machines at the recovery site startup. By using the data replicated from the protected site these virtual machines assume responsibility for providing the same services.

![SRM Scenarios](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/Screenshot+at+Jul+20+18-03-34.png)

VMware Site Recovery can be used between a customers datacenter and an SDDC deployed on VMware Cloud on AWS or it can be used between two SDDCs deployed to different AWS availability zones or regions. The second option allows VMware Site Recovery to provide a fully VMware managed and maintained Disaster Recovery solution. Migration of protected inventory and services from one site to the other is controlled by a recovery plan that specifies the order in which virtual machines are shut down and started up, the resource pools to which they are allocated, and the networks they can access.

VMware Site Recovery enables the testing of recovery plans, using a temporary copy of the replicated data, and isolated networks in a way that does not disrupt ongoing operations at either site. Multiple recovery plans can be configured to migrate individual applications or entire sites providing finer control over what virtual machines are failed over and failed back. This also enables flexible testing schedules. VMware Site Recovery extends the feature set of the virtual infrastructure platform to provide for rapid business continuity through partial or complete site failures.

## Create a Cross SDDC VPN

We will be setting up an IPSEC VPN connection between your VPC and the VPC of the person you were paired with.

1. Go back to the **VMware Cloud on AWS** tab.
2. In the main SDDC window, click on **View Details**
3. Click on the **Network** menu

In the Management Gateway section, make a note of the Public IP and the Infrastructure Subnet CIDR

![Management Gteway IP](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/Screenshot+at+Jul+20+18-03-34.png)

In the Management Gateway settings below

1. Click the drop down arrow to open the **IPsec VPNs** section
2. Click on **ADD VPN**

Fill in the following information

1. Name: Student # MGMT GW (where # is your student number)
2. The **Public IP address** of the persons Gateway you were paired with
3. The **Infrastructure IP CIDR** of the person you were paired with
4. Pre-shared key is **VMware1!**
5. Click on **Save**

When both you and the person you were paired with have completed these steps you should see the status of the VPN turn to **Connected**

There will be a need to setup a second VPN to our Host infrastructure for this setup to work. This is not normally needed when setting up your on-premises environment but it's needed for the special setup in this workshop.

1. Make sure the IPSecVPNs drop down is opened, if not click it under **Management Gateway**
2. Click on **Add VPN**

    Fill in the following information
3. Name this VPN **Student# to Host** (where # is your student number)
4. Enter **54.70.191.234** for the Remote Gateway Public IP
5. Enter **192.168.30.0/24** under Remote Networks
6. Pre-shared key is **VMware1!**
7. Click on **Save**.

## Prepare and Pair Site Recovery

### Firewall Rules for Site Recovery

If you still have your OAuth Token from the Powershell module, you are free to use it, otherwise, follow the following instructions to obtain a new one.

1. Click on the drop down next to your **Name/Org ID** in the top right hand corner
2. Click **My Account**
3. Click on **API Tokens**

If you already have a token from the previous API lab, please reuse this, if not you can create a new one using the following steps

1. Click on **Create a new token**
2. Click on **Continue**
3. Click on **Copy to Clipboard**

Now let's attach to the VMC server

From your Horizon workstation, replacing the refreshtoken placeholder below with your token.

``` powershell
connect-vmc -refreshtoken "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
```

#### Import Firewall Rules for Site Recovery via PowerCLI

Open a Powershell window and enter the following commands:

``` powershell
Set-Location \\vmcwindc01\documents
.\import-dr-fw-rules.ps1 -refreshToken "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx" -orgName "VMC-WS#" -sddcName "Student Workshop #"
```

Where xxxx is the OAuth Token you generated in a previous step and # is your Student number.

Ensure the appropriate firewall rules were created by following the below instructions.

1. Click on the **Network** tab in your Student SDDC
2. Expand **Firewall Rules** under the **Management Gateway**

Confirm the following Firewall Rules have been created:

SRM 9086
VR Replication
VR 8043
VR 443
VR Out
SRM Out

Please note that the rules may have been created in a different order than shown above.

### VMware Site Recovery - Site Pairing

**IMPORTANT NOTE**: Only one person can do the Site Pairing exercise. Please decide between you and your partner who performs this step.

1. On your VMware Cloud on AWS Portal click on the **Add Ons** tab
2. Click **Open Site Recovery**
3. Click on **New Site Pair**
    You will be pairing the partner site that was assigned to you by your instructor, note that this is not the information for your SDDC used up until now.

    This is the information your partner will need from you and you will need from your partner's site.
4. Click on the **Settings** tab in your SDDC
5. Copy or note the URL for the vCenter Server
6. Now Click on **Open vCenter** in the top right corner for the VMWare Cloud on AWS console
7. Note the username
8. Note the password
9. Make sure your local vCenter is selected
10. Enter the information from your partner's SDDC:
    a. PSC host name
    b. User name
    c. Password
11. Make sure local vCenter server is selected
12. Select **all Services**
13. Click **Next**
14. Click **Finish** button

### Configure Network Mappings

1. Click **Network Mappings** in the left pane of the Site Recovery page
2. Click **New**
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

## SRM - Protect a VM

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

## Perform a Recovery Test

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