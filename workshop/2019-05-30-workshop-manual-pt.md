---
layout: single
title: "Rule Enforcement"
permalink: /workshop-manual-pt/
date: 2019-05-20
tags: workshop
classes: wide
author_profile: false
---
## Introdcution to Rule Enforcement: 
After the security scope is in the Protected Mode, you can still review and edit services associated with the scope.

There are four vectors that are used to alert and remediate. By default, the only action for the Remediation rules is set to Alert and the enforcement is automatic. You can edit the rule settings. You can edit the settings based on the action that you want AppDefense to take.

Enforce Outbound Connections: If AppDefense sees a new outbound connection from an allowed process, what do you want it to do?
Enforce Inbound Connections: If AppDefense sees a new inbound connection from an allowed process, what do you want it to do?
Enforce Guest OS Integrity: Windows-only. If AppDefense detects that the integrity of your operating system (OS) has been compromised, what do you like it to do?
Enforce AppDefense Module Integrity: Windows-only. If AppDefense detects the integrity, the AppDefense Module has been compromised (potentially turned off), what do you like it to do?
You cannot set automatic remediation action for the Guest module down alert. Remediation for this action can only be taken manually.


### NSX Only Quarantine: 

Quarantine is a remediation action that can be enforced by following this procedure: 
1. Select your Scope from Dashboard. 
2. Click on "Services" tab. 
3. Select "Edit service."
4. Click on "Rules" tab. 
5. For your remediation 
action select "Quarantine.

### Block 

Block is a remediation action that can be enforced by following this procedure: 
1. Select your Scope from Dashboard. 
2. Click on "Services" tab. 
3. Select "Edit service."
4. Click on "Rules" tab. 
5. For your remediation 
action select "Block and send alert"


### Suspend 

Tgus action syspens the virtual Machine directly on Vcenter Server. 

Suspend is a remediation action that can be enforced by following this procedure: 
1. Select your Scope from Dashboard. 
2. Click on "Services" tab. 
3. Select "Edit service."
4. Click on "Rules" tab. 
5. For your remediation 
action select "Block and send alert"

### Power Off

Virtual machine is powerted off directly on the vCetner Server.

"Power Off" is a remediation action that can be enforced by following this procedure: 
1. Select your Scope from Dashboard. 
2. Click on "Services" tab. 
3. Select "Edit service."
4. Click on "Rules" tab. 
5. For your remediation 
action select "Power Off"

### Snapshot 

This action takes a snapshot of the virtual machine.

Snapshot is a remediation action that can be enforced by following this procedure: 
1. Select your Scope from Dashboard. 
2. Click on "Services" tab. 
3. Select "Edit service."
4. Click on "Rules" tab. 
5. For your remediation 
action select "Snapshot"


