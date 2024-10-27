# ACL

An **access control list** (ACL) is a list of **access control entries** (ACE).\
Each **ACE** specifies the access rights of a user or group.

The **security descriptor** for an object can contain two types of ACLs:

* **DACL** - Defines the permission of  a user or group have on object
* **SACL** - Logs success and failure messages when an object is accessed.\


Active Directory object permissions:

* **GenericAll** - full rights to the object (add users to a group or reset user's password)
* **GenericWrite** - update object's attributes (i.e logon script)
* **WriteOwner** - change object owner to attacker controlled user take over the object
* **WriteDACL** - modify object's ACEs and give attacker full control right over the object
* **AllExtendedRights** - ability to add user to a group or reset password
* **ForceChangePassword** - ability to change user's password
* **Self (Self-Membership)** - ability to add yourself to a group



{% tabs %}
{% tab title="PowerView" %}
Get the ACLs associated with the specified object

<pre class="language-powershell" data-overflow="wrap"><code class="lang-powershell">Get-DomainObjectAcl -SamAccountName student1 -ResolveGUIDs

 Get-DomainObjectAcl -ResolveGUIDs -Identity "target" | ? {$_.SecurityIdentifier -eq (Convert-NameToSid foothold)}
<strong>
</strong></code></pre>

Get the ACLs associated with the specified group

{% code overflow="wrap" %}
```powershell
Get-DomainObjectAcl -SearchBase "LDAP://CN=Domain Admins,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local" -ResolveGUIDs -Verbose

Get-DomainObjectAcl -Identity "Domain Admins" -ResolveGUIDs -Verbose

# Check replication permission
Get-DomainObjectAcl -SearchBase "DC=dollarcorp,DC=moneycorp,DC=local" -SearchScope Base -ResolveGUIDs | ?{($_.ObjectAceType -match 'replication-get') -or ($_.ActiveDirectoryRights -match 'GenericAll')} | ForEach-Object {$_ | Add-Member NoteProperty 'IdentityName' $(Convert-SidToName $_.SecurityIdentifier);$_} 
```
{% endcode %}

Search for interesting ACEs

{% code overflow="wrap" %}
```powershell
Find-InterestingDomainAcl -ResolveGUIDs | ?{$_.IdentityReferenceName -match "student1"}
```
{% endcode %}

Get the ACLs associated with the specified path

```powershell
Get-PathAcl -Path "\\dcorp-dc.dollarcorp.moneycorp.local\sysvol"
```
{% endtab %}

{% tab title="AD Module" %}
Get the ACLs associated with the specified prefix to be used for search

{% code overflow="wrap" %}
```powershell
(Get-Acl 'AD:\CN=Administrator,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local').Access
```
{% endcode %}
{% endtab %}
{% endtabs %}

