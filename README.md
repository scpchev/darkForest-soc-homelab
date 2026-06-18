# DarkForest V2

## Overview

DarkForest V2 is a self-hosted cybersecurity and infrastructure laboratory designed to simulate a small enterprise environment.

The project focuses on:

* System Administration
* Infrastructure Monitoring
* PKI and TLS Management
* Centralized Logging
* SIEM Operations
* Network Security
* Documentation and Infrastructure-as-Documentation

The environment is fully documented and versioned through Gitea, allowing complete reconstruction of the infrastructure from exported configurations and procedures.

---

# Objectives

DarkForest was built to:

* Learn enterprise infrastructure management
* Deploy and maintain internal services
* Practice monitoring and observability
* Implement centralized logging and SIEM workflows
* Operate an internal PKI
* Document production-like environments
* Develop incident investigation and troubleshooting skills

---

# Infrastructure Overview

The infrastructure is segmented into four logical network zones.

```text
                    DarkForest Lab

                           pfSense
                              │
 ┌───────────────┬────────────┼────────────┬───────────────┐
 │               │            │            │
 ▼               ▼            ▼            ▼

Management   Infrastructure  Services   Security
10.10.1.0    10.10.2.0       10.10.3.0  10.10.5.0
```

Segmentation is currently implemented through dedicated pfSense interfaces rather than 802.1Q VLAN tagging.

---

# Network Architecture

## Management Network

| Host       | FQDN                   | IP        | Role                       |
| ---------- | ---------------------- | --------- | -------------------------- |
| Guacamole  | guaca.darkforest.lab   | 10.10.1.1 | Central administration     |
| Grafana    | grafana.darkforest.lab | 10.10.1.2 | Visualization & dashboards |
| Prometheus | prmths.darkforest.lab  | 10.10.1.3 | Metrics collection         |

Management systems provide administration, monitoring and observability capabilities.

---

## Infrastructure Network

| Host | FQDN               | IP        | Role                 |
| ---- | ------------------ | --------- | -------------------- |
| DNS  | dns.darkforest.lab | 10.10.2.1 | Name resolution      |
| NTP  | ntp.darkforest.lab | 10.10.2.2 | Time synchronization |
| CA   | ca.darkforest.lab  | 10.10.2.3 | Internal PKI         |

## These systems provide core infrastructure services consumed by all other machines.

## Services Network

| Host      | FQDN                  | IP        | Role                   |
| --------- | --------------------- | --------- | ---------------------- |
| BookStack | wiki.darkforest.lab   | 10.10.3.1 | Documentation platform |
| Gitea     | gitea.darkforest.lab  | 10.10.3.2 | Git repository         |
| 404jun    | 404jun.darkforest.lab | 10.10.3.9 | Web application        |

## The Services network hosts business and user-facing applications.

## Security Network

| Host   | FQDN                  | IP        | Role                |
| ------ | --------------------- | --------- | ------------------- |
| Wazuh  | wazuh.darkforest.lab  | 10.10.5.1 | XDR / SIEM Platform |
| Splunk | splunk.darkforest.lab | 10.10.5.2 | Log Analysis & SIEM |

The Security network provides centralized security monitoring and event analysis.

---

# Core Components

## pfSense

Acts as the central firewall and routing platform.

Responsibilities:

* Inter-network routing
* Internet access
* Security policies
* Suricata IDS/IPS

Interfaces:

```text
WAN
Management
Infrastructure
Servers
Clients
Security
```

---

## DNS

The DNS server hosts the internal zone:

```text
darkforest.lab
```

and provides:

* Forward resolution
* Reverse resolution
* Recursive lookups
* Internal service discovery

All systems rely on DNS for name resolution.

---

## NTP

Chrony is used as the central time authority.

All systems synchronize against:

```text
ntp.darkforest.lab
10.10.2.2
```

to ensure:

* Log consistency
* SIEM correlation
* TLS certificate validity
* Dashboard accuracy

---

## PKI

DarkForest operates its own internal Public Key Infrastructure based on Smallstep.

Architecture:

```text
Root CA
    │
    ▼
Intermediate CA
    │
    ▼
TLS Certificates
```

The PKI issues certificates for:

* Gitea
* Grafana
* Guacamole
* BookStack
* Wazuh
* Splunk
* DNS
* NTP

Client systems trust the Root CA through their local trust stores. HTTPS is deployed across the infrastructure.

---

# Monitoring Stack

Monitoring is based on:

```text
Node Exporter
        │
        ▼
Prometheus
        │
        ▼
Grafana
```

Metrics are collected from:

* Linux servers
* pfSense
* DNS
* NTP
* Wazuh
* Splunk
* Gitea
* BookStack
* CA

All monitoring targets are currently operational.

---

# Security Monitoring

## Wazuh

Provides:

* Endpoint monitoring
* Log collection
* Security alerts
* Dashboard visualization

## Splunk

Provides:

* Centralized log indexing
* Search and correlation
* Security investigations
* Event analysis

Together they form the SOC layer of DarkForest.

---

# Documentation Philosophy

DarkForest V2 follows a strict documentation workflow.

```text
Production System
        │
        ▼
Configuration Export
        │
        ▼
Guacamole
        │
        ▼
Git
        │
        ▼
Gitea
```

Every documented host contains:

```text
README.md
configs/
exports/
```

allowing auditing, reconstruction and version control of the infrastructure.

---

# Current Status

| Component  | Status |
| ---------- | ------ |
| pfSense    | ✅      |
| DNS        | ✅      |
| NTP        | ✅      |
| CA         | ✅      |
| Guacamole  | ✅      |
| Grafana    | ✅      |
| Prometheus | ✅      |
| BookStack  | ✅      |
| Gitea      | ✅      |
| 404jun     | ✅      |
| Wazuh      | ✅      |
| Splunk     | ✅      |

DarkForest V2 documentation is complete and serves as the source of truth for the entire laboratory infrastructure.
