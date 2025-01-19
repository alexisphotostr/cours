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

### RAID (Redundant Array of Independent Disks)

RAID regroupe plusieurs disques pour améliorer la capacité, les performances, ou la sécurité des données.
Fonctionne via une gestion logicielle ou matérielle.

#### Objectifs du RAID
1. Amélioration des performances : Lecture et écriture des données sur plusieurs disques simultanément.
2. Sécurisation des données : Protection contre la perte de données via la redondance.
3. Augmentation de la capacité de stockage : Combinaison de l'espace disponible sur plusieurs disques.

#### Types de RAID et leurs caractéristiques

##### RAID 0 : Performances maximales
- **Principe** : Répartition des données sur plusieurs disques (stripping).
- **Avantages** :
  - Excellentes performances en lecture et écriture.
  - Utilisation maximale de l’espace disponible.
- **Inconvénients** :
  - Aucune redondance : la perte d’un disque entraîne la perte de toutes les données.
- **Utilisations** :
  - Jeux vidéo.
  - Montage vidéo.
  - Applications nécessitant une grande vitesse.

##### RAID 1 : Sécurité maximale
- **Principe** : Copie identique des données sur deux disques (mirroring).
- **Avantages** :
  - Haute fiabilité : les données restent disponibles en cas de panne d’un disque.
  - Performances améliorées en lecture (lecture simultanée sur deux disques).
- **Inconvénients** :
  - Espace disponible réduit à la taille d’un seul disque.
  - Coût élevé (doublement du nombre de disques nécessaires).
- **Utilisations** :
  - Applications critiques nécessitant une haute disponibilité des données.

##### RAID 5 : Équilibre entre performance et sécurité
- **Principe** : Répartition des données avec une parité répartie sur les disques.
  - La parité permet de reconstituer les données en cas de panne d’un disque.
- **Avantages** :
  - Bonne performance en lecture et écriture.
  - Tolérance à la panne d’un disque.
  - Utilisation optimisée de l’espace disque.
- **Inconvénients** :
  - Temps de reconstruction long en cas de panne.
  - Performances d’écriture légèrement réduites à cause des calculs de parité.
- **Configuration minimale** : 3 disques.

##### RAID 6 : Redondance accrue
- **Principe** : Similaire au RAID 5, mais avec une parité répartie sur deux disques.
- **Avantages** :
  - Tolérance à la panne de deux disques.
  - Fiabilité accrue par rapport au RAID 5.
- **Inconvénients** :
  - Performances d’écriture plus faibles.
  - Temps de reconstruction encore plus long.
  - Coût élevé en raison du besoin d’au moins 4 disques.
- **Utilisations** :
  - Environnements critiques nécessitant une redondance élevée.

##### RAID 10 (1+0) : Performance et sécurité combinées
- **Principe** : Association de RAID 1 et RAID 0 (stripping + mirroring).
- **Avantages** :
  - Haute performance et haute sécurité.
  - Tolérance à plusieurs pannes, selon les disques défectueux.
- **Inconvénients** :
  - Nécessite au moins 4 disques.
  - Coût élevé.
- **Utilisations** :
  - Bases de données.
  - Serveurs à haute disponibilité.

---

#### Fonctionnalités avancées du RAID
1. **Disque de secours (spare)** :
   - Disque de remplacement prêt à être utilisé en cas de panne.
   - Permet une reconstruction automatique de la grappe.
2. **Hot-swap (échange à chaud)** :
   - Possibilité de remplacer un disque défectueux sans arrêter le système.

#### Les sauvegardes : Sécurisation des données
1. **Sauvegarde complète** :
   - Copie de toutes les données.
   - **Avantages** : Facile à restaurer, fiable.
   - **Inconvénients** : Lente, consomme beaucoup d’espace.
2. **Sauvegarde incrémentielle** :
   - Copie des données modifiées depuis la dernière sauvegarde (complète ou incrémentielle).
   - **Avantages** : Rapide, faible consommation d’espace.
   - **Inconvénients** : Restauration longue (chaque sauvegarde doit être restaurée).
3. **Sauvegarde différentielle** :
   - Copie des données modifiées depuis la dernière sauvegarde complète.
   - **Avantages** : Plus fiable que l’incrémentielle, restauration plus rapide.
   - **Inconvénients** : Plus lente et consomme plus d’espace que l’incrémentielle.

---

# Conclusion RAID

## Comparatif des Sauvegardes

| Type de sauvegarde | Temps de sauvegarde | Temps de restauration | Espace requis |
|--------------------|---------------------|-----------------------|---------------|
| Complète           | Long               | Court                | Élevé         |
| Incrémentielle     | Court              | Long                 | Faible        |
| Différentielle     | Moyen              | Moyen                | Modéré        |







# Architecture Réseau

## Notions de Base

### Réseau
Un réseau est un ensemble d'ordinateurs, de périphériques et d'autres équipements interconnectés afin de partager des ressources (comme des fichiers, des imprimantes ou une connexion internet) et de communiquer entre eux. Les réseaux peuvent être constitués de connexions filaires (câbles) ou sans fil (Wi-Fi).

### Réseau Local / Réseau Public
- **Réseau Local (LAN)** : Un réseau local (Local Area Network, LAN) est un réseau qui relie des périphériques sur une zone géographique restreinte, comme une maison, un bureau, ou un bâtiment. Il est privé et conçu pour permettre aux utilisateurs de partager des ressources facilement au sein d’un espace limité.
- **Réseau Public** : Un réseau public est accessible à tous, souvent à travers Internet. Il n'est pas privé, et les données transmises sur ce type de réseau sont exposées à plus de risques d'interception. Exemple : un réseau Wi-Fi dans un café ou un aéroport.

### Masque (Masque de Sous-Réseau)
Le masque de sous-réseau (subnet mask) est un nombre de 32 bits utilisé en complément de l’adresse IP pour diviser un réseau en sous-réseaux plus petits. Le masque de sous-réseau aide à déterminer la portion de l’adresse IP correspondant au réseau et celle qui désigne les hôtes individuels dans le réseau. Il est souvent exprimé sous forme de nombres comme `255.255.255.0`.

### Adresse IP
Une adresse IP (Internet Protocol address) est un identifiant unique assigné à chaque appareil connecté à un réseau. Elle permet de reconnaître et de localiser les appareils au sein du réseau.
- **IPv4** : Exemple : `192.168.1.1`
- **IPv6** : Exemple : `2001:0db8:85a3:0000:0000:8a2e:0370:7334`

### Submask (ex. 255)
Un submask ou masque de sous-réseau (comme `255.255.255.0`) sert à diviser un réseau en sous-réseaux plus petits pour mieux gérer les adresses IP et le trafic. Par exemple, un masque de sous-réseau avec des valeurs `255` signifie que cette portion de l'adresse IP est réservée pour le réseau, tandis que les zéros indiquent la portion allouée aux appareils ou hôtes.

### Switch (Commutateur)
Un switch (ou commutateur) est un appareil réseau qui connecte plusieurs périphériques sur un même réseau local. Il fonctionne au niveau de la couche 2 (couche de liaison de données) du modèle OSI, et il permet la transmission directe de données d'un appareil à un autre au sein du réseau, en utilisant les adresses MAC pour diriger les paquets de données vers leur destination correcte.

### Routeur
Un routeur est un appareil réseau qui connecte différents réseaux entre eux, par exemple, un réseau local à Internet. Il travaille au niveau de la couche 3 (couche réseau) du modèle OSI et utilise des adresses IP pour transmettre les données d'un réseau à un autre. Le routeur décide du chemin que les données doivent prendre pour atteindre leur destination.

### Hub
Un hub est un dispositif réseau qui relie plusieurs appareils dans un même réseau local (LAN). Contrairement au switch, il ne connaît pas les adresses MAC des appareils et diffuse donc les données reçues à tous les appareils connectés, ce qui entraîne une diffusion de données moins efficace et augmente le trafic réseau. Le hub fonctionne également au niveau de la couche 1 (couche physique) du modèle OSI.

---

## Cisco Packet Tracer

### Commandes CLI de Base
1. **Mode Admin** :
   ```bash
   Router> enable
   ```
2. **Accès au Menu de Configuration** :
   ```bash
   Router# conf t
   ```
3. **Configuration de l'Interface GigabitEthernet 0/0** :
   ```bash
   Router(config)# interface gi 0/0
   ```
4. **Activation de l'Interface** :
   Activez l'interface en utilisant la commande "no shut", ce qui change l'état de l'interface à "up" (actif) :
   ```bash
   Router(config-if)# no shut
   ```
5. **Configuration de l'Adresse IP** :
   Attribuez une adresse IP à l'interface sélectionnée. Exemple :
   ```bash
   Router(config-if)# ip add 192.168.0.254 255.255.255.0
   ```

---

## Topologie Réseau
- **Routeur** : Il est au centre et gère les communications.
- **Commutateurs (Switches)** : Connectent plusieurs appareils entre eux.
- **PCs et Laptops** : Sont connectés aux commutateurs.

---

## Modèle OSI

Le modèle OSI (Open Systems Interconnection) est un modèle théorique qui décrit comment les systèmes de communication numérique fonctionnent pour permettre l'interopérabilité entre différents systèmes. Ce modèle est divisé en 7 couches, chacune ayant un rôle spécifique :

### Les 7 Couches du Modèle OSI
1. **Couche Physique (Physical Layer)** :
   - Transmission des données brutes sous forme de signaux électriques, optiques ou radio.
   - Exemples : Câbles, connecteurs, signaux, normes électriques (Ethernet, USB).

2. **Couche Liaison de Données (Data Link Layer)** :
   - Assure la transmission sans erreur des données entre deux nœuds connectés directement.
   - Exemples : Ethernet, Wi-Fi (normes IEEE 802.3 et 802.11), commutation (switches).

3. **Couche Réseau (Network Layer)** :
   - Gestion de l'adressage, du routage et de la livraison des paquets entre des réseaux différents.
   - Exemples : IPv4, IPv6, routage (routers).

4. **Couche Transport (Transport Layer)** :
   - Assure le transfert fiable des données entre les hôtes, avec des mécanismes de contrôle de flux et de correction des erreurs.
   - Exemples : TCP, UDP.

5. **Couche Session (Session Layer)** :
   - Gère les sessions de communication entre applications, y compris l’ouverture, la maintenance et la fermeture des sessions.
   - Exemples : RPC (Remote Procedure Call), PPTP (Point-to-Point Tunneling Protocol).

6. **Couche Présentation (Presentation Layer)** :
   - Traduit les données entre le format utilisé par l'application et celui utilisé pour la transmission. Gère aussi le chiffrement et la compression.
   - Exemples : SSL/TLS, JPEG, MPEG.

7. **Couche Application (Application Layer)** :
   - Interagit directement avec les utilisateurs pour fournir des services réseau, comme le courrier électronique ou le partage de fichiers.
   - Exemples : HTTP, FTP, SMTP, DNS.

---

## Adressage IPv4

### Introduction à l’Adressage IPv4
Chaque hôte a besoin d’une adresse IPv4 unique pour communiquer sur un réseau local (LAN) ou sur Internet. Une adresse IPv4 est associée à une interface réseau (exemple : carte réseau). Les paquets envoyés disposent toujours d’une adresse source et d’une adresse de destination pour identifier l’origine et la destination des données.

### Format des Adresses IPv4
- **Longueur** : 32 bits, divisés en 4 octets.
- **Représentation** :
  - Exemple binaire : `11010001.10100101.11001000.00000001`
  - Exemple décimal : `209.165.200.1`

### Structure d’une Adresse IPv4
- **Réseau** : Identifie le réseau auquel l’hôte appartient.
- **Hôte** : Identifie un hôte unique dans ce réseau.
- **Masque de Sous-Réseau** :
  - Indique la séparation entre les parties réseau et hôte.
  - Exemple : `192.168.5.11` avec masque `255.255.255.0`
    - Réseau : `192.168.5`
    - Hôte : `11`

---

## Classes d’Adresses IPv4

| Classe | Plage IP                  | Masque CIDR | Utilisation                |
|--------|---------------------------|-------------|---------------------------|
| A      | `1.0.0.0 - 126.255.255.255` | `/8`        | Grandes organisations      |
| B      | `128.0.0.0 - 191.255.255.255` | `/16`       | Réseaux moyens            |
| C      | `192.0.0.0 - 223.255.255.255` | `/24`       | Petits réseaux            |
| D      | `224.0.0.0 - 239.255.255.255` | `N/A`       | Multicast (diffusion à groupe) |
| E      | `240.0.0.0 - 247.255.255.255` | `N/A`       | Expérimentation           |

---

## Techniques de Diffusion
- **Monodiffusion (Unicast)** :
  - Communication entre un hôte et un autre (ex. client-serveur).
- **Diffusion (Broadcast)** :
  - Envoi à tous les hôtes d’un réseau. Exemple : Protocoles comme DHCP.
- **Multidiffusion (Multicast)** :
  - Transmission à un groupe spécifique d’hôtes inscrits. Plage réservée : `224.0.0.0 - 239.255.255.255`.

---

## Attribution des Adresses IPv4

### Méthodes d’Attribution
1. **Manuelle (statique)** :
   - Configuration par l’administrateur réseau.
   - Adaptée aux petits réseaux ou appareils fixes (ex. serveurs).
2. **Automatique (dynamique)** :
   - Utilisation du protocole DHCP pour attribuer automatiquement :
     - Adresse IPv4.
     - Masque de sous-réseau.
     - Passerelle par défaut.
   - **Avantage** : Réduction des erreurs et facilité de gestion pour les grands réseaux.

---

## Conclusion
Les réseaux permettent une communication efficace et flexible. Les notions comme les masques de sous-réseaux, les classes d’adresses, et les techniques de diffusion sont essentielles pour comprendre et gérer les réseaux modernes. Le modèle OSI et l’adressage IPv4 fournissent une base théorique et pratique pour une meilleure interopérabilité des systèmes.
