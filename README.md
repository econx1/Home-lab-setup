# 🛡️ SOC Automation & Active Directory Home Lab
## End-to-End Telemetry Pipeline & Threat Detection

![Network Diagram](https://via.placeholder.com/800x400?text=Insert+Your+Network+Diagram+Here)

## 📝 Overview
This project demonstrates the deployment of a virtualized SOC (Security Operations Center) environment. The lab features a functional **Active Directory** forest integrated with **Wazuh SIEM/EDR** to monitor, detect, and analyze real-world attack vectors.

### 🎯 Key Objectives
* **Infrastructure:** Isolate a vulnerable network using a VirtualBox NAT Network.
* **Identity Management:** Configure a Windows Server 2022 Domain Controller (`lab.local`).
* **Telemetry:** Deploy **Sysmon** and **Wazuh** agents to capture high-fidelity endpoint logs.
* **Security Monitoring:** Centralize log analysis via the Wazuh Indexer and Dashboard.
* **Offensive Testing:** Simulate TTPs (Tactics, Techniques, and Procedures) from a Kali Linux workstation.

---

## 🏗️ Technical Architecture
* **Virtualization:** Oracle VirtualBox
* **Network:** NAT Network Subnet `10.0.2.0/24`
* **SIEM/EDR:** Wazuh Stack (v4.7+)
* **Endpoint Visibility:** Microsoft Sysmon (SwiftOnSecurity Config)

| Hostname | OS | IP Address | Role |
| :--- | :--- | :--- | :--- |
| **KALI-ATTACK** | Kali Linux | `10.0.2.4` | Attacker Workstation |
| **WAZUH-MGR** | Ubuntu 22.04 | `10.0.2.5` | SIEM Manager & Dashboard |
| **DC-01** | Win Server 2022 | `10.0.2.10` | Domain Controller (ADDS/DNS) |
| **PC-01** | Windows 11 | `10.0.2.15` | Victim Workstation (Domain Joined) |

---

## 🛠️ Configuration Steps

### 1. Active Directory Setup
* Promoted `DC-01` to a Domain Controller for `lab.local`.
* Configured static IPv4 addressing and DNS pointers for domain persistence.
* Established a "Victim" user account with standard privileges to simulate a corporate employee.

### 2. Telemetry & Monitoring
* Installed **Wazuh Manager** on Ubuntu using the automated installation script.
* Deployed **Sysmon** on Windows endpoints to capture **Event ID 1** (Process Creation) and **Event ID 3** (Network Connection).
* Configured `ossec.conf` on agents to forward Sysmon logs to the Manager.

---

## ⚔️ Attack & Detection Scenarios
### Scenario 1: Credential Brute Force (T1110)
* **Attack:** Performed RDP brute-force attack from Kali using `Hydra`.
* **Detection:** Observed a spike in **Event ID 4625** (Failed Logon) alerts in the Wazuh dashboard.

### Scenario 2: Network Reconnaissance (T1046)
* **Attack:** Executed `nmap -sV` scan against the `10.0.2.0/24` range.
* **Analysis:** Wazuh's decoders identified the scan patterns, triggering an "Active response" or "Level 10" alert.

---

## 🚀 Future Enhancements
* [ ] Integrate **pfSense** for network-level firewall logging.
* [ ] Implement **SOAR** (Shuffle) to automate incident response (e.g., auto-isolating a compromised VM).
* [ ] Add a **Linux Target** to monitor SSH brute-force attempts.

---

## 📚 Resources & Credits
* [MyDFIR YouTube Channel](https://www.youtube.com/@MyDFIR) - Inspiration for the SOC project.
* [SwiftOnSecurity Sysmon Config](https://github.com/SwiftOnSecurity/sysmon-config) - Industry standard for endpoint logging.
