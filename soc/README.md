# DarkForest SOC

The **SOC** section extends the DarkForest homelab with offensive security simulations, detection engineering and incident investigation. It reproduces realistic attack scenarios against multiple operating systems to validate monitoring capabilities using **Suricata**, **Splunk** and **Wazuh**.

---

# Objectives

* Generate realistic attack traffic
* Validate detection coverage
* Develop Splunk detection searches and dashboards
* Test Suricata IDS signatures
* Develop Wazuh detection rules
* Practice incident response and threat hunting

---

# Attack Platform

| Host       | Role                        | Network   |
| ---------- | --------------------------- | --------- |
| **Exegol** | Offensive Security Platform | 10.10.4.1 |

Exegol is used as the dedicated attack workstation to perform reconnaissance, enumeration, authentication testing and other offensive security exercises against the lab infrastructure.

---

# Targets

The SOC environment currently includes attack simulations against the following systems:

| Host       | Role                               | Network   |
| ---------- | ---------------------------------- | --------- |
| **DC1**    | Active Directory Domain Controller | 10.10.6.1 |
| **W11**    | Windows 11 Workstation             | 10.10.6.2 |
| **vUbu24** | Ubuntu 24 Workstation              | 10.10.6.3 |

Testing performed throughout the lab includes network reconnaissance, service enumeration, authentication attempts, protocol analysis and detection validation across Windows and Linux environments.

---

# Detection Stack

* **Suricata** for Network Intrusion Detection (IDS)
* **Splunk Enterprise** for centralized log collection and investigation
* **Wazuh** for endpoint monitoring and detection
* **Sysmon** for Windows telemetry
* **Linux Syslog / Audit Logs** for Ubuntu monitoring

---

# Repository Structure

```text
soc/
├── activedirectory/
├── exegol/
├── linux/
│   └── vUbu24/
├── splunk/
├── suricata/
└── README.md
```

