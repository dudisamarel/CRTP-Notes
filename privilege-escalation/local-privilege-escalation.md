# Local Privilege Escalation



{% hint style="info" %}
The CRTP exam consists of 5 target servers in addition to a foothold student machine.\
The goal is to OS level command execution on all 5 targets not matter what the privileges of the user.
{% endhint %}

####

## Vectors

#### There are various ways of locally escalating privileges on Windows box:&#x20;

* Missing patches – Automated deployment and AutoLogon passwords in clear text&#x20;
* &#x20;AlwaysInstallElevated (Any user can run MSI as SYSTEM)&#x20;
* &#x20;Misconfigured Services – DLL Hijacking and more&#x20;
* NTLM Relaying a.k.a. Won't Fix



This guide offer a sufficiently comprehensive overview of the course material for local privilege escalation

{% embed url="https://github.com/0xStarlight/CRTP-Notes/blob/main/2-Local-Priv-Esc/1-Local-PrivEsc.md" %}

## Tools

### PowerUp

{% embed url="https://github.com/PowerShellMafia/PowerSploit/blob/master/Privesc/PowerUp.ps1" %}

### WinPEAS

{% embed url="https://github.com/carlospolop/PEASS-ng/tree/master/winPEAS" %}

### Privesc

{% embed url="https://github.com/enjoiz/Privesc" %}

### Automated checks

```powershell
# PowerUp
Invoke-AllChecks

# winPEAS
winPEASx64.exe 

# Privesc
Invoke-PrivEsc
```

## Services

#### PowerUp

```powershell
# Get services with unquoted paths and spaces
Get-ServiceUnquoted -Verbose

# Get services where current user can write to binary path
Get-ModifiableServiceFile -Verbose

# Get the services whose configuration current user can modify
Get-ModifiableService -Verbose
```

##

