---
description: Search for protections so we will need to bypass or evade.
---

# Protection

### Windows Defender Status

```powershell
Get-MpComputerStatus
```

### Language Mode

```powershell
$ExecutionContext.SessionState.LanguageMode
```

### List AppLocker Rules

<pre class="language-powershell"><code class="lang-powershell"><strong>Get-AppLockerPolicy -Effective | select -ExpandProperty RuleCollections
</strong></code></pre>

Test AppLocker Policy

{% code fullWidth="false" %}
```powershell
Get-AppLockerPolicy -Local | Test-AppLockerPolicy -path C:\Windows\System32\cmd.exe -User Everyone
```
{% endcode %}
