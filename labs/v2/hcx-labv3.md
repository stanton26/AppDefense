---
layout: single
title: "Hybrid Cloud Extension (HCX) Lab Manual"
categories: labs
date: 2019-02-21 
tags: workshop
toc: true
classes: wide
author_profile: false
comments: true
---
# Introduction

In this lab exercise you will learn about Hybr Cloud Extension (HCX), Primarily this is a tool, bundled with VMware Cloud on AWS, which will allow you to bulk migrate workloads to VMware Cloud on AWS and significantly reduce the time and complexity of moving workloads into the public cloud sphere.

## What is Hybrid Cloud Extension (HCX)

![HCX1](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX1.jpg)

Hybrid Cloud Extension abstracts on-premises and cloud resources and presents them to the apps as one continuous hybrid cloud. On this, Hybrid Cloud Extension provides high-performance, secure and optimized multisite interconnects. The abstraction and interconnects create infrastructure hybridity. Over this hybridity, Hybrid Cloud Extension facilitates secure and seamless app mobility and disaster recovery across on-premises vSphere platforms and VMware Clouds. Hybrid Cloud Extension is a multi-site, multi cloud service, facilitating true hybrid cloud.

### Hybrid Cloud Extension Features

![HCX2](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX2.jpg)

#### Any-to-Any vSphere Cloud App Mobility

* Rapidly move existing workloads from a vSphere platform to the latest SDDC
* Reduce upfront planning time for cost and resource analysis
* Accelerate cloud adoption and avoid retrofitting on-premises environment Business Continuity with Lower TCO

#### Business Continuity with Lower TCO

* IP and MAC address remapping is not required
* No need to retrofit existing VM environment
* Hybrid Cloud Extension provides warm and cold bulk migration, and bidirectional migration
* Hybrid Cloud Extension simplifies your operational model

#### Architected for Security

* Ensure highly secure tethering of private and public clouds
* Protect resources with resilient disaster recovery capabilities
* Hybrid Cloud Extension hybrid DMZ enables portability of enterprise network and security practices to the cloud
* Security policies migrate with applications High-Performance Infrastructure Hybridity

#### High-Performance Infrastructure Hybridity

* In-built WAN optimization is tuned for the needs of hybrid use cases
* Hybrid Cloud Extension provides agile, intelligent routing
* Traffic load balancing overlay is policy-enforced
* Multiple VM migration models (including vMotion, warm, cold) make it easy to migrate to and from the cloud without any changes

## Configure HCX

Click on the link below to walk through on how to install and configure HCX within your on-prem vCenter environment.

[HCX Install and Configuration](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX-OnPrem-Installation.htm){:target="_blank"}

## HCX - vMotion Migration

Now that you are familar with installing and configuring HCX.  Let's do an actual vMotion (live) migration of a virtual machine to VMware Cloud on AWS.

### Log into On-Prem vCenter

We have provided an on-prem vCenter with virtual machines to migrate.  Based on your student ## please select the appropriate VM to migrate.

From your Horizon desktop (workshop.vmc.ninja) open google and access the on-prem vCenter

**Note: Refer to the Student Access page to log into your Horizon desktop https://vmc-field-team.github.io/student-access/**

![HCX01](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/hcx01.jpg)

1. Open Google and enter https://vcenter-workshop.workshop.set.local/ui for the URL.
2. Type in your student (vmcws#@set.local) credentials.
3. Type in your password assigned to you.
4. Click **Login** to continue.

### Migrate Virtual Machine to the Cloud

![HCX02](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/hcx02.jpg)

1. Click the arrow to expand **Datacenter**.
2. Click the arrow to expand **VSAN-Cluster** cluster.
3. Click the arrow to expand **Migrate** resource pool.
4. Click on the **Student##** virtual machine.
5. Make note of the **IP Address** to ping later.

![HCX02-1](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/hcx02-1.jpg)

1. Right click on the Windows Icon in the lower left of your desktop.
2. Click on **Command Prompt**.

![HCX02-2](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/hcx02-2.jpg)

1. In case you didn't capture the IP Address of the virtual machine.
2. Type in the command prompt **ping -t 172.60.2.xxx**.

![HCX03](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/hcx03.jpg)

1. Go back to vCenter console and right click on your **Student##** virtual machine.
2. Hover your mouse over **Hybridity Actions**.
3. Click on **Migrate to the Cloud**.

![HCX04](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/hcx04.jpg)

To migrate your virtual machine VMware Cloud on AWS you will have to select the destinations for the folder, resource pool and datastore.

**Note: Make sure to select the Migrate folder and resource pool to ensure you can find the same virtual machine when migrating back to on-prem.**

1. Click on **Specify Destination Folder** and select the **Migrate** folder.
2. Click on **Specify Destination Container** and select the **Migrate** resource pool.
3. Click on **Select Storage** and select the **WorkloadDatastore** datastore.
4. Click on **Select Migration Type** and select **vMotion**.
5. Click **Next** to validate your selection.

![HCX05](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/hcx05.jpg)

1. Verify the validation is successful.
2. Click on **Finish** to migrate your virtual machine to VMware Cloud on AWS.

### Migrate Virtual Machine Back to On-Prem Datacenter

![HCX06](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/hcx06.jpg)

1. Click on **Menu**.
2. Click on **HCX**.
   
![HCX07](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/hcx07.jpg)

Dashboard gives you the number of virtual machines migrated, in progress and scheduled.

1. Click on **Migration** on the left side.

![HCX08](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/hcx08.jpg)

1. Make note of the progress of the vMotion migration.
2. Click on the **Refresh** button to update the progress.

![HCX09](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/hcx09.jpg)

While the migration is in progress lets look at the ping response.

1. Click on the **Command Prompt** to return to the ping test.
2. Observe the ping test left running from a previous step and notice it hasn't dropped.

![HCX010](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/hcx010.jpg)

Once the virtual machine has been successfully migrated to VMware Cloud on AWS, lets take the same virtual machine and migrate it back to the on-prem vCenter.

1. Verify the virtual machine has been migrated to the SDDC in VMware Cloud on AWS.
2. Click on **Migrate Virtual Machines** button.

![HCX011](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/hcx011.jpg)

1. Click on **Reverse Migration** checkbox to switch to the VMware Cloud on AWS vCenter.
2. Click on **Migrate** resource pool to display the migrated virtual machines.
3. Click on your **Student##**.
4. Click on **Specify Destination Folder** and select the **Migrate** folder.
5. Click on **Specify Destination Container** and select the **Migrate** resource pool.
6. Click on **Select Storage** and select the **vsanDatastore** datastore.
7. Click on **Select Migration Type** and select **vMotion**.
8. Click **Next** to validate your selection.

![HCX012](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/hcx012.jpg)

1. Verify the validaiton is successful.
2. Click on **Finish** to migrate your virtual machine back to your on-prem vCenter.

![HCX013](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/hcx013.jpg)

1. Make note of the progress of the vMotion migration.
2. Click on the **Refresh** button to update the progress.

![HCX014](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/hcx014.jpg)

**Optional**

1. Click on the **Command Prompt** to return to the ping test.
2. Observe the ping test left running from a previous step and notice it hasn't dropped while migrating back to on-prem vCenter.
3. Use ctrl-c to cancel ping.

### Other Migration Methods

Feel free to try the other migration Types.  Use the same virtual machine **Student##** and follow the same steps but instead of vMotion try **Bulk Migration **or **Cloud Motion with vSphere Replication**.

![HCX015](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/hcx015.jpg)

#### Cloud Motion with vSphere Replication

This latest option provides zero downtime for workload mobility from source to Destination.  First, the workload disks get replicated to the destination site.  The replication is handled using the HCX built-in vSphere replication.  This process is dependent on the amount of data and available network bandwidth.  Once the data sync is complete the HCX switchover initiates a vMotion.  The vMotion migrates the workload to the destination site and synchronizes only the remaining data (delta) and workload memory state.  There is an option to schedule a maintenance window for the vMotion data sync swithcover otherwise it happens immediately.

#### Bulk Migration

Bulk Migration creates a new virtual machine on the destination site.  This can either be on-premises or VMC and retains the workload UUID.  Then it uses vSphere Replication to copy a snapshot of the workload from source to destination site while the workload is still powered on.  In this case, a snapshot is a point of the time of the workload disk state, but not the traditional vSphere snapshot.  The Bulk Migration is managed by the HCX interconnect cloud gateway proxy.  During the data sync, there is no interruption to the workloads.  The data sync is dependent on the amount of data and available bandwidth.  There is an option to schedule a maintenance window for the switchover otherwise, the switchover happens immediately.  Once the initial data sync completes, a switchover takes place (unless scheduled). The source site workloads are quiesced and shut down leveraging VMware Tools. If VMware Tools is not available, HCX will prompt you to force power off the workload(s) to initiate the switchover. During the switchover process, a delta sync occurs based on changed block tracking (CBT) to sync the changes since the original snapshot. The workloads on the destination site will begin to power on once the data sync is complete (including delta data changes). There are checks in place to ensure resources are available to power on the workloads. If a destination workload cannot power on due to resources, the source workload will get powered back on.