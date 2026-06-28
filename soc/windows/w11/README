# Scenario 001 - Aggressive Reconnaissance Scan

## Command

```bash
nmap -Pn -A -T4 10.10.6.2
```

### Description

Performs an aggressive reconnaissance scan against the Windows 11 host.

### Enabled Features

- Operating System Detection
- Service Version Detection
- Default NSE Scripts
- Traceroute
- TCP Fingerprinting

### Objective

Obtain the maximum amount of information about the target host while generating rich network traffic for Suricata detection.

---

# Scenario 002 - Full TCP Port Scan

## Command

```bash
nmap -Pn -p- -T4 10.10.6.2
```

### Description

Scans all 65,535 TCP ports of the target.

### Objective

Enumerate every accessible TCP service and identify unexpected open ports.

---

# Scenario 003 - UDP Top Ports Scan

## Command

```bash
nmap -Pn -sU --top-ports 100 10.10.6.2
```

### Description

Scans the 100 most common UDP ports.

### Objective

Identify exposed UDP services such as NetBIOS, ISAKMP, UPnP and mDNS.

---

# Summary

| Scenario | Command | Goal |
|----------|---------|------|
| Scenario 001 | `nmap -Pn -A -T4 10.10.6.2` | Aggressive host and service fingerprinting |
| Scenario 002 | `nmap -Pn -p- -T4 10.10.6.2` | Full TCP port enumeration |
| Scenario 003 | `nmap -Pn -sU --top-ports 100 10.10.6.2` | UDP reconnaissance |
