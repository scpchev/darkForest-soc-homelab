# DarkForest SOC

The **SOC** section of the DarkForest homelab focuses on offensive security simulations, detection engineering and incident investigation. It reproduces realistic attack scenarios against a controlled environment in order to validate monitoring capabilities using **Suricata**, **Splunk** and **Wazuh**.

---

# Objectives

* Generate realistic attack traffic
* Validate detection coverage
* Develop Splunk detection rules and dashboards
* Test Suricata IDS signatures
* Develop Wazuh detection rules
* Practice incident investigation and threat hunting

---

# Attack Platform

| Host       | Role                        | Network   |
| ---------- | --------------------------- | --------- |
| **Exegol** | Offensive Security Platform | 10.10.4.1 |

Exegol serves as the attack workstation and contains offensive security tooling used to simulate reconnaissance, enumeration and authentication attacks against the lab infrastructure.

---

# Attack Lab

| Host          | Role                               | Network   |
| ------------- | ---------------------------------- | --------- |
| **DC1**       | Active Directory Domain Controller | 10.10.6.1 |
| **Windows11** | User Workstation                   | 10.10.6.2 |
| **Ubuntu24**  | Linux Workstation                  | 10.10.6.3 |

---

# Implemented Scenarios

## Scenario 001 - Aggressive Reconnaissance

Target:

* Windows11 (10.10.6.2)

Command:

```bash
nmap -Pn -A -T4 10.10.6.2
```

Purpose:

* Operating System Fingerprinting
* Service Enumeration
* Version Detection
* Default NSE Scripts
* Traceroute

Expected detections:

* Suricata protocol anomalies
* SMB fingerprinting
* Application layer events

---

## Scenario 002 - Full TCP Port Enumeration

Target:

* Windows11 (10.10.6.2)

Command:

```bash
nmap -Pn -p- -T4 10.10.6.2
```

Purpose:

* Enumerate all TCP ports
* Discover exposed services
* Generate reconnaissance traffic

Expected detections:

* TCP scan activity
* Port enumeration
* Reconnaissance alerts

---

## Scenario 003 - UDP Service Discovery

Target:

* Windows11 (10.10.6.2)

Command:

```bash
nmap -Pn -sU --top-ports 100 10.10.6.2
```

Purpose:

* Enumerate common UDP services
* Discover NetBIOS, ISAKMP, mDNS and UPnP services

Expected detections:

* UDP reconnaissance
* Protocol decoder events
* ICMP Port Unreachable responses

---

## Scenario 004 - SSH Authentication

Target:

* Windows11 (10.10.6.2)

Command:

```bash
hydra -l nadine -P passwords.txt ssh://10.10.6.2
```

Purpose:

* Simulate an SSH password attack
* Validate SSH monitoring and Suricata protocol events

Result:

* Successful authentication using the test account
* Suricata generated protocol-related SSH alerts

---

# Detection Stack

* **Suricata** for network intrusion detection
* **Splunk Enterprise** for log aggregation and investigation
* **Wazuh** for endpoint monitoring
* **Sysmon** for Windows telemetry
* **Linux audit and syslog** for Ubuntu monitoring

---

# Repository Structure

```
soc/
├── activedirectory/
├── exegol/
├── linux/
│   └── vUbu24/
├── splunk/
├── suricata/
└── README.md
```
