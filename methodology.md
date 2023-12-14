# ⚙ Methodology

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

### Firewall

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

* Domain
* Domain Controller
* Users
* Computers
* Domain and Enterprise Administrators
* OUs
* GPOs
* SPNs&#x20;

### Forests and Trusts

[forests-and-trusts.md](ad-enumeration/forests-and-trusts.md "mention")





