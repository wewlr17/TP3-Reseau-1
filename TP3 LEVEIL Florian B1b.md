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
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE1OTA4Nzc3NDksLTExMTE2MDA4ODcsOD
g2MzM0Nzg3LC0yMDg4NzQ2NjEyLDczMDk5ODExNl19
-->