# Quick Reference: Malware Detection & Removal

## 🚨 EMERGENCY RESPONSE GUIDE 🚨

### If you've executed files from this repository:

#### Immediate Actions (First 5 Minutes)

1. **Disconnect Network**
   ```
   - Unplug Ethernet cable
   - Disable WiFi
   - Turn off mobile hotspot
   ```

2. **Kill Suspicious Processes**
   ```powershell
   # Open PowerShell as Administrator and run:
   Get-Process | Where-Object {$_.ProcessName -match "powershell|wscript|cscript"} | Stop-Process -Force
   ```

3. **Check for Dropped Files**
   ```powershell
   # Check LocalAppData for malware
   dir $env:LOCALAPPDATA\*.vbe
   dir $env:LOCALAPPDATA\*.txt
   
   # Check Public folder
   dir C:\Users\Public\wallpaper.jpg
   ```

4. **Remove Malicious Files**
   ```powershell
   # If found, delete them
   Remove-Item $env:LOCALAPPDATA\ryuk.vbe -Force -ErrorAction SilentlyContinue
   Remove-Item C:\Users\Public\wallpaper.jpg -Force -ErrorAction SilentlyContinue
   ```

#### Detection (Next 15 Minutes)

5. **Check Running Processes**
   ```powershell
   # List all PowerShell processes
   Get-Process powershell | Format-List *
   
   # Check for hidden windows
   Get-Process | Where-Object {$_.MainWindowTitle -eq ""}
   ```

6. **Check Network Connections**
   ```powershell
   # Look for connections to Telegram
   netstat -ano | findstr "443"
   Get-NetTCPConnection | Where-Object {$_.RemoteAddress -notlike "127.*"}
   ```

7. **Review Event Logs**
   ```powershell
   # Check PowerShell execution logs
   Get-WinEvent -LogName "Windows PowerShell" -MaxEvents 50 | Format-List TimeCreated,Message
   
   # Check for script block logging
   Get-WinEvent -LogName "Microsoft-Windows-PowerShell/Operational" -MaxEvents 50
   ```

#### Cleanup (Next 30 Minutes)

8. **Full Antivirus Scan**
   - Update antivirus definitions
   - Run full system scan
   - Quarantine detected threats

9. **Clear Temporary Files**
   ```powershell
   # Clear temp directories
   Remove-Item $env:TEMP\* -Recurse -Force -ErrorAction SilentlyContinue
   Remove-Item $env:TMP\* -Recurse -Force -ErrorAction SilentlyContinue
   ```

10. **Check Scheduled Tasks**
    ```powershell
    # Look for suspicious scheduled tasks
    Get-ScheduledTask | Where-Object {$_.TaskPath -notlike "\Microsoft\*"} | Format-List TaskName,TaskPath,State
    ```

11. **Check Startup Programs**
    ```powershell
    # Check registry Run keys
    Get-ItemProperty "HKCU:\Software\Microsoft\Windows\CurrentVersion\Run"
    Get-ItemProperty "HKLM:\Software\Microsoft\Windows\CurrentVersion\Run"
    ```

#### Recovery (Next Hour)

12. **Change All Passwords**
    - Email accounts
    - Banking and financial accounts
    - Social media accounts
    - Work accounts
    - Shopping sites
    
    ⚠️ **Change passwords from a different, clean device**

13. **Monitor Accounts**
    - Check for unauthorized transactions
    - Review login history
    - Enable 2FA on all accounts

14. **Consider System Reinstallation**
    - For critical systems: Full OS reinstall recommended
    - For home systems: Deep clean + monitoring may be acceptable
    - Backup data first (scan backups for malware)

---

## 🔍 Detection Rules for Security Teams

### YARA Rule

```yara
rule Ryuk_VBE_RAT_Downloader {
    meta:
        description = "Detects PowerShell scripts downloading Ryuk VBE RAT"
        author = "Security Analysis"
        date = "2026-01-04"
        severity = "critical"
    
    strings:
        $telegram_api = "api.telegram.org/bot" ascii wide
        $bot_token = "8263447327:AAH2UWtHaUU0i_3OmxK7mzmRLTK8MfsWzSk" ascii wide
        $ryuk_url = "SCRRC4ryuk.vbe" ascii wide nocase
        $hide_window = "ShowWindow" ascii wide
        $github_source = "github.com/Mafin111/MafinREP111" ascii wide
        $sleep_delay = "Start-Sleep -Seconds 444" ascii wide
        
    condition:
        3 of them
}
```

### Sigma Rule (Windows Event Logs)

```yaml
title: Ryuk VBE RAT PowerShell Execution
status: experimental
description: Detects PowerShell downloading and executing Ryuk VBE RAT
author: Security Analysis
date: 2026/01/04
logsource:
    product: windows
    service: powershell
detection:
    selection:
        EventID: 4104
    keywords:
        - 'api.telegram.org'
        - 'SCRRC4ryuk.vbe'
        - 'ShowWindow'
        - 'Start-Sleep -Seconds 444'
    condition: selection and 2 of keywords
falsepositives:
    - Unlikely
level: critical
```

### Network Detection (Snort/Suricata)

```
alert http any any -> any any (msg:"Ryuk RAT Download Attempt"; \
    content:"github.com/Mafin111/MafinREP111"; http_uri; \
    content:"SCRRC4ryuk.vbe"; http_uri; \
    sid:1000001; rev:1;)

alert http any any -> any any (msg:"Ryuk RAT Telegram C2 Communication"; \
    content:"api.telegram.org/bot8263447327"; http_uri; \
    sid:1000002; rev:1;)
```

### Firewall Blocking Rules

```powershell
# Block malicious GitHub repository
New-NetFirewallRule -DisplayName "Block Malicious GitHub Repo" `
    -Direction Outbound -Action Block `
    -RemoteAddress "github.com" `
    -RemotePort 443 -Protocol TCP `
    -Program "%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe"

# Block Telegram API from PowerShell
New-NetFirewallRule -DisplayName "Block PowerShell Telegram Access" `
    -Direction Outbound -Action Block `
    -RemoteAddress "149.154.160.0/20" `
    -Program "%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe"
```

---

## 📊 Malware Behavior Timeline

```
T+0s     : Script execution starts
T+0s     : PowerShell window hidden
T+1s     : Decoy document created in %LocalAppData%
T+2s     : Decoy document opened (distracts victim)
T+3s     : Username collected
T+4s     : Telegram notification sent to attacker
T+444s   : Sleep timer ends (7 minutes 24 seconds)
T+445s   : Download ryuk.vbe from GitHub
T+450s   : Execute ryuk.vbe hidden
T+455s   : RAT payload active, C2 established
T+460s   : Script exits (malware persists)
```

---

## 🎯 Attack Flow Diagram

```
┌─────────────────────────────────────────────────────────────┐
│  INITIAL COMPROMISE                                         │
│  • Victim receives fake business document                   │
│  • Opens .ps1 script (thinking it's a document)            │
└────────────────────────┬────────────────────────────────────┘
                         ↓
┌─────────────────────────────────────────────────────────────┐
│  STAGE 1: CONCEALMENT                                       │
│  • Hide PowerShell window using Windows API                 │
│  • Suppress error messages                                  │
└────────────────────────┬────────────────────────────────────┘
                         ↓
┌─────────────────────────────────────────────────────────────┐
│  STAGE 2: SOCIAL ENGINEERING                                │
│  • Create legitimate-looking text file                      │
│  • Open document in Notepad (victim distracted)            │
└────────────────────────┬────────────────────────────────────┘
                         ↓
┌─────────────────────────────────────────────────────────────┐
│  STAGE 3: DATA EXFILTRATION                                 │
│  • Collect Windows username                                 │
│  • Send to attacker via Telegram Bot API                   │
└────────────────────────┬────────────────────────────────────┘
                         ↓
┌─────────────────────────────────────────────────────────────┐
│  STAGE 4: ANTI-DETECTION DELAY                              │
│  • Sleep for 444 seconds (7.4 minutes)                      │
│  • Evade behavioral detection systems                       │
└────────────────────────┬────────────────────────────────────┘
                         ↓
┌─────────────────────────────────────────────────────────────┐
│  STAGE 5: PAYLOAD DELIVERY                                  │
│  • Download ryuk.vbe from GitHub                            │
│  • Save to %LocalAppData%                                   │
└────────────────────────┬────────────────────────────────────┘
                         ↓
┌─────────────────────────────────────────────────────────────┐
│  STAGE 6: PAYLOAD EXECUTION                                 │
│  • Execute VBE script hidden                                │
│  • No window displayed                                      │
└────────────────────────┬────────────────────────────────────┘
                         ↓
┌─────────────────────────────────────────────────────────────┐
│  COMPROMISE COMPLETE                                        │
│  • RAT installed and active                                 │
│  • Attacker has remote access                               │
│  • System fully compromised                                 │
└─────────────────────────────────────────────────────────────┘
```

---

## 🛡️ Prevention Best Practices

### For Organizations

1. **Email Security**
   - Implement attachment filtering
   - Block .ps1, .vbe, .scr file extensions
   - Use sandboxing for attachments

2. **Endpoint Protection**
   - Enable PowerShell logging (Module, Script Block, Transcription)
   - Deploy EDR solutions
   - Use application whitelisting

3. **Network Security**
   - Block Telegram API from workstations
   - Monitor for GitHub downloads from scripts
   - Implement web filtering

4. **User Training**
   - Regular security awareness training
   - Phishing simulations
   - Incident reporting procedures

### For Individuals

1. **File Handling**
   - Never open email attachments from unknown senders
   - Verify sender identity before opening files
   - Be suspicious of business documents as script files

2. **System Configuration**
   - Keep Windows and antivirus updated
   - Enable Windows Defender real-time protection
   - Use standard user accounts (not Administrator)

3. **PowerShell Security**
   - Set PowerShell execution policy to restricted:
     ```powershell
     Set-ExecutionPolicy Restricted -Scope CurrentUser
     ```
   - Enable script block logging
   - Review PowerShell history regularly

---

## 📞 Reporting & Resources

### Report Malicious Activity

- **GitHub:** https://support.github.com/contact/report-abuse
- **Telegram:** https://telegram.org/faq#q-there-39s-illegal-content-on-telegram-how-do-i-take-it-down
- **Microsoft Security:** https://www.microsoft.com/en-us/wdsi/filesubmission
- **US-CERT:** https://www.cisa.gov/report

### Additional Resources

- [MITRE ATT&CK Framework](https://attack.mitre.org/)
- [Malware Analysis Tutorials](https://www.malware-traffic-analysis.net/)
- [PowerShell Security Best Practices](https://docs.microsoft.com/en-us/powershell/scripting/learn/security/)

---

**Last Updated:** 2026-01-04  
**Document Version:** 1.0  
**Status:** Active Threat
