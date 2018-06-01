---
layout: single
title: "Working with your SDDC Lab Manual"
date: 2018-06-01
tags: workshop
toc: true
classes: wide
author_profile: false
---
# Introduction

## Add a Host to your SDDC

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-11-Image-5.png)

1. On your "Student Workshop #" SDDC, click on the "View Details" button.
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-11-Image-6.png)
2. Click on the "Actions" button
3. Click on "Add Hosts"
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-12-Image-7.png)
4. As you will only be adding only one host, review the field highlighted
5. Click the "Add Hosts" button

Congratulations! You have completed this step. The adding of an additional host to an existing SDDC should take approximately 10 minutes to complete.

## Configuring SDDC Firewall Rules

### Management Gateway Firewall Rules

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-13-Image-8.png)

By default, the firewall for the management gateway is set to deny all inbound and outbound traffic. In this exercise, you will add a firewall rule to allow vCenter traffic. In order to access vCenter Server in your SDDC, you must set a firewall rule to allow traffic to the vCenter Server.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-13-Image-9.png)

1. Under **Management Gateway** click the arrow to expand **Firewall Rules**
2. Click **Add Rule**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-13-Image-10.png)
3. Enter a name for your rule under **Rule Name**
4. Type **Any** for Source
5. Make sure **vCenter** is selected as Destination
6. Select **HTTPS (TCP 443)** from the drop down box for Service
7. Click the **SAVE** button, your rule should look like the below image

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-14-Image-11.png)

### Compute Gateway Firewall Rules

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-14-Image-12.png)

By default, the Compute NSX Edge Services Gateway is also set to deny all inbound and outbound traffic. You need to add additional firewall rules to allow traffic as needed.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-14-Image-13.png)

#### Create Firewall Rule under Compute Gateway for Inbound Native AWS Services access

1. Under **Network** tab, navigate to **Compute Gateway**
2. Expand **Firewall Rules**
3. Click **ADD RULE**

### AWS Inbound Firewall Rule

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-15-Image-14.png)

#### Create AWS Inbound Firewall Rule

1. **Name** - AWS Inbound
2. **Action** - Allow
3. **Source** - All connected Amazon VPC
4. **Destination** - 192.168.#.0/24 (Where # is your student number)
5. **Service** - ANY
6. Click **SAVE** button.

#### Create AWS Outbound Firewall Rule

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-15-Image-15.png)

Follow the same process as in the previous step and create AWS Outbound Firewall Rule following these instructions:

1. **Name** - AWS Outbound
2. **Action** - Allow
3. **Source** - 192.168.#.0/24 (Where # is your student number)
4. **Destination** - All connected Amazon VPC
5. **Service** - ANY
6. Click **SAVE** button.

## Log into VMware Cloud on AWS vCenter

## Create Content Library

## Create Logical Network

## Create Linux Customization Spec

## Deploy Virtual Machine

## Convert Virtual Machine to Template