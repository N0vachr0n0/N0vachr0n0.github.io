# **Chapitre 9: Pare-feu**
# **Avant-propos**

Nous vous recommandons de ne pas utiliser d'IA pour faire les exercices car vous êtes en phase d'apprentissage.

# **Introduction**

Dans ce chapitre, nous parlerons de l'exploitation des firewalls sous Linux.

## Prérequis 

Toujours la même histoire. 😉

<br>
<br>

---

# **Qu'est-ce qu'un firewall ?**

Un firewall (ou pare-feu) est un système de sécurité réseau qui contrôle les accès entre un réseau interne et un réseau externe, généralement Internet. Son rôle est de protéger le réseau interne contre les attaques et les accès non autorisés.

**Types de firewalls**

Il existe plusieurs types de firewalls :

1. **Firewall matériel** : il s'agit d'un appareil dédié placé entre le réseau interne et Internet.
2. **Firewall logiciel** : il s'agit d'un logiciel qui est installé sur un serveur ou un ordinateur et qui contrôle les accès réseau.

<br>
<br>

---

# **Les firewalls sous Linux**

Sous Linux, nous allons aborder trois outils populaires pour la gestion des firewalls :

1. **iptables** : c'est un outil de gestion de règles de firewall intégré à Linux.
2. **ufw** (Uncomplicated Firewall) : c'est un outil de gestion de firewall simplifié pour Ubuntu et les autres distributions Linux.
3. **firewalld** : c'est un outil de gestion de firewall qui utilise les règles iptables sous le capot.

## Iptables

**Qu'est-ce qu'iptables ?**

iptables est un outil de gestion de règles de firewall intégré à Linux. Il permet de définir des règles pour contrôler les paquets réseau qui entrent ou sortent de votre système.

**Commandes de base**

Voici quelques commandes de base pour utiliser iptables :

* **Lister les règles** : `iptables -n -L`
* **Ajouter une règle** : `iptables -A INPUT -p tcp --dport 22 -j ACCEPT`
* **Supprimer une règle** : `iptables -D INPUT -p tcp --dport 22 -j ACCEPT`

**Exemple:**

 Pour autoriser les connexions SSH (port 22) :
```bash
iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```
 Pour interdire les connexions ICMP (ping) :
```bash
iptables -A INPUT -p icmp -j DROP
```

## UFW (Uncomplicated Firewall)

**Qu'est-ce qu'ufw ?**

ufw est un outil de gestion de firewall simplifié pour Ubuntu et les autres distributions Linux. Il est conçu pour être facile à utiliser et à comprendre.

**Commandes de base**

Voici quelques commandes de base pour utiliser ufw :

* **Activer ufw** : `ufw enable`
* **Désactiver ufw** : `ufw disable`
* **Lister les règles** : `ufw status`
* **Autoriser un port** : `ufw allow 22`
* **Bloquer un port** : `ufw deny 22`

**Exemple:**

 Pour autoriser les connexions SSH (port 22) :
```bash
ufw allow 22
```


## Firewalld

**Qu'est-ce que firewalld ?**

firewalld est un outil de gestion de firewall qui utilise les règles iptables sous le capot. Il est conçu pour être plus simple à utiliser que iptables.

**Commandes de base**

Voici quelques commandes de base pour utiliser firewalld :

* **Lister les zones** : `firewall-cmd --list-all-zones`
* **Lister les règles** : `firewall-cmd --list-all`
* **Ajouter une règle** : `firewall-cmd --zone=public --add-port=22/tcp --permanent`
* **Supprimer une règle** : `firewall-cmd --zone=public --remove-port=22/tcp --permanent`

**Exemple:**

Pour autoriser les connexions SSH (port 22) :
```bash
firewall-cmd --zone=public --add-port=22/tcp --permanent
```
 Pour interdire les connexions ICMP (ping) :
```bash
firewall-cmd --zone=public --add-icmp-block=echo-request --permanent
```

<br>

**Points Importants:**
- Faites des recherches sur les différentes zones de firewalld. 


<br>
<br>

---

# **Entraînement ⚔️**

En libre-service 🙂

---
---

## Feedback

> Faites-nous part de votre avis sur ce chapitre.

> 👉🏾 [Cliquez ici](https://forms.gle/88jPmFLnNPtjdgqv8)


---
---

## Navigation 

* [Chapitre précédent](./08-network-management.md)

* [Chapitre suivant](./10-tasks-planification.md)

* [Allez au sommaire](./index.md)
