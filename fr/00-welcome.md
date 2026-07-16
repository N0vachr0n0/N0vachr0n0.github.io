# **Welcome**

Bienvenue !<br>
Welcome !<br>
(いらっしゃいませ) Irasshaimase !<br>
Willkommen !<br>

Salut l'ami(e),
Tu veux donc apprendre Linux, c'est bien ! Tu es au bon endroit selon moi 😉.
Ce cours regorge d'exercices afin de te permettre d'avoir une immersion assez complète dans le monde de Linux pour t'aventurer dans l'administration système ou DevOps ou dans la Cybersec.

Si tu n'es pas un **débutant**, je t'invite à faire le challenge ci-dessous sans IA pour t'évaluer.



Sur ce, bonne aventure, Ciao !

![](https://raw.githubusercontent.com/N0vachr0n0/Hands-on-Linux-course/refs/heads/main/fr/pictures/yugioh.jpeg)

**A toi de jouer !!!**


<br>
<br>

---

# **Challenge**

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


<br>
<br>

---
---

## Navigation 

* [Débuter l'aventure](./01-inside-linux-world.md)

* [Allez au sommaire](./index.md)