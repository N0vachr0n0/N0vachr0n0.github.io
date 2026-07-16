# **Chapitre 5: Permissions des fichiers**
# **Avant-propos**

Nous vous recommandons de ne pas utiliser d'IA pour faire les exercices car vous êtes en phase d'apprentissage.

# **Introduction**

Dans cette partie nous aborderons la gestion des fichiers sous Linux. Plus précisément, nous parlerons des droits et de la gestion des accès sur les fichiers.

## Prérequis 

Toujours la même histoire. 😉

<br>
<br>

---

# **La Gestion des fichiers**

## Les permissions sur les fichiers
### Généralités

Sous Linux, les permissions sur les fichiers constituent une barrière essentielle contre les accès non autorisés.

* Les permissions d'un fichier limitent 3 actions :
    * lecture du fichier (caractérisé par la lettre **r**)
    * écriture ou modification du fichier (**w**)
    * exécution (binaire, script shell, etc...) (**x**)

* Chacune des 3 actions peut être assignée à 3 types d'utilisateur :
    * l'utilisateur propriétaire du fichier (caractérisé par la lettre **u**)
    * tout utilisateur du groupe d'appartenance du fichier (**g**)
    * les autres (ni propriétaire du fichier, ni groupe du fichier) (**o**)

Ces permissions peuvent être consultées grâce à la commande `ls -l` qui les affiche sous la forme symbolique suivante :

```
-rw-rw-r-- 1 widal widal 244 avril 23 01:48 pubspec.yaml
```

Décortiquons chaque partie de cette ligne :

1. **`-rw-rw-r--`** : Il s'agit des permissions du fichier.
   - Le premier caractère (`-`) indique qu'il s'agit d'un **fichier ordinaire** (plutôt qu'un répertoire ou un lien symbolique, par exemple). Ci-dessous tous les types de fichiers avec leur caractère.

            `-`: fichier ordinaire
            `d`: Répertoire
            `c`: Périphérique caractère
            `b`: Périphérique bloc
            `l`: Lien symbolique
            `p`: Tube nommé (IPC)
            `s`: Socket locale (IPC)


   - Les trois premiers caractères (`rw-`) indiquent les **permissions** pour le **propriétaire** du fichier (ici, "widal"). Le propriétaire peut lire et écrire le fichier, mais ne peut pas l'exécuter.
   - Les trois caractères suivants (`rw-`) indiquent les **permissions** pour le **groupe** du fichier. Le groupe "widal" peut aussi lire et écrire le fichier, mais ne peut pas l'exécuter.
   - Les trois derniers caractères (`r--`) indiquent les **permissions** pour les **autres utilisateurs** (ceux qui ne sont ni le propriétaire ni membres du groupe). Ces utilisateurs peuvent seulement lire le fichier, mais ne peuvent pas l'écrire ni l'exécuter.

2. **`1`** : Cela indique le nombre de liens physiques (c'est-à-dire, le nombre de noms de fichiers pointant vers ce fichier) dans le système. Généralement, pour un fichier simple, cela vaut 1 (un).

3. **`widal`** : Le nom du **propriétaire** du fichier, ici, c'est "widal".

4. **`widal`** : Le nom du **groupe** associé au fichier. Ici aussi, le groupe est "widal".

5. **`244`** : La taille du fichier en octets. Ce fichier pèse 244 octets.

6. **`avril 23 01:48`** : La **date et l'heure** de la dernière modification du fichier. Ici, le fichier a été modifié le 23 avril à 01:48.

7. **`pubspec.yaml`** : C'est le **nom du fichier**.

#### Résumé :
Le fichier `pubspec.yaml` appartient à l'utilisateur "widal" et au groupe "widal", il est lisible et modifiable par ces deux entités, mais seulement lisible par les autres. Il fait 244 octets et a été modifié pour la dernière fois le 23 avril à 01:48.

### Changement de permissions des fichiers

Sous Linux, les permissions des fichiers et des répertoires définissent qui peut lire, écrire ou exécuter un fichier. Vous pouvez changer ces permissions avec la commande `chmod`. Le changement de permissions peut se faire de deux façons principales : en utilisant des **nombres** ou des **symboles**.

#### 1. **Utilisation des Nombres pour `chmod`**

Chaque type de permission (lecture, écriture, exécution) est représenté par un chiffre :
- **Lecture** (`r`) = 4
- **Écriture** (`w`) = 2
- **Exécution** (`x`) = 1

Les permissions sont attribuées par un nombre à trois chiffres qui représentent respectivement les permissions du **propriétaire**, du **groupe** et des **autres utilisateurs**.

- Le premier chiffre est pour le propriétaire.
- Le deuxième chiffre est pour le groupe.
- Le troisième chiffre est pour les autres utilisateurs.

Exemples :
- `chmod 755 fichier.txt` : Le propriétaire a tous les droits (`7` = `rwx`; décortiquons: `7` = `r(4) + w(2) + x(1)`), le groupe a des droits de lecture et d'exécution (`5` = `r-x`), et les autres ont aussi des droits de lecture et d'exécution (`5` = `r-x`).
- `chmod 644 fichier.txt` : Le propriétaire a des droits de lecture et d'écriture (`6` = `rw-`), et les autres ont des droits de lecture seulement (`4` = `r--`).

#### 2. **Utilisation des Symboles pour `chmod`**

On peut aussi spécifier les permissions en utilisant des symboles :
- `+` : Ajouter une permission.
- `-` : Retirer une permission.
- `=` : Définir explicitement les permissions.

Exemples :
- `chmod u+x fichier.txt` : Ajoute la permission d'exécution pour le **propriétaire** (`u` = user).
- `chmod g-w fichier.txt` : Retire la permission d'écriture pour le **groupe** (`g` = group).
- `chmod o=r fichier.txt` : Définit les permissions des autres utilisateurs à "lecture seulement" (`o` = others).

#### 3. **Utilisation de `umask`** (ℹ️ Bon à savoir !)

La commande `umask` détermine les permissions par défaut des fichiers et répertoires lorsque vous les créez. Le `umask` est une sorte de "masque" qui **enlève** certaines permissions des fichiers créés. Par défaut, lorsqu'un fichier est créé, il a des permissions larges (comme `777` pour un répertoire ou `666` pour un fichier), et le `umask` va ajuster ces permissions selon les valeurs spécifiées.

La commande `umask` peut être utilisée pour afficher ou changer le masque des permissions par défaut.

- Par exemple, si `umask` est réglé sur `022`, cela signifie que les nouveaux fichiers auront des permissions par défaut de `644` (c'est-à-dire que les autres utilisateurs ne peuvent pas écrire dans le fichier).
- Si `umask` est réglé sur `0777`, tous les fichiers auront des permissions très restrictives.

Exemple :
- `umask 0022` : Pour un fichier, les permissions seront `644` (propriétaire : `rw-`, groupe et autres : `r--`).
- `umask 0777` : Pour un fichier, les permissions seront `000`, ce qui empêche tout accès au fichier.

#### 4. Bonus: Gestion des permissions lors d'une copie de fichier ( **`cp -p`** )

La commande `cp` sert à copier des fichiers ou des répertoires. L'option `-p` de `cp` permet de **préserver** les attributs du fichier source lors de la copie, tels que :
- Les permissions.
- Le propriétaire.
- La date de modification.

Par exemple :
```bash
cp -p fichier_source.txt fichier_copie.txt
```
Cela crée une copie du fichier tout en maintenant les mêmes permissions et autres métadonnées que le fichier d'origine.


### Tableau des permissions et valeurs de `chmod`

| Permission | Valeur numérique | Description       | Symboles associés |
|------------|------------------|-------------------|-------------------|
| **Lecture** (`r`)  | 4                | Lire le fichier   | `r`               |
| **Écriture** (`w`)  | 2                | Modifier le fichier| `w`               |
| **Exécution** (`x`) | 1                | Exécuter le fichier| `x`               |
| **Aucun**           | 0                | Pas de permission |                   |

<br>
<br>

| Commande | Propriétaire (u) | Groupe (g) | Autres (o) | Cas d'usage typique |
|---|---|---|---|---|
| `chmod 777 fichier.txt` | rwx | rwx | rwx | Tous droits pour tout le monde (à éviter en général) |
| `chmod 750 fichier.txt` | rwx | r-x | --- | Propriétaire complet, groupe en lecture seule, autres aucun accès |
| `chmod 640 fichier.txt` | rw- | r-- | --- | Fichier lisible/modifiable par le propriétaire, lisible par le groupe uniquement |
| `chmod 755 fichier.txt` | rwx | r-x | r-x | Cas classique pour un script ou exécutable partagé |
| `chmod 644 fichier.txt` | rw- | r-- | r-- | Cas classique pour un fichier texte/config non exécutable |
| `chmod 000 fichier.txt` | --- | --- | --- | Aucun accès pour personne (sauf root) |


### Exemple pratique

Imaginons que vous avez un fichier `script.sh` avec les permissions suivantes :

```bash
-rw-r--r-- 1 utilisateur groupe 1000 avril 23 01:48 script.sh
```

Si vous voulez donner des permissions d'exécution à tous les utilisateurs, vous pouvez faire :

```bash
chmod +x script.sh
```

Cela modifiera les permissions pour donner l'exécution :

```bash
-rwxr-xr-x 1 utilisateur groupe 1000 avril 23 01:48 script.sh
```

Vous pouvez également utiliser `cp -p` pour copier un fichier tout en préservant ses permissions et son propriétaire :

```bash
cp -p script.sh /chemin/vers/nouveau_script.sh
```

Cela gardera les mêmes métadonnées et permissions dans la copie.

<br>

## La gestion du propriétaire et du groupe d'un fichier

* Tout fichier a un seul propriétaire et un seul groupe d'appartenance
* Généralement, le propriétaire initial du fichier est l'utilisateur lié au
processus qui a créé le fichier
* Le groupe initial du fichier est le groupe de base de l'utilisateur lié au
processus qui a créé le fichier

Ci-dessous un tableau qui montre comment se fait la gestion du propriétaire et du groupe d'un fichier.


| **Commande**                               | **Description**                                      |
|--------------------------------------------|------------------------------------------------------|
| `chown alice fichier.txt`                  | Change le propriétaire du fichier à `alice`.         |
| `chown alice:dev fichier.txt`              | Change le propriétaire à `alice` et le groupe à `dev`. |
| `chown -R alice:dev /chemin/vers/dossier`  | Change récursivement le propriétaire et le groupe dans un répertoire. |
| `chgrp dev fichier.txt`                    | Change le groupe du fichier à `dev`.                 |
| `chgrp -R dev /chemin/vers/dossier`        | Change récursivement le groupe dans un répertoire.   |
| `ls -l fichier.txt`                        | Affiche les permissions, le propriétaire et le groupe du fichier. |


<br>

## Le Sticky Bit ( Bon à savoir !)

Le **sticky bit** est une permission spéciale qui peut être définie sur un répertoire. Lorsqu'il est activé, ce bit modifie le comportement des fichiers placés dans ce répertoire. Il permet essentiellement de **restreindre la suppression des fichiers** à leur propriétaire. Autrement dit, **seul le propriétaire d'un fichier ou l'administrateur (root)** peut supprimer ou renommer ce fichier dans un répertoire marqué avec le sticky bit, même si d'autres utilisateurs ont des permissions en écriture sur le répertoire.

### Utilité du Sticky Bit

Le sticky bit est souvent utilisé dans des répertoires partagés, comme `/tmp`, où de nombreux utilisateurs peuvent créer et modifier des fichiers, mais où il est essentiel que les utilisateurs ne puissent pas supprimer ou modifier les fichiers des autres.

**Cas typique d'utilisation** :
- **Répertoire `/tmp`** : C’est un répertoire utilisé par le système et les utilisateurs pour stocker des fichiers temporaires. Il est fréquent de définir le sticky bit sur ce répertoire pour empêcher les utilisateurs de supprimer des fichiers créés par d'autres utilisateurs dans ce répertoire.

### Comment définir et vérifier le Sticky Bit ?

#### 1. **Définir le Sticky Bit**

Pour définir le sticky bit, vous utilisez la commande `chmod` avec le **`+t`**. Cela se fait généralement sur des répertoires. Par exemple :

```bash
chmod +t /tmp
```

Cette commande ajoute le sticky bit sur le répertoire `/tmp`. Si vous voulez vérifier que le sticky bit est activé, vous pouvez utiliser la commande `ls -ld` pour afficher les permissions du répertoire :

```bash
ls -ld /tmp
```

La sortie pourrait ressembler à cela :

```bash
drwxrwxrwt 10 root root 4096 avril 23 10:00 /tmp
```

Ici, la `t` dans les **permissions `rwxrwxrwt`** (au lieu de `x` à la fin) indique que le sticky bit est activé.

#### 2. **Enlever le Sticky Bit**

Si vous voulez retirer le sticky bit d'un répertoire, vous pouvez utiliser la commande suivante :

```bash
chmod -t /tmp
```

Cela supprime le sticky bit, et les utilisateurs ayant des permissions d'écriture sur ce répertoire peuvent alors supprimer ou modifier les fichiers des autres utilisateurs.

### Résumé - Comportement du Sticky Bit

- **Avec sticky bit** : Si le sticky bit est activé sur un répertoire, seuls le **propriétaire du fichier** et **root** peuvent supprimer ou renommer ce fichier. Les autres utilisateurs ayant des permissions d'écriture sur le répertoire ne peuvent pas affecter les fichiers appartenant à d'autres utilisateurs.

- **Sans sticky bit** : Si le sticky bit n'est pas activé, tous les utilisateurs ayant des permissions d'écriture sur le répertoire peuvent supprimer ou renommer les fichiers des autres utilisateurs, indépendamment de qui en est le propriétaire.

<br>

## SETUID et SETGID

Le **SETUID** (Set User ID) et le **SETGID** (Set Group ID) sont des bits spéciaux qui peuvent être définis sur des fichiers exécutables sous Linux. Lorsqu'ils sont activés, ces bits modifient la façon dont le système gère l'exécution des fichiers. Leur objectif principal est de donner des permissions temporaires à l'exécution d'un programme, permettant à un utilisateur d'exécuter un fichier avec les permissions d'un autre utilisateur (généralement un utilisateur privilégié, comme root).

### SETUID (Set User ID)
- **But** : Permet à un utilisateur d'exécuter un fichier avec les **permissions du propriétaire** du fichier, **pas de l'utilisateur** qui lance l'exécution.
- **Cas d'utilisation** : C'est souvent utilisé pour des programmes nécessitant des privilèges élevés pour accomplir une tâche spécifique, comme `passwd` (changer un mot de passe), qui doit être exécuté avec les privilèges de root, même si l'utilisateur qui l'exécute n'est pas root.

#### Exemple :
```bash
chmod u+s /chemin/vers/fichier
```
Cela ajoute le bit **SETUID** à un fichier exécutable.

**Point important:** Les systèmes Linux modernes n'autorisent pas le SETUID sur les scripts shell (comme les .sh) pour des raisons de sécurité. Le script va donc s’exécuter avec les privilèges de l’utilisateur courant, pas ceux du propriétaire (root), même si le bit SETUID est actif.

<br>

**À tester (sans être root) 👨🏾‍💻👩🏾‍💻:**
- Ouvrir son terminal
- Exécuter: 
    ```bash
    $ ls -l /usr/bin/passwd ## Vous verez le 's' représentant le bit SETUID au niveau de la partie utilisateur. Ce fichier binaire appartient à root, mais tout le monde peut l'exécuter en tant que root.

    $ passwd ## cela vous permet de changer votre mot de passe sans utiliser 'sudo', grâce au bit SETUID.
    ```
<br>

### SETGID (Set Group ID)
- **But** : Permet à un utilisateur d'exécuter un fichier avec les **permissions du groupe** du fichier, plutôt que les permissions du groupe de l'utilisateur qui lance l'exécution.
- **Cas d'utilisation** : Très utile pour des programmes ou des répertoires partagés où il est important de maintenir des permissions de groupe spécifiques.

#### Exemple :
```bash
chmod g+s /chemin/vers/fichier
```
Cela ajoute le bit **SETGID** à un fichier exécutable ou à un répertoire.

### En résumé :
- **SETUID** : Permet d'exécuter un fichier avec les **permissions du propriétaire** du fichier (souvent root).
- **SETGID** : Permet d'exécuter un fichier avec les **permissions du groupe** du fichier.

Ces deux bits sont principalement utilisés pour la gestion des permissions temporaires lors de l'exécution de programmes, et doivent être utilisés avec précaution pour éviter des problèmes de sécurité.

**Point important:** Les systèmes Linux modernes n'autorisent pas le SETUID et SETGID sur les scripts shell (comme les .sh) pour des raisons de sécurité. Le script va donc s’exécuter avec les privilèges de l’utilisateur courant, pas ceux du propriétaire (root), même si le bit SETUID est actif. Voir la capture ci-dessous.

![](https://raw.githubusercontent.com/N0vachr0n0/Hands-on-Linux-course/refs/heads/main/fr/pictures/setuid_test.png)


<br>
<br>

---

# **Entraînement ⚔️**
## Exercice 1 
* Faire des recherches sur:
    * setfacl
    * getfacl
    * les inodes (dans le contexte de Linux bien sûr !)

## Exercice 2 (🧪)

Exécuter le script pour débuter le challenge comme un grand 😉.

* Lien du script du challenge: <https://raw.githubusercontent.com/N0vachr0n0/NoFD/refs/heads/main/FilesPerms_EXO_1.sh>

## Exercice 3 (Deep Dive)

* Faire ce challenge: <https://sadservers.com/scenario/budapest>    

---
---

## Feedback

> Faites-nous part de votre avis sur ce chapitre.

> 👉🏾 [Cliquez ici](https://forms.gle/nJHWw4uqLuAUjAyi7)

---
---

## Navigation 

* [Chapitre précédent](./04-users-groups.md)

* [Chapitre suivant](./06-package-management.md)

* [Allez au sommaire](./index.md)
