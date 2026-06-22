# Exegol Attack Platform

## Overview

The DarkForest offensive security platform is hosted on a dedicated Debian 13 virtual machine and leverages Exegol as its primary attack environment.

Exegol is used to perform attack simulations, adversary emulation, reconnaissance, and validation of Splunk detections throughout the homelab.

---

## Host Information

| Parameter        | Value           |
| ---------------- | --------------- |
| Hostname         | Debian13        |
| IP Address       | 10.10.4.1       |
| Operating System | Debian 13       |
| Role             | Attack Platform |

---

## Exegol Deployment

| Parameter      | Value      |
| -------------- | ---------- |
| Exegol Version | 5.1.10     |
| Container Name | darkforest |
| Image          | free       |
| Image Version  | 3.1.11     |
| Network Mode   | Host       |
| Status         | Running    |

---

## Workspace

```text
/home/chev/.exegol/workspaces/darkforest
```

The workspace contains attack scripts, wordlists, reports, screenshots, and scenario artifacts used during attack simulations.

---

## Available Tooling

### Reconnaissance

* Nmap

### Password Attacks

* Hydra

### Active Directory

* NetExec
* BloodHound-Python

### Additional Tooling

The Exegol Free image includes a large collection of offensive security tooling used for adversary emulation and detection engineering.

---

## DarkForest Integration

```text
Exegol
    │
    ▼
Attack Simulation
    │
    ▼
Target Systems
    │
    ▼
Logs & Telemetry
    │
    ▼
Splunk Enterprise
    │
    ▼
Detection & Investigation
```

---

## Current Use Cases

* SSH Brute Force Testing
* Linux Administrative Activity Simulation
* Linux Persistence Testing
* Active Directory Enumeration
* Password Spraying
* BloodHound Collection
* Detection Validation

---

## Configuration Exports

Deployment exports and configuration references are available in the `config/` directory.
