# **Chapter 9: Firewall**
# **Foreword**

We recommend that you do not use AI to do the exercises, as you are in the learning phase.

# **Introduction**

In this chapter, we will talk about exploiting firewalls on Linux.

## Prerequisites 

Same old story. 😉

<br>
<br>

---

# **What is a firewall?**

A firewall is a network security system that controls access between an internal network and an external network, usually the Internet. Its role is to protect the internal network against attacks and unauthorized access.

**Types of firewalls**

There are several types of firewalls:

1. **Hardware firewall**: this is a dedicated appliance placed between the internal network and the Internet.
2. **Software firewall**: this is software installed on a server or computer that controls network access.

<br>
<br>

---

# **Firewalls on Linux**

On Linux, we will cover three popular tools for managing firewalls:

1. **iptables**: this is a firewall rule management tool built into Linux.
2. **ufw** (Uncomplicated Firewall): this is a simplified firewall management tool for Ubuntu and other Linux distributions.
3. **firewalld**: this is a firewall management tool that uses iptables rules under the hood.

## Iptables

**What is iptables?**

iptables is a firewall rule management tool built into Linux. It lets you define rules to control network packets entering or leaving your system.

**Basic commands**

Here are some basic commands for using iptables:

* **List rules**: `iptables -n -L`
* **Add a rule**: `iptables -A INPUT -p tcp --dport 22 -j ACCEPT`
* **Delete a rule**: `iptables -D INPUT -p tcp --dport 22 -j ACCEPT`

**Example:**

 To allow SSH connections (port 22):
```bash
iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```
 To block ICMP (ping) connections:
```bash
iptables -A INPUT -p icmp -j DROP
```

## UFW (Uncomplicated Firewall)

**What is ufw?**

ufw is a simplified firewall management tool for Ubuntu and other Linux distributions. It is designed to be easy to use and understand.

**Basic commands**

Here are some basic commands for using ufw:

* **Enable ufw**: `ufw enable`
* **Disable ufw**: `ufw disable`
* **List rules**: `ufw status`
* **Allow a port**: `ufw allow 22`
* **Block a port**: `ufw deny 22`

**Example:**

 To allow SSH connections (port 22):
```bash
ufw allow 22
```


## Firewalld

**What is firewalld?**

firewalld is a firewall management tool that uses iptables rules under the hood. It is designed to be simpler to use than iptables.

**Basic commands**

Here are some basic commands for using firewalld:

* **List zones**: `firewall-cmd --list-all-zones`
* **List rules**: `firewall-cmd --list-all`
* **Add a rule**: `firewall-cmd --zone=public --add-port=22/tcp --permanent`
* **Delete a rule**: `firewall-cmd --zone=public --remove-port=22/tcp --permanent`

**Example:**

To allow SSH connections (port 22):
```bash
firewall-cmd --zone=public --add-port=22/tcp --permanent
```
 To block ICMP (ping) connections:
```bash
firewall-cmd --zone=public --add-icmp-block=echo-request --permanent
```

<br>

**Important points:**
- Do some research on the different firewalld zones. 


<br>
<br>

---

# **Training ⚔️**

Self-service 🙂

---
---

## Feedback

> Give us your feedback on this chapter.

> 👉🏾 [Click here](https://forms.gle/88jPmFLnNPtjdgqv8)


---
---

## Navigation 

* [Previous chapter](./08-network-management.md)

* [Next chapter](./10-tasks-planification.md)

* [Go To Summary](./index.md)
