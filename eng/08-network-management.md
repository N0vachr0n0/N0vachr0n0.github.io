# **Chapter 8: Network Management**
# **Foreword**

We recommend that you do not use AI to do the exercises, as you are still in the learning phase.

# **Introduction**

In this section, we will cover the following points:
* Computer networks (briefly)
* Network troubleshooting on Linux 

## Prerequisites 

Same old story. 😉

<br>
<br>

---

# **Computer networks (briefly)**

## Introduction to computer networks

A computer network is a set of computers and devices connected to each other to share resources, exchange data, and provide services. Computer networks are essential in the modern world, enabling communication, file sharing, and access to remote resources.


### How does it all work?

For computers to communicate with each other, **they must speak a common language**: these are **network protocols**. The most fundamental of all is **TCP/IP**.


### The TCP/IP model (how the Internet really works)

**TCP/IP** is **the set of rules (or "protocol stack") used to run the Internet and most networks**.
It is made up of several layers, **each with a specific role**.

#### Layers of the TCP/IP model (simplified):

| Layer                  | Role                                                                        |
| ---------------------- | --------------------------------------------------------------------------- |
| **1. Network access**  | Manages communication with the hardware (Wi-Fi, Ethernet cable, etc.)       |
| **2. Internet**        | Finds the IP address of a computer on the network (IP protocol)             |
| **3. Transport**       | Ensures the reliability or speed of communication (TCP or UDP)              |
| **4. Application**     | What the user uses: HTTP, FTP, email, etc.                                  |


### TCP and UDP: two ways to transport data

At the **Transport layer**, two protocols are mainly used:

#### TCP (Transmission Control Protocol)

* **Reliable**: guarantees that all data arrives **in the correct order**, without error.
* **Connection-based**: establishes a "link" between the sender and the recipient before sending the data.
* **Example uses**: websites (HTTP/HTTPS), email (SMTP, IMAP), files (FTP).

#### UDP (User Datagram Protocol)

* **Fast**: sends data without checking whether it was actually received.
* **Connectionless**: no prior exchange, less overhead.
* **Example uses**: online games, audio/video calls, live streaming.

> **Metaphor**:
>
> * TCP is like sending a **parcel with proof of delivery**.
> * UDP is like sending a **postcard with no guarantee it will arrive**.

---

### The OSI model (theoretical 7-layer model)

The **OSI model** (*Open Systems Interconnection*) is a **theoretical representation** used to understand how data flows through a network, dividing it into **7 layers**.

| Layer  | Name                    | Simplified role                                        |
| ------ | ----------------------- | ------------------------------------------------------- |
| 7      | **Application**         | What the user sees (browser, email, etc.)                |
| 6      | **Presentation**        | Encoding, encryption, compression                        |
| 5      | **Session**             | Managing connections between applications                |
| 4      | **Transport**           | Reliability and ordering of data (TCP/UDP)                |
| 3      | **Network**             | Addressing and routing (IP)                               |
| 2      | **Data link**           | Communication between two directly connected machines     |
| 1      | **Physical**            | Cables, waves, electrical signals                          |

> **Mnemonic tip** to remember the order:
> "All People Seem To Need Data Processing" (Application, Presentation, Session, Transport, Network, Data link, Physical)


### OSI vs TCP/IP — What's the difference?

| OSI (7 layers)               | TCP/IP (4 layers)                                                       |
| ----------------------------- | ------------------------------------------------------------------------ |
| **Theoretical** model         | **Real** model, used on the Internet                                     |
| More **detailed**             | More **practical and implemented**                                       |
| Clearly separates functions   | Merges certain layers (e.g., application + presentation + session)       |

> In practice, **networks use TCP/IP**, but **OSI helps to understand** what happens at each stage.


<br>


## Types of Computer Networks

You should know that there are different types of networks:

- **PAN (Personal Area Network)**: A network at the scale of a single person, just a few meters. E.g.: Bluetooth between your phone and your headset, or a USB connection.
- **LAN (Local Area Network)**: Connects equipment within a limited area (home, building, campus). Can be wired (Ethernet) or wireless (**WLAN**, Wi-Fi/802.11, which is therefore a *subtype* of LAN, not a separate category).
- **MAN (Metropolitan Area Network)**: The scale of a city or metropolitan area. E.g.: the fiber-optic network linking several sites of the same company across a city, or the infrastructure of a local telecom operator.
- **WAN (Wide Area Network)**: Connects several LANs over long distances (cities, countries, continents). The Internet is the largest WAN that exists.
- **GAN (Global Area Network)**: A network at a worldwide scale, meant to describe a WAN that literally covers the entire planet, often via satellites (Starlink is a good concrete example).

## Network Equipment

Now, let's talk about network equipment:

- **Router**: A router connects several networks and routes traffic between them. It's like a switchyard for your data packets!
- **Switch**: A switch connects several computers within a network and routes data packets to their intended recipient.

## Network Addresses

In a computer network, there are two main elements used to identify a machine: the IP address and the MAC address.

- **IP address**: An IP address is a unique identifier assigned to a computer on a network, used for communication. You can have a static or dynamic IP address. 
Example: **192.168.34.3**.

- **MAC address**: A MAC address is a unique identifier assigned to a network card, used as a network address.
Example: **00:1A:2B:3C:4D:5E**.

<br>

**To test 👨🏾‍💻👩🏾‍💻:**
- Open your terminal
- Run: 
    ```bash
    $ ip a ## Displays your system's network interfaces along with their IP addresses and your various MAC addresses
    ```
<br>

Below is an example of the output.

```
$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute 
       valid_lft forever preferred_lft forever
2: enp1s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 52:54:00:c8:9a:ac brd ff:ff:ff:ff:ff:ff
    inet 172.30.1.2/24 brd 172.30.1.255 scope global dynamic noprefixroute enp1s0
       valid_lft 86302846sec preferred_lft 75513646sec
    inet6 fe80::7008:be3c:4abc:6bcd/64 scope link 
       valid_lft forever preferred_lft forever
3: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1454 qdisc noqueue state DOWN group default 
    link/ether 02:42:4c:21:02:63 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever
```

In our case, we have 3 network interfaces: **lo**, **enp1s0**, and **docker0**.

| Interface | Type      | IPv4 Address  | IPv6 Address              | MAC Address        | Status             |
| --------- | --------- | ------------- | ------------------------- | ------------------ | ------------------ |
| lo        | Loopback  | 127.0.0.1/8   | ::1/128                   | 00:00:00:00:00:00  | UP                 |
| enp1s0    | Ethernet  | 172.30.1.2/24 | fe80::.../64 (link-local) | 52:54:00:c8:9a:ac  | UP                 |
| docker0   | Virtual   | 172.17.0.1/16 | (no IPv6 here)            | 02:42:4c:21:02:63  | DOWN (no carrier)  |


- **lo**: lo is the loopback interface, used for the machine to communicate with itself.
- **enp1s0**: enp1s0 is a physical Ethernet network interface.
- **docker0**: docker0 is a virtual interface created by Docker to allow containers to communicate with each other and with the host.


## Focus on the loopback interface

The **loopback** interface (`lo`) lets a machine **send messages to itself**, as if it were going through the network. This might seem pointless, but in reality, it's fundamental for:

### **1. Local network testing (localhost)**

* **127.0.0.1** (or `localhost`) is used to **test network applications locally**, without needing a real network connection.
* Example: you're developing a website on your PC. You can launch a local server on `127.0.0.1:8000`, and access it via a browser — it never leaves the machine.


### **2. Internal system services**

* Many **system services communicate with each other over the network**, even when running on the same machine (for the sake of modularity or security).
* Example: a PostgreSQL database, a Redis server, etc. They often listen on `127.0.0.1`, and only allow local connections.


### **3. Network diagnostics**

* You can test the **network stack** without needing a cable or an Internet connection:

  ```bash
  ping 127.0.0.1
  ```

  This verifies that the operating system can send and receive TCP/IP packets.


### **4. Security**

* Services that **must not be accessible from the outside** can be bound only to `127.0.0.1`.
* Example: an administration interface accessible **only locally**.


###  Concrete example:

Let's imagine you're developing a local web application:


```bash
mkdir app_web
# Creates a new directory named "app_web"

cd app_web
# Moves you into this directory

echo "hello world" > index.html
# Creates the index.html file and writes "hello world" into it
# (index.html will be served automatically by default)

python3 -m http.server 8000
# Launches a minimal HTTP web server, built into Python (http.server module)
# -m http.server: runs this module as a script
# 8000: listening port (instead of the default port 80, which often requires root privileges)
# Serves the content of the current directory (here app_web) as read-only, by default
```

You can access it from your browser via:

```
http://127.0.0.1:8000
```

Even if you have **no Internet access**, this communication works via the loopback interface.

### In summary:

The `lo` interface lets a computer **address itself** using standard network protocols. This is **essential** for:

* Testing local apps,
* Running system services,
* Debugging,
* Isolating services from the outside world.

<br>

## Network configuration and routing on Linux

### Understanding **routing**

In networking, **"routing"** means **deciding which path (interface, gateway) to send IP packets through** in order to reach a destination. Let's look at a few commands related to routing.


#### `route` (old command)

The `route` command is **obsolete**, but is still sometimes used to display the **routing table**:

```bash
route -n
```

> This shows the paths used to reach known networks.
> The `-n` avoids DNS resolution for faster display.



#### `ip r` or `ip route` (modern)

```bash
ip route
# or shorter:
ip r
```

> Displays the system's current routing table.

##### Example:

```bash
default via 192.168.1.1 dev eth0
192.168.1.0/24 dev eth0 proto kernel scope link src 192.168.1.100
```

* **default**: the default route → anything unknown goes through here.
* **via 192.168.1.1**: the **gateway** used.
* **dev eth0**: network interface used.
* **192.168.1.0/24**: local subnet.


#### `ip route get`

```bash
ip route get 8.8.8.8
```

> Displays **the exact path** a packet would take to reach a given IP.

##### Example result:

```bash
8.8.8.8 via 192.168.1.1 dev eth0 src 192.168.1.100
```

* This confirms which interface, source IP, and gateway will be used.

---

### Gateway

The **gateway** is **the IP address of a router** on the local network, through which **traffic heading out to the outside world** (e.g., the Internet) is redirected.

* **It's the link between your local network (LAN) and the outside world (WAN/Internet).**
* It's generally **the Internet box or the router**.

Without a defined gateway → the system **cannot reach the outside world**.

<br>

###  Configuring a network interface

#### Method 1 – **Temporary** (non-persistent)

Using `ip` commands:

```bash
# Assign an IP to the eth0 interface
sudo ip addr add 192.168.1.100/24 dev eth0

# Enable the interface
sudo ip link set eth0 up

# Add a gateway
sudo ip route add default via 192.168.1.1
```

> ⚠️ This configuration is **temporary**: it disappears after a reboot or when the interface is disabled.

<br>

#### Method 2 – **Persistent** (configuration kept after reboot)

Depends on the distribution:



#### On Debian / Ubuntu (with Netplan or interfaces)

##### With `netplan` (Ubuntu 18.04+)

Edit the YAML file:

```bash
sudo nano /etc/netplan/01-netcfg.yaml
```

Example static configuration:

```yaml
network:
  version: 2
  ethernets:
    eth0:
      dhcp4: no
      addresses: [192.168.1.100/24]
      gateway4: 192.168.1.1
      nameservers:
        addresses: [8.8.8.8, 1.1.1.1]
```

Then apply:

```bash
sudo netplan apply
```



##### With `/etc/network/interfaces` (older systems):

```bash
auto eth0
iface eth0 inet static
  address 192.168.1.100
  netmask 255.255.255.0
  gateway 192.168.1.1
  dns-nameservers 8.8.8.8
```

Restart the network:

```bash
sudo systemctl restart networking
```

<br>

---

####  On Red Hat / CentOS / Fedora (with `nmcli` or `ifcfg-` files)

Example file:

```bash
/etc/sysconfig/network-scripts/ifcfg-eth0
```

Contents:

```ini
DEVICE=eth0
BOOTPROTO=static
ONBOOT=yes
IPADDR=192.168.1.100
NETMASK=255.255.255.0
GATEWAY=192.168.1.1
DNS1=8.8.8.8
```

Restarting the interface:

```bash
sudo ifdown eth0 && sudo ifup eth0
```

<br>


### Summary

| Command / Element              | Role                                             |
| ------------------------------- | ------------------------------------------------- |
| `ip r` / `ip route`             | Displays the routing table                        |
| `ip route get <IP>`             | Shows the network path used                        |
| `route -n`                      | Old method for viewing the routing table           |
| `gateway`                       | Gateway to the outside world (router)              |
| `ip addr add` / `ip link`       | **Temporary** interface configuration              |
| YAML files or `interfaces`      | **Persistent** configuration                       |

<br>

## DNS

DNS (Domain Name System) is a system that translates domain names into IP addresses. It's essential for browsing the Internet!
You can change your DNS in different ways. Either via the `/etc/resolv.conf` file. Or via **the Network Manager graphical interface**. Or **via `resolv.conf`**.

**Example with resolv.conf:**

 ```bash
 # Edit the resolv.conf file
 sudo nano /etc/resolv.conf

 # Add the following line:
 nameserver 8.8.8.8
 ```

 A few explanations:
* `nameserver`: This is a directive used to specify a **DNS server** (Domain Name System).
* `8.8.8.8`: This is the **IP address of the DNS server** that the system will use to **translate domain names** (e.g., `www.google.com`) into IP addresses (e.g., `142.250.190.68`).

<br>

**⚠️ Warning**:

On some modern distributions (Ubuntu, Fedora…), this file is generated automatically by services such as systemd-resolved or NetworkManager. So any manual edit to /etc/resolv.conf risks being overwritten on reboot.


<br>

Now let's talk about the **/etc/hosts** file!!!

Even before querying a DNS server, Linux first checks the **/etc/hosts** file.
This file lets you manually associate hostnames with IP addresses. It is used first, before DNS, for local resolution.


**Example of an `/etc/hosts` file**:

```bash
127.0.0.1       localhost
127.0.1.1       mypc.local mypc
192.168.1.100   web-server.local mysite
```
Explanation:

* **127.0.0.1**: corresponds to the machine itself (loopback).
* **localhost**: default hostname.
* **192.168.1.100 web-server.local**: allows access to this server (web-server.local) without DNS.

**Use cases for `/etc/hosts`**

* **Test a site locally** before changing the public DNS.
* **Force a domain name** to point to a specific IP.
* **Block a site** by redirecting it to `127.0.0.1`.
* **Work offline** without a DNS server.

<br>

## Role of Network Ports and Services

Ports are numbers that identify the services listening on a machine. You can see the standard port numbers and their services in the `/etc/services` file.

- **Example**:
    ```bash
    cat /etc/services
    ```

    ```
    ...
    ftp-data	20/tcp
    ftp		    21/tcp
    fsp		    21/udp		fspd
    ssh		    22/tcp				# SSH Remote Login Protocol
    telnet		23/tcp
    smtp		25/tcp		mail
    ```

<br>
<br>

---

# **Some network troubleshooting tools on Linux**

**INFO:** <br>
* netstat, ifconfig, route are included in the net-tools package.
* ping => iputils
* ss => iproute2
* traceroute/tcpdump/nmap/mtr/netcat/wireshark => separate packages. 

So you'll need to install it in case you don't have it.🙂


## Ping

- **What is Ping?**: Ping tests connectivity and measures response time.
- **How to use Ping**:
 ```bash
 # Test connectivity with Google
 ping google.com
 ```

## Traceroute

- **What is Traceroute?**: Traceroute follows the path that data packets take from source to destination.
- **How to use Traceroute**:
 ```bash
 # Trace the path to Google
 traceroute google.com
 ```

## Netstat

- **What is Netstat?**: Netstat displays active Internet/network connections, routing tables, and interface statistics.
- **How to use Netstat**:
 ```bash
 # View active connections
 netstat -tuln
 ```

## Tcpdump

- **What is Tcpdump?**: Tcpdump captures and displays network traffic.
- **How to use Tcpdump**:
 ```bash
 # Capture traffic on the eth0 interface
 sudo tcpdump -i eth0
 ```

## Wireshark

- **What is Wireshark?**: Wireshark analyzes network traffic.
- **How to use Wireshark**:
 ```bash
 # Launch Wireshark
 sudo wireshark
 ```

## Nmap

- **What is Nmap?**: Nmap scans networks for hosts and services.
- **How to use Nmap**:
 ```bash
 # Scan the 192.168.1.0/24 network
 nmap -sS 192.168.1.0/24
 ```

## Netcat

- **What is Netcat?**: Netcat reads and writes network connections.
- **How to use Netcat**:
 ```bash
 # Connect to Google on port 80
 nc google.com 80
 ```

## SS

- **What is SS?**: SS is a tool for investigating sockets.
- **How to use SS**:
 ```bash
 # View active sockets
 ss -tuln
 ```

## MTR

- **What is MTR?**: MTR combines Ping and Traceroute.
- **How to use MTR**:
 ```bash
 # Test connectivity and trace the path to Google
 mtr google.com
 ```

<br>
<br>

---

# **Training ⚔️**

## EXERCISE 1

Complete the level 0 to 20 challenges of the "Bandit" category on the "overthewire" platform.
Link: <https://overthewire.org/wargames/bandit/> (This link points directly to the "Bandit" category)

The first challenges will serve as a review 🙂.

## EXERCISE 2

You must perform a complete network analysis on the public server scanme.nmap.org using the Nmap tool, in order to answer the following questions: 

1. What is the version of the SSH service (port 22) hosted on this server?
2. What service is available on port 80/tcp?
3. How many TCP ports are in the open state?
4. What service is associated with port 9929/tcp?
     

**⚠️ Warning**: It is illegal to scan a network or website that is not yours without authorization. "<scanme.nmap.org>" is a testing platform provided by nmap so you can freely test the nmap tool.

## EXERCISE 3

Challenge script link: <https://raw.githubusercontent.com/N0vachr0n0/NoFD/refs/heads/main/Network_EXO_1.sh>


---
---

## Feedback

> Give us your feedback on this chapter.

> 👉🏾 [Click here](https://forms.gle/xnuAAfbBtxyGFz8h9)


---
---

## Navigation 

* [Previous chapter](./07-service-management.md)

* [Next chapter](./09-firewall.md)

* [Go To Summary](./index.md)
