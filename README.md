# ⚠️ MALWARE REPOSITORY - DO NOT USE ⚠️

## CRITICAL SECURITY WARNING

**THIS REPOSITORY CONTAINS MALICIOUS SOFTWARE (MALWARE)**

🚨 **DO NOT DOWNLOAD OR EXECUTE ANY FILES FROM THIS REPOSITORY** 🚨

---

## What is this repository?

This repository has been analyzed and confirmed to contain:

- **Remote Access Trojan (RAT)** payloads
- **Information stealing** malware
- **Social engineering** attack scripts
- **Credential theft** mechanisms

## Threat Summary

- **Threat Level:** CRITICAL
- **Malware Type:** Remote Access Trojan, Information Stealer
- **Target:** Russian-speaking users (small businesses, freelancers, designers)
- **Distribution:** Social engineering via fake business documents

## What does this malware do?

1. **Hides itself** using Windows API calls
2. **Displays fake business documents** to deceive victims
3. **Steals your username** and sends it to attackers via Telegram
4. **Downloads additional malware** (Ryuk RAT variant)
5. **Establishes remote access** to your computer
6. **Runs hidden** in the background

## Files in this repository

### PowerShell Malware Scripts (`ps1/` directory)
All 8 PowerShell scripts follow the same attack pattern:
- `ego.ps1`, `fifia.ps1`, `gogo.ps1`, `kira.ps1`, `kisa.ps1`, `meow.ps1`, `torch.ps1`, `var.ps1`

### Binary Malware
- `SCRRC4ryuk.vbe` - VBScript encoded RAT payload
- `TelegramWorker.scr` - Malicious executable disguised as screensaver
- `WmiPrvSE.scr` - Malicious executable disguised as WMI process
- `gedion.scr` - Malicious executable
- `defendnot-loader.exe` - Malware loader with anti-defense capabilities
- `defendnot.dll` - Malicious DLL library
- `install.exe` - Malware installer
- `wallpaper.ps1` - Desktop wallpaper hijacking script

## Indicators of Compromise (IOCs)

> **Note:** The credentials listed below belong to the malware operators (attackers), not victims. They are intentionally unredacted for:
> - Security team detection and blocking
> - Reporting to Telegram abuse team
> - Law enforcement investigation
> - Creating detection signatures

### Network Indicators
- **C2 Server:** Telegram Bot API (`api.telegram.org`)
- **Bot Token:** `8263447327:AAH2UWtHaUU0i_3OmxK7mzmRLTK8MfsWzSk` (attacker's token)
- **Chat ID:** `7063407604` (attacker's chat)
- **Malware Source:** `https://github.com/Mafin111/MafinREP111/`

### File Indicators
- `%LocalAppData%\ryuk.vbe`
- Desktop wallpaper modifications
- Hidden PowerShell processes
- Suspicious `.scr` file executions

## If you've run these files

**Take immediate action:**

1. ✅ **Disconnect from the internet** immediately
2. ✅ **Run a full antivirus scan** with updated definitions
3. ✅ **Check for suspicious files** in `%LocalAppData%`
4. ✅ **Look for unusual processes** (especially hidden PowerShell)
5. ✅ **Change all your passwords** from a clean device
6. ✅ **Contact your IT security team** (if applicable)
7. ✅ **Consider full system reinstallation** for sensitive systems

## Reporting

This malware should be reported to:

- **GitHub:** Report repository `Mafin111/MafinREP111` as malicious
- **Telegram:** Report bot token to Telegram abuse team
- **Antivirus vendors:** Submit samples for detection updates
- **Law enforcement:** If you are a victim

## Detailed Analysis

For a complete technical analysis, see: [SECURITY_ANALYSIS.md](./SECURITY_ANALYSIS.md)

---

## Legal Notice

**This code is illegal malware.** Creating, distributing, or using this software violates computer crime laws in most countries including:

- Computer Fraud and Abuse Act (CFAA) - United States
- Computer Misuse Act - United Kingdom
- Similar laws in the European Union, Russia, and worldwide

**Possession and distribution of this malware may be a criminal offense.**

---

## Purpose of This Analysis

This analysis document was created to:

✅ **Identify** the malicious nature of the repository  
✅ **Document** the malware's capabilities and behavior  
✅ **Provide IOCs** for security teams to detect and block  
✅ **Warn users** about the dangers of these files  
✅ **Assist** in malware removal and system recovery  

---

## Repository Status

**Analysis Date:** 2026-01-04  
**Status:** MALWARE CONFIRMED  
**Recommendation:** DELETE OR QUARANTINE THIS REPOSITORY  

---

**🔒 Stay Safe - Do Not Execute Unknown Scripts 🔒**

For questions about this analysis, please consult with cybersecurity professionals.
