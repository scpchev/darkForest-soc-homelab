# Gitea (10.10.3.2) - Documentation Technique Complète DarkForest V2

## Présentation

Gitea constitue le référentiel Git central de l'infrastructure DarkForest.

Il héberge :

* documentation technique
* exports de configuration
* procédures de reconstruction
* documentation SOC
* documentation PKI
* projets DarkForest

L'objectif est de permettre la reconstruction complète de l'infrastructure à partir des dépôts Git hébergés sur Gitea.

---

# Informations générales

| Paramètre       | Valeur                       |
| --------------- | ---------------------------- |
| Hostname        | Gitea                        |
| FQDN            | gitea.darkforest.lab         |
| IP              | 10.10.3.2                    |
| VLAN            | Services                     |
| OS              | Debian GNU/Linux 13 (Trixie) |
| Type            | LXC Proxmox                  |
| Application     | Gitea                        |
| Version         | Installation native          |
| Base de données | MariaDB 11.8.6               |
| Reverse Proxy   | Nginx                        |
| HTTPS           | DarkForest CA                |
| Monitoring      | Node Exporter                |

---

# Architecture

```text
Administrateur
        │
        ▼
https://gitea.darkforest.lab
        │
        ▼
Nginx
443/TCP
        │
        ▼
Gitea
3000/TCP
        │
        ▼
MariaDB
3306/TCP
```

---

# Configuration réelle

## Application Gitea

Configuration principale :

```text
/etc/gitea/app.ini
```

Service :

```text
/etc/systemd/system/gitea.service
```

Binaire :

```text
/usr/local/bin/gitea
```

Utilisateur :

```text
git
```

Mode :

```text
prod
```

Répertoire de travail :

```text
/var/lib/gitea
```

Dépôts Git :

```text
/var/lib/gitea/data/gitea-repositories
```

---

## Configuration base de données

Type :

```text
MariaDB
```

Version :

```text
11.8.6
```

Configuration observée :

```text
HOST = 127.0.0.1:3306
NAME = gitea
USER = gitea
```

Base utilisée :

```text
gitea
```

Bases présentes :

```text
gitea
information_schema
mysql
performance_schema
sys
```

---

## Configuration Web

Gitea écoute sur :

```text
3000/TCP
```

Configuration :

```ini
HTTP_PORT = 3000
ROOT_URL = http://gitea.darkforest.lab:3000/
```

---

## Reverse Proxy

Configuration :

```text
/etc/nginx/sites-available/gitea
```

Publication :

```text
https://gitea.darkforest.lab
```

Backend :

```text
http://127.0.0.1:3000
```

Certificats :

```text
/etc/nginx/ssl/gitea.crt
/etc/nginx/ssl/gitea.key
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

État :

```text
active (running)
```

---

# Audit complet

## Vérification système

```bash
hostname -f

cat /etc/os-release

ip -br a
```

Résultat attendu :

```text
Gitea.darkforest.lab
Debian GNU/Linux 13
10.10.3.2/24
```

---

## Vérification des services

```bash
systemctl list-units \
--type=service \
--state=running
```

Services attendus :

```text
gitea
mariadb
nginx
node_exporter
postfix
ssh
```

---

## Vérification Gitea

```bash
systemctl status gitea --no-pager
```

Résultat attendu :

```text
active (running)
```

---

## Vérification MariaDB

```bash
systemctl status mariadb --no-pager
```

Résultat attendu :

```text
active (running)
```

---

## Vérification Nginx

```bash
systemctl status nginx --no-pager

nginx -t
```

Résultat attendu :

```text
active (running)
syntax is ok
test is successful
```

---

## Vérification HTTPS

```bash
openssl s_client \
-connect gitea.darkforest.lab:443 \
-servername gitea.darkforest.lab \
</dev/null 2>/dev/null \
| openssl x509 -noout -subject -issuer -dates
```

Résultat attendu :

```text
CN=gitea.darkforest.lab

Issuer=
DarkForest Root CA
DarkForest Intermediate CA
```

Validité :

```text
365 jours
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

## Vérification accès Web

```bash
curl -Ik https://gitea.darkforest.lab
```

Résultat attendu :

```text
HTTP/1.1 200 OK
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
3000/tcp
3306/tcp
9100/tcp
```

---

# Export

Répertoire :

```text
docs/gitea/
```

Contenu exporté :

```text
configs/
├── app.ini
├── gitea
├── gitea.service
├── nginx.conf
└── node_exporter.service

exports/
├── databases.txt
├── gitea-cert.txt
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

Dépôt utilisé :

```text
darkForest-v2-private
```

Branche :

```text
master
```

---

# Validation

Validation applicative :

```text
https://gitea.darkforest.lab
```

Critères :

```text
Connexion utilisateur
Création dépôt
Clone dépôt
Push dépôt
Pull dépôt
```

Validation HTTPS :

```text
Certificat DarkForest CA
365 jours
```

Validation Monitoring :

```text
Target Prometheus = UP
```

---

# Reconstruction

## Création du conteneur

```text
Hostname : Gitea
IP       : 10.10.3.2
OS       : Debian 13
VLAN     : Services
```

---

## Installation

Installer :

```text
Gitea
MariaDB
Nginx
Node Exporter
OpenSSH
```

---

## Restauration

Restaurer :

```text
app.ini
gitea.service
nginx.conf
node_exporter.service
```

depuis :

```text
darkForest-v2-private/docs/gitea
```

---

## HTTPS

Restaurer :

```text
gitea.crt
gitea.key
```

Configurer Nginx.

---

## Validation finale

Contrôles :

```bash
systemctl status gitea

systemctl status mariadb

systemctl status nginx

systemctl status node_exporter

curl -Ik https://gitea.darkforest.lab
```

Résultat attendu :

```text
Infrastructure opérationnelle
Accès Git fonctionnel
HTTPS valide
```
