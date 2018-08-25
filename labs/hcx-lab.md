---
layout: single
title: "Hybrid Cloud Extension (HCX) Lab Manual"
categories: labs
date: 2018-07-20
tags: workshop
toc: true
classes: wide
author_profile: false
comments: true
---
# Introduction

In this lab exercise you will learn about Hybr Cloud Extension (HCX), Primarily this is a tool, bundled with VMware Cloud on AWS, which will allow you to bulk migrate workloads to VMware Cloud on AWS and significantly reduce the time and complexity of moving workloads into the public cloud sphere.

## What is Hybrid Cloud Extension (HCX)

Hybrid Cloud Extension abstracts on-premises and cloud resources and presents them to the apps as one continuous hybrid cloud. On this, Hybrid Cloud Extension provides high-performance, secure and optimized multisite interconnects. The abstraction and interconnects create infrastructure hybridity. Over this hybridity, Hybrid Cloud Extension facilitates secure and seamless app mobility and disaster recovery across on-premises vSphere platforms and VMware Clouds. Hybrid Cloud Extension is a multi-site, multi cloud service, facilitating true hybrid cloud.

### Hybrid Cloud Extension Features

Any-to-Any vSphere Cloud App Mobility


- Rapidly move existing workloads from a vSphere platform to the latest SDDC
- Reduce upfront planning time for cost and resource analysis
- Accelerate cloud adoption and avoid retrofitting on-premises environment Business Continuity with Lower TCOBusiness Continuity with Lower TCO
- IP and MAC address remapping is not required
- No need to retrofit existing VM environment
- Hybrid Cloud Extension provides warm and cold bulk migration, and bidirectional migration
- Hybrid Cloud Extension simplifies your operational model Architected for Security
- Ensure highly secure tethering of private and public clouds
- Protect resources with resilient disaster recovery capabilities
- Hybrid Cloud Extension hybrid DMZ enables portability of enterprise network and security practices to the cloud
- Security policies migrate with applications High-Performance Infrastructure HybridityHigh-Performance Infrastructure Hybridity
- In-built WAN optimization is tuned for the needs of hybrid use cases
- Hybrid Cloud Extension provides agile, intelligent routing
- Traffic load balancing overlay is policy-enforced
- Multiple VM migration models (including vMotion, warm, cold) make it easy to migrate to and from the cloud without any changes

## HCX - vMotion Migration

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

## HCX - Bulk Migration

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