# Wazuh (10.10.5.1) - Documentation Technique Complète DarkForest V2

## Présentation

Wazuh constitue la plateforme SIEM principale de l'infrastructure DarkForest.

Le serveur regroupe sur une seule machine :

* Wazuh Manager
* Wazuh Indexer
* Wazuh Dashboard
* Filebeat
* Node Exporter

L'objectif est de centraliser la collecte, l'indexation, la corrélation et la visualisation des événements de sécurité.

---

# Informations générales

| Paramètre       | Valeur          |
| --------------- | --------------- |
| Hostname        | wazuh           |
| IP              | 10.10.5.1       |
| VLAN            | Security        |
| OS              | Rocky Linux 9.5 |
| Type            | Serveur SIEM    |
| Dashboard       | Wazuh Dashboard |
| Indexer         | Wazuh Indexer   |
| Manager         | Wazuh Manager   |
| Monitoring      | Node Exporter   |
| Synchronisation | Chrony          |
| HTTPS           | DarkForest CA   |

---

# Architecture

```text
Agents Wazuh
       │
       ▼
Wazuh Manager
       │
       ▼
Filebeat
       │
       ▼
Wazuh Indexer
       │
       ▼
Wazuh Dashboard
       │
       ▼
HTTPS
```

---

# Configuration réelle

## Wazuh Manager

Configuration :

```text
/var/ossec/etc/ossec.conf
```

Service :

```text
wazuh-manager.service
```

Ports observés :

```text
1514/tcp
1515/tcp
55000/tcp
```

---

## Wazuh Indexer

Configuration :

```text
/etc/wazuh-indexer/opensearch.yml
```

Service :

```text
wazuh-indexer.service
```

Ports observés :

```text
9200/tcp
9300/tcp
```

---

## Wazuh Dashboard

Configuration :

```text
/etc/wazuh-dashboard/opensearch_dashboards.yml
```

Service :

```text
wazuh-dashboard.service
```

Publication :

```text
https://wazuh.darkforest.lab
```

Port :

```text
443/tcp
```

---

## Filebeat

Configuration :

```text
/etc/filebeat/filebeat.yml
```

Service :

```text
filebeat.service
```

---

## Monitoring

Configuration :

```text
node_exporter.service
```

Port :

```text
9100/tcp
```

---

## Synchronisation temporelle

Configuration :

```text
/etc/chrony.conf
```

Service :

```text
chronyd.service
```

---

# Audit complet

## Vérification système

```bash
hostname -f

cat /etc/os-release

ip -br a
```

Résultat :

```text
Hostname : wazuh
IP       : 10.10.5.1
OS       : Rocky Linux 9.5
```

---

## Vérification services

```bash
systemctl list-units \
--type=service \
--state=running
```

Services observés :

```text
wazuh-manager
wazuh-indexer
wazuh-dashboard
filebeat
chronyd
node_exporter
```

---

## Vérification Dashboard

```bash
curl -Ik https://wazuh.darkforest.lab
```

Résultat :

```text
HTTP/1.1 302 Found
location: /app/login
```

---

## Vérification HTTPS

```bash
openssl s_client \
-connect wazuh.darkforest.lab:443 \
-servername wazuh.darkforest.lab \
</dev/null 2>/dev/null \
| openssl x509 -noout -subject -issuer -dates
```

Résultat :

```text
CN=wazuh.darkforest.lab

Issuer=
DarkForest Root CA Intermediate CA
```

Validité :

```text
15/06/2026
→
15/06/2027
```

---

## Vérification agents

Commande :

```bash
sudo /var/ossec/bin/agent_control -l
```

Résultat :

```text
ID 000
wazuh (server)
127.0.0.1
```

État actuel :

```text
Aucun agent distant enregistré
Serveur local uniquement
```

---

## Vérification ports

```bash
ss -tulpn
```

Ports observés :

```text
22/tcp
443/tcp
1514/tcp
1515/tcp
55000/tcp
9100/tcp
9200/tcp
9300/tcp
```

---

# Export

Répertoire :

```text
docs/wazuh
```

Contenu :

```text
configs/
├── chrony.conf
├── filebeat.yml
├── node_exporter.service
├── opensearch.yml
├── opensearch_dashboards.yml
└── ossec.conf

exports/
├── chrony-status.txt
├── listening-ports.txt
└── system-info.txt
```

---

# Upload Gitea

Workflow DarkForest :

```text
Serveur
   ↓
Export
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

Validation Dashboard :

```text
https://wazuh.darkforest.lab
```

Validation HTTPS :

```text
Certificat DarkForest CA
365 jours
```

Validation Monitoring :

```text
Node Exporter actif
```

Validation NTP :

```text
Chrony synchronisé
```

---

# Reconstruction

## Création machine

```text
Hostname : wazuh
IP       : 10.10.5.1
OS       : Rocky Linux 9
VLAN     : Security
```

---

## Installation

Installer :

```text
Wazuh Manager
Wazuh Indexer
Wazuh Dashboard
Filebeat
Node Exporter
Chrony
```

---

## Restauration

Restaurer :

```text
ossec.conf
filebeat.yml
opensearch.yml
opensearch_dashboards.yml
chrony.conf
node_exporter.service
```

depuis :

```text
darkForest-v2-private/docs/wazuh
```

---

## Validation finale

```bash
systemctl status wazuh-manager

systemctl status wazuh-indexer

systemctl status wazuh-dashboard

systemctl status filebeat

curl -Ik https://wazuh.darkforest.lab
```

Résultat attendu :

```text
Dashboard accessible
Indexer actif
Manager actif
HTTPS valide
```
