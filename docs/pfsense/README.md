# pfSense (192.168.1.50) - Documentation Technique Complète DarkForest V2

# Présentation

pfSense constitue le cœur de l'infrastructure réseau DarkForest.

Le firewall assure :

- Routage inter-réseaux
- Contrôle d'accès
- NAT
- Passerelle Internet
- SNMP pour Prometheus
- NTP
- IDS/IPS Suricata
- Publication des services internes

pfSense est la source de vérité de l'architecture réseau DarkForest.

---

# Informations générales

| Paramètre | Valeur |
|------------|----------|
| Produit | pfSense CE |
| Version | 2.8.1-RELEASE |
| OS | FreeBSD 15 |
| Rôle | Firewall principal |
| WAN Gateway | 192.168.1.1 |
| IDS/IPS | Suricata |
| SNMP | Activé |
| NTP | Activé |
| DNS Resolver | Désactivé |

---

# Architecture

## Interfaces

```text
em0 → WAN
em1 → Management
em2 → Infrastructure
em3 → Servers
em4 → Clients
em5 → Security
```

## Segmentation réseau

```text
WAN
└── 192.168.1.0/24

Management
└── 10.10.1.0/24

Infrastructure
└── 10.10.2.0/24

Servers
└── 10.10.3.0/24

Clients
└── 10.10.4.0/24

Security
└── 10.10.5.0/24
```

## Architecture logique

```text
Internet
    │
    ▼
pfSense
    │
    ├── Management
    │     ├── Guacamole
    │     ├── Grafana
    │     └── Prometheus
    │
    ├── Infrastructure
    │     ├── DNS
    │     ├── NTP
    │     └── CA
    │
    ├── Services
    │     ├── BookStack
    │     ├── Gitea
    │     └── 404jun
    │
    ├── Clients
    │
    └── Security
          ├── Wazuh
          └── Splunk
```

---

# Configuration réelle

## Configuration principale

```text
/cf/conf/config.xml
```

Ce fichier contient :

```text
Interfaces
Routage
Firewall Rules
NAT
DHCP
SNMP
NTP
Utilisateurs
Certificats
Aliases
Suricata
```

## Historique des configurations

```text
/cf/conf/backup/
```

Sauvegardes automatiques pfSense :

```text
Mai 2026
↓
Juin 2026
```

Conservées dans :

```text
docs/pfsense/backup
```

## Suricata

Packages installés :

```text
pfSense-pkg-suricata 7.0.8_5
suricata 7.0.11
```

Fonctions :

```text
IDS
IPS
Détection réseau
Inspection protocoles
```

## DNS

État observé :

```text
Unbound désactivé
```

Validation :

```xml
<unbound>
    <enable></enable>
</unbound>
```

Le DNS est assuré par :

```text
dns.darkforest.lab
10.10.2.1
```

---

# Audit complet

## Vérification système

```bash
uname -a

cat /etc/version
```

Résultat :

```text
pfSense CE 2.8.1-RELEASE
FreeBSD 15
```

## Vérification configuration

```bash
ls -lah /conf

ls -lah /cf/conf
```

Résultat :

```text
/conf -> /cf/conf
```

Configuration active :

```text
/cf/conf/config.xml
```

## Vérification interfaces

```bash
grep "<if>" /cf/conf/config.xml
```

Résultat :

```text
em0
em1
em2
em3
em4
em5
```

## Vérification descriptions

```bash
grep "<descr>" /cf/conf/config.xml
```

Résultat :

```text
WAN
Management
Infra
Servers
Clients
Security
```

## Vérification VLANs

```bash
grep "<vlan>" /cf/conf/config.xml
```

Résultat :

```text
Aucun résultat
```

Validation :

```text
Aucun VLAN configuré
```

La segmentation est réalisée via :

```text
Interfaces dédiées
```

## Vérification DNS Resolver

```bash
grep -A20 unbound /cf/conf/config.xml
```

Résultat :

```text
Unbound désactivé
```

## Vérification Gateway

```bash
grep -A10 gateways /cf/conf/config.xml
```

Résultat :

```text
WANGW
192.168.1.1
```

## Vérification Suricata

```bash
pkg info | grep suricata
```

Résultat :

```text
pfSense-pkg-suricata-7.0.8_5

suricata-7.0.11
```

## Vérification sauvegardes

```bash
ls -lah /cf/conf/backup
```

Résultat :

```text
Historique complet des configurations
```

---

# Export

Répertoire :

```text
docs/pfsense
```

Contenu :

```text
configs/
└── config.xml

backup/
├── backup.cache
├── config-*.xml
└── ...
```

---

# Upload Gitea

Workflow DarkForest :

```text
pfSense
   ↓
SCP
   ↓
Guacamole
   ↓
Git Add
   ↓
Git Commit
   ↓
Git Push
   ↓
Gitea
```

Dépôt :

```text
darkForest-v2-private
```

Branche :

```text
master
```

---

# Validation

## Vérification configuration

```bash
ls -lah /cf/conf/config.xml
```

## Vérification Suricata

```bash
pkg info | grep suricata
```

## Vérification NTP

```bash
ntpq -pn
```

Résultat attendu :

```text
10.10.2.2
```

## Vérification SNMP

Validation depuis Prometheus :

```text
SNMP Exporter
UP
```

---

# Reconstruction

## Création machine

```text
Produit : pfSense CE
Version : 2.8.1
```

## Configuration interfaces

```text
WAN
Management
Infrastructure
Servers
Clients
Security
```

## Restauration

Restaurer :

```text
config.xml
```

depuis :

```text
docs/pfsense/configs
```

ou restaurer une version historique :

```text
docs/pfsense/backup
```

## Validation finale

```bash
ntpq -pn

pkg info | grep suricata
```

Validation attendue :

```text
Routage opérationnel
Firewall actif
Suricata actif
SNMP actif
NTP synchronisé
```
