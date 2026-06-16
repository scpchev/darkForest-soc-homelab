# Guacamole

## Overview

Apache Guacamole provides centralized remote administration access for the DarkForest infrastructure.

The service is hosted on an Ubuntu 24.04 VM located in the Management VLAN.

## System Information

| Property         | Value              |
| ---------------- | ------------------ |
| Service          | Apache Guacamole   |
| IP Address       | 10.10.1.1          |
| VLAN             | Management         |
| Operating System | Ubuntu 24.04.4 LTS |

## Components

### Docker Containers

* guacamole
* guacd
* guac-db (MySQL 8)

### Reverse Proxy

Nginx provides HTTPS access to Guacamole.

### TLS

Certificates are issued by the DarkForest internal Certificate Authority.

### Monitoring

Node Exporter is installed and monitored by Prometheus.

### Time Synchronization

Chrony synchronizes against ntp.darkforest.lab (10.10.2.2).

## Additional Functions

This VM also hosts reverse proxy access for:

* Splunk
* Wazuh

## Configuration Files

The complete configuration is stored in the configs directory.
