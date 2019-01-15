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

;; AUTHORITY SECTION:
com.                    60662   IN      NS      h.gtld-servers.net.
com.                    60662   IN      NS      k.gtld-servers.net.
com.                    60662   IN      NS      l.gtld-servers.net.
com.                    60662   IN      NS      f.gtld-servers.net.
com.                    60662   IN      NS      j.gtld-servers.net.
com.                    60662   IN      NS      b.gtld-servers.net.
com.                    60662   IN      NS      i.gtld-servers.net.
com.                    60662   IN      NS      a.gtld-servers.net.
com.                    60662   IN      NS      m.gtld-servers.net.
com.                    60662   IN      NS      g.gtld-servers.net.
com.                    60662   IN      NS      e.gtld-servers.net.
com.                    60662   IN      NS      c.gtld-servers.net.
com.                    60662   IN      NS      d.gtld-servers.net.

;; ADDITIONAL SECTION:
a.gtld-servers.net.     60662   IN      A       192.5.6.30
a.gtld-servers.net.     60662   IN      AAAA    2001:503:a83e::2:30
b.gtld-servers.net.     60662   IN      A       192.33.14.30
b.gtld-servers.net.     60662   IN      AAAA    2001:503:231d::2:30
c.gtld-servers.net.     60662   IN      A       192.26.92.30
c.gtld-servers.net.     60662   IN      AAAA    2001:503:83eb::30
d.gtld-servers.net.     60662   IN      A       192.31.80.30
d.gtld-servers.net.     60662   IN      AAAA    2001:500:856e::30
e.gtld-servers.net.     60662   IN      A       192.12.94.30
e.gtld-servers.net.     60662   IN      AAAA    2001:502:1ca1::30
f.gtld-servers.net.     60662   IN      A       192.35.51.30
f.gtld-servers.net.     60662   IN      AAAA    2001:503:d414::30
g.gtld-servers.net.     60662   IN      A       192.42.93.30
g.gtld-servers.net.     60662   IN      AAAA    2001:503:eea3::30
h.gtld-servers.net.     60662   IN      A       192.54.112.30
h.gtld-servers.net.     60662   IN      AAAA    2001:502:8cc::30
i.gtld-servers.net.     60662   IN      A       192.43.172.30
i.gtld-servers.net.     60662   IN      AAAA    2001:503:39c1::30
j.gtld-servers.net.     60662   IN      A       192.48.79.30
j.gtld-servers.net.     60662   IN      AAAA    2001:502:7094::30
k.gtld-servers.net.     60662   IN      A       192.52.178.30
k.gtld-servers.net.     60662   IN      AAAA    2001:503:d2d::30
l.gtld-servers.net.     60662   IN      A       192.41.162.30
l.gtld-servers.net.     60662   IN      AAAA    2001:500:d937::30
m.gtld-servers.net.     60662   IN      A       192.55.83.30
m.gtld-servers.net.     60662   IN      AAAA    2001:501:b1f9::30

;; Query time: 10 msec
;; SERVER: 10.33.10.20#53(10.33.10.20)
;; WHEN: Tue Jan 15 15:26:33 CET 2019
;; MSG SIZE  rcvd: 849
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

;; AUTHORITY SECTION:
com.                    60588   IN      NS      g.gtld-servers.net.
com.                    60588   IN      NS      c.gtld-servers.net.
com.                    60588   IN      NS      e.gtld-servers.net.
com.                    60588   IN      NS      j.gtld-servers.net.
com.                    60588   IN      NS      l.gtld-servers.net.
com.                    60588   IN      NS      m.gtld-servers.net.
com.                    60588   IN      NS      d.gtld-servers.net.
com.                    60588   IN      NS      i.gtld-servers.net.
com.                    60588   IN      NS      f.gtld-servers.net.
com.                    60588   IN      NS      a.gtld-servers.net.
com.                    60588   IN      NS      k.gtld-servers.net.
com.                    60588   IN      NS      h.gtld-servers.net.
com.                    60588   IN      NS      b.gtld-servers.net.

;; ADDITIONAL SECTION:
a.gtld-servers.net.     60588   IN      A       192.5.6.30
a.gtld-servers.net.     60588   IN      AAAA    2001:503:a83e::2:30
b.gtld-servers.net.     60588   IN      A       192.33.14.30
b.gtld-servers.net.     60588   IN      AAAA    2001:503:231d::2:30
c.gtld-servers.net.     60588   IN      A       192.26.92.30
c.gtld-servers.net.     60588   IN      AAAA    2001:503:83eb::30
d.gtld-servers.net.     60588   IN      A       192.31.80.30
d.gtld-servers.net.     60588   IN      AAAA    2001:500:856e::30
e.gtld-servers.net.     60588   IN      A       192.12.94.30
e.gtld-servers.net.     60588   IN      AAAA    2001:502:1ca1::30
f.gtld-servers.net.     60588   IN      A       192.35.51.30
f.gtld-servers.net.     60588   IN      AAAA    2001:503:d414::30
g.gtld-servers.net.     60588   IN      A       192.42.93.30
g.gtld-servers.net.     60588   IN      AAAA    2001:503:eea3::30
h.gtld-servers.net.     60588   IN      A       192.54.112.30
h.gtld-servers.net.     60588   IN      AAAA    2001:502:8cc::30
i.gtld-servers.net.     60588   IN      A       192.43.172.30
i.gtld-servers.net.     60588   IN      AAAA    2001:503:39c1::30
j.gtld-servers.net.     60588   IN      A       192.48.79.30
j.gtld-servers.net.     60588   IN      AAAA    2001:502:7094::30
k.gtld-servers.net.     60588   IN      A       192.52.178.30
k.gtld-servers.net.     60588   IN      AAAA    2001:503:d2d::30
l.gtld-servers.net.     60588   IN      A       192.41.162.30
l.gtld-servers.net.     60588   IN      AAAA    2001:500:d937::30
m.gtld-servers.net.     60588   IN      A       192.55.83.30
m.gtld-servers.net.     60588   IN      AAAA    2001:501:b1f9::30

;; Query time: 4 msec
;; SERVER: 10.33.10.20#53(10.33.10.20)
;; WHEN: Tue Jan 15 15:27:46 CET 2019
;; MSG SIZE  rcvd: 851
```
# II. Notion de ports et SSH
## 1. Exploration des ports locaux
**Utilisation de la commande SS**
* Pour TCP:
```

```
* Pour Listening:
```

```
**Utilisation des options**
* `-n` :
```

```
* `-p` :
```

```

**Application qui écoute sur le port 22:**
```

```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEzNDM3NzMzNjgsNDY5NDk4NjE5LDg1NT
k5ODI4MSwyMDI0NjMyOTYyLC0xNzIxOTI0MTgzLC0xODQxMDA4
ODg3LC0xMTExNjAwODg3LDg4NjMzNDc4NywtMjA4ODc0NjYxMi
w3MzA5OTgxMTZdfQ==
-->