# Installation et configuration de Bind9

Cette documentation sur l'installation et la configuration d'un service DNS utilisant le paquet Bind9 sur Rasbian.

## Installation de Bind9

Commande d'installation de Bind9:</br>
``apt-get update``</br>
``apt-get install bind9``</br>

## Configuration de Bind9

Localisation des fichiers de configuration de Bind9: /etc/bind/

Pour commencer nous devons créer une zone DNS.
Dans notre exemple la zone DNS sera example.local.</br>
1- Création de la zone DNS</br>
``nano /etc/bind/named.conf.local``</br>

Voici la configuration à ajouter à la fin du fichier named.conf.local:
```
zone "example.local." {
        type master;
        file "/etc/bind/db.example.local";
};
```

Le type master indique que le serveur DNS et maitre il fait donc autorité sur la zone example.local. File permet de specifier la localisation du fichier pour définir les noms comme par exemple cam1.example.local.

nano /etc/bind/named.conf.options

Voici la configuration à décommenter 
forwarders {
    1.1.1.1;
};

2- Création des péripheriques

nano /etc/bind/db.example.local

Voici la configuration à ajouter dans le fichier db.example.local

; BIND reverse data file for empty rfc1918 zone
;
; DO NOT EDIT THIS FILE - it is used for multiple zones.
; Instead, copy it, edit named.conf, and use that copy.
;
$TTL    86400
example.local.    IN      SOA     ns.example.local. root.example.local. (
                              1         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                          86400 )       ; Negative Cache TTL
;
example.local.    IN      NS      ns.example.local.
ns.example.local. IN      A       127.0.0.1

cam1.example.local. IN      A       192.168.1.253
cam2.example.local. IN      A       192.168.1.252
cam3.example.local. IN      A       192.168.1.251

3- Reboot du serveur Bind 

systemctl restart bind9 Ou service bind9 restart
systemctl status bind9 Ou service bind9 status
