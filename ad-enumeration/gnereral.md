# Gnereral

### Tools

[PowerView](https://github.com/ZeroDayLab/PowerSploit/blob/master/Recon/PowerView.ps1)

```powershell
. C:\AD\Tools\PowerView.ps1
```

[AD module](https://github.com/samratashok/ADModule) - MS singed&#x20;

<pre class="language-powershell"><code class="lang-powershell"><strong>Import-Module C:\AD\Tools\ADModule-master\Microsoft.ActiveDirectory.Management.dll
</strong>Import-Module C:\AD\Tools\ADModule-master\ActiveDirectory\ActiveDirectory.psd1
</code></pre>

### BloodHound

BloodHound Versions:

* [BloodHound Legacy](https://github.com/BloodHoundAD/BloodHound)&#x20;
* [BloodHound ](https://github.com/SpecterOps/BloodHound)

```bash
# start db server
sudo neo4j console

# run bloodhound
bloodhound 
```

SharpHound collector:

* [SharpHound PowerShell](https://github.com/BloodHoundAD/BloodHound/blob/master/Collectors/SharpHound.ps1)
* [SharpHound executable](https://github.com/BloodHoundAD/BloodHound/blob/master/Collectors/SharpHound.exe)

```powershell
# PowerShell
. SharpHound.ps1
Invoke-BloodHound -CollectionMethod All 
Invoke-BloodHound –Steatlh # Remove noisy collections like RDP,DCOM,PSRemote and Local Admin
Invoke-BloodHound -ExcludeDCs # Avoid MDI

# executable
SharpHound.exe
SharpHound.exe –-steatlh # Remove noisy collections like RDP,DCOM,PSRemote and Local Admin
```

### Domain

{% tabs %}
{% tab title="PowerView" %}
Get Current domain

```powershell
Get-Domain
```

Get Object of another domain

```powershell
Get-Domain -Domain moneycorp.local
```

Get domain SID for the current domain

<pre class="language-powershell"><code class="lang-powershell"><strong>Get-DomainSID
</strong></code></pre>

Get domain policy for the current domain

```powershell
Get-DomainPolicyData
(Get-DomainPolicyData).systemaccess
```

Get domain policy for another domain

```powershell
(Get-DomainPolicyData -domain moneycorp.local).systemaccess
```
{% endtab %}

{% tab title="AD module" %}
Get Current domain

```powershell
Get-ADDomain
```

Get Object of another domain

```powershell
Get-ADDomain -Identity moneycorp.local
```

Get domain SID for the current domain

```powershell
(Get-ADDomain).DomainSID
```
{% endtab %}
{% endtabs %}

### Domain controller

{% tabs %}
{% tab title="PowerView" %}
Get domain controllers for the current domain

```powershell
Get-DomainController
```

Get domain controllers for another domain

```powershell
Get-DomainController -Domain moneycorp.local
```
{% endtab %}

{% tab title="AD module" %}
Get domain controllers for the current domain

```powershell
Get-ADDomainController
```

Get domain controllers for another domain

```powershell
Get-ADDomainController -DomainName moneycorp.local -Discover
```
{% endtab %}
{% endtabs %}

### Domain users

{% tabs %}
{% tab title="PowerView" %}
Get a list of users in the current domain

```powershell
Get-DomainUser
Get-DomainUser -Identity student1
```

Get list of all properties for users in the current domain

```powershell
Get-DomainUser -Identity student1 -Properties * 
Get-DomainUser -Properties samaccountname,logonCount
```

Search for a particular string in a user's attributes

```powershell
Get-DomainUser -LDAPFilter "Description=*built*" | Select name,Description
```

Get actively logged users on a computer (requires local admin privileges)

```powershell
Get-NetLoggedon -ComputerName dcorp-adminsrv

```

Get locally logged users on a computer (requires remote registry)

```powershell
Get-LoggedonLocal -ComputerName dcorp-adminsrv
```

Get the last logged user on a computer (requires admin privileges and remote registry)

```powershell
Get-LastLoggedOn -ComputerName dcorp-adminsrv
```
{% endtab %}

{% tab title="AD module" %}
Get a list of users in the current domain

```powershell
Get-ADUser -Filter * -Properties *
Get-ADUser -Identity student1 -Properties *
```

Get list of all properties for users in the current domain

```powershell
Get-ADUser -Filter * -Properties * | select -First 1 | Get-Member -MemberType *Property | select Name
Get-ADUser -Filter * -Properties * | select name,logoncount,@{expression={[datetime]::fromFileTime($_.pwdlastset)}}
```

Search for a particular string in a user's attributes

```powershell
Get-ADUser -Filter 'Description -like "*built*"' -Properties Description | select name,Desc
```
{% endtab %}
{% endtabs %}

### Domain Computers

{% tabs %}
{% tab title="PowerView" %}
Get a list of computers in the current domain

```powershell
Get-DomainComputer | select Name
Get-DomainComputer -OperatingSystem "*Server 2022*"
Get-DomainComputer -Ping
```
{% endtab %}

{% tab title="AD Module" %}
Get a list of computers in the current domain

```powershell
Get-ADComputer -Filter * | select Name
Get-ADComputer -Filter * -Properties *
Get-ADComputer -Filter 'OperatingSystem -like "*Server 2022*"' -Properties OperatingSystem | select Name,OperatingSystem
Get-ADComputer -Filter * -Properties DNSHostName | %{TestConnection -Count 1 -ComputerName $_.DNSHostName}
```
{% endtab %}
{% endtabs %}

### Domain Groups



{% tabs %}
{% tab title="PowerView" %}
Get all the groups

```powershell
Get-DomainGroup | select Name
Get-DomainGroup -Domain <targetdomain>
```

Get all groups containing the word "admin" in group name

```powershell
Get-DomainGroup *admin*
```

Get all the members of the Domain Admins group

```powershell
Get-DomainGroupMember -Identity "Domain Admins" -Recurse
```

Get the group membership for a user

```powershell
Get-DomainGroup -UserName "student1"
```
{% endtab %}

{% tab title="AD Module" %}
Get all the groups

```powershell
Get-ADGroup -Filter * | select Name 
Get-ADGroup -Filter * -Properties *
```

Get all groups containing the word "admin" in group name

```powershell
Get-ADGroup -Filter 'Name -like "*admin*"' | select Name
```

Get all the members of the Domain Admins group

```powershell
Get-ADGroupMember -Identity "Domain Admins" -Recursive
```

Get the group membership for a user

```powershell
Get-ADPrincipalGroupMembership -Identity student1
```
{% endtab %}
{% endtabs %}

### Group Policy

{% tabs %}
{% tab title="PowerView" %}
Get list of GPO in current domain

```powershell
Get-DomainGPO
Get-DomainGPO -ComputerIdentity dcorp-student1
```

Get GPO(s) which use Restricted Groups

```powershell
Get-DomainGPOLocalGroup
```

Get users which are in a local group of a machine using GPO

```powershell
Get-DomainGPOComputerLocalGroupMapping -ComputerIdentity dcorp-student1
```

Get machines where the given user is member of a specific group

```powershell
Get-DomainGPOUserLocalGroupMapping -Identity student1 -Verbose 
```
{% endtab %}
{% endtabs %}

### Organization Units



{% tabs %}
{% tab title="PowerView" %}
Get OUs in a domain

{% code overflow="wrap" %}
```powershell
# Get all domain OUs
Get-DomainOU

# Get all computers inside an OU
(Get-DomainOU -Identity StudentMachines).distinguishedname | %{Get-DomainComputer -SearchBase $_} | select name
```
{% endcode %}

Using `Get-NetOU`

{% code overflow="wrap" %}
```powershell
# Get all computers inside an OU
(Get-NetOU -Identity StudentMachines).distinguishedname | %{Get-DomainComputer -SearchBase $_} | select name

# Get GPO applied on an OU 
Get-NetOU -Identity "StudentMachines" | select gplink # Get GPO ID
Get-DomainGPO -Identity "{0D1CC23D-1F20-4EEE-AF64-D99597AE2A6E}" # Get GPO Info
```
{% endcode %}
{% endtab %}

{% tab title="AD Module" %}
Get OUs in a domain

```powershell
Get-ADOrganizationalUnit -Filter * -Properties *
```
{% endtab %}
{% endtabs %}

### Local Groups



{% tabs %}
{% tab title="PowerView" %}
List all the local groups on a machine (requires admin privileges)

```powershell
Get-NetLocalGroup -ComputerName dcorp-dc
```

Get members of the local group "Administrators" on a machine  (requires admin privileges)

```powershell
Get-NetLocalGroupMember -ComputerName dcorp-dc -GroupName Administrators
```
{% endtab %}
{% endtabs %}



### Shares



{% tabs %}
{% tab title="PowerView" %}
Find shares on hosts in current domain.

```powershell
Invoke-ShareFinder -Verbose

```

Find sensitive files on computers in the domain

```powershell
Invoke-FileFinder -Verbose
```

Get all file servers of the domain

```powershell
Get-NetFileServer
```
{% endtab %}
{% endtabs %}

### User Hunting



{% tabs %}
{% tab title="PowerView" %}
Find all machines on the current domain where the current user has local admin access

```powershell
# Very noisy
Find-LocalAdminAccess -Verbose

# Very noisy
# When SMB and RPC are blocked

# Load
. C:\AD\Tools\Find-WMILocalAdminAccess.ps1
# execute
Find-WMILocalAdminAccess

#Load
. C:\AD\Tools\Find-PSRemotingLocalAdminAccess.ps1
# execute
Find-PSRemotingLocalAdminAccess.ps1


```

Find machines  where a domain admin has sessions

<pre class="language-powershell"><code class="lang-powershell"># Very noisy
<strong>Find-DomainUserLocation -Verbose
</strong>Find-DomainUserLocation -UserGroupIdentity "RDPUsers"
Find-DomainUserLocation -CheckAccess 
Find-DomainUserLocation -Stealth # less noisy, targeting file servers

</code></pre>

List sessions on remote machines ([source](https://github.com/Leo4j/Invoke-SessionHunter))

```powershell
# Doesn’t need admin access on remote machines. 
# Uses Remote Registry and queries HKEY_USERS hive.
Invoke-SessionHunter -FailSafe

# Opsec friendly
Invoke-SessionHunter -NoPortScan -Targets C:\AD\Tools\servers.txt
```
{% endtab %}
{% endtabs %}

