---
title: Remote Control prompts for user permission
description: Provides a solution to an issue that the user is still prompted for permissions to allow the Remote Control when you try to Remote Control this user.
ms.date: 09/16/2020
author: Deland-Han 
ms.author: delhan
manager: dscontentpm
audience: itpro
ms.topic: troubleshooting
ms.prod: windows-server
localization_priority: medium
ms.reviewer: kaushika
ms.prod-support-area-path: Connecting to a session or desktop
ms.technology: RDS
---
# Remote Control always prompts for user permission

This article provides a solution to an issue that the user is still prompted for permissions to allow the Remote Control when you try to Remote Control this user.

_Original product version:_ &nbsp; Windows Server 2012 R2  
_Original KB number:_ &nbsp; 2690875

## Symptoms

You configure a user in Active Directory and uncheck "Require User's permission" under the "Remote Control" tab.

However, when you try to Remote Control this user, and the user is on the physical Console Session, the user is still prompted for permissions to allow the Remote Control.

## Cause

In the case of Remote Control the Console Session, the user property is currently not taken into consideration.

## Workaround

To work around this issue, disable the user prompt for all users with a machine policy:

Set rules for remote control of Remote Desktop Services user sessions
found under:

`computer configuration\Administrative Templates\Windows Components\Remote Desktop Services\Remote Desktop Session Host\Connections`