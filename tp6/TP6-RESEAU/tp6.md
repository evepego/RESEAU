# TP 6 - Une topologie qui ressemble un peu à quelque chose, enfin ?

### Réseaux IP et aires OSPF

Dans chacune des aires OSPF se trouveront des réseaux IP. Un bon vieux tableau pour ça (tout ça est sur le schéma).

Réseaux | `area 0` | `area 1` | `area 2` | Commentaire
--- | :---: | :---: | :---: | ---
`10.6.100.0/30` | X | - | - | Liaison entre `r1` et `r2`
`10.6.100.4/30` | X | - | - | Liaison entre `r1` et `r4`
`10.6.100.8/30` | X | - | - | Liaison entre `r2` et `r3` 
`10.6.100.12/30` | X | - | - | Liaison entre `r3` et `r4`
`10.6.101.0/30` | - | X | - | Liaison entre `r3` et `r5`
`10.6.201.0/24` | - | X | - | Réseau des clients
`10.6.202.0/24` | - | - | X | Réseau des serveurs

### Adressage IP de chacune des machines

Machines | `10.6.100.0/30` | `10.6.100.4/30` | `10.6.100.8/30` | `10.6.100.12/30` | `10.6.101.0/30` | `10.6.201.0/24` | `10.6.202.0/24`
--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: 
`r1.tp6.b1` | `10.6.100.1` | `10.6.100.5` | - | - | - | - | `10.6.202.254`
`r2.tp6.b1` | `10.6.100.2` | - |  `10.6.100.9` | - | - | - | -
`r3.tp6.b1` | - | - | `10.6.100.10` | `10.6.100.14` | `10.6.101.1` | - | -
`r4.tp6.b1` | - |  `10.6.100.6` | - | `10.6.100.13` | - | - | -
`r5.tp6.b1` | - | - | - | - |  `10.6.101.2` |  `10.6.201.254` | -
`client1.tp6.b1` | - | - | - | - | - |  `10.6.201.10` | -
`client2.tp6.b1` | - | - | - | - | - |  `10.6.201.11` | -
`server1.tp6.b1` | - | - | - | - | - | - | `10.6.202.10`

## 2. Mise en place du lab

### Checklist IP Routeurs 

On parle de `r1.tp6.b1`, `r2.tp6.b1`, `r3.tp6.b1`, `r4.tp6.b1` et `r5.tp6.b1` :
* [X] Définition des IPs statiques
* Vérifications :
    * Router 1 :
    ```bash
    R1#show ip int br
    Interface                  IP-Address      OK? Method Status                Protocol
    Ethernet0/0                10.6.100.1      YES manual up                    up
    Ethernet0/1                10.6.202.254    YES manual up                    up
    Ethernet0/2                10.6.100.5      YES manual up                    up
    ```
    * Router 2 :
    ```bash
    R2#show ip int br
    Interface                  IP-Address      OK? Method Status                Protocol
    Ethernet0/0                10.6.100.2      YES manual up                    up
    Ethernet0/1                10.6.100.9      YES manual up                    up
    ```
    * Router 3 :
    ```bash
    R3#show ip int br
    Interface                  IP-Address      OK? Method Status                Protocol
    Ethernet0/0                10.6.100.14     YES manual up                    up
    Ethernet0/1                10.6.100.10     YES manual up                    up
    Ethernet0/2                10.6.101.1      YES manual up                    up
    ```
    * Router 4 :
    ```bash
    R4#show ip int br
    Interface                  IP-Address      OK? Method Status                Protocol
    Ethernet0/0                10.6.100.13     YES manual up                    up
    Ethernet0/2                10.6.100.6      YES manual up                    up
    ```
    * Router 5 :
    ```bash
    R5#show ip int br
    Interface                  IP-Address      OK? Method Status                Protocol
    Ethernet0/0                10.6.201.254    YES manual up                    up
    Ethernet0/2                10.6.101.2      YES manual up                    up
    ```
* [X] Définition du nom de domaine
* Vérifications :
    * Router1 :
    ```bash
    router1#
    *Mar  1 00:00:50.747: %SYS-5-CONFIG_I: Configured from console by console
    router1#
    ```
    * Router2 :
    ```bash
    router2#
    *Mar  1 00:00:50.747: %SYS-5-CONFIG_I: Configured from console by console
    router2#
    ```
    * Router 3 :
    ```bash
    router3#
    *Mar  1 00:00:50.747: %SYS-5-CONFIG_I: Configured from console by console
    router3#
    ```
    * Router 4 :
    ```bash
    router4#
    *Mar  1 00:00:50.747: %SYS-5-CONFIG_I: Configured from console by console
    router4#
    ```
    * Router 5 :
    ```bash
    router5#
    *Mar  1 00:00:50.747: %SYS-5-CONFIG_I: Configured from console by console
    router5#
    ```

### Checklist VMs

On parle de `client1.tp6.b1`, `client2.tp6.b1` et `server1.tp6.b1` :
* [X] Désactiver SELinux
  * déja fait dans le patron
* [X] Installation de certains paquets réseau
  * déja fait dans le patron
* [X] Enlever la carte NAT
* [X] Définition des IPs statiques
* [X] Définition du nom de domaine
* [X] remplir les fichiers `hosts`
* Vérifications :
    * Client1 :
    ```bash
    [root@client1 ~]# ip a
    2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
        link/ether 08:00:27:99:90:09 brd ff:ff:ff:ff:ff:ff
        inet 10.6.201.10/24 brd 10.6.201.255 scope global noprefixroute enp0s3
        valid_lft forever preferred_lft forever
        inet6 fe80::a00:27ff:fe99:9009/64 scope link
        valid_lft forever preferred_lft forever
    3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
        link/ether 08:00:27:54:17:4f brd ff:ff:ff:ff:ff:ff
        inet 192.168.36.4/24 brd 192.168.36.255 scope global noprefixroute dynamic enp0s8
        valid_lft 840sec preferred_lft 840sec
        inet6 fe80::6669:2400:ab4:1393/64 scope link noprefixroute
        valid_lft forever preferred_lft forever
    ```
    * Client2 :
    ```bash
    [root@client2 ~]# ip a

    2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
        link/ether 08:00:27:1b:ec:08 brd ff:ff:ff:ff:ff:ff
        inet 10.6.201.11/24 brd 10.6.201.255 scope global noprefixroute enp0s3
        valid_lft forever preferred_lft forever
        inet6 fe80::a00:27ff:fe1b:ec08/64 scope link
        valid_lft forever preferred_lft forever
    3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
        link/ether 08:00:27:16:de:06 brd ff:ff:ff:ff:ff:ff
        inet 192.168.36.6/24 brd 192.168.36.255 scope global noprefixroute dynamic enp0s8
        valid_lft 849sec preferred_lft 849sec
        inet6 fe80::9f4a:c147:4c88:2940/64 scope link noprefixroute
        valid_lft forever preferred_lft forever
    ```
    * Server1 :
    ```bash
    [root@server1 ~]# ip a
    2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
        link/ether 08:00:27:d2:a4:e0 brd ff:ff:ff:ff:ff:ff
        inet 10.6.202.10/24 brd 10.6.202.255 scope global noprefixroute enp0s3
        valid_lft forever preferred_lft forever
        inet6 fe80::a00:27ff:fed2:a4e0/64 scope link
        valid_lft forever preferred_lft forever
    3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
        link/ether 08:00:27:52:c2:fe brd ff:ff:ff:ff:ff:ff
        inet 192.168.36.5/24 brd 192.168.36.255 scope global noprefixroute dynamic enp0s8
        valid_lft 842sec preferred_lft 842sec
        inet6 fe80::4c2e:340b:939e:563/64 scope link noprefixroute
        valid_lft forever preferred_lft forever
    ```

### [Configuration de OSPF]

Ca se passe sur les routeurs uniquement. On parle de `r1.tp6.b1`, `r2.tp6.b1`, `r3.tp6.b1`, `r4.tp6.b1` et `r5.tp6.b1` :
* [X] activation de OSPF
* [X] configurer le `router-id`
* [X] configurer les routes à partager
* Vérifications :
    * Client 1 vers Server 1:
    ```bash
    [root@client1 ~]# traceroute server1
    traceroute to server1 (10.6.202.10), 30 hops max, 60 byte packets
    1  gateway (10.6.201.254)  4.977 ms  4.832 ms  4.859 ms
    2  10.6.101.1 (10.6.101.1)  25.549 ms  25.481 ms  25.526 ms
    3  10.6.100.13 (10.6.100.13)  45.562 ms  45.574 ms  45.726 ms
    4  10.6.100.5 (10.6.100.5)  66.400 ms  66.565 ms  66.454 ms
    5  server1 (10.6.202.10)  76.675 ms !X  76.606 ms !X  76.503 ms !X
    ```
    * Client 2 vers Server 1 :
    ```bash
    [root@client2 ~]# traceroute server1
    traceroute to server1 (10.6.202.10), 30 hops max, 60 byte packets
    1  gateway (10.6.201.254)  7.173 ms  7.048 ms  7.029 ms
    2  10.6.101.1 (10.6.101.1)  27.542 ms  27.471 ms  27.679 ms
    3  10.6.100.13 (10.6.100.13)  48.250 ms  48.149 ms  48.179 ms
    4  10.6.100.5 (10.6.100.5)  58.425 ms  58.332 ms  68.831 ms
    5  server1 (10.6.202.10)  68.752 ms !X  68.639 ms !X  68.698 ms !X
    ```
    * Server 1 vers Client 1 :
    ```bash
    [root@server1 ~]# traceroute client1
    traceroute to client1 (10.6.201.10), 30 hops max, 60 byte packets
    1  gateway (10.6.202.254)  10.169 ms  10.035 ms  10.933 ms
    2  10.6.100.6 (10.6.100.6)  31.120 ms  31.067 ms  30.984 ms
    3  10.6.100.14 (10.6.100.14)  51.604 ms  51.831 ms  51.831 ms
    4  10.6.101.2 (10.6.101.2)  72.940 ms  72.860 ms  72.752 ms
    5  client1 (10.6.201.10)  83.293 ms !X  83.208 ms !X  83.099 ms !X
    ```
    * Server 1 vers Client 2 :
    ```bash
    [root@server1 ~]# traceroute client2
    traceroute to client2 (10.6.201.11), 30 hops max, 60 byte packets
    1  gateway (10.6.202.254)  11.238 ms  11.105 ms  10.970 ms
    2  10.6.100.2 (10.6.100.2)  31.972 ms  31.866 ms  31.735 ms
    3  10.6.100.10 (10.6.100.10)  53.190 ms  53.011 ms  52.922 ms
    4  10.6.101.2 (10.6.101.2)  74.490 ms  74.846 ms  74.832 ms
    5  client2 (10.6.201.11)  85.426 ms !X  85.338 ms !X  85.302 ms !X
    ```
# Lab 3 : Let's end this properly

Service | Qui porte le service ? | Pour qui ? | Pourquoi ? 
--- | --- | --- | ---
**NAT** | `r4.tp6.b1` | tout le monde (routeurs & VMs) | Le NAT permet d'accéder à l'extérieur, il permet de sortir du LAN. Toutes les machines peuvent en avoir besoin dans notre petite infra
**Serveur Web** | `server1.tp6.b1` | réseau client `10.6.201.0/24` | Le serveur Web symbolise un service d'infra en interne. Dispo pour nos clients. Plus de détails dans la section dédiée.
**DHCP** | `client2.tp6.b1` | réseau client `10.6.201.0/24` | Le DHCP (qui permet d'attribuer des IPs automatiquement) c'est pour des clients. Pas pour des serveurs. Un serveur, on veut qu'il ait une IP fixe. 
**DNS** | `server1.tp6.b1` | tout le monde (routeurs & VMs) | Le DNS nous permettra de résoudre les noms de domaines en local et nous passer du fichier `/etc/hosts`
**NTP** | `server1.tp6.b1` | réseau serveur `10.6.202.0/24` | Le NTP, qui permet la synchronisation de l'heure, est souvent indispensable pourdes serveurs mais totalement négligeable pour des clients (genre vos PCs, s'ils sont pas à l'heure, tout le monde s'en fout)
---

## 1. NAT : accès internet
# Vérification : `ping 8.8.8.8`
* Server 1 :
```bash
[root@server1 etc]# ping 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=118 time=85.1 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=118 time=72.3 ms
```
* Client 1 :
```bash
[root@client1 ~]# ping 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=117 time=88.1 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=117 time=163 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=117 time=131 ms
```
* Client 2 :
```bash
[root@client2 ~]# ping 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=117 time=102 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=117 time=101 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=117 time=105 ms
```
## 2. Un service d'infra
# Vérifications :
```bash
[root@client1 ~]# curl server1
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.1//EN" "http://www.w3.org/TR/xhtml11/DTD/xhtml11.dtd">

<html xmlns="http://www.w3.org/1999/xhtml" xml:lang="en">
    <head>
        <title>Test Page for the Nginx HTTP Server on Fedora</title>
        <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
        <style type="text/css">
            /*<![CDATA[*/
            body {
                background-color: #fff;
                color: #000;
                font-size: 0.9em;
                font-family: sans-serif,helvetica;
                margin: 0;
                padding: 0;
            }
            ...
```
## 3. Serveur DHCP
# Vérifications :
```bash
[root@dhcp dhcp]# cat dhcpd.conf
# dhcpd.conf

# option definitions common to all supported networks
option domain-name "tp6.b1";

default-lease-time 600;
max-lease-time 7200;

# If this DHCP server is the official DHCP server for the local
# network, the authoritative directive should be uncommented.
authoritative;

# Use this to send dhcp log messages to a different log file (you also
# have to hack syslog.conf to complete the redirection).
log-facility local7;

subnet 10.6.201.0 netmask 255.255.255.0 {
  range 10.6.201.50 10.6.201.70;
  option domain-name "tp6.b1";
  option routers 10.6.201.254;
  option broadcast-address 10.6.201.255;
}
```
```bash
[root@client2 network-scripts]# ip a
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 08:00:27:1b:ec:08 brd ff:ff:ff:ff:ff:ff
    inet 10.6.201.50/24 brd 10.6.201.255 scope global noprefixroute dynamic enp0s3
       valid_lft 592sec preferred_lft 592sec
    inet6 fe80::a00:27ff:fe1b:ec08/64 scope link
       valid_lft forever preferred_lft forever
```