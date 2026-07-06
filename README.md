# Windows RAM Optimization & Performance Guide

## 🚀 Overview
This guide provides a comprehensive, two-part approach to optimizing Windows performance and freeing up RAM. It combines a **safe, automated PowerShell script** for registry tweaks with **essential manual steps** to ensure maximum performance without compromising system stability.

---

## PART 1: Automated Optimizations (The PowerShell Script)

This section uses a script to automatically adjust your system for peak performance. 

### ⚙️ What the Script Does:
1. **Adjusts Visual Performance:** Modifies registry settings to prioritize system performance over visual effects (e.g., disables window animations).
2. **Disables Windows Services:** Stops and disables the **Background Intelligent Transfer Service (BITS)**, which can consume significant background memory and network bandwidth. *(Note: If you experience issues with Windows Update later, you can turn this back on).*
3. **Clears Paging Memory at Shutdown:** Tweaks the registry to ensure your system's PageFile (virtual memory) is completely wiped every time you shut down, giving you a clean slate on your next boot.

### 🛠️ How to Run the Script:
**⚠️ WARNING: You must run this script as an Administrator.**

1. Click the Windows **Start menu** and type **PowerShell**.
2. Right-click **Windows PowerShell** and select **Run as Administrator**.
3. Copy the entire block of code below.
4. Paste it into the PowerShell window and press **Enter**.
5. **Restart your computer** for all changes to take effect.

### 💻 The Code (Copy and Paste):
```powershell
# =====================================================================
# Windows RAM Optimization Script
# WARNING: Run this as Administrator!
# =====================================================================

Write-Host "Starting Windows Optimization..." -ForegroundColor Cyan

# --- 1. Adjusting Visual Performance ---
Write-Host "Step 1: Setting visual effects to 'Adjust for best performance'..." -ForegroundColor Yellow
# Sets the master switch to "Best Performance" (Value 2)
Set-ItemProperty -Path "HKCU:\Software\Microsoft\Windows\CurrentVersion\Explorer\VisualEffects" -Name "VisualFXSetting" -Value 2
# Disables window animation and other UI heavy effects
Set-ItemProperty -Path "HKCU:\Control Panel\Desktop" -Name "UserPreferencesMask" -Value ([byte[]](90,12,03,80,10,00,00,00))

# --- 3. Disabling Windows Services ---
Write-Host "Step 3: Disabling Background Intelligent Transfer Service (BITS)..." -ForegroundColor Yellow
# Note: If Windows Update stops working, change this back to 'Manual'.
Set-Service -Name "BITS" -StartupType Disabled
Stop-Service -Name "BITS" -Force -ErrorAction SilentlyContinue

# --- 6. Clearing Paging Memory at Shutdown ---
Write-Host "Step 6: Enabling Clear PageFile at Shutdown..." -ForegroundColor Yellow
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management" -Name "ClearPageFileAtShutdown" -Value 1

Write-Host "=====================================================================" -ForegroundColor Cyan
Write-Host "Done! Please restart your computer for all changes to take effect." -ForegroundColor Green
