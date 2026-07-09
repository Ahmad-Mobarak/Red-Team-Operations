<p align="center">
  <img src="https://img.shields.io/badge/Red%20Team-Operations-dc3545?style=for-the-badge&logo=hackthebox&logoColor=white" alt="Red Team Operations"/>
  <img src="https://img.shields.io/badge/MITRE%20ATT%26CK-Aligned-ff6600?style=for-the-badge&logo=shield&logoColor=white" alt="MITRE ATT&CK"/>
  <img src="https://img.shields.io/badge/DEPI-Graduation%20Project-0d6efd?style=for-the-badge&logo=graduation-cap&logoColor=white" alt="DEPI Project"/>
  <img src="https://img.shields.io/badge/Status-Completed-28a745?style=for-the-badge" alt="Status"/>
  <img src="https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge" alt="MIT License"/>
</p>

<h1 align="center">🔴 Red Team Operations & Simulated Corporate Attack</h1>

<p align="center">
  <strong>A full-scale adversary simulation and corporate attack chain engagement conducted in an isolated lab environment</strong><br/>
  <em>DEPI — Vulnerability Analyst and Penetration Tester · Graduation Project 2026</em>
</p>

<p align="center">
  <a href="#-project-overview">Overview</a> •
  <a href="#-lab-architecture">Architecture</a> •
  <a href="#-attack-chain-phases">Attack Phases</a> •
  <a href="#-tools--technology-stack">Tools</a> •
  <a href="#-screenshots--evidence">Screenshots</a> •
  <a href="#-documentation">Docs</a> •
  <a href="#-team">Team</a> •
  <a href="#%EF%B8%8F-disclaimer">Disclaimer</a>
</p>

---

## 📋 Project Overview

This project delivers a **complete end-to-end Red Team engagement** simulating a real-world corporate cyberattack against a fully isolated lab environment. Developed as the **graduation project for the DEPI (Digital Egypt Pioneers Initiative) — Vulnerability Analyst and Penetration Tester track**, it demonstrates advanced offensive security skills and professional adversary simulation methodology.

The engagement replicates the behavior of a **modern Advanced Persistent Threat (APT)** actor, following a structured kill chain from initial reconnaissance through full domain compromise — all mapped to the **MITRE ATT&CK** framework.

### 🎯 Objectives

- Simulate a **complete corporate attack lifecycle** (Initial Access → Privilege Escalation → Lateral Movement → Domain Compromise)
- Demonstrate realistic **adversary Tactics, Techniques & Procedures (TTPs)**
- Assess detection and response gaps across the defensive stack
- Map every technique to the **MITRE ATT&CK** framework
- Deliver professional **technical and executive-level reporting**
- Provide a **prioritized remediation roadmap**

### 📚 Frameworks & Standards

| Framework | Purpose |
|-----------|---------|
| **MITRE ATT&CK** | Technique mapping & classification |
| **Lockheed Martin Cyber Kill Chain** | Attack phase structuring |
| **NIST SP 800-115** | Penetration testing methodology |
| **PTES** | Engagement execution standards |

---

## 🏗️ Lab Architecture

The entire engagement was conducted within a **fully isolated virtual network** designed to simulate a realistic corporate environment, connected via **Tailscale mesh VPN** for distributed team collaboration.

### Network Topology

```
┌─────────────────────────────────────────────────────────────────┐
│                    SIMULATED CORPORATE NETWORK                  │
│                                                                 │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────────────┐   │
│  │   Kali Linux  │  │   Kali Linux  │  │    Kali Linux        │   │
│  │  (Attacker 1) │  │  (Attacker 2) │  │   (Attacker N)       │   │
│  │  All-Attackers│  │  All-Attackers│  │   All-Attackers      │   │
│  └──────┬───────┘  └──────┬───────┘  └──────────┬───────────┘   │
│         │                 │                      │               │
│         ▼                 ▼                      ▼               │
│  ┌────────────────────────────────────────────────────────┐      │
│  │              External Routers / Tailscale Mesh         │      │
│  └───────┬──────────┬──────────────┬─────────────────────┘      │
│          │          │              │                              │
│          ▼          ▼              ▼                              │
│  ┌──────────┐ ┌──────────┐ ┌────────────┐ ┌──────────────┐      │
│  │  Meta2   │ │  Meta3   │ │   DC01     │ │ Win10-Client │      │
│  │ (Linux)  │ │ (Linux)  │ │ (Win 2019) │ │  (Win 10)    │      │
│  │ .56.101  │ │ .55.103  │ │  .57.10    │ │   .57.20     │      │
│  │ Internal │ │ External │ │  AD / DNS  │ │  Domain User │      │
│  └──────────┘ └──────────┘ └────────────┘ └──────────────┘      │
│                                                                  │
│  Domain: corp.local                                              │
└──────────────────────────────────────────────────────────────────┘
```

### Lab Machines

| Machine | OS | IP Address | Role |
|---------|-----|-----------|------|
| **Meta2** | Ubuntu (Metasploitable 2) | `192.168.56.101` | Vulnerable Linux target |
| **Meta3** | Ubuntu (Metasploitable 3) | `192.168.55.103` | Vulnerable Linux target with web services |
| **DC01** | Windows Server 2019 | `192.168.57.10` | Active Directory Domain Controller (`corp.local`) |
| **Win10-Client** | Windows 10 | `192.168.57.20` | Domain-joined workstation |
| **Kali Linux** (Multiple) | Kali GNU/Linux | Various | Attacker machines (team members) |

### Network Segmentation & Access Control

The lab utilized **9 Access Control Policies** managing traffic between External-Routers, Internal-Routers, and All-Attackers groups, ensuring controlled and realistic network segmentation.

---

## ⚔️ Attack Chain Phases

The engagement followed a structured **4-phase Red Team lifecycle** model aligned with the Cyber Kill Chain:

### 🟢 Phase 1 — Reconnaissance & Intelligence Gathering

> **Objective:** Build a complete intelligence profile of the target environment.

| Activity | Details |
|----------|---------|
| Network Discovery | Nmap service scans across all target subnets |
| Web Enumeration | Gobuster directory brute-forcing, Nikto vulnerability scanning |
| DNS Analysis | `nslookup` on `corp.local` domain to map infrastructure |
| Service Fingerprinting | Identified Apache 2.4.7, ProFTPD 1.3.5, OpenSSH 6.6, MySQL, Samba, phpMyAdmin, Drupal, and more |
| OSINT | theHarvester, Maltego for target profiling |

**Key Findings:**
- Metasploitable 3 exposed **11+ open ports** including FTP, SSH, HTTP, SMB, MySQL, IRC
- Directory listing enabled on Apache web server
- Multiple web applications discovered (`/drupal/`, `/phpmyadmin/`, `/chat/`, `payroll_app.php`)
- Domain Controller exposing Kerberos, LDAP, SMB, MSRPC services

---

### 🟡 Phase 2 — Initial Access & Exploitation

> **Objective:** Achieve initial compromise and establish foothold.

#### 2a. Web Application Attack (Metasploitable 3)
- **SQL Injection** on `payroll_app.php` using `UNION SELECT` to dump `users` table
- Extracted usernames and passwords for all accounts (Star Wars–themed users)
- Leveraged extracted credentials (`leia_organa`) for **SSH access** to Meta3
- Achieved **root privilege escalation** via `sudo -s`

#### 2b. Active Directory Attack (corp.local Domain)
- **Password spraying** against DC01 using CrackMapExec with custom wordlists
- Successfully authenticated as `ajohnson:Welcome1!` — domain user
- **Domain enumeration** via CrackMapExec:
  - Users: `Administrator`, `ajohnson`, `bsmith`, `cbrown`, `dbbackup`, `sqlsvc`, `krbtgt`
  - Computers: `DESKTOP-ADR2HV4$`, `DC01$`
  - Groups: Full AD group enumeration (Domain Admins, Enterprise Admins, etc.)
  - Shares: `ADMIN$`, `C$`, `IPC$`, `NETLOGON`, `SYSVOL`
- **Kerberoasting** attack to extract TGS-REP hashes for service accounts
- **Hashcat cracking** (mode 13100) — recovered `Service2024!` and `Backup2024!` credentials

---

### 🔵 Phase 3 — Lateral Movement & Domain Compromise

> **Objective:** Escalate privileges and achieve full domain control.

- Used `bsmith:Admin123!` (Domain Admin) credentials obtained via password spray
- **Impacket PsExec** via Proxychains for remote execution on DC01
- Achieved **`NT AUTHORITY\SYSTEM`** on the Domain Controller
- **Impacket secretsdump** to extract:
  - Local SAM hashes
  - LSA Secrets (machine account keys, DPAPI secrets)
  - **Full NTDS.DIT dump** — all domain user NTLM hashes
  - Kerberos encryption keys for all domain accounts
  - Machine account hashes for all domain-joined workstations and servers

**Result: Complete Domain Compromise — Full control of the `corp.local` Active Directory environment.**

---

### 🔴 Phase 4 — Reporting & Remediation

> **Objective:** Document findings and deliver actionable security improvements.

#### Deliverables Produced:
1. **Technical Report** — Full attack lifecycle narrative with evidence
2. **Executive Report** — High-level risk summary and business impact
3. **MITRE ATT&CK Mapping** — All techniques classified and mapped
4. **Remediation Roadmap** — Prioritized security improvements
5. **Corporate Attack Chain Playbook** — Step-by-step engagement playbook

---

## 🛠️ Tools & Technology Stack

### Reconnaissance & OSINT
| Tool | Purpose |
|------|---------|
| Nmap | Network discovery & service enumeration |
| Gobuster | Directory & file brute-forcing |
| Nikto | Web vulnerability scanning |
| Wireshark | Network traffic analysis |
| theHarvester | OSINT email & subdomain harvesting |
| Maltego | Relationship & infrastructure mapping |
| Shodan | External exposure analysis |
| Recon-ng | OSINT automation framework |

### Exploitation & Access
| Tool | Purpose |
|------|---------|
| Metasploit Framework | Exploitation framework |
| Burp Suite | Web application testing |
| Hydra / Medusa | Authentication brute-forcing |
| SQL Injection (Manual) | Web app database extraction |
| Netcat | Reverse shell & connectivity |

### Post-Exploitation & Lateral Movement
| Tool | Purpose |
|------|---------|
| CrackMapExec | AD enumeration, password spraying, share enumeration |
| Impacket (PsExec, secretsdump) | Remote execution & credential dumping |
| BloodHound | Active Directory attack path mapping |
| Hashcat | Offline password cracking (Kerberoast TGS-REP) |
| Mimikatz | Credential extraction (lab-only) |
| Proxychains | Traffic pivoting through SOCKS proxy |

### Persistence & Analysis
| Tool | Purpose |
|------|---------|
| PowerShell | Automation & scripting |
| Sysinternals Suite | Process & system analysis |
| Autoruns | Startup & persistence enumeration |
| Windows Event Viewer | Log analysis |

### Infrastructure & Collaboration
| Tool | Purpose |
|------|---------|
| Oracle VirtualBox | Virtual machine hosting |
| Tailscale | Mesh VPN for team connectivity |
| Kali Linux | Primary attack platform |

---

## 📸 Screenshots & Evidence

<details>
<summary><strong>🔍 Click to expand — Reconnaissance Evidence</strong></summary>

### Nmap Service Scan (Metasploitable 3)
![Nmap scan results showing 11+ open services](Docs/Project%20Screenshots/meta3%20nmap.png)

### Gobuster Directory Enumeration
![Gobuster discovering /chat/, /drupal/, /phpmyadmin/, /uploads/](Docs/Project%20Screenshots/meta3%20gobuster.png)

### Nikto Vulnerability Scan
![Nikto identifying 23 items including directory indexing, outdated Apache](Docs/Project%20Screenshots/meta3%20nikto.png)

### DNS Lookup — Domain Controller Discovery
![nslookup resolving corp.local to DC01](Docs/Project%20Screenshots/nslookup.png)

### Web Server Directory Listing
![Apache directory listing on Metasploitable 3](Docs/Project%20Screenshots/meta3%20web%20page.png)

</details>

<details>
<summary><strong>💉 Click to expand — Exploitation Evidence</strong></summary>

### SQL Injection — Database Dump
![UNION SELECT extracting usernames and passwords from users table](Docs/Project%20Screenshots/meta3%20sql_inj.png)

### SSH Access via Extracted Credentials
![SSH login as leia_organa, privilege escalation to root](Docs/Project%20Screenshots/ssh%20from%20sqli.png)

### Password Spraying — AD Credential Discovery
![CrackMapExec password spray with Pwn3d! results](Docs/Project%20Screenshots/Screenshot%202026-07-07%20215901.png)

### Kerberoast Hash Cracking (Hashcat)
![Hashcat cracking Kerberos TGS-REP hashes](Docs/Project%20Screenshots/meta3%20ahshcat%20AD%20cracked.png)

</details>

<details>
<summary><strong>🏰 Click to expand — Domain Compromise Evidence</strong></summary>

### AD User Enumeration via CrackMapExec
![Domain users enumerated via SMB](Docs/Project%20Screenshots/Screenshot%202026-07-07%20214922.png)

### AD Computer Enumeration
![Domain computers enumerated](Docs/Project%20Screenshots/Screenshot%202026-07-07%20214956.png)

### AD Share Enumeration
![Network shares including ADMIN$, C$, SYSVOL](Docs/Project%20Screenshots/Screenshot%202026-07-07%20215045.png)

### AD Group Enumeration
![Full Active Directory group listing](Docs/Project%20Screenshots/Screenshot%202026-07-07%20215132.png)

### Domain Controller Compromise — NT AUTHORITY\SYSTEM
![Impacket PsExec achieving SYSTEM on DC01](Docs/Project%20Screenshots/meta3%20proof%20of%20compromise.png)

### NTDS.DIT Dump — Full Domain Credential Extraction
![Impacket secretsdump extracting SAM, LSA, and NTDS.DIT hashes](Docs/Project%20Screenshots/dc%20hashes%20from%20meta3.png)

### Complete Domain Hash Dump
![All domain user and machine account hashes](Docs/Project%20Screenshots/more%20ahshes.png)

</details>

<details>
<summary><strong>🌐 Click to expand — Lab Infrastructure</strong></summary>

### Tailscale Mesh Network — Team Peers
![All team Kali machines connected via Tailscale](Docs/Project%20Screenshots/peers%20and%20hosts.png)

### Network Topology & Connections
![Network connections between routers, attackers, and targets](Docs/Project%20Screenshots/connections.png)

### Access Control Policies
![9 network access policies controlling lab traffic](Docs/Project%20Screenshots/network%20policies.png)

</details>

---

## 📄 Documentation

| Document | Description |
|----------|-------------|
| [Project Overview](Docs/Project%20Over%20View.md) | Full system analysis & design document |
| [Corporate Attack Chain Playbook](Docs/Project%20Screenshots/Corporate_Attack_Chain_Playbook%20(3).pdf) | Detailed step-by-step attack playbook |
| [Final Presentation](Docs/Project%20Screenshots/Final%20project%20presentation%20depi.pptx.pdf) | DEPI graduation presentation slides |

---

## 🧑‍💻 Team

This project was developed as a **collaborative team effort** for the DEPI Vulnerability Analyst and Penetration Tester track graduation:

| Member | Role | GitHub |
|--------|------|--------|
| **Ahmed Mobarak** | Red Team Operator | [@Ahmad-Mobarak](https://github.com/Ahmad-Mobarak) |
| **Nour** | Red Team Operator | [@nourzein006](https://github.com/nourzein006) |
| **Ahmed M. Kamal** | Red Team Operator | [@Ahmed268453](https://github.com/Ahmed268453) |
| **Abdo** | Red Team Operator | — |

---

## 📐 MITRE ATT&CK Mapping

| Tactic | Technique ID | Technique Name |
|--------|-------------|----------------|
| **Reconnaissance** | T1595 | Active Scanning |
| **Reconnaissance** | T1592 | Gather Victim Host Information |
| **Initial Access** | T1190 | Exploit Public-Facing Application |
| **Initial Access** | T1078 | Valid Accounts |
| **Credential Access** | T1110 | Brute Force (Password Spraying) |
| **Credential Access** | T1558.003 | Kerberoasting |
| **Credential Access** | T1003.003 | NTDS (DCSync / secretsdump) |
| **Credential Access** | T1003.001 | LSASS Memory |
| **Discovery** | T1087 | Account Discovery |
| **Discovery** | T1069 | Permission Groups Discovery |
| **Discovery** | T1018 | Remote System Discovery |
| **Lateral Movement** | T1021.002 | SMB/Windows Admin Shares |
| **Lateral Movement** | T1569.002 | Service Execution (PsExec) |
| **Privilege Escalation** | T1078.002 | Valid Accounts: Domain Accounts |
| **Execution** | T1059 | Command and Scripting Interpreter |
| **Collection** | T1005 | Data from Local System |

---

## ⚠️ Disclaimer

> **🚨 EDUCATIONAL PURPOSE ONLY**
>
> This project was conducted in a **fully isolated, authorized lab environment** as part of an academic cybersecurity program. All activities were performed under controlled conditions with explicit authorization.
>
> - ❌ **No real-world systems** were targeted or affected
> - ❌ **No unauthorized access** was attempted or gained
> - ❌ **No malicious intent** — purely educational and skill-development focused
> - ✅ All techniques are demonstrated for **defensive improvement** and **security awareness**
>
> **The authors are not responsible for any misuse of the techniques, tools, or methodologies described in this repository.** Unauthorized use of these techniques against systems you do not own or have explicit permission to test is **illegal** and **unethical**.

---

## 📜 License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

---

<p align="center">
  <strong>Developed with 🔴 by the DEPI Red Team · 2026</strong><br/>
  <em>Digital Egypt Pioneers Initiative — Vulnerability Analyst and Penetration Tester</em>
</p>
