---
description: >-
  This Methodology is intended to help to stay focus and don't miss anything
  during the exam.
---

# ⚙ CRTP Methodology

{% hint style="info" %}
### **Important to know:**

{% hint style="warning" %}
CRTP consists on **Live Of The Land** then no phishing, no exploits, and no CVEs.
{% endhint %}

{% hint style="info" %}
The exam environment won't include any tools, but we can add the tools we need to a specific folder in Windows Defender exclusion settings.
{% endhint %}

{% hint style="success" %}
What you have learned in the course is everything you need in order to pass the exam.
{% endhint %}

{% hint style="info" %}
The exam goal is to execute OS command on 5 targets not matter what privileges the user have
{% endhint %}
{% endhint %}



## 0. Defenses&#x20;

### Policy language mode

Enumerate PS Language Mode - [#language-mode](basic-enumeration/protection.md#language-mode "mention")

if the Language is constrained:\
try to bypass using PS v2 or  [#list-applocker-rules](basic-enumeration/protection.md#list-applocker-rules "mention")

### Bypass AMSI&#x20;

Bypass AMSI every new PowerShell session -[#amsi-bypass](misc/bypass-defenses.md#amsi-bypass "mention")

Also [#invisi-shell](misc/bypass-defenses.md#invisi-shell "mention") can be used

### Bypass Defender

[#windows-defender](misc/bypass-defenses.md#windows-defender "mention")

### Turn off Firewall

[#firewall](misc/bypass-defenses.md#firewall "mention")

## 1. Local Privilege Escalation

check our privileges using `whoami /all`

Use [#powerup-1](privilege-escalation/local-privilege-escalation.md#powerup-1 "mention") and discover the vectors:

* Unquoted Service
* Modifiable Service File
* Modifiable Service

Once we are Local admin use [lsass-dump.md](lateral-movement/lsass-dump.md "mention") to find other users logons

## 2. Domain Enumeration

### ACL

[acl.md](ad-enumeration/acl.md "mention")

Better to write down the interesting ACL so they might be useful later

**BloodHound** is very useful visualizing ACLs

### General

[gnereral.md](ad-enumeration/gnereral.md "mention")

Start to build up a mind map for attacking paths

* Domain
* Domain Controller&#x20;
* Users
* Computers
* Domain and Enterprise Administrators
* OUs
* GPOs
* SPNs

### Forests and Trusts

Understand trusts and map them between the domains

[forests-and-trusts.md](ad-enumeration/forests-and-trusts.md "mention")

## 3. Domain Privileges Escalation

### Reverse shell by abusing Jenkins

### User Hunting

[#user-hunting](ad-enumeration/gnereral.md#user-hunting "mention") - Hunt for local admin access

### AD CS

[ad-cs.md](privilege-escalation/cross-domain-privilege-escalation/ad-cs.md "mention")&#x20;

### Delegations

[delegations.md](privilege-escalation/domain-privilege-escalation/delegations.md "mention") - Search for Delegations

### ACL

[acl.md](persistence/acl.md "mention")&#x20;

abusing ACL can lead&#x20;

* &#x20;[#resource-based-delegation](privilege-escalation/domain-privilege-escalation/delegations.md#resource-based-delegation "mention")
* [dc-sync.md](lateral-movement/dc-sync.md "mention")
* [security-descriptors.md](persistence/security-descriptors.md "mention")
* Reset user password

## 4. Lateral Movement

### Bypass Defenses

[#0.-defenses](crtp-methodology.md#0.-defenses "mention")

### OPTH

[lsass-dump.md](lateral-movement/lsass-dump.md "mention") after getting Domain privileges then moving inside the domain using [over-pass-the-hash.md](lateral-movement/over-pass-the-hash.md "mention")then spawn PowerShell session.

### User Hunting

[#user-hunting](ad-enumeration/gnereral.md#user-hunting "mention") - Hunt for local admin access

## 5. Cross Domain Privilege Escalation

### MSSQL

[mssql-servers.md](privilege-escalation/cross-domain-privilege-escalation/mssql-servers.md "mention")&#x20;

### Inter-Realm TGT

[trusts.md](privilege-escalation/cross-domain-privilege-escalation/trusts.md "mention") - After abusing Trust keys or krbtgt of trusted domain it is possible to abuse other forest abusing Inter-Realm TGT&#x20;





