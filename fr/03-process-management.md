# **Chapitre 3: Gestion des processus**
# **Avant-propos**

Nous vous recommandons de ne pas utiliser d'IA pour faire les exercices car vous êtes en phase d'apprentissage.

# **Introduction**

Dans un système Linux, la gestion des processus est un aspect fondamental de l'administration du système et du fonctionnement des applications. Un processus est simplement un programme en cours d'exécution. Linux, comme tous les systèmes d'exploitation modernes, utilise des processus pour exécuter des tâches et organiser le travail.

## Prérequis 

* Avoir une machine virtuelle ou un PC ou un environnement sous Linux (Ubuntu idéalement)
* Être résilient 😜

**Info:** Si vous n'avez pas d'environnement Linux à votre disposition, vous pouvez vous inscrire sur <https://killercoda.com> et vous rendre ici <https://killercoda.com/playgrounds/scenario/ubuntu> pour avoir accès à une machine virtuelle sous Ubuntu 24.04 (sans interface graphique bien sûr !!) pendant 1 heure renouvelable gratuitement.

Vous aurez donc cette vue:  

![](https://raw.githubusercontent.com/N0vachr0n0/Hands-on-Linux-course/refs/heads/main/fr/pictures/killerkoda_vm.png)


<br>
<br>

---

# **Généralités**

Un processus est une instance d'un programme en exécution. Il peut être aussi simple qu'une commande lancée dans le terminal ou aussi complexe qu'une application en arrière-plan. Chaque processus possède son propre espace mémoire, son propre identifiant unique (PID) et des informations relatives à son état d'exécution. Si le processus 2 a été lancé par le processus 1, on l’appelle un **processus fils**. Le processus qui l’a lancé est appelé **processus parent**.

<br>
Il existe différents types de processus dans Linux :

* **Processus interactifs** : Ce sont les processus lancés par un utilisateur directement via un terminal ou une interface graphique. Par exemple, lancer un éditeur de texte ou un navigateur web.

* **Processus en arrière-plan (démon)** : Ce sont des processus qui fonctionnent sans interaction directe avec l'utilisateur, souvent utilisés pour les services du système, comme un serveur web (Apache, par exemple).

* **Processus zombies** : Ce sont des processus qui ont terminé leur exécution mais dont les informations restent dans la table des processus parce que leur parent n’a pas encore récupéré leur état de sortie.


## TLDR (Résumé rapide)

* Un processus est une instance d'un programme en exécution.
* Chaque processus dispose :
    * d’un PID : Process IDentifier, identifiant unique de processus ;
    * d’un PPID : Parent Process IDentifier, identifiant unique de processus parent.
* Types de processus: interactif, en arrière-plan et zombie


## Tapons un peu le clavier

**A tester 👨🏾‍💻👩🏾‍💻:**
- Ouvrir son terminal
- Exécuter: 
  ```bash
  ps
  ```

<br>

Pour info, **ps** signifie **p**rocess **s**tatus. Elle nous permet d'afficher les processus en cours d'exécution.
Vous aurez un retour similaire à: 

```
PID    TTY          TIME CMD
380123 pts/3    00:00:00 bash
427931 pts/3    00:00:00 ps
```

Ci-dessous un tableau explicatif.

| Colonne | Signification | Exemple |
|---------|---------------|---------|
| **PID** | Identifiant unique attribué par Linux à chaque processus. | `bash`: **380123**<br>`ps`: **427931** |
| **TTY** | Terminal auquel le processus est attaché. Les `pts/*` sont des pseudo-terminaux, souvent ouverts lors d'une connexion SSH ou d'un terminal graphique. | `pts/3` |
| **TIME** | Temps CPU utilisé par le processus depuis son lancement. | `00:00:00` (temps minimal pour des processus rapides ou inactifs) |
| **CMD** | Commande ou programme lancé à l'origine du processus. | `bash` (shell interactif)<br>`ps` (commande d'affichage des processus) |


<br>

De ce résultat, on pourrait se poser deux (2) questions:
* Pourquoi **ps** s'affiche-t-il lui-même ? 
* Pourquoi est-ce qu'on ne voit pas tous les processus du système après avoir tapé **ps** ?

1. La commande ps est elle-même un processus Linux. Lorsqu'elle est exécutée, elle se crée temporairement comme un processus, puis elle analyse les processus en cours d’exécution à ce moment précis. Ainsi, elle apparaît naturellement dans ses propres résultats.

2. Par défaut, cette commande (**ps**) sans argument particulier affiche uniquement les processus associés à son terminal actuel. Cela explique pourquoi on ne voit que le shell (bash) qu'on utilise actuellement, ainsi que la commande ps elle-même.

<br>

**A tester 👨🏾‍💻👩🏾‍💻:**
- Ouvrir son terminal
- Exécuter: 
  ```bash
  ps
  echo 'foo' > myfile.txt
  tail -f myfile.txt & #Pour faire tourner la tâche en arrière plan
  jobs #Pour visualiser les tâches en arrière plan
  ps
  ```

<br>
<br>

---

# **Gestion des processus**

## Commandes permettant d'afficher les processus en cours d'exécution 

- **En temps réel :** `top`  
- **À un instant donné :** `ps`  
  - `-e` : tous les processus  
  - `-f` : affichage détaillé  

**Exemples pratiques :**  

- Lister tous les processus avec détails : `ps -ef`  
- Lister les processus sous forme d'arbre : `ps -faux`  
- Vérifier si le processus apache est lancé : `ps -ef | grep httpd`  (apache2=Debian, httpd=RedHat) 
- Arrêter le processus `123` proprement (signal SIGTERM) : `kill 123`  
- Forcer l'arrêt immédiat du processus `123` (signal SIGKILL) : `kill -9 123`

<br>

**Rappel:** La commande **man** et l'option **--help** (deux tirets svp🫠) sont votre meilleur ami.

<br>

## Autres commandes

### Les commandes pgrep et pkill

La commande **pgrep** cherche, parmi les processus en cours d’exécution, un nom de processus et affiche sur la sortie standard les PID correspondants aux critères de sélection.

La commande **pkill** enverra le signal indiqué (par défaut **SIGTERM**) à chaque processus correspondant aux critères spécifiés.

**Syntaxe :**

- `pgrep processus`  
- `pkill [-signal] processus`

**Exemples :**

- Récupérer le numéro de processus du service **sshd** pour l’utilisateur root :  
  ```bash
  pgrep -u root sshd
  ```

- Tuer tous les processus tomcat :
    ```bash
    pkill tomcat
    ```
    
<br>
<br>

---

# **Entraînement ⚔️**

## Exercice 1

* Lien du script du challenge: <https://raw.githubusercontent.com/N0vachr0n0/NoFD/refs/heads/main/PRM_EXO_1.sh>

Ci-dessous un exemple d'exécution:

```bash
# On télécharge le script du challenge 1
curl -LO https://raw.githubusercontent.com/N0vachr0n0/NoFD/refs/heads/main/PRM_EXO_1.sh

# On le rend exécutable
chmod +x PRM_EXO_1.sh

# On l'exécute pour démarrer le challenge
./PRM_EXO_1.sh
```

## Exercice 2

C'est par ici: <https://sadservers.com/scenario/saint-john>

---
---

## Feedback


> Faites-nous part de votre avis sur ce chapitre.

> 👉🏾 [Cliquez ici](https://forms.gle/dNjUFEXf6saN8RTEA)

---
---

## Navigation 

* [Chapitre précédent](./02-basic-commands.md)

* [Chapitre suivant](./04-users-groups.md)

* [Allez au sommaire](./index.md)
