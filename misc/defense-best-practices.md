# Defense  Best Practices

## Golden Ticket

As a good practice, it is recommended to change the password of the krbtgt account twice as password history is maintained for the account.





## DSRM

* Regularly change DSRM passwords on all Domain Controllers that run DSRM. Ensuring the passwords are different across controllers.
* Monitor for the registry key `DsrmAdminLogonBehaviour` in `HKLM:\System\CurrentControlSet\Control\Lsa\` being set to the value of 1 or 2.

