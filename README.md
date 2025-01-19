# Linux Utilisation Avancée

## Base de Linux

### 1. Introduction au Shell Linux

#### 1.1 Connexion et Types d'Utilisateurs
- Connexion avec login et mot de passe (invisible à la saisie)
- Types d'invite de commande :
  - `$` : utilisateur standard
  - `#` : administrateur (root)

#### 1.2 Structure des Commandes
- Le shell par défaut est Bash (`/bin/bash`)
- Auto-complétion avec la touche Tab
- Commandes de base pour la navigation :
  - `pwd` : affiche le répertoire courant
  - `cd` : change de répertoire
  - `clear` : efface l'écran

---

## 2. Gestion des Fichiers et Répertoires

### 2.1 Commandes Essentielles
- `ls` : liste les fichiers
  - `-a` : affiche les fichiers cachés
  - `-l` : affiche les détails
- `touch` : crée un fichier
- `mkdir` : crée un répertoire
- `rm` : supprime fichiers/répertoires
  - `-r` : suppression récursive
- `cp` : copie
- `mv` : déplace/renomme

### 2.2 Manipulation de Fichiers
- `cat` : affiche le contenu
- `head` : affiche le début
- `tail` : affiche la fin
- `more`/`less` : pagination
- `grep` : recherche dans les fichiers
- Éditeurs :
  - `nano` : simple et intuitif
  - `vim` : plus puissant mais complexe

---

## 3. Gestion des Utilisateurs et Droits

### 3.1 Utilisateurs
- `adduser` : crée un utilisateur
- `passwd` : change le mot de passe
- `usermod` : modifie les paramètres
- `userdel` : supprime un utilisateur
- Fichiers importants :
  - `/etc/passwd` : informations utilisateurs
  - `/etc/shadow` : mots de passe chiffrés

### 3.2 Groupes
- `groupadd` : crée un groupe
- `groupdel` : supprime un groupe
- `adduser user group` : ajoute un utilisateur à un groupe
- `gpasswd` : gère les groupes

### 3.3 Permissions
- Format : `rwx` (lecture/écriture/exécution)
- Commandes :
  - `chmod` : modifie les permissions
  - `chown` : change le propriétaire
  - `chgrp` : change le groupe

---

## 4. Administration Système

### 4.1 Gestion des Paquets (APT)
- `apt update` : met à jour la liste des paquets
- `apt upgrade` : met à jour le système
- `apt install` : installe un paquet
- `apt remove` : désinstalle un paquet
  - `--purge` : supprime aussi la configuration

### 4.2 Commandes Système
- `ip a` : affiche la configuration réseau
- `date` : affiche date et heure
- `who` : utilisateurs connectés
- `uname -a` : version du noyau
- `man` : manuel des commandes

### 4.3 Redirections
- `>` : redirige la sortie (écrase)
- `>>` : redirige la sortie (ajoute)
- `<` : redirige l'entrée

---

## 5. Structure des Répertoires Linux

### 5.1 Répertoires Principaux
- `/` : racine
- `/home` : répertoires utilisateurs
- `/etc` : fichiers de configuration
- `/bin`, `/sbin` : exécutables système
- `/usr` : programmes et données
- `/var` : données variables
- `/tmp` : fichiers temporaires
- `/root` : répertoire administrateur

### 5.2 Organisation
- Séparation claire entre :
  - Fichiers système
  - Données utilisateurs
  - Configurations
  - Logs
  - Applications installées

---

## Guide de Configuration SSH sur Debian 12

### 1. Installation du Serveur SSH
Pour permettre les connexions à distance sur votre serveur Debian, vous devez installer le paquet OpenSSH :
```bash
apt install ssh
```

### 2. Configuration de Base

#### 2.1 Vérification de l'adresse IP
Pour connaître l'adresse IP de votre serveur Debian :
```bash
ip a
# ou
ip address
```

#### 2.2 Connexion SSH Standard
1. Utilisez un client SSH comme PuTTY.
2. Entrez l'adresse IP de votre serveur Debian.
3. Lors de la première connexion, acceptez l'alerte de sécurité (fingerprint).
4. Par défaut, la connexion directe avec root est désactivée.
5. Connectez-vous avec un compte utilisateur standard.

#### 2.3 Passage en Mode Root
Une fois connecté avec un utilisateur standard, pour passer en root :
```bash
su -l
```
Puis entrez le mot de passe root.

### 3. Autoriser la Connexion Root en SSH
Si vous souhaitez autoriser la connexion SSH directe avec root (non recommandé pour la sécurité) :

1. Éditez le fichier de configuration SSH :
```bash
nano /etc/ssh/sshd_config
```
2. Modifiez la ligne suivante :
   - **Avant** :
     ```
     PermitRootLogin prohibit-password
     ```
   - **Après** :
     ```
     PermitRootLogin yes
     ```
3. Redémarrez le service SSH pour appliquer les changements :
```bash
service ssh restart
```

### 4. Bonnes Pratiques de Sécurité
- Évitez d'autoriser la connexion directe en root via SSH.
- Utilisez toujours des mots de passe forts.
- Privilégiez l'authentification par clé plutôt que par mot de passe.
- Limitez les adresses IP autorisées à se connecter quand c'est possible.
- Changez le port SSH par défaut (22) si nécessaire.
- Maintenez votre système à jour régulièrement.

### 5. Troubleshooting
- Si la connexion est refusée, vérifiez :
  - Le service SSH est bien démarré
  - Le pare-feu autorise le port SSH
  - Les identifiants sont corrects
  - Les permissions dans le fichier `sshd_config`

---

## Gestion des Disques et Systèmes de Fichiers sous Linux

### Introduction à la Gestion des Disques
- Les disques sont nommés selon leur type de connexion :
  - IDE : `hdX`
    - `hda` : IDE0 (Master)
    - `hdb` : IDE1 (Slave)
  - SCSI, SATA, USB, etc. : `sdX`
    - `sda` : Premier disque
    - `sdb` : Second disque

### Systèmes de Fichiers
- `ext2` :
  - Taille maximale des fichiers : 2 To
  - Taille maximale d’une partition : 32 To
- `ext3` :
  - Successeur de ext2, mêmes limitations
- `ext4` :
  - Taille maximale des fichiers : 16 To
  - Taille maximale d’une partition : 1024 Po
- `xfs` :
  - Système de fichiers par défaut de Red Hat (RHEL, CentOS, Fedora)

### Commandes de Base
- `lsblk` : Liste les périphériques de stockage présents dans le système
- `mount` :
  - Permet de monter un système de fichiers
  - Sans argument : Affiche les points de montage actifs
- `umount` : Démontage d’un système de fichiers

---

## Configuration RAID sous Linux

### 1. Préparation du RAID
- Ajout de disques : Ajouter 3 disques virtuels (VHD) à la machine virtuelle
- Vérifiez leur présence avec :
```bash
lsblk
```
- Installation du logiciel RAID :
```bash
apt install mdadm
```
- Démarrage du service :
```bash
service mdmonitor start
systemctl enable mdmonitor
```

### 2. Création du RAID
- Configuration d’un RAID1 avec disque de spare :
```bash
mdadm --create --verbose /dev/md0 --level=raid1 --raid-devices=2 /dev/sdb /dev/sdc --spare-device=1 /dev/sdd
```
- Vérifiez la configuration avec :
```bash
lsblk
```

---

# Conclusion RAID

## Comparatif des Sauvegardes

| Type de sauvegarde | Temps de sauvegarde | Temps de restauration | Espace requis |
|--------------------|---------------------|-----------------------|---------------|
| Complète           | Long               | Court                | Élevé         |
| Incrémentielle     | Court              | Long                 | Faible        |
| Différentielle     | Moyen              | Moyen                | Modéré        |
