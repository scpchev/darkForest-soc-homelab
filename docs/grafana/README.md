# Grafana

## Overview

Grafana provides visualization and monitoring dashboards for the DarkForest infrastructure.

The service is hosted in the Management VLAN and accessed through HTTPS using a reverse proxy.

## System Information

| Property         | Value          |
| ---------------- | -------------- |
| Service          | Grafana        |
| IP Address       | 10.10.1.2      |
| VLAN             | Management     |
| Platform         | Proxmox LXC    |
| Operating System | Debian 13      |
| Version          | Grafana 13.0.2 |

## Architecture

Client
↓
HTTPS
↓
Nginx
↓
Grafana (localhost:3000)

## Monitoring

Node Exporter is installed and monitored by Prometheus.

## TLS

TLS certificates are issued by the DarkForest internal CA.

## Time Synchronization

Time synchronization is inherited from the Proxmox host.

Chrony was tested but is not required inside the LXC container.

## Configuration

Main configuration files are stored in the configs directory.
