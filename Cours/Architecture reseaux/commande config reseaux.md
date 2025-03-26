
Déterminer l'adresse réseau, l'adresse de broadcast et la passerelle pour l'adresse IP `168.192.37.33` avec un masque de `/12`.

1. **Adresse réseau** :
    
    - **Adresse IP** : `168.192.37.33`
        
    - **Masque de sous-réseau** : `/12` (qui correspond à `255.240.0.0` en notation décimale)
        
    
    Pour trouver l'adresse réseau, nous effectuons une opération AND bit à bit entre l'adresse IP et le masque de sous-réseau :
    
    - Adresse IP (binaire) : `10101000.11000000.00100101.00100001`
        
    - Masque de sous-réseau (binaire) : `11111111.11110000.00000000.00000000`
        
    - Adresse réseau (binaire) : `10101000.11000000.00000000.00000000` (ou `168.192.0.0` en décimal)
        
    
    **Adresse réseau** : `168.192.0.0`
    
2. **Adresse de broadcast** :
    
    - Pour trouver l'adresse de broadcast, nous mettons tous les bits de l'hôte à 1 dans l'adresse réseau :
        
    - Adresse réseau (binaire) : `10101000.11000000.00000000.00000000`
        
    - Adresse de broadcast (binaire) : `10101000.11001111.11111111.11111111` (ou `168.207.255.255` en décimal)
        
    
    **Adresse de broadcast** : `168.207.255.255`
    
3. **Passerelle par défaut** :
    
    - La passerelle par défaut est généralement la première adresse disponible dans le sous-réseau. Pour `168.192.0.0`, cela serait `168.192.0.1`.
        
    
    **Passerelle par défaut** : `168.192.0.1`
    

Voici un résumé :

|**Paramètre**|**Valeur**|
|---|---|
|**Adresse réseau**|`168.192.0.0`|
|**Adresse de broadcast**|`168.207.255.255`|
|**Passerelle par défaut**|`168.192.0.1`|

168.192.0.2 - 168.207.255.254

CLass A /8 16M (16.777.216)
Class B /12 1M (1.048.576)
Class C /16  65k (65.536)

192.168.0.0/24 => 255 poste
192.168.0.1 -> 192.168.0.254 
BC : 192.168.0.255 (dernière ip du réseau)
GW : 192.168.0.1 || 192.168.0.254 (première ou dernière de la plage dispo)
poste dispo 192.168.0.1 -> 192.168.0.254


# Config réseau

## Définir les VLAN dans les switch
	   
```
en
conf t
vlan X
name NAME
exit
```

## Attribuer les interface au vlan

```
en
conf t
int range eth X-X
switchport mode access
switch access vlan X
no shut
exit
```

## définir des sous réseau dans le routeur

```
en
conf t
int eth X/X.X
encaps dot1Q X
ip add gateway mask 
no shut
exit
```

Sauvegarder les configurations
```
wr
```


# VTP

## switch server

```
Switch> enable
Switch# configure terminal
Switch(config)# vtp domain NOM_DU_DOMAINE
Switch(config)# vtp mode server
Switch(config)# vtp password MOT_DE_PASSE
```

## switch client

```
Switch> enable
Switch# configure terminal
Switch(config)# vtp domain NOM_DU_DOMAINE
Switch(config)# vtp mode client
Switch(config)# vtp password MOT_DE_PASSE
Switch(config)# end
Switch# wr
```

# Interface -> VLAN

```
en
conf t
int range int_start-int_end
sw mode acc
sw acc vlan X
end
wr
```

# interface vlan 

- Après avoir Créer les VLAN dans le switch L3
- Après avoir set les mode trunk entre les switchs
- *(partager avec le vtp les config vlan)*
- Après avoir set les mode access dans les switch L2 vers les end point
dans le switch layer 3

```
en
conf t
int vlan X
ip address gateway mask
no shut
end
wr
```

# Router Config

attribuer une IP à une interface
```
en
conf t
int e0/0
ip add IP MASK
no shut
end
wr
```

définir une route par défaut
```
en
conf t
ip route 0.0.0.0 0.0.0.0 GATEWAY
end
wr
```

# Router & L3

il faut toujours un réseau propre/personnel entre un router et switch L3

pour mettre une ip sur une interface dans un switch 

```
en
conf t
int INT
no sw
ip add IP MASK
no shut
end
wr
```

# nat un routeur
1. 
```
interface [interface_inside]
ip nat inside
exit
interface [interface_outside]
ip nat outside
exit
```

2. Créer les access list
```
access-list [ACL] permit [adresse_ip_reseau]
```

3. 
```
ip nat inside source list [ACL] interface [interface_outside] overload
```

example result expected :
![[Pasted image 20250115110303.png]]
![[Pasted image 20250115110343.png]]

# Interface en dhcp (routeur vers Ext.)

```
en
conf t
int INT
ip add dhcp
no shut
end
wr
```


# lier VM avec Pnet

1. clique droit -> Network 
2. choisir le Cloud correspondant au VMnet Host-only
3. mettre une icon de server/pc/cloud comme on souhaite
4. connecter au réseau (à un switch routeur etc selon le besoin)
5. Créer une vm (windows/linux)
6. La carte réseau attribué doit être le VMnet host-only (VMnet1 souvent)
7. dans la VM, définir l'IP de la machine en fonction de la répartition (ex: le server sont sur la plage .130 à .142)
8. mettre la gateway
9. mettre le dns (google par exemple)
10.  établir une regle de pare feu entrant (autoriser les entrées)
11. redémarrer l'ordi le réseau devrait être 





# explication vidéos pour la sécurisation 

### Partie 1 : Configuration de l'accès au routeur via Telnet et SSH

1. **Bannière de message du jour (MOTD)** : Tu as commencé par configurer une bannière de message du jour sur le routeur avec la commande `banner motd`. Cette bannière affiche un message aux utilisateurs avant qu'ils ne tentent de se connecter au routeur, les avertissant de ne pas essayer d'accéder au système sans autorisation.
    
2. **Configuration de l'accès via Telnet** :
    
    - **Telnet** est un protocole qui permet l'accès distant au routeur. Cependant, il a un gros défaut : les données (y compris les mots de passe) sont envoyées en texte clair, ce qui signifie qu'une personne malveillante pourrait capturer ces informations avec un outil comme Wireshark.
    - Tu as configuré l'accès Telnet en accédant aux lignes **vty** du routeur et en spécifiant d'utiliser les identifiants locaux (`login local`) déjà configurés. Cependant, cette méthode n’est pas sécurisée car Telnet ne chiffre pas les données.
3. **Problème de sécurité avec Telnet** : Tu as souligné qu'utiliser Telnet est risqué car les informations d'identification sont envoyées en clair, ce qui rend le réseau vulnérable à une attaque par interception. Pour résoudre ce problème, tu as ensuite configuré **SSH**, qui offre une connexion sécurisée en chiffrant les données échangées.
    
4. **Configuration de l'accès SSH** :
    
    - **SSH (Secure Shell)** est un protocole qui permet d'accéder au routeur de manière sécurisée. Contrairement à Telnet, il chiffre les communications, rendant difficile l'interception des informations.
    - Tu as généré des clés cryptographiques RSA pour SSH. Le routeur a besoin d'un nom d'hôte non par défaut et d'un domaine IP pour créer ces clés.
    - Après avoir configuré le nom de domaine et généré les clés RSA, tu as modifié les lignes vty pour spécifier que l'accès ne doit se faire que par **SSH** en utilisant la commande `transport input ssh`.
    - Finalement, tu as testé l'accès SSH depuis un ordinateur distant, ce qui a confirmé que la connexion était sécurisée.

### Partie 2 : Configuration de la sécurité des ports du switch

1. **Introduction au Port Security** :
    
    - Tu es ensuite passé à la configuration du switch. Tu as travaillé sur l'interface **FastEthernet 03** et tu as activé une fonctionnalité appelée **Port Security**. Cela permet de restreindre l'accès au réseau aux appareils ayant des adresses MAC autorisées.
2. **Configuration du Port Security avec l'option sticky** :
    
    - En activant la sécurité de port avec l'option **sticky**, le switch apprend automatiquement l'adresse MAC du premier appareil connecté à ce port et l'autorise.
    - Tu as configuré le nombre maximum d'adresses MAC autorisées à **1**, ce qui signifie que seul l'appareil avec l'adresse MAC apprise sera autorisé à utiliser ce port.
3. **Modes de violation** :
    
    - Tu as configuré le mode de violation à **shutdown**, ce qui signifie que si un appareil non autorisé essaie de se connecter, le port sera automatiquement désactivé, et il faudra manuellement le réactiver avec la commande `no shutdown`.
    - Il existe d'autres modes de violation comme **restrict** (qui log les violations sans désactiver le port) et **protect** (qui ne log pas les violations et se contente de bloquer les adresses non autorisées).
4. **Test de la sécurité du port** :
    
    - Tu as connecté un PC légitime au port et le switch a appris son adresse MAC. Ensuite, tu as simulé l'utilisation d'un appareil rogue en débranchant le PC légitime et en branchant un autre PC. Comme le switch avait déjà appris l'adresse MAC du premier PC, il a immédiatement désactivé le port dès qu'il a détecté l'adresse MAC non autorisée.

### Résumé des deux parties :

- **Partie 1** : Tu as configuré l'accès au routeur à distance en utilisant SSH, un protocole sécurisé, et désactivé l'accès via Telnet, qui n'est pas sécurisé.
- **Partie 2** : Tu as sécurisé les ports du switch en autorisant seulement les appareils avec des adresses MAC spécifiques à se connecter, ce qui empêche les appareils non autorisés d'accéder au réseau.



# Commande et configuration 

### 1. **Configurer une bannière de message du jour (MOTD)**

La bannière MOTD est une manière d'afficher un message aux utilisateurs avant qu'ils ne se connectent à l'appareil. C'est utile pour afficher des avertissements de sécurité. Voici comment configurer une bannière :



`Router(config)# banner motd #Attention : Accès interdit aux utilisateurs non autorisés.#`

Cela affiche le message d'avertissement aux utilisateurs qui tentent d'accéder au routeur.

### 2. **Configurer l'accès Telnet**

Telnet permet d'accéder à distance au routeur, mais n'est pas sécurisé car il envoie les informations en texte clair. Voici comment configurer l'accès Telnet :

#### a. Activer l'accès aux lignes vty (lignes virtuelles) pour Telnet


`Router(config)# line vty 0 4 Router(config-line)# password cisco Router(config-line)# login`

Cela définit un mot de passe pour l'accès Telnet et demande aux utilisateurs de s'authentifier.

#### b. Activer les identifiants locaux

Tu peux aussi utiliser des identifiants locaux déjà créés :



`Router(config)# username admin privilege 15 secret cisco123 Router(config)# line vty 0 4 Router(config-line)# login local`

Cela permet d'utiliser un nom d'utilisateur (ici **admin**) et un mot de passe (**cisco123**) pour se connecter via Telnet.

#### c. Permettre uniquement Telnet sur les lignes vty



`Router(config-line)# transport input telnet`

### 3. **Configurer l'accès SSH**

Contrairement à Telnet, SSH chiffre toutes les communications et est beaucoup plus sécurisé. Voici comment configurer SSH :

#### a. Configurer le nom d'hôte et le domaine pour générer les clés SSH



`Router(config)# hostname MyRouter MyRouter(config)# ip domain-name example.com`

#### b. Générer les clés RSA pour SSH



`MyRouter(config)# crypto key generate rsa`

Il te sera demandé de spécifier la taille des clés. Par exemple, 1024 bits est un bon choix pour la sécurité :



`How many bits in the modulus [512]: 1024`

#### c. Activer SSH sur les lignes vty

Après avoir généré les clés, tu dois configurer les lignes vty pour autoriser uniquement SSH :



`MyRouter(config)# line vty 0 4 MyRouter(config-line)# transport input ssh`

#### d. Vérifier la version SSH (optionnel)

Tu peux vérifier que SSH est activé et fonctionner correctement en vérifiant la version SSH :



`MyRouter# show ip ssh`

### 4. **Configurer la sécurité des ports sur un switch**

La sécurité des ports sur un switch permet de limiter les adresses MAC autorisées à se connecter à chaque port. Voici comment la configurer sur un switch :

#### a. Accéder à l'interface du port

Tu dois d'abord entrer dans la configuration de l'interface sur laquelle tu veux activer la sécurité des ports. Par exemple, pour l'interface **FastEthernet0/3** :


`Switch(config)# interface fastethernet 0/3`

#### b. Activer la sécurité des ports

Une fois dans l'interface, active la sécurité des ports :



`Switch(config-if)# switchport port-security`

#### c. Limiter le nombre d'adresses MAC autorisées

Ensuite, tu peux définir le nombre d'adresses MAC autorisées sur ce port. Par exemple, pour limiter à une seule adresse MAC :



`Switch(config-if)# switchport port-security maximum 1`

#### d. Utiliser l'option sticky

L'option **sticky** permet au switch d'apprendre dynamiquement l'adresse MAC du premier appareil connecté et de l'ajouter à la configuration de manière permanente :



`Switch(config-if)# switchport port-security mac-address sticky`

#### e. Configurer l'action en cas de violation

Si un appareil non autorisé tente de se connecter au port, tu peux définir une action à prendre. Le mode **shutdown** désactive le port automatiquement en cas de violation :



`Switch(config-if)# switchport port-security violation shutdown`

#### f. Réactiver le port après une violation

Si une violation a eu lieu et que le port a été désactivé, tu peux le réactiver avec la commande suivante :


`Switch(config)# interface fastethernet 0/3 Switch(config-if)# shutdown Switch(config-if)# no shutdown`

### 5. **Vérifier la configuration**

#### a. Vérifier la configuration SSH

Pour vérifier que SSH est correctement configuré sur le routeur :



`MyRouter# show ip ssh`

#### b. Vérifier la configuration de la sécurité des ports

Pour vérifier la configuration de la sécurité des ports sur le switch :



`Switch# show port-security interface fastethernet 0/3`

Cela te montrera les adresses MAC apprises et le statut du port.

### Résumé des commandes utilisées :

- **Routeur : Telnet**
    - `line vty 0 4`
    - `password [mot_de_passe]`
    - `login`
    - `transport input telnet`
- **Routeur : SSH**
    - `hostname [nom_du_routeur]`
    - `ip domain-name [nom_de_domaine]`
    - `crypto key generate rsa`
    - `line vty 0 4`
    - `transport input ssh`
- **Switch : Sécurité des ports**
    - `interface fastethernet [port]`
    - `switchport port-security`
    - `switchport port-security maximum [nombre]`
    - `switchport port-security mac-address sticky`
    - `switchport port-security violation [shutdown | restrict | protect]`










# ACL packet tracer



### Objectifs :

- **Partie 1** : Tester la connectivité locale et vérifier l'effet d'une liste de contrôle d'accès (ACL) sur la capacité d'un dispositif à atteindre des hôtes sur des réseaux distants.
- **Partie 2** : Supprimer l'ACL et vérifier si la communication redevient possible.

### Partie 1 : Vérification de la connectivité locale et test de l'ACL

1. **Étape 1** : Vérification de la connectivité locale
    
    - Depuis PC1, tu dois faire un **ping** vers PC2 (qui est sur le même réseau local). Le ping devrait être **réussi**.
    - Fais ensuite un **ping** de PC1 vers PC3 (qui est aussi dans le même réseau local mais sur une autre interface). Le ping devrait aussi être **réussi**.
    
    **Pourquoi les pings sont-ils réussis ?**  
    Les pings sont réussis parce qu'il n'y a pas de restriction dans le réseau local entre les PC (pas de règles ACL appliquées sur ces interfaces locales). Les dispositifs peuvent communiquer librement au sein du même réseau.
    
2. **Étape 2** : Test de la connectivité vers des réseaux distants
    
    - Fais un **ping** de PC1 vers PC4 et un autre ping vers le serveur DNS.
    
    **Pourquoi ces pings échouent-ils ?**  
    Ces pings échouent parce qu'une ACL est configurée pour bloquer le trafic ICMP (comme les requêtes ping) provenant du réseau de PC1 (192.168.10.0/24). L'ACL empêche les paquets de quitter l'interface de R1 en direction des réseaux distants (incluant PC4 et le serveur DNS).
    

### Partie 2 : Suppression de l'ACL et répétition du test

1. **Étape 1** : Vérifier la configuration de l'ACL
    
    - Depuis le routeur R1, tu dois utiliser la commande `show access-lists` pour afficher les ACLs configurées. L'ACL 11 est utilisée pour **bloquer tout le trafic venant du réseau 192.168.10.0/24** et permettre tout autre trafic.
        
    - En utilisant la commande `show run | include interface|access`, tu peux voir que l'ACL est appliquée à l'interface **Serial0/0/0** en sortie (direction "out"). Cela signifie que tout le trafic quittant cette interface est filtré par l'ACL.
        
    
    **Question : Sur quelle interface et dans quelle direction l'ACL est-elle appliquée ?**  
    L'ACL est appliquée à l'interface **Serial0/0/0** en direction "out".
    
2. **Étape 2** : Suppression de l'ACL
    
    - Pour supprimer l'ACL de l'interface, tu dois entrer dans la configuration de l'interface et retirer la règle qui applique l'ACL :


```
R1(config)# interface s0/0/0
R1(config-if)# no ip access-group 11 out

```

Ensuite, pour supprimer complètement l'ACL de la configuration du routeur

```
R1(config)# no access-list 11

```
1. - Enfin, tu vérifies à nouveau la connectivité en faisant un **ping** de PC1 vers PC4 et le serveur DNS. Ces pings devraient maintenant réussir car il n'y a plus d'ACL bloquant le trafic.


une fois que les ACL supprimer le pc arrive a ping 
![[Pasted image 20250305104509.png]]





### **Partie 1 : Planification de la mise en œuvre de l'ACL**

#### **Étape 1 : Vérification de la connectivité réseau actuelle**

Avant de configurer une ACL, il est essentiel de vérifier que tout le réseau fonctionne correctement et que la connectivité est totale.

1. **Vérification de la connectivité :**
    - Utilisez un PC (comme PC1) pour faire des **ping** vers les autres appareils (PC2, PC3, WebServer). Si le réseau est correctement configuré, vous devez pouvoir pinguer tous les dispositifs.

#### **Étape 2 : Évaluation des politiques réseau et planification des ACL**

- **Politiques sur R2** :
    
    - Le réseau **192.168.11.0/24** ne doit pas avoir accès au **WebServer** situé sur le réseau **192.168.20.0/24**.
    - Tout autre trafic est autorisé.
    
    Cela signifie qu'il faut une ACL sur R2 pour bloquer l'accès du réseau 192.168.11.0/24 au WebServer (192.168.20.254), tout en permettant le reste du trafic. L'ACL doit être placée sur l'interface sortante de R2 vers le WebServer.
    
- **Politiques sur R3** :
    
    - Le réseau **192.168.10.0/24** ne doit pas avoir accès au réseau **192.168.30.0/24**.
    - Tout autre trafic est autorisé.
    
    Ici, une ACL doit être placée sur R3 pour bloquer l'accès du réseau 192.168.10.0/24 au réseau 192.168.30.0/24, tout en permettant le reste du trafic. L'ACL doit être appliquée sur l'interface sortante vers PC3.
    

### **Partie 2 : Configuration, application et vérification d'une ACL standard**

#### **Étape 1 : Configuration et application d'une ACL numérotée standard sur R2**

1. **Création de l'ACL** sur R2 pour bloquer l'accès de **192.168.11.0/24** au WebServer :