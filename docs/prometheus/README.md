# Prometheus

## Overview

Prometheus is the central monitoring platform for the DarkForest infrastructure.

It collects metrics from Linux servers through Node Exporter and network equipment through SNMP Exporter.

## System Information

* Hostname: prmths.darkforest.lab
* IP Address: 10.10.1.3
* Platform: Debian LXC
* Prometheus Version: 3.12.0

## Monitoring Architecture

Prometheus
├── Node Exporter
│   ├── Guacamole
│   ├── Grafana
│   ├── Prometheus
│   ├── DNS
│   ├── NTP
│   ├── CA
│   ├── BookStack
│   ├── Gitea
│   ├── 404jun
│   ├── Wazuh
│   └── Splunk
│
└── SNMP Exporter
└── pfSense

## Collection Settings

* Scrape Interval: 15 seconds
* Evaluation Interval: 15 seconds

## Configuration Files

All exported configuration files are located in the configs directory.

## Notes

CA, Gitea and 404jun monitoring were added during the DarkForest v2 documentation rebuild project.
