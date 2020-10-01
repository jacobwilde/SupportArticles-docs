---
title: All members of a group may not be returned
description: Describes a problem that may occur where the Active Directory Service Interfaces WinNT provider does not return all members of a group. This problem occurs when you try to enumerate a local group or a domain local group.
ms.date: 09/21/2020
author: Deland-Han 
ms.author: delhan
manager: dscontentpm
audience: itpro
ms.topic: troubleshooting
ms.prod: windows-server
localization_priority: medium
ms.reviewer: kaushika
ms.prod-support-area-path: User, computer, group, and object management
ms.technology: ActiveDirectory
---
# All members of a group may not be returned when you enumerate members of a group by using the Active Directory Service Interfaces WinNT provider

This article provides a solution to an issue that all members of a group may not be returned when you enumerate members of a group by using the Active Directory Service Interfaces WinNT provider.

_Original product version:_ &nbsp; Windows Server 2012 R2  
_Original KB number:_ &nbsp; 321538

## Symptoms

When you enumerate members of a group by using the Active Directory Service Interfaces (ADSI) WinNT provider, and you use a binding string, a problem may occur. Some members of the group may not be returned if the group that you are enumerating has members of the following:

- A local group that contains domain users and domain groups as members
- A domain local group that contains groups from trusted domains as members

## Workaround

You can use the GetObject method to obtain the full member list. The GetObject method uses the credentials for the currently logged on user. The following code example demonstrates this.

```console
GetObject("WinNT://<server>/<group>,group")
```

If the account that you want to use for enumerating the group is not the currently logged on user, you can use impersonation before you use the GetObject method.

## Status

This behavior is by design.

## More information

### Steps to reproduce the problem

The Active Directory Service Interfaces WinNT provider does not connect to more than one server to compile the member list. This problem occurs if explicit credentials are passed. Therefore, only a partial member list is returned. For example, if you run the following script to enumerate a local group that contains a group from a trusted domain, all members of the group are returned, except the group from the trusted domain.

```vb
'Start of the script
Dim oRoot
Dim oSourceGroup
Dim oMember
Const ADS_SECURE_AUTHENTICATION=1

'Binding
Set oRoot = GetObject("WinNT:")
Set oTargetGroup = oRoot.OpenDSObject("WinNT://<server>/<group>,group", "<domain>\<user>", "<password>",ADS_SECURE_AUTHENTICATION)'All of the following are placeholders: <server> <group> <domain> <user> <password>
msgbox oTargetGroup.ADSPath

For Each oMember in oTargetGroup.Members
msgbox oMember.ADsPath
Next
'End of the script
```

> [!IMPORTANT]
> We do not recommend that you pass credentials in Active Directory Service Interfaces by using the Active Directory Service Interfaces WinNT provider.

For additional information, click the following article number to view the article in the Microsoft Knowledge Base:

[218497](https://support.microsoft.com/help/218497) How to impersonate a user from Active Server Pages  

## References

For additional information about programming with Active Directory Service Interfaces, visit the following Microsoft Developer Network (MSDN) Web site:  
[https://msdn.microsoft.com/library/aa772170.aspx](https://msdn.microsoft.com/library/aa772170.aspx)