---
layout: single
title: "Protected Mode"
permalink: /workshop-manual-horizon/
date: 2019-03-19
tags: workshop
classes: wide
author_profile: false
---
## What is Protected Mode? 
In proteced Mode, application beahavior is locked down and rules are applied.

### What happens when I move my scope into Protected Mode? 
When a scope is moved into protected mode,all application behavior is now locked down and rules and remediation actions are now enforced. The armor has been applied, and your application is now safe in Protected Mode.


## Alerts 
What are Alerts? 
- AppDefense learns the behaviors (discovery mode)  and sends alerts based on the rule settings.
- Alert is a method available to monitor behaviors in AppDefense Manager. Any deviation from the known good state triggers an event or alert. The deviation can be due to process characteristics, process behavior, integrity of the Operating System (OS), or AppDefense module. The different kinds of events are seen on UI at two locations, Alerts page and under Events > Monitoring page.

AppDefense classifies alerts based on their threat level. When you log in to AppDefense Manager, in the left navigation, click Alerts. The alerts are grouped based on the process name. (They will appear as: Critical, Serious, Minor)

## Events
AppDefense generates different types and subtypes (categories) of alerts and events, with different severity levels. Alerts page displays the Event Type column. Following are the main events types that are listed under the Event Type column.

Process driven events = Process Monitoring, Inbound coonnections, Outbound connections. 

System integrity driven events = Appdefense modeule Integrity, Gues OS Integrity

Managment Driven Events= Management Alert, Guest module down, 

## Blacklist and Whiteliost 
A Blacklist is an explicit listing of processes that are not allowed, when added to the Blacklist a process will be removed from every service in every scope.  A Whitelist is an explicit listing of processes that are allowed, once added to the Whitelist a process will be added to every service within every scope.  This can be applied in the service or in the "manage process" tab. 

### Helpful URL's

Below are a list of the URLs related to protected mode: 

Alert Classification by Nolan Karpinski <https://www.youtube.com/watch?v=QbmfQBalTGw&t=2s>
