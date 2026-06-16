# 404jun (10.10.3.9) - Documentation Technique Complète DarkForest V2

# Présentation

404jun est un site web statique hébergé dans le VLAN Services de l'infrastructure DarkForest.

Le service est publié via Nginx avec un certificat TLS délivré par la PKI DarkForest.

Le serveur participe également au monitoring de l'infrastructure via Node Exporter et à la synchronisation temporelle via Chrony.

---

# Informations générales

| Paramètre | Valeur |
|------------|----------|
| Hostname | 404junior |
| FQDN public | 404jun.darkforest.lab |
| IP | 10.10.3.9 |
| VLAN | Services |
| OS | Rocky Linux 9.8 (Blue Onyx) |
| Serveur Web | Nginx |
| Monitoring | Node Exporter |
| Synchronisation | Chrony |
| HTTPS | DarkForest CA |

---

# Architecture

```text
Utilisateur
      │
      ▼
https://404jun.darkforest.lab
      │
      ▼
Nginx
      │
      ▼
Site statique
```

---

# Configuration réelle

## Nginx

Configuration principale :

```text
/etc/nginx/nginx.conf
```

VirtualHost :

```text
/etc/nginx/conf.d/404jun.conf
```

Certificats :

```text
/etc/nginx/ssl/404jun.crt
/etc/nginx/ssl/404jun.key
```

---

## Monitoring

Service :

```text
node_exporter.service
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
Hostname : 404junior
IP       : 10.10.3.9
OS       : Rocky Linux 9.8
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
nginx
chronyd
node_exporter
sshd
firewalld
rsyslog
NetworkManager
```

---

## Vérification Nginx

```bash
systemctl status nginx

nginx -t
```

Résultat attendu :

```text
active (running)
configuration file test successful
```

---

## Vérification HTTPS

```bash
openssl s_client \
-connect 404jun.darkforest.lab:443 \
-servername 404jun.darkforest.lab \
</dev/null 2>/dev/null \
| openssl x509 -noout -subject -issuer -dates
```

Validation :

```text
CN=404jun.darkforest.lab

Issuer=
DarkForest Root CA Intermediate CA
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

## Vérification NTP

```bash
chronyc tracking

chronyc sources -v
```

Validation :

```text
Synchronisation via ntp.darkforest.lab
```

---

## Vérification accès Web

```bash
curl -Ik https://404jun.darkforest.lab
```

Résultat attendu :

```text
HTTP/2 200
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
9100/tcp
```

---

# Export

Répertoire :

```text
docs/404jun
```

Contenu :

```text
configs/
├── 404jun.conf
├── nginx.conf
├── chrony.conf
└── node_exporter.service

exports/
├── 404jun-cert.txt
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

Validation Web :

```text
https://404jun.darkforest.lab
```

Validation HTTPS :

```text
Certificat DarkForest CA
```

Validation Monitoring :

```text
Target Prometheus = UP
```

Validation NTP :

```text
Chrony synchronisé
```

---

# Reconstruction

## Création machine

```text
Hostname : 404junior
IP       : 10.10.3.9
OS       : Rocky Linux 9
VLAN     : Services
```

---

## Installation

Installer :

```text
Nginx
Chrony
Node Exporter
OpenSSH
```

---

## Restauration

Restaurer :

```text
404jun.conf
nginx.conf
chrony.conf
node_exporter.service
```

depuis :

```text
darkForest-v2-private/docs/404jun
```

---

## HTTPS

Restaurer :

```text
404jun.crt
404jun.key
```

Configurer Nginx puis :

```bash
nginx -t

systemctl restart nginx
```

---

## Validation finale

```bash
systemctl status nginx

systemctl status chronyd

systemctl status node_exporter

curl -Ik https://404jun.darkforest.lab
```

Résultat attendu :

```text
Site accessible
HTTPS valide
Monitoring actif
NTP synchronisé
```
