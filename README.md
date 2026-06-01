[README.md](https://github.com/user-attachments/files/28416752/README.md)
# 🏢 Enterprise Network Security Lab — Zero Day Media Inc.

**Academic Capstone Project | Texas State Technical College | Cybersecurity Program**

A fully configured, multi-floor enterprise network built from the ground up covering physical cabling, VLAN segmentation, Active Directory, firewall policy, web/FTP services, VPN, and host hardening. The simulated organization is **Zero Day Media Inc.**, a multi-department media company with 4 floors, 70+ users, and a DMZ-facing public web presence.

---

## 📁 Repository Contents

| File / Folder | Description |
|---|---|
| `Network_Security_Policies_Manual_Actual_Final.docx` | Full network security policy manual |
| `IP_Plan.xlsx` | IP addressing plan for all network segments |
| `Port_Allocation_Plan.xlsx` | Switch port assignments per floor/device |
| `Device_Port_and_Port_Range_Labels.xlsx` | Device labeling and port range reference |
| `Zero_Day_Media_Inc_Proposed_Expansion_Hierarchical_Diagram.vsdx` | Hierarchical network topology diagram |
| `ZeroDay_Wiring_diagrams.vsdx` | Physical floor wiring diagrams |
| `Rack_Elevation_Diagram.vsdx` | Server rack elevation layout |
| `Zero_Day_Inc_Floor_Cabling_Diagrams_Assessment_Legend.docx` | Cabling legend and drop counts by floor |
| `Completed_ZeroDay_S2S_VPN.pka` | Cisco Packet Tracer — Site-to-Site VPN lab file |
| `ASA_internal_running-config-AMT-1.txt` | Internal ASA 5506 firewall running config |
| `ASA_DMZ_running-config-AMT-1.txt` | DMZ ASA firewall running config |
| `Perimeter_Router_running-config-AMT-1.txt` | Perimeter router running config |
| `DMZ_Switch_running-config-AMT-1.txt` | DMZ switch running config |
| `Floor_1_switch_running-config-AMT-1.txt` | Floor 1 access switch running config |
| `Floor_2_switch_running-config-AMT-1.txt` | Floor 2 access switch running config |
| `Floor_3_switch_running-config-AMT-1.txt` | Floor 3 access switch running config |
| `Floor_4_switch_running-config-AMT-1.txt` | Floor 4 access switch running config |
| `SC-_Assessment_Secure_Domain_Services.pdf` | Domain services assessment (static IP, AD, DNS, GPO) |
| `Criteria__3_-_Criteria_2.pdf` | Network expansion assessment (Wi-Fi, routing, connectivity) |
| `FINAL_SCREENSHOT_CHECKLIST___3_1_1b_Network_Services.pdf` | File shares, ACLs, printer deployment, DHCP |
| `3_1_1c_Mastery_Assessment_FTP_and_Web_Services.pdf` | FTP and web server configuration |
| `3_1_1d_Desktop_Services.pdf` | Client domain join, DHCP, mapped drives, printer |
| `4_1_1a_Mastery_Assessment_Technical_configuration.pdf` | VLAN isolation, password policy, wireless, firewall/ACL |
| `4_1_1b_Mastery_Assessment_Secure_Services.pdf` | SSH, certificate authority, HTTPS, NOC switch |
| `4_1_1d_Assessment_Harden_Host_Devices.pdf` | OS hardening, antivirus, browser security, UAC, GPO |

---

## 🗺️ Network Overview

### IP Addressing Summary

| Segment | Subnet | Purpose |
|---|---|---|
| Internal Corporate | `172.21.1.0/24` | Employee VLAN 100 |
| Guest Wi-Fi | `172.21.2.0/24` | Guest VLAN 200 |
| DMZ | `10.10.10.0/24` | Public-facing servers |
| ISP / Public | `199.199.199.0/29` | Internet-facing interfaces |
| Internal Domain | `192.168.10.0/24` | AD domain (zeroday.local) |

### Key Devices

| Device | IP | Role |
|---|---|---|
| ZDSrv1-JAAB | 192.168.10.10 | Primary Domain Controller / DNS |
| ZDSrv2-JAAB | 192.168.10.15 | Secondary DC / DNS |
| ZDDMZ1-JAAB | 192.168.10.30 | DMZ Server (FTP, Web, Secondary DNS) |
| ASA5506-Internal | 172.21.1.1 (inside) | Internal firewall |
| DMZASA | 10.10.10.1 (DMZ) | DMZ firewall |
| Perimeter-Router | 199.199.199.2 | Edge router to ISP |
| NOC-Dist-Sw | 172.21.1.10 | Core distribution switch |

### Physical Cabling (Cat6 throughout)

| Floor | Network Drops | Cable Length |
|---|---|---|
| Floor 1 | 17 drops | ~1,053 ft |
| Floor 2 | 20 drops | ~1,154 ft |
| Floor 3 | 18 drops | ~1,025 ft |
| Floor 4 | 15 drops | ~974 ft |

---

## 🔢 Step-by-Step Project Walkthrough

---

### Step 1 — Physical Design & IP Planning

**What was done:**
Designed the physical layout of the network across 4 floors using blue Cat6 Ethernet cable. Created a hierarchical network diagram showing how floor switches connect up to the NOC distribution switch and then to the firewalls.

**Files:**
- `Zero_Day_Media_Inc_Proposed_Expansion_Hierarchical_Diagram.vsdx` — logical topology
- `ZeroDay_Wiring_diagrams.vsdx` — floor-by-floor cabling
- `Rack_Elevation_Diagram.vsdx` — how devices are physically mounted
- `IP_Plan.xlsx` — full IP assignment table
- `Port_Allocation_Plan.xlsx` — which device connects to which port
- `Zero_Day_Inc_Floor_Cabling_Diagrams_Assessment_Legend.docx` — cabling legend

**Key decisions:**
- Hierarchical 3-tier design: access (floor switches) → distribution (NOC switch) → core (firewalls/router)
- Two VLANs across all floors: VLAN 100 (employees) and VLAN 200 (guests)
- All trunk links carry VLANs 2–99 and 101–1001 to maintain VLAN 100 isolation

---

### Step 2 — VLAN Segmentation & Switch Configuration

**What was done:**
Configured all four floor access switches and the NOC distribution switch with VLAN 100 (corporate) and VLAN 200 (guest Wi-Fi). Unused ports were shut down. Trunk links connect each floor to the NOC switch.

**Files:**
- `Floor_1_switch_running-config-AMT-1.txt` through `Floor_4_switch_running-config-AMT-1.txt`
- `4_1_1a_Mastery_Assessment_Technical_configuration.pdf` — VLAN isolation screenshots

**Key configurations:**
```
! Example — Floor 1 Access Switch
interface GigabitEthernet0/1
 switchport access vlan 100
 switchport mode access

interface GigabitEthernet1/1
 switchport access vlan 200
 switchport mode access

interface GigabitEthernet8/1
 switchport mode trunk

interface Vlan100
 ip address 172.21.1.11 255.255.255.0

interface Vlan200
 ip address 172.21.2.11 255.255.255.0
```

**Guest VLAN isolation policy:**
- Guests (VLAN 200) can only reach the internet — blocked from all internal server VLANs and employee VLAN 100
- DHCP pool for VLAN 200 served by NOC-Dist-Sw with DNS pointing to DMZ server

---

### Step 3 — Firewall Configuration (ASA 5506 Internal + DMZ ASA)

**What was done:**
Configured two Cisco ASA 5506 firewalls — one protecting the internal corporate network, one isolating the DMZ. The perimeter router sits between both ASAs and the ISP.

**Files:**
- `ASA_internal_running-config-AMT-1.txt`
- `ASA_DMZ_running-config-AMT-1.txt`
- `Perimeter_Router_running-config-AMT-1.txt`
- `4_1_1a_Mastery_Assessment_Technical_configuration.pdf` — ACL screenshots
- `4_1_1b_Mastery_Assessment_Secure_Services.pdf` — SSH and policy map

**Internal ASA highlights:**
```
! Two interfaces — inside (security 100) and guest (security 50)
interface GigabitEthernet1/1
 nameif inside
 security-level 100
 ip address 172.21.1.1 255.255.255.0

interface GigabitEthernet1/2
 nameif guest
 security-level 50
 ip address 172.21.2.1 255.255.255.0

! NAT both networks out through outside interface
object network INSIDE-NET
 nat (inside,outside) dynamic interface

object network GUEST-NET
 nat (guest,outside) dynamic interface
```

**DMZ ASA highlights:**
```
! Static NAT for public web server
object network webserver
 host 10.10.10.11
 nat (DMZ,outside) static 199.199.199.11

! Only permit HTTP, HTTPS, DNS, and ICMP inbound
access-list OUTSIDEIN extended permit tcp any object webserver eq www
access-list OUTSIDEIN extended permit tcp any object webserver eq 443
```

**Perimeter Router ACL (anti-spoofing + service whitelist):**
```
access-list 110 deny ip 10.0.0.0 0.255.255.255 any       ! Block RFC1918
access-list 110 deny ip 172.16.0.0 0.15.255.255 any
access-list 110 deny ip 192.168.0.0 0.0.255.255 any
access-list 110 permit tcp any host 199.199.199.11 eq www
access-list 110 permit tcp any host 199.199.199.11 eq ftp
access-list 110 deny ip any any
```

**SSH access was configured on both ASAs** to replace Telnet for secure management.

---

### Step 4 — Active Directory Domain Services

**What was done:**
Built the `zeroday.local` domain with two domain controllers (ZDSrv1-JAAB as primary, ZDSrv2-JAAB as secondary). Created 9 Organizational Units matching company departments, populated with 36 domain users.

**Files:**
- `SC-_Assessment_Secure_Domain_Services.pdf` — screenshots of all criteria

**Organizational Units created:**

| OU | Sample Users |
|---|---|
| Animation | Henry Lane, Isabella Ramos, Oliver Morris, Zoe Nightshade |
| Broadcast | Carlos Nguyen, James Coleman, Mia Thompson, Nina Jones |
| Finance | Ahsley Patel, Dainel Moore, Kevin Brooks, Rebecca Clark |
| HR | Brandon Lee, Lisa Martinez, Steven King, Victoria Golden |
| Public Relations | Alyssa Green, David Ross, Lauren Cho, Marco Fernandez |
| Publishing | Aaron Foster, Michael Kim, Rachel Bennett, Susan Baker |
| Social Media | Brian Owens, Emma Scott, Tyler Hughes, Vanessa Liu |
| Video Production | Chloe Anderson, Ethan Nakamura, Luke Evans, Madison Gold |
| Web Hosting | Grace Kelly, Jacob Hall, Leo Thomas, Samantha Ward |

**Fault tolerance:** Both ZDSrv1-JAAB and ZDSrv2-JAAB are domain controllers with replicated AD, providing redundancy if either server goes offline.

---

### Step 5 — DNS Configuration (Forward, Reverse & Zone Transfer)

**What was done:**
Configured DNS forward and reverse lookup zones on ZDSrv1-JAAB (master). Zone transfers enabled to ZDDMZ1-JAAB (secondary). Created host A records for FTP, web, and intranet services.

**Files:**
- `SC-_Assessment_Secure_Domain_Services.pdf` — DNS Manager screenshots

**DNS Records created:**

| Hostname | FQDN | IP |
|---|---|---|
| zdftp | zdftp.zeroday.local | 192.168.10.30 |
| zdweb1 | zdweb1.zeroday.local | 192.168.10.30 |
| intranet | intranet.zeroday.local | 192.168.10.30 |

**Zone transfer config:** Forward and reverse zones on ZDSrv1-JAAB set to allow transfers only to `192.168.10.30` (ZDDMZ1-JAAB). Secondary zones verified as Running status on the DMZ server.

---

### Step 6 — GPO: Domain Password & Lockout Policy

**What was done:**
Created a Default Domain Policy GPO enforcing password complexity, rotation, and account lockout across all domain users.

**Files:**
- `SC-_Assessment_Secure_Domain_Services.pdf`
- `4_1_1a_Mastery_Assessment_Technical_configuration.pdf`
- `4_1_1d_Assessment_Harden_Host_Devices.pdf`

**Policy settings applied:**

| Policy | Setting |
|---|---|
| Minimum password length | 10 characters |
| Password complexity | Enabled (upper, lower, number, special) |
| Maximum password age | 30 days |
| Minimum password age | 1 day |
| Account lockout threshold | 3 invalid attempts |
| Account lockout duration | 30 minutes |
| Reset lockout counter | 30 minutes |
| Reversible encryption | Disabled |

---

### Step 7 — File Shares, Home Directories & ACLs

**What was done:**
Provisioned a dedicated data volume (E:) on ZDSrv1-JAAB with two share types: per-user home directories (HomeDirs$) and department shared folders (DeptShares). Access control lists enforce that users can only access their own home folder and their own department share.

**Files:**
- `FINAL_SCREENSHOT_CHECKLIST___3_1_1b_Network_Services.pdf`

**ACL results verified:**
- **Home folder owner** — full control (all permissions allowed)
- **Non-owner accessing another user's folder** — all permissions denied via file permissions
- **Department group member** — full access to their OU's share
- **Member of a different department** — blocked via share-level and NTFS permissions

**Home drive mapping example (user Grace Kelly):**
```
Connect: E: → \\ZDSrv1-Jaab\HomeDirs$\gkelly
```

---

### Step 8 — FTP Service (DMZ)

**What was done:**
Configured Microsoft IIS FTP on ZDDMZ1-JAAB with authenticated access. FTP users are domain accounts with NTFS permissions on `C:\inetpub\ftproot`. Clients connect using the FQDN `zdftp.zeroday.local`.

**Files:**
- `3_1_1c_Mastery_Assessment_FTP_and_Web_Services.pdf`

**IIS FTP Site settings:**
- Site name: `FTP-DMZ`
- Binding: `192.168.10.30:21:zdftp.zeroday.local`
- Physical path: `C:\inetpub\ftproot`
- Authentication: domain user `ftpuser1`
- NTFS permissions for ftpuser1: Read, Write, List, Execute

**Verified:** Client ZDClient1 successfully authenticated and listed files via command-line FTP.

---

### Step 9 — Web Services (DMZ & Internal)

**What was done:**
Set up two separate IIS websites on ZDDMZ1-JAAB: a public-facing DMZ web server and an internal intranet site. Both are accessible via DNS name. HTTPS was later enforced using a certificate issued by the internal Certificate Authority.

**Files:**
- `3_1_1c_Mastery_Assessment_FTP_and_Web_Services.pdf`
- `4_1_1b_Mastery_Assessment_Secure_Services.pdf`

**Sites configured:**

| Site | Binding | Path | Access |
|---|---|---|---|
| DMZWeb | `http://192.168.10.30:80:zdweb1.zeroday.local` | `C:\inetpub\dmzweb` | Internal clients |
| InternalWeb | `http://192.168.10.30:80:intranet.zeroday.local` | `C:\inetpub\intranet` | Internal clients |
| Zeroday (HTTPS) | `https://www.zeroday.local:443` | `C:\inetpub\zeroday` | Domain clients via SSL |

---

### Step 10 — Certificate Authority & HTTPS

**What was done:**
Deployed an internal Certificate Authority on ZDSrv1-JAAB (`zeroday-ZDSRV1-JAAB-CA`). Issued SSL certificates to the internal web server and FTP server. Enabled HTTPS binding on the Zeroday IIS site and required SSL.

**Files:**
- `4_1_1b_Mastery_Assessment_Secure_Services.pdf`

**CA details:**
- CA Name: `zeroday-ZDSRV1-JAAB-CA`
- Hash algorithm: SHA256
- Certificate issued to: `www.zeroday.local` — valid 7/17/2025 to 7/17/2026
- FTP certificate: `ZDDMZ1-JAAB.zeroday.local`

**Result:** Clients browse `https://www.zeroday.local` with a valid internal certificate and no security warning.

---

### Step 11 — Printer Deployment via GPO

**What was done:**
Installed a shared network printer (`CompanyPrinter`) on ZDSrv1-JAAB and deployed it automatically to all domain workstations using a Group Policy Preferences printer deployment GPO (`DeployPrinter`).

**Files:**
- `FINAL_SCREENSHOT_CHECKLIST___3_1_1b_Network_Services.pdf`

**GPO config:**
- Share path: `\\ZDSrv1-JAAB\CompanyPrinter`
- Action: Update (deploys and updates on login)
- Result: Printer automatically appears on client machines without manual installation

---

### Step 12 — Desktop Services (Client Validation)

**What was done:**
Validated that a domain client (ZDClient) was fully functional — joined to the domain, receiving DHCP, able to access mapped drives, the intranet site, FTP, and the network printer.

**Files:**
- `3_1_1d_Desktop_Services.pdf`

**Checklist verified:**

| Item | Result |
|---|---|
| Joined to `zeroday.local` domain | ✅ |
| IP received from DHCP | ✅ |
| LibreOffice installed (GPO software deployment) | ✅ |
| Home drive mapped (E:) | ✅ |
| Network printer installed automatically | ✅ |
| Intranet website accessible | ✅ (`intranet.zeroday.local`) |
| FTP access to DMZ server | ✅ (`zdftp.zeroday.local`) |

---

### Step 13 — Wireless Network Configuration

**What was done:**
Configured two separate wireless access points — one for employees (SSID: `Corp`, VLAN 100) and one for guests (SSID: `Guest`, VLAN 200). Both APs were configured on the Linksys WRT300N platform inside the Packet Tracer simulation.

**Files:**
- `Criteria__3_-_Criteria_2.pdf`
- `4_1_1a_Mastery_Assessment_Technical_configuration.pdf`

**Wireless policy enforced:**
- Separate SSIDs and VLANs for employee vs. guest
- WPA2/WPA3 encryption with AES
- Guest traffic routed through ASA and limited to internet/DNS only
- Employee Wi-Fi routes through internal ASA with NAT

---

### Step 14 — Site-to-Site VPN

**What was done:**
Configured an IPSec Site-to-Site VPN between two Zero Day Inc. locations using Cisco Packet Tracer. The completed `.pka` lab file is included.

**File:**
- `Completed_ZeroDay_S2S_VPN.pka` — open in Cisco Packet Tracer to explore

---

### Step 15 — Host Hardening

**What was done:**
Applied OS-level security hardening to domain workstations beyond the GPO password policy, including antivirus, browser hardening, firewall, and UAC configuration.

**Files:**
- `4_1_1d_Assessment_Harden_Host_Devices.pdf`

**Hardening items applied:**

| Item | Configuration |
|---|---|
| Password policy via GPO | 10-char min, complexity, 30-day rotation, lockout after 3 attempts |
| Windows Defender Antivirus | Enabled, daily scheduled scan, definitions up to date |
| Browser security (Edge) | 3rd-party cookies blocked, SmartScreen on, pop-ups blocked, Do Not Track on |
| Windows Defender Firewall | Enabled for domain, private, and public profiles |
| User Account Control (UAC) | Set to "Always Notify" |
| Account lockout policy | 30-min lockout, 3-attempt threshold |
| USB AutoRun | Disabled via Control Panel AutoPlay settings |

---

## 🛡️ Security Policies Summary

The full policy manual is in `Network_Security_Policies_Manual_Actual_Final.docx`. Key policies include:

- **Guest Policy (4.1):** Guests restricted to VLAN 200 with internet/DNS only — no access to internal VLANs
- **Authentication Policy (4.2):** 10-char passwords, complexity required, 30-day rotation, lockout after 3 failed attempts
- **Wireless Policy (4.3):** Separate VLANs, WPA3-Enterprise encryption, MAC filtering for internal Wi-Fi
- **Firewall Policy (5.1):** Default deny all inbound, DMZ on isolated VLAN, traffic restricted between DMZ and internal
- **Router/Switch Policy (5.2):** VLAN segmentation by function, unused ports disabled, secure management access only

---

## 🛠️ Tools & Technologies Used

- **Cisco Packet Tracer** — network simulation and VPN lab
- **Windows Server 2019** — AD DS, DNS, DHCP, IIS, Certificate Authority, GPO
- **Cisco ASA 5506** — stateful firewall, NAT, ACLs, SSH management
- **Cisco IOS switches** — VLANs, trunking, STP, DHCP relay
- **Microsoft IIS** — FTP, HTTP, and HTTPS web services
- **Visio (.vsdx)** — network diagrams and rack elevation
- **Active Directory** — domain users, OUs, group policies

---

## 📌 About This Project

This lab was completed as part of the **Cybersecurity AAS program at Texas State Technical College** (Rosenberg, TX). It represents the capstone of network infrastructure and security coursework covering TCP/IP networking, Windows Server administration, Cisco device configuration, and enterprise security policy implementation.

**Student:** Abdulmalik Taiwo
**Program:** Associate of Applied Science — Cybersecurity
**Institution:** Texas State Technical College
