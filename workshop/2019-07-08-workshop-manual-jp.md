---
layout: single
title: "Scopes and Services"
permalink: /workshop-manual-jp/
date: 2019-04-02
tags: workshop
classes: wide
author_profile: false
---

# Setting up Scopes and Services

After you have finished the deployment of the Appliance, Host, and Guest Modules you can then begin to set up the scopes and services. Please follow the best practices on how to set up scopes and services.

## What is a Scope? 

A data cetner application. 

## What is the purpose of a Scope? 

To estbalish the inteneded state of an application and it's alowed behaviors. 

## What is a service? 

A tier within a scope application which contains desciption, member workloads, rules and behaviors. 

# How to create a Scope and Service

You can manually create a scope and then add a service with members that you have deployed the AppDefense Module to. You can also automate this process through Puppet, Ansible or vRA. Follow the best practices below to create a Scope and Service.

Create a scope (In the AppDefense Manager) : 
Click the + icon next to scopes on the left navigation pane, then insert application name and click create. 

Create a Service within a Scope: 
Click add Service, select a Service name then select Service type (ex. app server or web server) 
Enter description of this Service and click next.
On the Allowed Behavior page, do not add any allowed behaviors right now. System learns the behavior in the Discovery Mode, so you can skip adding any allowed behaviors initially.

Helpful URl's For Scopes and Services: 



<!-- [VMware Site Recovery Manager](https://vmc-field-team.github.io/labs-jp/srm-lab-jp/){:target="_blank"} | 災害対策のユースケースにおいて、どのように VMware Cloud on AWS を利用するのかを学びます -->
<!-- [VMware vRealize Automation](https://vmc-field-team.github.io/labs-jp/vra-lab-jp/){:target="_blank"} | VMware のクラウド管理ツールを用いた連携を学びます -->