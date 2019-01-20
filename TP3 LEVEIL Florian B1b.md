# TP 3 - Plusieurs réseaux : routage statique
# I. Création et utilisation simples d'une VM CentOS
## 4. Configuration réseau d'une machine CentOS

-   _a._  Commande pour prouver que vous avez internet depuis la VM ( Un peu raccourcie ):
```
[root@localhost ~]# curl www.google.com
<!doctype html><html itemscope="" itemtype="http://schema.org/WebPage" lang="fr"><head><meta content="text/html; charset=UTF-8" http-equiv="Content-Type"><m#ccc;height:30px}.lsbb{display:block}.ftl,#fll a{display:inline-blocksaisie\x22,\x22psrc\x22:\x22Cette suggestion a bien t supprime de votre \\u003Ca href\x3d\\\x22/history\\\x22\\u003Ehistorique Web\\u003C/a\\u003E.\x22,\x22psrl\x22:\x22Supprimer\x22,\x22sbit\x22:\x22Recherche par image\x22,\x22srch\x22:\x22Recherche Google\x22},\x22ovr\x22:{},\x22pq\x22:\x22\x22,\x22refpd\x22:true,\x22rfs\x22:[],\x22sbpl\x22:24,\x22sbpr\x22:24,\x22scd\x22:10,\x22sce\x22:5,\x22stok\x22:\x22oi4N-z_nn7qQMJN_PPivto1S3F8\x22,\x22uhde\x22:false}}';google.pmc=JSON.parse(pmc);})();(function(){var r=['aa','async','ipv6','mu','sf'];google.plm(r);})();</script>     </body></html>[root@localhost ~]#
```
-   _b._  Prouvez que votre PC hôte et la VM peuvent communiquer:
Windows vers VM:
```
PS C:\Users\Florian> ping 192.168.127.10

Envoi d’une requête 'Ping'  192.168.127.10 avec 32 octets de données :
Réponse de 192.168.127.10 : octets=32 temps<1ms TTL=64
Réponse de 192.168.127.10 : octets=32 temps<1ms TTL=64
Réponse de 192.168.127.10 : octets=32 temps<1ms TTL=64
Réponse de 192.168.127.10 : octets=32 temps<1ms TTL=64

Statistiques Ping pour 192.168.127.10:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 0ms, Maximum = 0ms, Moyenne = 0ms
```
VM vers Windows:
```
[root@localhost ~]# ping 192.168.127.1
PING 192.168.127.1 (192.168.127.1) 56(84) bytes of data.
64 bytes from 192.168.127.1: icmp_seq=1 ttl=128 time=0.242 ms
64 bytes from 192.168.127.1: icmp_seq=2 ttl=128 time=0.281 ms
64 bytes from 192.168.127.1: icmp_seq=3 ttl=128 time=0.345 ms
64 bytes from 192.168.127.1: icmp_seq=4 ttl=128 time=0.352 ms
64 bytes from 192.168.127.1: icmp_seq=5 ttl=128 time=0.348 ms
64 bytes from 192.168.127.1: icmp_seq=6 ttl=128 time=0.386 ms
64 bytes from 192.168.127.1: icmp_seq=7 ttl=128 time=0.350 ms
^C
--- 192.168.127.1 ping statistics ---
7 packets transmitted, 7 received, 0% packet loss, time 6002ms
rtt min/avg/max/mdev = 0.242/0.329/0.386/0.046 ms
[root@localhost ~]#
```
-   _c._ Table de routage  sur VM:
```
[root@localhost ~]# ip route
default via 10.0.2.2 dev enp0s3 proto dhcp metric 100
10.0.2.0/24 dev enp0s3 proto kernel scope link src 10.0.2.15 metric 100
192.168.127.0/24 dev enp0s8 proto kernel scope link src 192.168.127.10 metric 101
```

## 5. Faire joujou avec quelques commandes

###   Ping
   * ping  hôte -> VM
```
PS C:\Users\Florian> ping 192.168.127.10

Envoi d’une requête 'Ping'  192.168.127.10 avec 32 octets de données :
Réponse de 192.168.127.10 : octets=32 temps<1ms TTL=64
Réponse de 192.168.127.10 : octets=32 temps<1ms TTL=64
Réponse de 192.168.127.10 : octets=32 temps<1ms TTL=64
Réponse de 192.168.127.10 : octets=32 temps<1ms TTL=64

Statistiques Ping pour 192.168.127.10:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 0ms, Maximum = 0ms, Moyenne = 0ms
```
   * ping  VM -> hôte
```
[root@localhost ~]# ping 192.168.127.1
PING 192.168.127.1 (192.168.127.1) 56(84) bytes of data.
64 bytes from 192.168.127.1: icmp_seq=1 ttl=128 time=0.242 ms
64 bytes from 192.168.127.1: icmp_seq=2 ttl=128 time=0.281 ms
64 bytes from 192.168.127.1: icmp_seq=3 ttl=128 time=0.345 ms
64 bytes from 192.168.127.1: icmp_seq=4 ttl=128 time=0.352 ms
64 bytes from 192.168.127.1: icmp_seq=5 ttl=128 time=0.348 ms
64 bytes from 192.168.127.1: icmp_seq=6 ttl=128 time=0.386 ms
64 bytes from 192.168.127.1: icmp_seq=7 ttl=128 time=0.350 ms
^C
--- 192.168.127.1 ping statistics ---
7 packets transmitted, 7 received, 0% packet loss, time 6002ms
rtt min/avg/max/mdev = 0.242/0.329/0.386/0.046 ms
[root@localhost ~]#
```
### Afficher la table de routage
* de l'hôte
```
PS C:\Users\Florian> ROUTE PRINT
===========================================================================
Liste d'Interfaces
  9...d8 cb 8a f3 f1 d5 ......Killer E2400 Gigabit Ethernet Controller
 56...0a 00 27 00 00 38 ......VirtualBox Host-Only Ethernet Adapter #2
  3...9e b6 d0 09 ab 45 ......Microsoft Wi-Fi Direct Virtual Adapter
  4...ae b6 d0 09 ab 45 ......Microsoft Wi-Fi Direct Virtual Adapter #2
 20...9c b6 d0 09 ab 45 ......Killer Wireless-n/a/ac 1535 Wireless Network Adapter
 16...9c b6 d0 09 ab 46 ......Bluetooth Device (Personal Area Network)
  1...........................Software Loopback Interface 1
===========================================================================

IPv4 Table de routage
===========================================================================
Itinéraires actifs :
Destination réseau    Masque réseau  Adr. passerelle   Adr. interface Métrique
          0.0.0.0          0.0.0.0      10.33.3.253      10.33.2.111     35
        10.33.0.0    255.255.252.0         On-link       10.33.2.111    291
      10.33.2.111  255.255.255.255         On-link       10.33.2.111    291
      10.33.3.255  255.255.255.255         On-link       10.33.2.111    291
        127.0.0.0        255.0.0.0         On-link         127.0.0.1    331
        127.0.0.1  255.255.255.255         On-link         127.0.0.1    331
  127.255.255.255  255.255.255.255         On-link         127.0.0.1    331
    192.168.127.0    255.255.255.0         On-link     192.168.127.1    281
    192.168.127.1  255.255.255.255         On-link     192.168.127.1    281
  192.168.127.255  255.255.255.255         On-link     192.168.127.1    281
        224.0.0.0        240.0.0.0         On-link         127.0.0.1    331
        224.0.0.0        240.0.0.0         On-link       10.33.2.111    291
        224.0.0.0        240.0.0.0         On-link     192.168.127.1    281
  255.255.255.255  255.255.255.255         On-link         127.0.0.1    331
  255.255.255.255  255.255.255.255         On-link       10.33.2.111    291
  255.255.255.255  255.255.255.255         On-link     192.168.127.1    281
===========================================================================
Itinéraires persistants :
  Aucun

IPv6 Table de routage
===========================================================================
Itinéraires actifs :
 If Metric Network Destination      Gateway
  1    331 ::1/128                  On-link
 20    291 fe80::/64                On-link
 56    281 fe80::/64                On-link
 56    281 fe80::41c5:8215:30d8:c93c/128
                                    On-link
 20    291 fe80::5c55:b0b4:ada6:95d9/128
                                    On-link
  1    331 ff00::/8                 On-link
 20    291 ff00::/8                 On-link
 56    281 ff00::/8                 On-link
===========================================================================
Itinéraires persistants :
  Aucun

```
* de la VM
```
[florian@localhost ~]$ ip route
default via 10.0.2.2 dev enp0s3 proto dhcp metric 100
10.0.2.0/24 dev enp0s3 proto kernel scope link src 10.0.2.15 metric 100
192.168.127.0/24 dev enp0s8 proto kernel scope link src 192.168.127.10 metric 101
```
**Mettre en évidence la ligne qui leur permet de discuter:**
* de l'hôte
```
192.168.127.0    255.255.255.0         On-link     192.168.127.1    281
```
* de la VM
```
192.168.127.0/24 dev enp0s8 proto kernel scope link src 192.168.127.10 metric 101
```
**Depuis la VM utilisez  `curl`  (ou  `wget`) pour télécharger un fichier sur internet:**
```
[florian@localhost ~]$ wget www.google.com
--2019-01-15 15:23:58--  http://www.google.com/
Resolving www.google.com (www.google.com)... 216.58.209.228, 2a00:1450:4007:80f::2004
Connecting to www.google.com (www.google.com)|216.58.209.228|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: unspecified [text/html]
Saving to: ‘index.html’

    [ <=>                                                                           ] 11,326      --.-K/s   in 0.004s

2019-01-15 15:23:58 (2.50 MB/s) - ‘index.html’ saved [11326]
```
 **Depuis la VM utilisez  `dig`  pour connaître l'IP de :**
 * ynov.com:
```
[florian@localhost ~]$ dig ynov.com

; <<>> DiG 9.9.4-RedHat-9.9.4-72.el7 <<>> ynov.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 4405
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 13, ADDITIONAL: 27

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;ynov.com.                      IN      A

;; ANSWER SECTION:
ynov.com.               458     IN      A       217.70.184.38
```
* google.com:
```
[florian@localhost ~]$ dig google.com

; <<>> DiG 9.9.4-RedHat-9.9.4-72.el7 <<>> google.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 34008
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 13, ADDITIONAL: 27

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;google.com.                    IN      A

;; ANSWER SECTION:
google.com.             206     IN      A       216.58.204.142
```
# II. Notion de ports et SSH
## 1. Exploration des ports locaux
**Utilisation de la commande SS**
* Pour TCP:
```
[florian@localhost ~]$ ss -t
State      Recv-Q Send-Q             Local Address:Port                              Peer Address:Port
ESTAB      0      0                 192.168.127.10:ssh                              192.168.127.1:capioverlan
```
* Pour Listening/TCP:
```
[florian@localhost ~]$ ss -tl
State      Recv-Q Send-Q             Local Address:Port                              Peer Address:Port
LISTEN     0      128                            *:ssh                                          *:*
LISTEN     0      100                    127.0.0.1:smtp                                         *:*
LISTEN     0      128                           :::ssh                                         :::*
LISTEN     0      100                          ::1:smtp                                        :::*
```
**Utilisation des options**
* `-n` :
```
[florian@localhost ~]$ ss -tn
State       Recv-Q Send-Q              Local Address:Port                             Peer Address:Port
ESTAB       0      0                  192.168.127.10:22                              192.168.127.1:1147
```
* `-p` :
```
[florian@localhost ~]$ sudo ss -tp
[sudo] password for florian:
State       Recv-Q Send-Q                                                                                    Local Address:Port                                                                                                     Peer Address:Port
ESTAB       0      0                                                                                        192.168.127.10:ssh                                                                                                     192.168.127.1:capioverlan           
users:(("sshd",pid=3475,fd=3),("sshd",pid=3471,fd=3))
```

**Application qui écoute sur le port 22:**
```
[florian@localhost ~]$ sudo ss -tpn
State       Recv-Q Send-Q                                                                                      Local Address:Port                                                                                                     Peer Address:Port
ESTAB       0      0                                                                                          192.168.127.10:22                                                                                                      192.168.127.1:1147              
users:(("sshd",pid=3475,fd=3),("sshd",pid=3471,fd=3))
```
## 2. SSH
**Connexion au SSH:** 
```
PS C:\Users\Florian> ssh florian@192.168.127.10
florian@192.168.127.10's password:
Last login: Tue Jan 15 15:05:07 2019 from 192.168.127.1
Last login: Tue Jan 15 15:05:07 2019 from 192.168.127.1
[florian@localhost ~]$
```
## Firewall
 **A. SSH**  :
* Changement du numéro du port sur lequel le serveur SSH écoute:
```
[florian@localhost ~]$ sudo firewall-cmd --add-port=2222/tcp --permanent
success
```
* Redémarrage du serveur SSH pour que le changement prenne effet:
```
[florian@localhost ~]$ sudo firewall-cmd --reload
success
```
* Le serveur SSH écoute sur un port différent de  `22` :
```
[florian@localhost ~]$ ss -lnt
State      Recv-Q Send-Q                   Local Address:Port                                  Peer Address:Port
LISTEN     0      128                                  *:2222                                             *:*
LISTEN     0      100                          127.0.0.1:25                                               *:*
LISTEN     0      128                                 :::2222                                            :::*
LISTEN     0      100                                ::1:25                                              :::*
```
* connectez-vous au serveur en utilisant ce port
```
PS C:\Users\Florian> ssh florian@192.168.127.10 -p 2222
florian@192.168.127.10's password:
Last login: Tue Jan 15 17:06:33 2019 from 192.168.127.1
Last login: Tue Jan 15 17:06:33 2019 from 192.168.127.1
```
 * pourquoi ça a échoué ?
   * Par ce que le port n'a pas été autorisé par le firewall
 * Solution:
 ```
[florian@localhost ~]$ sudo firewall-cmd --add-port=2222/tcp --permanent
success
[florian@localhost ~]$ sudo firewall-cmd --reload
success
```
**B.  `netcat`**
 **Dans un premier terminal**
 * Lancement du serveur  `netcat`:
 ```
[florian@localhost ~]$ nc -l 5454

```
* Ecoute du port  `5454`  en TCP:
 ```
[florian@localhost ~]$ nc -l 5454
```
* Autorisation de ce port dans le firewall:
 ```
[florian@localhost ~]$ sudo firewall-cmd --add-port=5454/tcp --permanent
success
[florian@localhost ~]$ sudo firewall-cmd --reload
success
```

**Dans un deuxième terminal**
* Connexion au serveur  `netcat`:
 ```
PS C:\Users\Florian\Desktop\netcat-1.11> ./nc 192.168.127.10 5454
Teste
Trop bien
ça marche
```

**Dans un troisième terminal**
* Visualisation de la connexion  `netcat`  en cours:
 ```
[florian@localhost ~]$ ss -atnp4
State      Recv-Q Send-Q               Local Address:Port                              Peer Address:Port
LISTEN     0      128                              *:2222                                         *:*
LISTEN     0      100                      127.0.0.1:25                                           *:*
ESTAB      0      0                   192.168.127.10:5454                             192.168.127.1:1118                users:(("nc",pid=4521,fd=5))
ESTAB      0      0                   192.168.127.10:2222                             192.168.127.1:1138
ESTAB      0      0                   192.168.127.10:2222                             192.168.127.1:1117
```

# III. Routage statique
## 1. Préparation des hôtes (vos PCs)
### Préparation avec câble
**Toutes les ip :**
* ip PC1: 192.168.112.1
* ip PC2: 192.168.112.2
* ip VM1: 192.168.101.10
* ip VM2: 192.168.102.10

Ping PC2(moi) vers PC1(Gabin):
```
PS C:\Users\Florian> ping 192.168.112.1

Envoi d’une requête 'Ping'  192.168.112.1 avec 32 octets de données :
Réponse de 192.168.112.1 : octets=32 temps<1ms TTL=128
Réponse de 192.168.112.1 : octets=32 temps<1ms TTL=128
Réponse de 192.168.112.1 : octets=32 temps<1ms TTL=128
Réponse de 192.168.112.1 : octets=32 temps<1ms TTL=128

Statistiques Ping pour 192.168.112.1:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 0ms, Maximum = 0ms, Moyenne = 0ms
PS C:\Users\Florian>
```
Ipconfig:
```
Configuration IP de Windows


Carte Ethernet Ethernet :

   Suffixe DNS propre à la connexion. . . :
   Adresse IPv6 de liaison locale. . . . .: fe80::70de:8f61:ddbb:d1fc%9
   Adresse IPv4. . . . . . . . . . . . . .: 192.168.112.2
   Masque de sous-réseau. . . . . . . . . : 255.255.255.252
   Passerelle par défaut. . . . . . . . . :
```
### Check
VM2 vers PC1:
```
[florian@localhost ~]$ ping 192.168.112.1
PING 192.168.112.1 (192.168.112.1) 56(84) bytes of data.
64 bytes from 192.168.112.1: icmp_seq=1 ttl=127 time=0.956 ms
64 bytes from 192.168.112.1: icmp_seq=2 ttl=127 time=1.33 ms
64 bytes from 192.168.112.1: icmp_seq=3 ttl=127 time=1.27 ms
64 bytes from 192.168.112.1: icmp_seq=4 ttl=127 time=0.861 ms
64 bytes from 192.168.112.1: icmp_seq=5 ttl=127 time=1.29 ms
64 bytes from 192.168.112.1: icmp_seq=6 ttl=127 time=1.14 ms
64 bytes from 192.168.112.1: icmp_seq=7 ttl=127 time=0.993 ms
64 bytes from 192.168.112.1: icmp_seq=8 ttl=127 time=0.913 ms
```

-   `ip route`  sur les Linux ou MacOS
```
[florian@localhost ~]$ ip route
default via 10.0.2.2 dev enp0s3 proto dhcp metric 100
10.0.2.0/24 dev enp0s3 proto kernel scope link src 10.0.2.15 metric 100
192.168.102.0/24 dev enp0s8 proto kernel scope link src 192.168.102.10 metric 101
[florian@localhost ~]$
```
-   `route print -4`  sur Windows
```
PS C:\Users\Florian> route print -4
===========================================================================
Liste d'Interfaces
  9...d8 cb 8a f3 f1 d5 ......Killer E2400 Gigabit Ethernet Controller
 12...0a 00 27 00 00 0c ......VirtualBox Host-Only Ethernet Adapter #2
  3...9e b6 d0 09 ab 45 ......Microsoft Wi-Fi Direct Virtual Adapter
  4...ae b6 d0 09 ab 45 ......Microsoft Wi-Fi Direct Virtual Adapter #2
 20...16 c2 13 9a 27 ed ......Apple Mobile Device Ethernet
 21...9c b6 d0 09 ab 45 ......Killer Wireless-n/a/ac 1535 Wireless Network Adapter
 17...9c b6 d0 09 ab 46 ......Bluetooth Device (Personal Area Network)
  1...........................Software Loopback Interface 1
===========================================================================

IPv4 Table de routage
===========================================================================
Itinéraires actifs :
Destination réseau    Masque réseau  Adr. passerelle   Adr. interface Métrique
          0.0.0.0          0.0.0.0      172.20.10.1      172.20.10.2     50
          0.0.0.0          0.0.0.0      172.20.10.1      172.20.10.3     35
        127.0.0.0        255.0.0.0         On-link         127.0.0.1    331
        127.0.0.1  255.255.255.255         On-link         127.0.0.1    331
  127.255.255.255  255.255.255.255         On-link         127.0.0.1    331
      172.20.10.0  255.255.255.240         On-link       172.20.10.2    306
      172.20.10.0  255.255.255.240         On-link       172.20.10.3    291
      172.20.10.2  255.255.255.255         On-link       172.20.10.2    306
      172.20.10.3  255.255.255.255         On-link       172.20.10.3    291
     172.20.10.15  255.255.255.255         On-link       172.20.10.2    306
     172.20.10.15  255.255.255.255         On-link       172.20.10.3    291
    192.168.102.0    255.255.255.0         On-link     192.168.102.2    281
    192.168.102.2  255.255.255.255         On-link     192.168.102.2    281
  192.168.102.255  255.255.255.255         On-link     192.168.102.2    281
    192.168.112.0  255.255.255.252         On-link     192.168.112.2    281
    192.168.112.2  255.255.255.255         On-link     192.168.112.2    281
    192.168.112.3  255.255.255.255         On-link     192.168.112.2    281
        224.0.0.0        240.0.0.0         On-link         127.0.0.1    331
        224.0.0.0        240.0.0.0         On-link     192.168.102.2    281
        224.0.0.0        240.0.0.0         On-link     192.168.112.2    281
        224.0.0.0        240.0.0.0         On-link       172.20.10.2    306
        224.0.0.0        240.0.0.0         On-link       172.20.10.3    291
  255.255.255.255  255.255.255.255         On-link         127.0.0.1    331
  255.255.255.255  255.255.255.255         On-link     192.168.102.2    281
  255.255.255.255  255.255.255.255         On-link     192.168.112.2    281
  255.255.255.255  255.255.255.255         On-link       172.20.10.2    306
  255.255.255.255  255.255.255.255         On-link       172.20.10.3    291
===========================================================================
Itinéraires persistants :
  Aucun
```

## 2. Configuration du routage

Commande pour les routes:
`ip route add 192.168.101.0/24 mask 255.255.255.0 192.168.112.1`
`ip route add 192.168.101.0/24 via 192.168.101.10 dev enp0s8`

`ip route add 192.168.101.0/24 mask 255.255.255.0 192.168.112.2`
`ip route add 192.168.101.0/24 via 192.168.102.10 dev enp0s8`

PC2 > PC1:
```
PS C:\WINDOWS\system32> ping 192.168.112.1

Envoi d’une requête 'Ping'  192.168.112.1 avec 32 octets de données :
Réponse de 192.168.112.1 : octets=32 temps<1ms TTL=128
Réponse de 192.168.112.1 : octets=32 temps<1ms TTL=128
Réponse de 192.168.112.1 : octets=32 temps<1ms TTL=128
Réponse de 192.168.112.1 : octets=32 temps<1ms TTL=128

Statistiques Ping pour 192.168.112.1:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 0ms, Maximum = 0ms, Moyenne = 0ms
```

PC2 > VM1:

```
PS C:\WINDOWS\system32> ping 192.168.101.10

Envoi d’une requête 'Ping'  192.168.101.10 avec 32 octets de données :
Réponse de 192.168.101.10 : octets=32 temps<1ms TTL=128
Réponse de 192.168.101.10 : octets=32 temps<1ms TTL=128
Réponse de 192.168.101.10 : octets=32 temps<1ms TTL=128
Réponse de 192.168.101.10 : octets=32 temps<1ms TTL=128

Statistiques Ping pour 192.168.101.10:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 0ms, Maximum = 0ms, Moyenne = 0ms
```
## 3. Configuration des noms de domaine


VM2 > PC2
```
ping pc2.tp3.b1
PING pc2 (192.168.102.1) 56(84) bytes of data.
64 bytes from pc2 (192.168.102.1): icmp_seq=1 ttl=128 time=0.326 ms

```
**VM2 > PC2**

```
ping pc1.tp3.b1
PING pc1 (192.168.112.1) 56(84) bytes of data.
64 bytes from pc1 (192.168.112.1): icmp_seq=1 ttl=127 time=5.72 ms

```

```
ping vm1.tp3.b1
PING vm1 (192.168.101.10) 56(84) bytes of data.
64 bytes from vm1 (192.168.101.10): icmp_seq=1 ttl=62 time=5.04 ms

```

**Résultats des ping depuis le PC :**

```
ping vm2.tp3.b1

Envoi d’une requête 'ping' sur vm2.tp3.b1 [192.168.102.10] avec 32 octets de données :
Réponse de 192.168.102.10 : octets=32 temps<1ms TTL=64

```

```
ping pc1.tp3.b1

Envoi d’une requête 'ping' sur pc1.tp3.b1 [192.168.112.1] avec 32 octets de données :
Réponse de 192.168.112.1 : octets=32 temps=4 ms TTL=128

```

```
ping vm1.tp3.b1

Envoi d’une requête 'ping' sur vm1.tp3.b1 [192.168.101.10] avec 32 octets de données :
Réponse de 192.168.101.10 : octets=32 temps=4 ms TTL=63
```


<!--stackedit_data:
eyJoaXN0b3J5IjpbMTE5MjAxMDM5NiwtMTAyNTIwMzE5LC01MT
M4NzI2NTksMTk5OTY1ODIxOSwtNTE3MzEwNjAwLDE3NjkwNTkz
NjAsLTQ5MDQwOTIxMSwzMzU2OSwxOTAyNzkyMjc5LDIxMTU4Mz
Q2MTQsLTU1NjQ4MjkxNSw1MDY1NDUzMzIsODA1MzY5MzcxLDM5
MzgxMTAzMiwxNzE3NTg3MDgyLDE0Mjg2ODUyMywxMjMyOTMwNj
IyLC0xMzQzNzczMzY4LDQ2OTQ5ODYxOSw4NTU5OTgyODFdfQ==

-->