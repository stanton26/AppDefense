---
layout: single
title: "VMware Cloud on AWS Workload Operations"
date: 2018-07-18
tags: workshop
toc: true
classes: wide
author_profile: false
categories: labs
comments: true
---
## Introduction
Go through the basic day 2 operations of managing the virtual infrastructure inside your SDDC. In this lab you will deploy several windows machines and will be performing operations such as customizing the OS and monitoring cluster performance. 

The virtual machines deployed in this lab are a requirement for your Site Recovery lab. 

## Task #1 Create Customization Specifications

1.  Log into your vCenter Web Client > click the **Menu** icon and slect **Policies and Profiles**
    
   ![Screenshot](https://github-partner-lab-screenshots.s3-us-west-2.amazonaws.com/operations+screenshots/1.jpg)

2.  Under **Policies and Profiles**, click VM Customizations Specifications and click "Create a new specification icon. 

    ![Screenshot](https://github-partner-lab-screenshots.s3-us-west-2.amazonaws.com/operations+screenshots/2.jpg)
The new VM guest Customization Spec Wizard opens

3. On the Name and target OS page, enter Student#Win10-CustomSpec in the Customization Spec Name text box. 
4. Under the Guest OS, verify that windows is selected as the Target Guest OS. 
5. Verify that Generate a new security identity (SID) is selected

    ![Screenshot](https://github-partner-lab-screenshots.s3-us-west-2.amazonaws.com/operations+screenshots/3.jpg)

6. Click Next

7. On the Registration Information page, enter VMware Student Number in the Owner name text box and enter VMware in the Owner organization text box. 

    ![Screenshot}](https://github-partner-lab-screenshots.s3-us-west-2.amazonaws.com/operations+screenshots/4.jpg)
