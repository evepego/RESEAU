# TP4 - Spéléologie réseau : descente dans les couches

## II. Spéléologie réseau :
### 1. ARP :
#### A. Manip 1
##### 1 : 
Pour vider la table ARP, j'utilise la commande suivante :
```bash
ip neigh flush all
```
##### 2 :
- Pour afficher la table ARP, j'utilise la commande suivante :
```bash
ip neigh show
```
- On obtiendra :
```bash
10.1.0.1 dev enp0s3 lladdr 0a:00:27:00:00:11 DELAY
```
- C'est la connexion entre putty et mon oridinateur.
##### 3 : 
- J'effectue la commande pour afficher la table ARP vue ci-dessus.
```bash
10.2.0.1 dev enp0s3 lladdr 0a:00:27:00:00:12 DELAY
```
- C'est la connexion entre putty et mon oridinateur.
##### 4 :
- J'effectue la commande : 
```bash
ping server1.tp4
```
- J'effectue la commande pour afficher la table ARP vue ci-dessus.
```bash
10.1.0.254 dev enp0s3 lladdr 08:00:27:16:c7:61 REACHABLE
10.1.0.1 dev enp0s3 lladdr 0a:00:27:00:00:11 REACHABLE
```
- 1ère ligne : C'est la connexion entre mon client et mon routeur.
- 2ème ligne : C'est la connexion entre putty et mon oridinateur.
##### 5 :
- J'effectue la commande pour afficher la table ARP vue ci-dessus.
```bash
10.2.0.1 dev enp0s3 lladdr 0a:00:27:00:00:12 DELAY
10.2.0.254 dev enp0s3 lladdr 08:00:27:1a:55:e9 STALE
```
- 1ère ligne : C'est la connexion entre putty et mon oridinateur.
- 2ème ligne : C'est la connexion entre mon client et mon routeur.

#### B. Manip 2
##### 1 :
Pour vider la table ARP, j'utilise la commande suivante :
```bash
ip neigh flush all
```
##### 2 :
- Pour afficher la table ARP, j'utilise la commande suivante :
```bash
ip neigh show
```
- J'effectue les commandes suivantes :
```bash
[root@router ~]# ip neigh flush all
[root@router ~]# ip neigh show
10.2.0.1 dev enp0s8 lladdr 0a:00:27:00:00:12 REACHABLE
```
- Il n'y a qu'une seule ligne car le router1 n'a communiqué avec aucune autre adresse MAC. Elles ne sont donc pas dans la table ARP.
##### 3 :
- J'effectue la commande : 
```bash
ping server1.tp4
```
##### 4 :
- J'effectue la commande vue ci-dessus pour afficher la table ARP.
```bash
[root@router1 /]$ ip neigh show
10.1.0.10 dev enp0s8 lladdr 08:00:27:96:37:a3 REACHABLE
10.1.0.1 dev enp0s8 lladdr 0a:00:27:00:00:46 DELAY
10.2.0.10 dev enp0s9 lladdr 08:00:27:fb:54:90 REACHABLE
```
- Les IPs MAC de client1 et server1 sont enregistrés par router1 qui sert d'intermédiaire lors du ping.
#### C. Manip 3
##### 1 :
- Pour vider la table ARP sur mon PC, j'utilise la commande suivante :
```bash
arp -d
```
##### 2 :
- Sur mon PC, j'effectue la commande suivante :
```bash
arp -a
``` 
- Table ARP après avoir été vidée :
```bash
Interface : 192.168.101.1 --- 0xc
  Adresse Internet      Adresse physique      Type
  224.0.0.22            01-00-5e-00-00-16     statique

Interface : 10.33.3.204 --- 0x1a
  Adresse Internet      Adresse physique      Type
  10.33.3.253           00-12-00-40-4c-bf     dynamique
  224.0.0.22            01-00-5e-00-00-16     statique

Interface : 10.1.0.1 --- 0x46
  Adresse Internet      Adresse physique      Type
  224.0.0.22            01-00-5e-00-00-16     statique

Interface : 10.2.0.1 --- 0x4b
  Adresse Internet      Adresse physique      Type
  224.0.0.22            01-00-5e-00-00-16     statique
```
- Table ARP après un peu d'attente :
```bash
Interface : 192.168.101.1 --- 0xc
  Adresse Internet      Adresse physique      Type
  192.168.101.255       ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique

Interface : 10.33.3.204 --- 0x1a
  Adresse Internet      Adresse physique      Type
  10.33.0.196           f8-59-71-83-5e-78     dynamique
  10.33.1.18            2c-20-0b-41-55-95     dynamique
  10.33.1.135           6c-e8-5c-3d-f4-85     dynamique
  10.33.1.149           dc-a2-66-21-d4-7d     dynamique
  10.33.3.253           00-12-00-40-4c-bf     dynamique
  10.33.3.255           ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique

Interface : 10.1.0.1 --- 0x46
  Adresse Internet      Adresse physique      Type
  10.1.0.10             08-00-27-96-37-a3     dynamique
  10.1.0.255            ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique

Interface : 10.2.0.1 --- 0x4b
  Adresse Internet      Adresse physique      Type
  10.2.0.10             08-00-27-fb-54-90     dynamique
  10.2.0.255            ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique
```

#### D. Manip 4
##### 1 :
- Pour vider la table ARP, j'utilise la commande suivante :
```bash
ip neigh flush all
```
##### 2 :
- J'affiche la table ARP de client1.
- Voici ce que affiche latable arp avant activation de la carte NAT :
```
[root@client ~]# ip neigh show
10.1.0.1 dev enp0s3 lladdr 0a:00:27:00:00:11 REACHABLE
```
- Pour activer la carte NAT :
    - J'éteind la machine client1
    - Dans **configurations** puis dans **réseau**, je crée une nouvelle carte en NAT.
    - Je rallume la VM client1.
- Voici ce que affiche la table arp après un **curl google.com** :
```
[root@client ~]# ip neigh show
10.1.0.1 dev enp0s3 lladdr 0a:00:27:00:00:11 REACHABLE
10.0.3.2 dev enp0s8 lladdr 52:54:00:12:35:02 REACHABLE
```
- EXPLIQUER LES COMMANDES

### 2. Wireshark :
- Voici un récapitulatif des IPs des machines :

| **Machine** | ```net1``` | ```net2``` | **Adresse MAC** |
|:-----------:|:----------:|:----------:|:---------------:|
|```client1```|```10.1.0.10```|X|```0a:00:27:00:00:46```|
|```router1```|```10.1.0.254```|```10.2.0.254```|```08:00:27:9c:42:37```|
|```server1```|X|```10.2.0.10```|```0a:00:27:00:00:4b```|

- Je télécharge Wireshark sur mon PC.
- Je télécharge tcpdump sur le **router1** à partir de la commande suivante :
```bash
yum install tcpdump
```

#### A. Interception d'ARP
##### 1 : sur router1
```bash
[root@router ~]# tcpdump -i enp0s8 -w ping.pcap
tcpdump: listening on enp0s8, link-type EN10MB (Ethernet), capture size 262144 bytes
```
##### 2 : Sur client1
```bash
[root@client ~]# ip neigh flush all
[root@client ~]# ping -c 4 server1.tp4
PING server1 (10.2.0.10) 56(84) bytes of data.

--- server1 ping statistics ---
4 packets transmitted, 0 received, 100% packet loss, time 3002ms
```
##### 3 : sur router1
```bash
[root@router ~]# tcpdump -i enp0s8 -w ping.pcap
tcpdump: listening on enp0s8, link-type EN10MB (Ethernet), capture size 262144 bytes
^C
16 packets captured
17 packets received by filter
0 packets dropped by kernel
[root@router ~]# ls
anaconda-ks.cfg  ping.pcap
```

#### B. Interception d'une communication
- ```SYN```
```bash
"7","34.700033","10.1.0.10","10.2.0.10","TCP","74","42470 → 8888 [SYN] Seq=0 Win=29200 Len=0 MSS=1460 SACK_PERM=1 TSval=3941926 TSecr=0 WS=64"
```
- ```SYN & ACK```
```bash
"8","34.700244","10.2.0.10","10.1.0.10","TCP","74","8888 → 42470 [SYN, ACK] Seq=0 Ack=1 Win=28960 Len=0 MSS=1460 SACK_PERM=1 TSval=3941300 TSecr=3941926 WS=64"
```
- ```ACK```
```bash
"9","34.700561","10.1.0.10","10.2.0.10","TCP","66","42470 → 8888 [ACK] Seq=1 Ack=1 Win=29248 Len=0 TSval=3941928 TSecr=3941300"
```
- 
```bash
"150","117.403421","10.2.0.10","10.1.0.10","ICMP","102","Destination unreachable (Host administratively prohibited)"
```
#### C. Interception d'un trafic HTTP (BONUS)
```bash
[root@server1 ~]$ sudo systemctl status nginx
nginx.service - The nginx HTTP and reverse proxy server
   Loaded: loaded (/usr/lib/systemd/system/nginx.service; disabled; vendor preset: disabled)
   Active: active (running) since Thu 2019-02-07 18:20:17 CET; 23s ago
  Process: 4105 ExecStart=/usr/sbin/nginx (code=exited, status=0/SUCCESS)
  Process: 4103 ExecStartPre=/usr/sbin/nginx -t (code=exited, status=0/SUCCESS)
  Process: 4102 ExecStartPre=/usr/bin/rm -f /run/nginx.pid (code=exited, status=0/SUCCESS)
 Main PID: 4107 (nginx)
   CGroup: /system.slice/nginx.service
           ├─4107 nginx: master process /usr/sbin/nginx
           └─4108 nginx: worker process

Feb 07 18:20:17 server1.tp4 systemd[1]: Starting The nginx HTTP and reverse ....
Feb 07 31 18:20:17 server1.tp4 nginx[4103]: nginx: the configuration file /etc/...k
 31 18:20:17 server1.tp4 nginx[4103]: nginx: configuration file /etc/ngin...l
Feb 07 18:20:17 server1.tp4 systemd[1]: Failed to read PID from file /run/ng...t
Feb 07 18:20:17 server1.tp4 systemd[1]: Started The nginx HTTP and reverse p....
Hint: Some lines were ellipsized, use -l to show in full.
```