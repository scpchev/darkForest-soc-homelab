# Splunk (10.10.5.2) - Documentation Technique Complète DarkForest V2

# Présentation

Splunk constitue la plateforme SIEM secondaire de l'infrastructure DarkForest.

Le serveur centralise la collecte, l'indexation, la recherche et la visualisation des événements de sécurité.

L'interface utilisateur est publiée via Nginx avec un certificat TLS délivré par la PKI DarkForest.

Le serveur est également supervisé par Prometheus via Node Exporter et synchronisé sur l'infrastructure NTP DarkForest.

---

# Informations générales

| Paramètre       | Valeur                   |
| --------------- | ------------------------ |
| Hostname        | Splunk                   |
| IP              | 10.10.5.2                |
| VLAN            | Security                 |
| OS              | Rocky Linux 9.7          |
| Version         | Splunk Enterprise 10.2.2 |
| Reverse Proxy   | Nginx                    |
| Monitoring      | Node Exporter            |
| Synchronisation | Chrony                   |
| HTTPS           | DarkForest CA            |

---

# Architecture

```text
Utilisateur
      │
      ▼
https://splunk.darkforest.lab
      │
      ▼
Nginx Reverse Proxy
443/TCP
      │
      ▼
Splunk Web
8000/TCP
      │
      ▼
Splunkd
8089/TCP
      │
      ▼
Indexes Splunk
```

---

# Configuration réelle

## Splunk Enterprise

Configuration locale :

```text
/opt/splunk/etc/system/local/server.conf
/opt/splunk/etc/system/local/web.conf
```

Configuration Search App :

```text
/opt/splunk/etc/apps/search/local/inputs.conf
```

Services :

```text
Splunkd.service
```

---

## Reverse Proxy

Configuration :

```text
/etc/nginx/conf.d/splunk.conf
/etc/nginx/nginx.conf
```

Services :

```text
nginx.service
```

---

## Monitoring

Configuration :

```text
/etc/systemd/system/node_exporter.service
```

Port :

```text
9100/TCP
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
Hostname : Splunk
IP       : 10.10.5.2
OS       : Rocky Linux 9.7
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
Splunkd.service
nginx.service
node_exporter.service
chronyd.service
```

---

## Vérification version Splunk

```bash
sudo /opt/splunk/bin/splunk version
```

Résultat :

```text
Splunk Enterprise 10.2.2
```

---

## Vérification serveur Splunk

```bash
sudo /opt/splunk/bin/splunk show servername
```

Résultat observé :

```text
localhost.localdomain
```

Remarque :

```text
Hostname système : Splunk
Servername Splunk : localhost.localdomain
```

Cette différence doit être prise en compte lors d'une reconstruction.

---

## Vérification HTTPS

```bash
openssl s_client \
-connect splunk.darkforest.lab:443 \
-servername splunk.darkforest.lab \
</dev/null 2>/dev/null \
| openssl x509 -noout -subject -issuer -dates
```

Résultat :

```text
CN=splunk.darkforest.lab

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

## Vérification accès Web

```bash
curl -Ik https://splunk.darkforest.lab
```

Résultat :

```text
HTTP/1.1 303 See Other
Location: /en-US/
```

Validation :

```text
Interface Splunk accessible
```

---

## Vérification NTP

```bash
chronyc tracking

chronyc sources -v
```

Résultat :

```text
Source : ntp.darkforest.lab
Synchronisation : OK
```

---

## Vérification Monitoring

```bash
systemctl status node_exporter
```

Résultat attendu :

```text
active (running)
```

---

## Vérification ports

```bash
ss -tulpn
```

Ports observés :

```text
22/tcp
80/tcp
443/tcp
8000/tcp
8089/tcp
9100/tcp
9997/tcp
5432/tcp
```

---

# Export

Répertoire :

```text
docs/splunk
```

Contenu :

```text
configs/
├── server.conf
├── web.conf
├── inputs.conf
├── splunk.conf
├── nginx.conf
├── node_exporter.service
└── chrony.conf

exports/
├── system-info.txt
├── listening-ports.txt
├── splunk-cert.txt
├── chrony-status.txt
└── splunk-version.txt
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

Validation Web :

```text
https://splunk.darkforest.lab
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
Synchronisation via ntp.darkforest.lab
```

Validation Splunk :

```text
Splunkd actif
Interface Web accessible
```

---

# Reconstruction

## Création machine

```text
Hostname : Splunk
IP       : 10.10.5.2
OS       : Rocky Linux 9
VLAN     : Security
```

---

## Installation

Installer :

```text
Splunk Enterprise
Nginx
Chrony
Node Exporter
```

---

## Restauration

Restaurer :

```text
server.conf
web.conf
inputs.conf
splunk.conf
nginx.conf
chrony.conf
node_exporter.service
```

depuis :

```text
darkForest-v2-private/docs/splunk
```

---

## Validation finale

```bash
systemctl status Splunkd

systemctl status nginx

systemctl status node_exporter

chronyc tracking

curl -Ik https://splunk.darkforest.lab
```

Résultat attendu :

```text
Splunk opérationnel
HTTPS valide
Monitoring actif
NTP synchronisé
```
