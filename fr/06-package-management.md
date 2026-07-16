# **Chapitre 6: Gestion des paquets**
# **Avant-propos**

Nous vous recommandons de ne pas utiliser d'IA pour faire les exercices car vous êtes en phase d'apprentissage.

# **Introduction**

Vous aurez souvent besoin d’installer des logiciels qui ne sont pas fournis avec votre distribution ou supprimer les logiciels indésirables afin qu’ils ne prennent pas de l’espace disque dur. Dans cette partie, nous aborderons la gestion des paquets de logiciels ou applications sur Linux. 

## Prérequis 

Toujours la même histoire. 😉

<br>
<br>

---

# **Les gestionnaires de paquets (packages) sous linux**

Tout d'abord un paquet est une archive qui contient un ensemble de fichiers et de répertoires à déployer sur le système d'exploitation pour permettre le bon fonctionnement d'un logiciel à installer. Un paquet ou package peut nécessiter la présence d'autres packages pour fonctionner. On parle alors de **dépendances**. 

## Les dépôts (repo or repository) linux

Un dépôt Linux  (en anglais repository ) est un endroit (souvent sur Internet) où sont stockés des packages logiciels , prêts à être téléchargés et installés. Nous pouvons aussi voir les dépôts comme des supermarchés ou des réserves bien organisées où nous trouvons ces paquets.

Sur linux, il existe plusieurs types de dépôts:

- **Les Dépôts officiels:** Ceux fournis par la distribution linux elle-même. 
- **Les Dépôts tiers (third-party repositories):** Ajoutés manuellement par l’utilisateur lors de l'installation d'un logiciel.

Les dépôts linux sont définis dans un ou plusieurs fichiers sur votre OS. Sous Debian/Ubuntu, ils sont listés dans  **/etc/apt/sources.list** et dans les fichiers du dossier **/etc/apt/sources.list.d/**.

**Exemples de lignes dans sources.list** :

```
Types: deb
URIs: http://archive.ubuntu.com/ubuntu
Suites: noble noble-updates noble-backports
Components: main universe restricted multiverse
Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg
```

```
deb http://archive.ubuntu.com/ubuntu/ focal main restricted universe multiverse
```

<br>

## Les outils de gestion de paquets sous linux

L'installation d'un package passe par un gestionnaire / outil. Ci-dessous un tableau présentant quelques gestionnaires.

| **Gestionnaire** | **Distributions concernées**               | **Fichier source.list ou équivalent**           | **Exemples de commandes**                          |
|------------------|---------------------------------------------|--------------------------------------------------|----------------------------------------------------|
| **APT / APT-GET** | Debian, Ubuntu, Linux Mint, Kali, etc.     | `/etc/apt/sources.list` et `/etc/apt/sources.list.d/` | `apt install`, `apt update`                      |
| **DPKG**          | Debian-based                                | Non applicable (paquet local)                    | `dpkg -i package.deb`, `dpkg -r package`         |
| **DNF**           | Fedora, RHEL 8+, CentOS Stream              | `/etc/yum.repos.d/`                              | `dnf install`, `dnf upgrade`                     |
| **YUM**           | CentOS 7, RHEL 7, Fedora <21, Rocky Linux 7                | `/etc/yum.repos.d/`                              | `yum install`, `yum update`                      |
| **ZYPPER**        | openSUSE, SUSE Linux Enterprise             | `/etc/zypp/repos.d/`                             | `zypper install`, `zypper update`                |
| **PACMAN**        | Arch Linux, Manjaro, EndeavourOS            | `/etc/pacman.conf` + `/etc/pacman.d/mirrorlist` | `pacman -S`, `pacman -Syu`                       |
| **SNAP**          | Toutes les distros supportant Snap          | Géré via `snap install` ou fichiers internes     | `snap install`, `snap refresh`                   |
| **FLATPAK**       | Toutes les distros supportant Flatpak       | `/var/lib/flatpak/` ou `~/.local/share/flatpak/` | `flatpak install flathub`, `flatpak update`      |
| **APPIMAGE**      | Toutes les distros                         | Aucun fichier de dépôt nécessaire                | Télécharger → rendre exécutable → lancer         |

<br>

**Info**:

⚠️ **dpkg** ne gère pas automatiquement les dépendances. En cas d’erreur, utilisez **apt --fix-broken install**.

<br>

## Exploration du gestionnaire APT (Advanced Package Tool)

APT est l’un des gestionnaires de paquets Linux les plus populaires, car il est fourni avec Ubuntu et d’autres distributions basées sur Debian. Voici quelques exemples en action.

| **Action**                          | **Commande APT**                                   | **Description** |
|------------------------------------|-----------------------------------------------------|------------------|
| Mettre à jour la liste des paquets | `sudo apt update`                                   | Télécharge la liste mise à jour des paquets depuis les dépôts. |
| Installer un paquet                | `sudo apt install nom_du_paquet`                  | Installe le paquet spécifié et ses dépendances. |
| Supprimer un paquet                | `sudo apt remove nom_du_paquet`                   | Désinstalle le paquet mais laisse les fichiers de configuration. |
| Supprimer un paquet + config       | `sudo apt purge nom_du_paquet`                    | Supprime le paquet ET ses fichiers de configuration. |
| Mettre à jour les paquets          | `sudo apt upgrade`                                  | Met à jour tous les paquets installés vers leur dernière version. |
| Mise à niveau du système           | `sudo apt full-upgrade`                           | Comme `upgrade`, mais supprime ou installe des paquets si nécessaire. |
| Rechercher un paquet               | `apt search nom_paquet`                          | Cherche un paquet contenant le mot-clé donné. |
| Obtenir des infos sur un paquet    | `apt show nom_du_paquet`                          | Affiche les détails d’un paquet (version, taille, dépendances, etc.). |
| Nettoyer les anciens téléchargements | `sudo apt clean`                                 | Supprime les anciens fichiers `.deb` téléchargés. |
| Supprimer les paquets inutiles     | `sudo apt autoremove`                             | Supprime les paquets installés automatiquement et qui ne sont plus nécessaires. |
| Lister les paquets disponibles     | `apt list`                                    | Affiche tous les paquets disponibles (installés ou non). |
| Lister les paquets installés       | `apt list --installed`                            | Montre uniquement les paquets actuellement installés. |

Désolé pour ceux qui ne sont pas sur une distribution basée sur debian 😝.

**INFO:** L'option **--help** (deux tirets svp🫠) ou **-h** et la commande **man** sont votre meilleur ami pour avoir des infos sur l'utilisation d'une commande.

## Bon à savoir

L'installation d'un logiciel sur linux se fait principalement via un gestionnaire de paquet mais il est aussi possible d'installer des logiciels manuellement via des dépôts GitHub ou d’autres sources et méthodes.

<br>
<br>

---

# **Entraînement ⚔️**

## Exercice de recherche 

1. Chercher pourquoi il y a des dépôts officiels et non officiels
2. Chercher les différences entre un dépôts free et non-free
3. Chercher les différences entre les gestionnaires de paquets flatpak et snap
4. Chercher les différences entre les gestionnaires de paquets snap et apt

## Exercice 1

1. Installer l'application **figlet** et afficher le texte "it's crazy" avec figlet
2. Installer l'application **cowsay** et afficher le texte "subarashi" avec cowsay
3. Installer l'application **atop** et visualiser vos métriques système.

**Test:** Tapez `figlet -v`, `cowsay -h` et `atop -V` pour vérifier l’installation.

## Exercice 2 (Deep dive)

Il est temps de passer aux choses sérieuses !!! <br>
Ce challenge consiste à rechercher le fichier flag.zip et de le décompresser afin d'obtenir le flag (chaîne de caractères spéciale).
Avant de démarrer le challenge, il faudra mettre en place l'environnement comme suit:

**Info:** Prêtez attention à votre prompt.

```bash
# Installation du prérequis: l'app docker
sudo apt update
sudo apt install -y docker.io
sudo systemctl enable docker --now
sudo usermod -aG docker $USER # Afin d'utiliser la commande docker sans sudo dans vos prochaines sessions

# Initialisation de l'environnement
sudo docker ps # Affiche les conteneurs en cours d'exécution
sudo docker run -dit --name ctf-sysadmin fs0ci3ty/adminsys_basic-ctf # Téléchargement et mise en place du conteneur du challenge
sudo docker ps
sudo docker exec -it ctf-sysadmin bash # Permet de rentrer dans le conteneur Docker et de lancer le terminal bash pour pouvoir y exécuter des commandes.
```

A ce stade vous devez être dans le conteneur docker et non dans le terminal rattaché à votre OS comme illustré ci-dessous.

![](https://raw.githubusercontent.com/N0vachr0n0/Hands-on-Linux-course/refs/heads/main/fr/pictures/prompt_docker_chall.png)

<br>

L'environnement du challenge étant prêt, vous pouvez débuter le challenge mais gardez à l'esprit les points suivant:
* La décompression de **flag.zip** doit se faire dans le container docker.
* Les problèmes réseaux, d'installation de paquets et autres rencontrés dans le container docker sont tout à fait normaux 😈.
* Evitez d'utiliser l'IA pour avoir la solution directement. Cela est contre productif dans votre apprentissage. Au mieux, vous pouvez lui donner le rôle de **tuteur adminsys linux** pour qu'il vous guide sans vous donnez la solution directement.

Bonne chance !

<br>

### INFO

* **Cours rapide:** Docker est un outil qui permet de lancer des applications dans des environnements isolés appelés "conteneurs" . C’est **comme** une **mini-machine** virtuelle , mais beaucoup plus légère et rapide à démarrer. Dans notre cas, Docker nous permet de mettre en place une version super légère d'une distribution linux prête à être utilisée dans un conteneur.
* Les indices du challenge se trouvent ici: <https://github.com/N0vachr0n0/NoFD/blob/main/Hint_PKG_EXO_2.md>
* La remise en place de l'environnement se fait comme suit:

```bash
exit # Pour quitter le conteneur et reprendre votre prompt initial
sudo docker rm -f ctf-sysadmin # Suppression du conteneur
sudo docker run -dit --name ctf-sysadmin fs0ci3ty/adminsys_basic-ctf 
sudo docker exec -it ctf-sysadmin bash
```
---

<br>

**Vous en voulez plus sur Docker ? Rdv ici => <https://openclassrooms.com/fr/courses/8431896-optimisez-votre-deploiement-en-creant-des-conteneurs-avec-docker>**

**Je vous invite aussi à faire une recherche sur "docker rootless" et "podman vs docker"**

**Dernier Point ⚠️ :** <br> 
Si vous utilisez Docker sur votre VPS ou serveur privé accessible sur internet, nous vous invitons à faire attention aux ports mis en écoute par Docker car ils **créent une règle qui contourne les règles du pare-feux**. Il s'agit d'un comportement normal dans Docker. Nous vous invitons donc à lire ces articles:
- <https://docs.docker.com/engine/network/packet-filtering-firewalls/>
- <https://forums.docker.com/t/need-more-clarifications-for-firewall-prerequisites/142657>
- <https://medium.com/@akhshyganesh/docker-vs-your-firewall-the-silent-port-sneak-6e4bad366a5e>
- <https://rithwik.hashnode.dev/how-docker-can-be-sneaky-around-your-ufw-firewall>

---
---

## Feedback

> Faites-nous part de votre avis sur ce chapitre.

> 👉🏾 [Cliquez ici](https://forms.gle/QxgTWzCfPTpg9Mks7)


---
---

## Navigation 

* [Chapitre précédent](./05-file-permissions.md)

* [Chapitre suivant](./07-service-management.md)

* [Allez au sommaire](./index.md)
