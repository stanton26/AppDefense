---
layout: single
title: "Distributed Firewall"
date: 2018-07-17
tags: workshop
toc: true
classes: wide
author_profile: false
categories: labs
comments: true
---
# Introduction

In this lab we are going to start with looking at the basic of creating east-west firewall rules using NSX Distributed Firewall functionality within the SDDC. 

Note: There is a requirement in this lab to have completed the steps in the [Working with your SDDC Lab](https://vmc-field-team.github.io/labs-partner/working-with-sddc-partner-lab/) concerning Content Library creation and Network creation and firewall rule creation.

## Distributed Firewall Rules

The distributed firewall rules are implemented to secure workload groups in the SDDC environment. A firewall is a network security system that monitors and controls the incoming and outgoing network traffic based on predetermined firewall rules.

The source of the rule is a single or multiple workload groups. The source matches to the default any if not defined. The destination of the rule is a single or multiple workloads. The destination matches to the default any if not defined.

When you log into your environment's VMC console you will see a section called **Networking & Security**. Navigate to **Distributed Firewall** under the **Security** tab.
    ![Distributed-Firewall](https://s3-us-west-2.amazonaws.com/partner-workshop-screenshots/distributed-firewall-01.jpg)

As you can see the distributed firewall has 5 different sections

**Emergency Rules** Applies to temporary rules needed in emergency situations. For example, block traffic to a web server due to maliciious content. Firewall rules defined in this section get executed prior to any rules defined in other sections. 

**Infrastructure Rules** Applies to infrastructure rules only. Such as ESXi, vCenter Server or connectivity to on-premise data center 

**Environment Rules** Applies to broad groups. Such as, setting rules so that the production environment cannot reach the test environment. Another common use of Environment rules are to easily create DMZ networks. 

**Application Rules** Applies to specific application rules. In this section is where we can provide microsegmentation like protectin to our workloads. For example blocking east west traffic between two web servers on the same l2. 

**Default Rules** The default rules is set to allow all traffic. This is important to understand since our pertimer firewalls are located at T1 tiers which we discussed in "working with your sddc lab" Our tier 1's provide perimeter protection while our distributed firewall rules allow us to define policies inside the sddc. 

Note: In this lab we will be focusing on **Application Rules**. We will be deploying two web servers within our SDDC in the same L2 network and block traffic between the two VM's. 

## Deploy two web servers
As a first step in our distributed firewall lab, we will be deploying two VM's acting as our web tiers. These two web servers will reside in the same subnet. 

1. If not already opened, open your VMware Cloud on AWS vCenter and click on the **Menu** drop down 
2. Select Content Libraries
3. Click on your previously created Content Library named **Student#** (where # is your student number)
4. Make sure you click on the **Template** tab
5. Right-click on the **EFS** template
6. Select **New VM from This Template**
7. Name the virtual machine **student#Web01** (where # is your student#)
8. Expand the location and select Workloads
9. Click **Next**
10. Expand the destination to select **Compute-ResourcePool** as the compute resource 
11. In the "Review details" step click **Next**
13. In the **Select storage** step, highlight the "WorkloadDatastore"
14. Click **Next**
15. Slect the network that elongs to your student number. 
16. Click **Next**
17. Click **Finish**
18. Repeat steps 1-17 and deploy another web VM and name this instance **student#Web02**
19. Check for completion of the deployment of your VM 
20. Click **Menu**
21. Select **VMs and Templates**
22. Check to make sure your VMs are powered on. If not power on your VM's. 

## Confirm L2 adjacency
Now that we have deployed two virtual machines in the same subnet let's log into the VM's and check the network settings and test network connectivity between both web servers. 

1. Open a console to both student#Web01 and studentWeb02. You can do this by right clicking on the VM and selecting **Open Remote Console**.
Note: ensure that your browser is not blocking popups in case you don't see a remote console window.
2. Log into both web servers with username: **root** and password **VMware1!**
3. Once you are logged into the servers enter command **ifconfig**. You should see the server IP address in the proper subnet that you created in lab **Working With Your SDDC**. Make note of both VM's IP addresses. 
4. Now that we have the IP addresses for each server let's do a ping test to verify connectivity. From student#Web01 ping studdent#Web02 IP address. (Enter ctrl+c to stop pings).
    ![Ping-Test](https://s3-us-west-2.amazonaws.com/partner-workshop-screenshots/ping-test.jpg)

5. 



