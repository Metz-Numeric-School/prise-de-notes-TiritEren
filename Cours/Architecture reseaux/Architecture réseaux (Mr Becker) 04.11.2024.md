
172.16.0.0/23

Sales 200
R&D 50 
MGT 25 
IT 10 
Printers 10
Serveur 10


### Étape 1 : Déterminer la taille des sous-réseaux

On doit utiliser une plage d'adresses qui couvre un nombre d'hôtes légèrement supérieur ou égal à chaque besoin.

- Sales : Besoin de 200 adresses ➔ **/24** (256 adresses)
- R&D : Besoin de 50 adresses ➔ **/26** (64 adresses)
- MGT : Besoin de 25 adresses ➔ **/27** (32 adresses)
- IT, Printers, Serveurs : Besoin de 10 adresses chacun ➔ **/28** (16 adresses)

### Étape 2 : Attribution des sous-réseaux à partir de 172.16.0.0/23

Le réseau 172.16.0.0/23 couvre 512 adresses IP, soit de 172.16.0.0 à 172.16.1.255.

- **Sales** : 172.16.0.0/24 ➔ Adresses de 172.16.0.0 à 172.16.0.255
- **R&D** : 172.16.1.0/26 ➔ Adresses de 172.16.1.0 à 172.16.1.63
- **MGT** : 172.16.1.64/27 ➔ Adresses de 172.16.1.64 à 172.16.1.95
- **IT** : 172.16.1.96/28 ➔ Adresses de 172.16.1.96 à 172.16.1.111
- **Printers** : 172.16.1.112/28 ➔ Adresses de 172.16.1.112 à 172.16.1.127
- **Serveurs** : 172.16.1.128/28 ➔ Adresses de 172.16.1.128 à 172.16.1.143

### Résumé du découpage :

1. Sales : 172.16.0.0/24 (256 adresses)
2. R&D : 172.16.1.0/26 (64 adresses)
3. MGT : 172.16.1.64/27 (32 adresses)
4. IT : 172.16.1.96/28 (16 adresses)
5. Printers : 172.16.1.112/28 (16 adresses)
6. Serveurs : 172.16.1.128/28 (16 adresses)
   



# Modèle OSI 
il existe 7 couche du modèle OSI 
![[Pasted image 20241104093221.png]]
![[Pasted image 20241104093411.png]]



7 Application =
6 Présentation = 
5 Session =
4 Transport = FireWall = (Port (16bits)) Segment
3 Réseau = Routeur (IP = IPV4(32bits) et IPV6(128bits)) Paquet/Packet  
2 Liaison de données = Switch (MAC 48 bits ) Trames/Frame
1 Physique = Cable Fibre Ondes électromagnétique Hub 


Le modèle TCP/IP et un modèle a 4 couches 


# Classe de réseaux

il existe 3 classe Principale A B et C et 2 classe moins connu la D et E 


la Classe D et un réseaux Multicast 
   +-----------------+------------------+------------------+-------------------+-------------------+
   | Classe A        | Classe B            | Classe C            | Classe D           | Classe E          |
   | (0.0.0.0/8)      | (128.0.0.0/16)   | (192.0.0.0/24)   | (224.0.0.0/4)     | (240.0.0.0/4)     |
   +-----------------+------------------+------------------+-------------------+-------------------+
        0.0.0.0                      128.0.0.0               192.0.0.0                224.0.0.0           240.0.0.0
          |                                    |                            |                               |                      |
     127.255.255.255    191.255.255.255    223.255.255.255     239.255.255.255     255.255.255.255


![[Pasted image 20241104095751.png]]


### Classes d'adresses IP (IPv4)

Les adresses IP dans IPv4 sont divisées en cinq classes principales : **A, B, C, D et E**. Chaque classe a une plage d'adresses spécifique et est conçue pour des usages différents.

- **Classe A** :
    
    - **Plage d'adresses** : de 0.0.0.0 à 127.255.255.255.
    - **Masque par défaut** : 255.0.0.0 (/8).
    - **Nombre d'hôtes** : plus de 16 millions par réseau.
    - **Usage** : destiné aux très grandes organisations ou réseaux gouvernementaux.
    - **Premier bit** : 0.
    
- **Classe B** :
    
    - **Plage d'adresses** : de 128.0.0.0 à 191.255.255.255.
    - **Masque par défaut** : 255.255.0.0 (/16).
    - **Nombre d'hôtes** : environ 65 536 adresses par réseau.
    - **Usage** : pour les moyennes à grandes entreprises.
    - **Premiers bits** : 10.
    
- **Classe C** :

    
    - **Plage d'adresses** : de 192.0.0.0 à 223.255.255.255.
    - **Masque par défaut** : 255.255.255.0 (/24).
    - **Nombre d'hôtes** : 256 adresses par réseau.
    - **Usage** : pour les petites entreprises ou les réseaux locaux.
    - **Premiers bits** : 110.
    
- **Classe D** (Multicast) :
    
    - **Plage d'adresses** : de 224.0.0.0 à 239.255.255.255.
    - **Usage** : utilisé pour les communications multicast, permettant l'envoi d'un message à plusieurs destinataires simultanément.
    - **Premier bit** : 1110.

- **Classe E** (Réservée) :
    
    - **Plage d'adresses** : de 240.0.0.0 à 255.255.255.255.
    - **Usage** : réservé à des fins expérimentales, non utilisé pour le routage sur Internet.
    - **Premier bit** : 1111.



### Schéma de répartition des classes IP

Les classes sont réparties comme suit :

| **Classe** | **Plage d'adresses**        | **Masque par défaut**     |
| ---------- | --------------------------- | ------------------------- |
| **A**      | 0.0.0.0 à 127.255.255.255   | 255.0.0.0 (/8)            |
| **B**      | 128.0.0.0 à 191.255.255.255 | 255.255.0.0 (/16)         |
| **C**      | 192.0.0.0 à 223.255.255.255 | 255.255.255.0 (/24)       |
| **D**      | 224.0.0.0 à 239.255.255.255 | Multicast (pas de masque) |
| **E**      | 240.0.0.0 à 255.255.255.255 | Réservée (pas de masque)  |

### Types de communication réseau

Dans un réseau, il existe trois principales méthodes d'envoi des paquets de données :

- **Unicast** :
    
    - **Définition** : communication point-à-point. Les paquets sont envoyés à un destinataire unique identifié par une adresse IP.
    - **Exemple** : lorsque vous accédez à un site Web, votre ordinateur communique avec un serveur web via unicast.
    
- **Multicast** :
    
    - **Définition** : communication point-à-multipoint. Un seul paquet est envoyé à plusieurs destinataires appartenant à un groupe multicast spécifique. Cela permet de limiter l'utilisation de la bande passante.
    - **Exemple** : le streaming vidéo en direct, où un serveur envoie un flux vidéo à plusieurs utilisateurs.


### Résumé des usages des classes d'adresses IP et des types de communication :

- **Classes A, B, et C** : utilisées pour l'adressage unicast, pour les réseaux de différentes tailles.
- **Classe D** : utilisée pour le multicast.
- **Classe E** : réservée à des fins expérimentales.
- **Types de communication** : unicast (un à un), multicast (un à plusieurs)



# Découpage des plages d'adresse IPV4
Le réseau 172.16.0.0/23 couvre 512 adresses IP, soit de 172.16.0.0 à 172.16.1.255
![[Pasted image 20241104111232.png]]



**Sales** : 172.16.0.0/24 ➔ Adresses de 172.16.0.0 à 172.16.0.255
**R&D** : 172.16.1.0/26 ➔ Adresses de 172.16.1.0 à 172.16.1.63
**MGT** : 172.16.1.64/27 ➔ Adresses de 172.16.1.64 à 172.16.1.95
 **IT** : 172.16.1.96/28 ➔ Adresses de 172.16.1.96 à 172.16.1.111
 **Printers** : 172.16.1.112/28 ➔ Adresses de 172.16.1.112 à 172.16.1.127
 **Serveurs** : 172.16.1.128/28 ➔ Adresses de 172.16.1.128 à 172.16.1.143

1. Sales : 172.16.0.0/24 (256 adresses)
2. R&D : 172.16.1.0/26 (64 adresses)
3. MGT : 172.16.1.64/27 (32 adresses)
4. IT : 172.16.1.96/28 (16 adresses)
5. Printers : 172.16.1.112/28 (16 adresses)
6. Serveurs : 172.16.1.128/28 (16 adresses)


Adresse privée qui ne change jamais et qui est utiliser en boucle dans tout les réseaux privée 
Classe A = 10.0.0.0/8 = 10.255.255.255 
Classe B = 172.16.0.0 = 172.31.255.255
Classe C = 192.168.0.0 = 192.168.255.255

