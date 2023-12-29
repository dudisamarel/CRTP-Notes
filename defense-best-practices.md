# 🛡 Mitigations

## Kerberoast

* Service Account Passwords should be hard to guess (greater than 35 characters)
* &#x20;Use Group Managed Service Accounts which automatic changes the password periodically

## Golden Ticket

* Change the password of the krbtgt account twice as password history is maintained for the account.

## Skeleton Keys

* &#x20;Running lsass.exe as a protected process is really handy as it forces an attacker to load a kernel mode driver.&#x20;
* &#x20;Make sure that you test it thoroughly as many drivers and plugins may not load with the protection.

## DSRM

* Regularly change DSRM passwords on all Domain Controllers that run DSRM. Ensuring the passwords are different across controllers.
* Monitor for the registry key `DsrmAdminLogonBehaviour` in `HKLM:\System\CurrentControlSet\Control\Lsa\` being set to the value of 1 or 2.

## Custom SSP

* Monitor for changes of the registry `HKLM:\System\CurrentControlSet\Control\Lsa\SecurityPackages`

## MITRE ATT\&CK

MITRE ATT\&CK® is a globally accessible knowledge base of adversary tactics and techniques based on real-world observations including mitigations.

[https://attack.mitre.org/](https://attack.mitre.org/)

