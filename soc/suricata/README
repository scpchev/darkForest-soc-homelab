# Suricata IDS

## Overview

Suricata is the network Intrusion Detection System (IDS) deployed within the DarkForest SOC Homelab. It is installed directly on the pfSense firewall and inspects traffic crossing every internal VLAN to detect reconnaissance, exploitation attempts, protocol anomalies, and known malicious network activity.

The current deployment operates in **IDS mode** only. Traffic is analyzed and logged without being blocked, allowing attack simulations to be performed safely while generating high-quality telemetry for the SOC.

---

## Objectives

- Monitor inter-VLAN network traffic
- Detect reconnaissance and scanning activity
- Detect protocol anomalies
- Generate structured security events
- Forward detections to the SOC platform (future Splunk integration)
- Build realistic blue-team detection scenarios

---

## Environment

| Component | Value |
|-----------|------|
| Firewall | pfSense CE 2.8.1 |
| IDS Engine | Suricata 7.0.11 |
| Detection Mode | IDS |
| Blocking | Disabled |
| Ruleset | Emerging Threats Open |
| Logging | EVE JSON + Alerts + HTTP + TLS |

---

# Network Architecture

Suricata monitors all internal VLAN interfaces configured on pfSense.

| Interface | Purpose |
|-----------|----------|
| INFRA | Infrastructure |
| SERVERS | Windows / Active Directory |
| CLIENTS | Client VLAN |
| ATTACKLAB | Offensive Lab |
| SECURITY | SIEM / SOC |

This allows traffic inspection between every segment of the lab.

---

# Logging

The following logs are enabled.

| Log | Purpose |
|------|---------|
| alerts.log | Detection alerts |
| eve.json | Structured JSON events |
| http.log | HTTP metadata |
| tls.log | TLS handshakes |
| suricata.log | Engine status |

---

# Enabled Rules

The deployment uses:

- Emerging Threats Open
- Default Suricata Decoder Rules
- Protocol Decoder Rules
- Application Layer Rules

Examples include:

- Scan Detection
- SMB
- SSH
- DNS
- HTTP
- TLS
- ICMP
- RDP
- Malware
- Exploit
- Coinminer
- DNS Abuse
- Botnet C2
- Web Attacks

---

# Enabled Rule Categories

Examples of enabled categories:

- emerging-scan.rules
- emerging-exploit.rules
- emerging-malware.rules
- emerging-web_server.rules
- emerging-web_client.rules
- emerging-shellcode.rules
- emerging-dns.rules
- emerging-rdp.rules
- emerging-smb.rules
- emerging-ssh.rules
- emerging-icmp.rules
- emerging-http.rules
- emerging-tls.rules

---

# Detection Scenarios

The following attack simulations have been validated.

## Nmap Reconnaissance

Attacker

```
Exegol
10.10.4.1
```

Targets

```
Windows Server (DC1)
Windows 11
Ubuntu 24
```

Example

```bash
nmap -A 10.10.6.1
```

Generated detections

- ET SCAN Possible Nmap User-Agent Observed
- ET SCAN RDP Connection Attempt from Nmap
- SMB malformed request dialects
- HTTP protocol anomalies
- Application Layer mismatches

---

## SSH Enumeration

Example

```bash
nc 10.10.6.3 22
```

Detection

```
SURICATA SSH invalid banner
```

---

## SMB Enumeration

Example

```bash
nmap --script smb* 10.10.6.1
```

Detection

```
SURICATA SMB malformed request dialects
```

---

## ICMP

Ping activity is detected through protocol decoder rules.

Example alerts:

```
GPL ICMP PING BSDtype
GPL ICMP PING *NIX
SURICATA ICMPv4 unknown code
```

---

## HTTP

Suricata detects malformed or incomplete HTTP exchanges.

Example:

```
SURICATA HTTP unable to match response to request
```

---

## RDP

Reconnaissance against Remote Desktop is detected.

Example alert

```
ET SCAN RDP Connection Attempt from Nmap
```

---

# Configuration

Current deployment characteristics:

- IDS mode
- No packet blocking
- Automatic Flowbit Resolution enabled
- EVE JSON enabled
- HTTP logging enabled
- TLS logging enabled
- Promiscuous Mode enabled
- AutoFP run mode
- Medium detection profile

---

# Repository Structure

```
suricata/
│
├── configs/
│   ├── classification.config
│   ├── passlist
│   ├── reference.config
│   ├── suricata.yaml
│   └── threshold.config
│
├── rules/
│   ├── flowbit-required.rules
│   ├── sid-msg.map
│   └── suricata.rules
│
└── docs/
```

---

# Future Improvements

- Splunk integration using EVE JSON
- Custom detection rules
- Malware detection scenarios
- Brute-force detection
- Exploit validation
- Automatic alert correlation
- Sigma mapping
- MITRE ATT&CK mapping

---

# Status

| Component | Status |
|-----------|--------|
| Installation | ✅ |
| Rules Loaded | ✅ |
| IDS Detection | ✅ |
| EVE JSON | ✅ |
| HTTP Logging | ✅ |
| TLS Logging | ✅ |
| Nmap Detection | ✅ |
| SMB Detection | ✅ |
| SSH Detection | ✅ |
| RDP Detection | ✅ |
| ICMP Detection | ✅ |
| Splunk Integration | Planned |

---

## Author

DarkForest SOC Homelab

Cybersecurity Detection Engineering Laboratory
