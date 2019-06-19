# TP 2 - Réseau

**I.Exploration locale en solo :**

**1.Affichage d&#39;informations sur la pile TCP/IP locale :**

Afficher les infos des cartes réseaux de notre PC (Windows 10):

**→** ouvrir powershell / invite de commande

**→** IPCONFIG /ALL

INTERFACE WIFI :

- **nom**** :** Intel(R) Dual Band Wireless\_AC 8265

- **adresse physique**** :** 60-F6-77-EF-EE-BC
- **adresse IPv4 :** 10.33.2.77
- **adresse réseau :** 10.33.2.0
- **adresse de broadcast :** 10.33.2.255
- **adresse IP passerelle :** 10.33.3.253

INTERFACE ETHERNET :

- **nom :** Realtek PCIe GBE Family Controller
- **adresse physique :** 18-31-BF-12-94-2D
- **adresse IPv4 :** aucune
- **adresse réseau :** aucune
- **adresse de broadcast :** aucune
- **adresse IP passerelle :** aucune

Trouver l&#39;IP, la MAC (adresse réseau) et la gateway pour l&#39;interface WIFI de notre PC sur interface graphique :

**→** ouvrir le **panneau de configuration** à partir du **menu démarrer**

**→** cliquer sur **réseau et internet**

**→** dans **centre réseau et partage,** cliquer sur **afficher l&#39;état et la gestion du réseau**

**→** dans **wifi** , dans **connexions** cliquer sur **WI-FI**

**→**  cliquer sur **détails…**

**→** Il ne reste plus qu&#39;à lire les infos.

Une passerelle est un système logiciel ou matériel permettant de passer d&#39;un environnement à un autre.

Cela veut dire que la passerelle YNOV sert à relier notre PC au wifi d&#39;YNOV.

**2.Modifications des informations :**

**A.** Modification d&#39;adresse IP - pt. 1

Je calcule la première et la dernière IP disponible :

- **première IP** : 10.33.0.1 ou 10.33.0.0
- **dernière IP** : 10.33.3.254 ou 10.33.3.255

Je change mon adresse IPv4 actuelle pour : 10.33.1.150

**B.** [nmap]

Pour connaître les IP disponibles sur un réseau, il suffit de taper la commande **nmap -sn -PE 10.33.3.253/24** (10.33.3.253/24 étant l'IP du serveur Ynov).

La commande affiche toute les adresses IP utilisés avec le nom du fabriquant.
![image](./capture.PNG)
Elle précise le nombre d'IP total ainsi que le nombre d'IP utilisées.
![image](./capture1.PNG)
Les IP disponibles sontoutes celles qui ne sont pas indiquées.

**II.Exploration locale en duo**

Nous allons maintenant connecter deux ordinateurs avec un câble RJ45. Pour permettre la liaison entre les deux postes nous allons désactiver les pares-feux.

Il faut ensuite modifier les adresses IP en allant dans :
- panneau de configuration,
- réseau et internet
- centre réseau et partage. 
- modifier les paramètres de la carte.

Il nous reste juste à sélectionner la carte réseau appropriée.

On ouvre un invite de commande, on tape **ipconfig /all** pour vérifier que tous les changements ont pris effet. 

Pour tester la connectivité entre les deux machines on va effectuer un **ping ip** ou **ping nom de la machine** avec la machine que l’on veut joindre.

Pour l’utilisation d’une des deux machines comme gateway il faut : 
-    Désactiver l’interface WiFi sur l’un des deux postes
-    S’assurer de la bonne connectivité entre les deux PCs à travers le câble RJ45

On teste la connectivité à internet avec un **ping** dans l’invite de commande.

Pour ce qui est du **ping 8.8.8.8** on ne peut pas l’effectuer car le réseau d’Ynov nous le permet pas.

On a téléchargé **Wireshark**. On a classé les trames par protocoles afin d’observer rien que les **ping**.

![image](./capture2.PNG)