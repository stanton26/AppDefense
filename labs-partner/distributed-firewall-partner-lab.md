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
