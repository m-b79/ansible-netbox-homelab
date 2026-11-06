# Ansible NetBox Homelab

Laboratoire Infrastructure as Code pour automatiser le déploiement, la configuration et la maintenance de NetBox avec Ansible.

## Infrastructure

- **Control Node** : Raspberry Pi, Debian 13, Python 3.13
- **Managed Node** : Oracle Cloud Always Free, Ubuntu 24.04 ARM64
- **NetBox** : installation native depuis Git
- **PostgreSQL** : base de données applicative
- **Redis** : cache et tâches asynchrones
- **Gunicorn** : serveur WSGI
- **Apache** : reverse proxy HTTP
- **Accès** : SSH et tunnels privés

## Prérequis

- Python compatible avec la version d'Ansible utilisée
- Git
- Accès SSH à la VM Oracle
- Identifiants Ansible Vault

## Installation du Control Node

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
ansible-galaxy collection install -r requirements.yml
```

## Inventaire

L'inventaire est situé dans `inventories/lab/hosts.yml`.

La connexion SSH utilise un alias défini dans `~/.ssh/config`.

## Déploiement

Depuis la racine du projet :

```bash
ansible-playbook playbooks/patch-linux.yml --ask-vault-pass
ansible-playbook playbooks/deploy-postgresql.yml --ask-vault-pass
ansible-playbook playbooks/deploy-redis.yml --ask-vault-pass
ansible-playbook playbooks/deploy-netbox.yml --ask-vault-pass
ansible-playbook playbooks/deploy-apache.yml --ask-vault-pass
```

## Gestion des secrets

Les secrets sont stockés dans des fichiers chiffrés avec Ansible Vault.

Aucun mot de passe ou clé privée ne doit être enregistré en clair dans le dépôt.

## Objectifs futurs

- Sauvegarde et restauration PostgreSQL
- Automatisation des upgrades NetBox
- Tests et validation des rôles
- Intégration CI/CD
- Gestion de plusieurs environnements

## Statut

Laboratoire fonctionnel. Les rôles sont encore en cours de fiabilisation et ne doivent pas être considérés comme prêts pour la production.
