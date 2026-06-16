# CA (10.10.2.3) - Documentation Technique Complète DarkForest V2

## Présentation

Le serveur CA constitue l'autorité de certification interne de l'infrastructure DarkForest.

Il repose sur **Smallstep CA** et fournit l'ensemble des certificats TLS utilisés dans le laboratoire.

La PKI permet :

- L'émission de certificats TLS internes
- La sécurisation des services DarkForest
- La validation des identités des serveurs
- L'administration centralisée des certificats
- Le renouvellement simplifié des certificats

---

# Informations générales

| Paramètre | Valeur |
|------------|----------|
| Hostname | CA |
| FQDN | ca.darkforest.lab |
| IP | 10.10.2.3 |
| VLAN | Infrastructure |
| OS | Debian GNU/Linux 13.5 |
| Type | LXC Proxmox |
| PKI | Smallstep CA |
| Monitoring | Node Exporter |
| HTTPS | TCP/443 |

---

# Architecture PKI

```text
DarkForest Root CA
        │
        ▼
DarkForest Intermediate CA
        │
        ▼
Smallstep CA
        │
        ▼
Services DarkForest
