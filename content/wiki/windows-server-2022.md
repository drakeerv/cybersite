+++
title = "Windows Server 2022"
description = "The latest Long-Term Servicing Channel (LTSC) release of the Windows Server operating system, featuring advanced multi-layer security."

[extra]
name = "Windows Server 2022"
wikipedia_article = "Windows_Server_2022"

[[extra.infobox]]
key = "Developer"
value = "[Microsoft](w:microsoft)"

[[extra.infobox]]
key = "Release Date"
value = "August 18, 2021"

[[extra.infobox]]
key = "Code Base"
value = "[NT Kernel](w:nt-kernel) (Version 21H2)"

[[extra.infobox]]
key = "License Type"
value = "[Proprietary/Commercial](wp:Proprietary_software)"

[[extra.references]]
text = "Windows Server 2022 Documentation"
url = "https://learn.microsoft.com/en-us/windows-server/get-started/windows-server-2022"

[[extra.references]]
text = "Department of Defense STIG for Server 2022"
url = "https://public.cyber.mil/stigs/"
+++

**Windows Server 2022** is the tenth and current major Long-Term Servicing Channel (LTSC) release of the Windows Server operating system by [Microsoft](w:microsoft). Built on the foundation of Windows Server 2019, it introduces significant improvements in hybrid cloud integration, application platform capabilities, and, most importantly for administrators, **security**.

For CyberPatriot competitors and cybersecurity professionals, Server 2022 is notable for being a "Secured-core" OS, meaning it comes with specific hardware and software defenses enabled by default.

## Key Security Features
Windows Server 2022 introduces several default security configurations that differ from previous versions (2016/2019).

### Secured-core Server
This feature applies a comprehensive security protection at the hardware, firmware, and driver levels. It relies on three pillars:
1.  **Hardware Root-of-Trust:** Requires TPM 2.0 and BitLocker to verify that the system has not been tampered with during boot.
2.  **Firmware Protection:** Utilizing Dynamic Root of Trust for Measurement (DRTM) to isolate the SMM (System Management Mode).
3.  **Virtualization-based Security (VBS):** Uses hardware virtualization features to create a secure region of memory from the normal operating system. This protects credential management (LSASS) from dumping attacks (like Mimikatz).

### Network Transport Security
*   **TLS 1.3 Enabled by Default:** Server 2022 defaults to the latest version of Transport Layer Security (TLS 1.3), which eliminates obsolete cryptographic algorithms and encrypts more of the handshake process.
*   **DNS-over-HTTPS (DoH):** The DNS client in Server 2022 supports DoH, which encrypts DNS queries using the HTTPS protocol. This prevents eavesdropping and manipulation of DNS data on the network.
*   **SMB Encryption:** Improvements to Server Message Block (SMB) include AES-256-GCM/CCM encryption standards for internal file sharing.

## Hardening for CyberPatriot
In a competition setting, Windows Server 2022 images often function as Domain Controllers (Active Directory), Web Servers (IIS), or File Servers. Below are common hardening vectors.

### 1. Account and Policy Management
*   **Password Policies:** Modify the **Default Domain Policy** (if AD is active) or **Local Security Policy** (`secpol.msc`) to enforce complexity, minimum length (14+ characters), and history requirements.
*   **Lockout Policies:** Ensure account lockout thresholds are set (usually 3 to 5 attempts) to prevent brute-force attacks.
*   **User Auditing:** Use `net user` or **Active Directory Users and Computers** to disable the Guest account, rename the Administrator account, and remove unauthorized users from the "Administrators" and "Backup Operators" groups.

### 2. Service Auditing
Server 2022 installs with a smaller footprint than desktop versions, but unnecessary roles may still be present.
*   **Remove Features:** Use **Server Manager** or PowerShell to remove unneeded roles. Common vulnerability points in competitions include **FTP Server**, **Telnet Client/Server**, and **SMBv1**.
*   **PowerShell Command:**
    ```powershell
    Get-WindowsFeature | Where-Object {$_.InstallState -eq "Installed"}
    ```

### 3. Network and Firewall
*   **Windows Defender Firewall:** Ensure all three profiles (Domain, Private, Public) are turned **ON**.
*   **Inbound Rules:** Audit inbound rules to ensure only necessary ports (e.g., Port 80/443 for IIS, Port 53 for DNS) are open. Delete "Any/Any" allow rules.
*   **Remote Desktop:** If RDP is required, ensure "Network Level Authentication" (NLA) is required.

### 4. Updates and Defender
*   **Windows Update:** Even in a simulated environment, ensure the settings are configured to Install Updates Automatically.
*   **Windows Defender:** Verify Real-time protection and Cloud-delivered protection are enabled. Exclusions (files told to be ignored by antivirus) are a common hiding spot for malware in competitions.

## Common Vulnerabilities (CVEs)
While Server 2022 is modern, it is not immune to vulnerabilities.
*   **PrintNightmare:** Vulnerabilities in the Print Spooler service. If the server does not need to print, the **Print Spooler** service should be disabled via `services.msc`.
*   **PetitPotam:** An NTLM relay attack. Mitigation often involves disabling NTLM where possible or enabling Extended Protection for Authentication (EPA).

## Administration Tools
*   **Server Manager:** The primary GUI for managing server roles and features.
*   **Windows Admin Center:** A browser-based app for managing servers, clusters, and hyper-converged infrastructure.
*   **PowerShell:** The most powerful tool for scripting security fixes across the server.