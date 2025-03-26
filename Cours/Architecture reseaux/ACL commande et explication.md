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

2. `R2(config)# access-list 1 deny 192.168.11.0 0.0.0.255`
    
2. **Permettre tout autre trafic** :
    
    `R2(config)# access-list 1 permit any`
    
3. **Vérification de l'ACL** :
    
    
    `R2# show access-lists`
    
    Vous devriez voir :
    
    `Standard IP access list 1 10 deny 192.168.11.0 0.0.0.255 20 permit any`
    
4. **Application de l'ACL sur l'interface GigabitEthernet 0/0 (sortante)** :
    
    
    
    `R2(config)# interface GigabitEthernet0/0 R2(config-if)# ip access-group 1 out`
    

#### **Étape 2 : Configuration et application d'une ACL numérotée standard sur R3**

1. **Création de l'ACL** sur R3 pour bloquer l'accès de **192.168.10.0/24** au réseau **192.168.30.0/24** :
    
    
    `R3(config)# access-list 1 deny 192.168.10.0 0.0.0.255`
    
2. **Permettre tout autre trafic** :
    
    
    
    `R3(config)# access-list 1 permit any`
    
3. **Vérification de l'ACL** :
    
    
    
    `R3# show access-lists`
    
    Vous devriez voir :
    
    
    
    `Standard IP access list 1 10 deny 192.168.10.0 0.0.0.255 20 permit any`
    
4. **Application de l'ACL sur l'interface GigabitEthernet 0/0 (sortante)** :
   
    `R3(config)# interface GigabitEthernet0/0 R3(config-if)# ip access-group 1 out`
    

#### **Étape 3 : Vérification de la configuration et des fonctionnalités de l'ACL**

1. **Vérifier les placements des ACL** :
    
    - Utilisez la commande suivante sur R2 et R3 pour vérifier l'application des ACL :
    
        
        `show run | include ip access-group`
        
    - Vous devez voir l'ACL appliquée à l'interface GigabitEthernet 0/0 sur les deux routeurs.
        
2. **Vérification des politiques ACL avec des pings** :
    
    - **Depuis PC1** (192.168.10.10) :
        
        - Un ping vers **PC2** (192.168.11.10) doit réussir.
        - Un ping vers le **WebServer** (192.168.20.254) doit réussir.
        - Un ping vers **PC3** (192.168.30.10) doit échouer.
    - **Depuis PC2** (192.168.11.10) :
        
        - Un ping vers le **WebServer** (192.168.20.254) doit échouer.
        - Un ping vers **PC3** (192.168.30.10) doit réussir.
    - **Depuis PC3** (192.168.30.10) :
        
        - Un ping vers le **WebServer** (192.168.20.254) doit réussir.
3. **Afficher les statistiques des ACL** :
    
    - Utilisez la commande suivante sur R2 et R3 pour afficher les statistiques des ACL et vérifier le nombre de paquets qui ont correspondu à chaque ligne de l'ACL :
        
        
        `show access-lists`
        
    
    Vous devez voir un nombre de "match" correspondant aux pings effectués.
    
    Exemple de sortie pour R2 :
    
    
    `Standard IP access list 1 10 deny 192.168.11.0 0.0.0.255 (4 match(es)) 20 permit any (8 match(es))`
    

### Conclusion :

Cette configuration d'ACL permet de restreindre le trafic selon les politiques définies, tout en permettant le reste du trafic.





#### Table d'adressage

|Appareil|Interface|Adresse IP|Masque de sous-réseau|Passerelle par défaut|
|---|---|---|---|---|
|R1|F0/0|192.168.100.1|255.255.255.0|N/A|
|R1|F0/1|192.168.200.1|255.255.255.0|N/A|
|R1|E0/0/0|192.168.10.1|255.255.255.0|N/A|
|R1|E0/1/0|192.168.20.1|255.255.255.0|N/A|
|Serveur de fichiers|NIC|192.168.200.100|255.255.255.0|192.168.200.1|
|Serveur Web|NIC|192.168.100.100|255.255.255.0|192.168.100.1|
|PC0|NIC|192.168.20.3|255.255.255.0|192.168.20.1|
|PC1|NIC|192.168.20.4|255.255.255.0|192.168.20.1|
|PC2|NIC|192.168.10.3|255.255.255.0|192.168.10.1|

---

### Objectifs

- **Partie 1** : Configurer et appliquer une ACL standard nommée.
- **Partie 2** : Vérifier la mise en œuvre de l'ACL.

---

### Contexte

Dans ce scénario, l'administrateur réseau principal vous demande de créer une ACL standard nommée pour empêcher l'accès à un serveur de fichiers qui contient la base de données de l'application web. Seuls le poste de travail PC1 (Gestionnaire Web) et le serveur Web doivent avoir accès au serveur de fichiers. Tout autre trafic vers le serveur de fichiers doit être refusé.

---

### Instructions

#### **Partie 1 : Configurer et appliquer une ACL standard nommée**

**Étape 1** : Vérifier la connectivité avant de configurer et d'appliquer l'ACL.

- Assurez-vous que toutes les stations de travail (PC0, PC1, et PC2) peuvent pinguer à la fois le serveur Web et le serveur de fichiers.

**Étape 2** : Configurer une ACL standard nommée.

1. Ouvrez la fenêtre de configuration et exécutez les commandes suivantes sur R1 pour configurer l'ACL :
    
    
    
    `R1(config)# ip access-list standard File_Server_Restrictions R1(config-std-nacl)# permit host 192.168.20.4 R1(config-std-nacl)# permit host 192.168.100.100 R1(config-std-nacl)# deny any`
    
2. Utilisez la commande `show access-lists` pour vérifier le contenu de l'ACL avant de l'appliquer à l'interface :
    
    
    
    `R1# show access-lists`
    
    Sortie :
    
    
    `Standard IP access list File_Server_Restrictions 10 permit host 192.168.20.4 20 permit host 192.168.100.100 30 deny any`
    

**Étape 3** : Appliquer l'ACL nommée.

1. Appliquez l'ACL au trafic sortant sur l'interface FastEthernet 0/1 :
    
    
    `R1(config-if)# interface FastEthernet 0/1 R1(config-if)# ip access-group File_Server_Restrictions out`
    
2. Sauvegardez la configuration :
    
    
    
    `R1# write memory`
    

---

#### **Partie 2 : Vérifier la mise en œuvre de l'ACL**

**Étape 1** : Vérifier la configuration et l'application de l'ACL sur l'interface.

1. Utilisez la commande `show access-lists` pour vérifier la configuration de l'ACL :
    
    
    
    `R1# show access-lists`
    
2. Utilisez la commande `show ip interface fastethernet 0/1` pour vérifier que l'ACL est correctement appliquée à l'interface :
    
    
    
    `R1# show ip interface fastethernet 0/1`
    

**Étape 2** : Vérifier que l'ACL fonctionne correctement.

1. Testez la connectivité :
    
    - Toutes les stations de travail (PC0, PC1 et PC2) devraient pouvoir pinguer le serveur Web.
    - Seuls PC1 et le serveur Web devraient pouvoir pinguer le serveur de fichiers.
    - PC0 et PC2 ne devraient pas pouvoir pinguer le serveur de fichiers.
2. Répétez la commande `show access-lists` pour vérifier le nombre de paquets qui ont correspondu à chaque règle de l'ACL :
    
    
    
    `R1# show access-lists`
    
    La sortie affichera le nombre de correspondances pour chaque règle, confirmant si l'ACL fonctionne comme prévu.





## Tableau d'adressage

|Appareil|Interface|Adresse IP|Masque de sous-réseau|Passerelle par défaut|
|---|---|---|---|---|
|**R1**|G0/0/0|192.168.10.1|255.255.255.0|N/A|
|**R1**|G0/0/1|192.168.20.1|255.255.255.0|N/A|
|**R1**|S0/1/0 (DCE)|10.1.1.1|255.255.255.252|N/A|
|**Edge**|S0/1/0|10.1.1.2|255.255.255.252|N/A|
|**Edge**|S0/1/1 (DCE)|10.2.2.2|255.255.255.252|N/A|
|**Edge**|S0/2/1|209.165.200.225|255.255.255.224|N/A|
|**R3**|G0/0/0|192.168.30.1|255.255.255.0|N/A|
|**R3**|G0/0/1|192.168.40.1|255.255.255.0|N/A|
|**R3**|S0/1/1|10.2.2.1|255.255.255.252|N/A|
|**S1**|VLAN 1|192.168.10.11|255.255.255.0|192.168.10.1|
|**S2**|VLAN 1|192.168.20.11|255.255.255.0|192.168.20.1|
|**S3**|VLAN 1|192.168.30.11|255.255.255.0|192.168.30.1|
|**S4**|VLAN 1|192.168.40.11|255.255.255.0|192.168.40.1|
|**PC-A**|NIC|192.168.10.3|255.255.255.0|192.168.10.1|
|**PC-B**|NIC|192.168.20.3|255.255.255.0|192.168.20.1|
|**PC-C**|NIC|192.168.30.3|255.255.255.0|192.168.30.1|
|**PC-D**|NIC|192.168.40.3|255.255.255.0|192.168.40.1|

## Objectifs

- Partie 1 : Vérifier la connectivité.
- Partie 2 : Configurer et vérifier des ACLs standard numérotées et nommées.
- Partie 3 : Modifier une ACL standard.

## Contexte / Scénario

La sécurité réseau et le contrôle du flux de trafic sont des aspects cruciaux dans la conception et la gestion des réseaux IP. Configurer des règles de filtrage basées sur des politiques de sécurité établies est une compétence précieuse. Dans ce TP, vous allez configurer des règles de filtrage pour deux sites d'entreprises représentés par R1 et R3. Vous allez appliquer des listes de contrôle d'accès (ACL) pour contrôler le trafic entre les réseaux locaux (LANs) connectés à ces routeurs.

Le routeur **Edge** entre R1 et R3 est fourni par un fournisseur de services internet (ISP) et ne doit pas être modifié. Vous n'avez pas de droits administratifs sur ce routeur.

## Instructions

### Partie 1 : Vérifier la connectivité

1. **Test de connectivité initial :** Il est essentiel de vérifier que la connectivité fonctionne avant de configurer et d'appliquer des listes de contrôle d'accès (ACL). Cela garantit que le réseau fonctionne correctement avant de commencer à filtrer le trafic.
    
2. **Tests de ping :** Depuis les différents PC et routeurs, effectuez des pings pour tester la connectivité entre les différents segments du réseau.
    

### Partie 2 : Configurer et vérifier des ACL standard numérotées et nommées

#### Étape 1 : Configurer une ACL standard numérotée

- **ACLs standards** : Elles filtrent le trafic uniquement en fonction de l'adresse IP source. Il est recommandé d'appliquer les ACLs standard aussi près que possible de la destination.
    
- **Exemple** : Vous devez créer une ACL numérotée qui permet aux hôtes des réseaux **192.168.10.0/24** et **192.168.20.0/24** d'accéder aux hôtes du réseau **192.168.30.0/24**.
    

1. **Créer l'ACL sur R3 :**
    
    
    
    `R3(config)# access-list 1 remark Autoriser les réseaux de R1 à accéder à R3 R3(config)# access-list 1 permit 192.168.10.0 0.0.0.255 
    ``R3(config)# access-list 1 permit 192.168.20.0 0.0.0.255 R3(config)# access-list 1 deny any`
    
2. **Appliquer l'ACL :**
    
    
    
    `R3(config)# interface g0/0/0 R3(config-if)# ip access-group 1 out`
    
3. **Vérification des ACL :**
    
    - Utilisez la commande `show access-lists` pour voir les règles définies dans l'ACL.
    - Utilisez `show ip interface` pour vérifier où l'ACL est appliquée et dans quelle direction.

#### Étape 2 : Configurer une ACL standard nommée

- **Nom de l'ACL** : Créez une ACL nommée **BRANCH-OFFICE-POLICY** pour autoriser les hôtes du réseau **192.168.40.0/24** à accéder à **192.168.10.0/24**. Seul **PC-C** doit également pouvoir accéder à ce réseau.

1. **Créer l'ACL nommée sur R1 :**
    
    
    
    `R1(config)# ip access-list standard BRANCH-OFFICE-POLICY R1(config-std-nacl)# permit host 192.168.30.3 R1(config-std-nacl)# permit 192.168.40.0 0.0.0.255`
    
2. **Appliquer l'ACL sur l'interface appropriée :**

    
    `R1(config)# interface g0/0/0 R1(config-if)# ip access-group BRANCH-OFFICE-POLICY out`
    

### Partie 3 : Modifier une ACL standard

- Les politiques de sécurité évoluent souvent, ce qui nécessite de modifier les ACLs existantes. Vous devrez ajuster l'ACL pour autoriser certains accès en fonction de nouvelles directives de la direction.




**Table d'adressage**

|Appareil|Interface|Adresse IP|Masque de sous-réseau|Passerelle par défaut|
|---|---|---|---|---|
|R1|G0/0|172.22.34.65|255.255.255.224|N/A|
|R1|G0/1|172.22.34.97|255.255.255.240|N/A|
|R1|G0/2|172.22.34.1|255.255.255.192|N/A|
|Serveur|NIC|172.22.34.62|255.255.255.192|172.22.34.1|
|PC1|NIC|172.22.34.66|255.255.255.224|172.22.34.65|
|PC2|NIC|172.22.34.98|255.255.255.240|172.22.34.97|

**Objectifs**

- Partie 1 : Configurer, appliquer et vérifier une ACL numérotée étendue.
- Partie 2 : Configurer, appliquer et vérifier une ACL nommée étendue.

**Contexte / Scénario** Deux employés ont besoin d'accéder aux services fournis par le serveur. PC1 a besoin d'un accès FTP uniquement, tandis que PC2 a besoin d'un accès web uniquement. Les deux ordinateurs doivent pouvoir effectuer des ping vers le serveur, mais ne doivent pas pouvoir se pinger l'un l'autre.

### **Instructions**

### Partie 1 : Configurer, appliquer et vérifier une ACL numérotée étendue

**Étape 1 : Configurer une ACL pour permettre le FTP et l'ICMP depuis le LAN de PC1.**

a. Depuis le mode de configuration globale sur R1, entrez la commande suivante pour déterminer le premier numéro valide pour une liste de contrôle d'accès étendue.



`R1(config)# access-list ?`

b. Ajoutez 100 à la commande, suivi d'un point d'interrogation.



`R1(config)# access-list 100 ?`

c. Pour permettre le trafic FTP, entrez "permit" suivi d'un point d'interrogation.



`R1(config)# access-list 100 permit ?`

d. Pour configurer le FTP et l'ICMP, utilisez TCP pour affiner l'ACL.



`R1(config)# access-list 100 permit tcp ?`

e. Utilisez l'adresse source du réseau 172.22.34.64/27 pour PC1.



`R1(config)# access-list 100 permit tcp 172.22.34.64 ?`

f. Calculez le masque générique pour /27 : 0.0.0.31.

g. Entrez le masque générique, suivi de l'adresse du serveur.



`R1(config)# access-list 100 permit tcp 172.22.34.64 0.0.0.31 host 172.22.34.62 ?`

h. Permettez uniquement le trafic FTP.



`R1(config)# access-list 100 permit tcp 172.22.34.64 0.0.0.31 host 172.22.34.62 eq ftp`

i. Créez une deuxième entrée pour permettre le trafic ICMP (ping) de PC1 vers le serveur.



`R1(config)# access-list 100 permit icmp 172.22.34.64 0.0.0.31 host 172.22.34.62`

j. Par défaut, tout autre trafic est bloqué.

k. Vérifiez la configuration de la liste d'accès.


`R1# show access-lists`

**Étape 2 : Appliquer l'ACL sur la bonne interface pour filtrer le trafic.**

Appliquez l'ACL à l'interface Gigabit Ethernet 0/0 pour le trafic entrant.


`R1(config)# interface gigabitEthernet 0/0 R1(config-if)# ip access-group 100 in`

**Étape 3 : Vérifier la mise en œuvre de l'ACL.**

a. Testez les pings entre PC1 et le serveur.

b. Testez l'accès FTP de PC1 au serveur.


`PC> ftp 172.22.34.62 ftp> quit`

c. Testez le ping de PC1 à PC2 (cela doit échouer).

---

### Partie 2 : Configurer, appliquer et vérifier une ACL nommée étendue

**Étape 1 : Configurer une ACL pour permettre l'accès HTTP et ICMP depuis le LAN de PC2.**

a. Les ACL nommées commencent avec le mot-clé "ip".



`R1(config)# ip access-list extended HTTP_ONLY`

b. Permettez le trafic TCP pour l'accès HTTP depuis le réseau de PC2.



`R1(config-ext-nacl)# permit tcp 172.22.34.96 0.0.0.15 host 172.22.34.62 eq www`

c. Permettez également le trafic ICMP depuis PC2 vers le serveur.



`R1(config-ext-nacl)# permit icmp 172.22.34.96 0.0.0.15 host 172.22.34.62`

d. Vérifiez la configuration.


`R1# show access-lists`

**Étape 2 : Appliquer l'ACL à la bonne interface.**

Appliquez l'ACL à l'interface Gigabit Ethernet 0/1 pour le trafic entrant.



`R1(config)# interface gigabitEthernet 0/1 R1(config-if)# ip access-group HTTP_ONLY in`

**Étape 3 : Vérifier la mise en œuvre de l'ACL.**

a. Testez les pings entre PC2 et le serveur.

b. Testez l'accès HTTP de PC2 au serveur en utilisant un navigateur web.

c. Testez l'accès FTP de PC2 au serveur (cela doit échouer).





**Tableau d’adressage :**

|**Appareil**|**Interface**|**Adresse IP**|**Masque de sous-réseau**|**Passerelle par défaut**|
|---|---|---|---|---|
|**RT1**|G0/0|172.31.1.126|255.255.255.224|N/A|
|**RT1**|S0/0/0|209.165.1.2|255.255.255.252|N/A|
|**PC1**|NIC|172.31.1.101|255.255.255.224|172.31.1.126|
|**PC2**|NIC|172.31.1.102|255.255.255.224|172.31.1.126|
|**PC3**|NIC|172.31.1.103|255.255.255.224|172.31.1.126|
|**Server1**|NIC|64.101.255.254|Blank|Blank|
|**Server2**|NIC|64.103.255.254|Blank|Blank|

---

### **Objectifs :**

- **Partie 1 : Configurer une ACL nommée étendue**
- **Partie 2 : Appliquer et vérifier l’ACL étendue**

---

### **Contexte / Scénario :**

Dans ce scénario, des appareils spécifiques du réseau local (LAN) doivent accéder à certains services sur des serveurs situés sur Internet.

---

### **Instructions :**

### **Partie 1 : Configurer une ACL nommée étendue**

Configurez une ACL nommée pour mettre en œuvre la politique suivante :

- Bloquer l'accès HTTP et HTTPS de PC1 vers Server1 et Server2. Les serveurs sont dans le cloud, et seules leurs adresses IP sont connues.
- Bloquer l'accès FTP de PC2 vers Server1 et Server2.
- Bloquer l'accès ICMP de PC3 vers Server1 et Server2.

**Remarque :** Pour des raisons d'évaluation, vous devez configurer les déclarations dans l'ordre spécifié dans les étapes suivantes.

---

### **Étape 1 : Refuser à PC1 l'accès aux services HTTP et HTTPS sur Server1 et Server2.**

a. Créez une liste d'accès IP étendue nommée sur le routeur RT1 qui interdira à PC1 d'accéder aux services HTTP et HTTPS de Server1 et Server2. Quatre déclarations de contrôle d'accès sont nécessaires. Utilisez "LimitedAccess" comme nom de la liste d'accès dans cette activité.

**Question :** Quelle est la commande pour commencer la configuration d'une liste d'accès étendue avec le nom "LimitedAccess" ?

Tapez vos réponses ici.

b. Commencez la configuration de l'ACL avec une déclaration qui interdit à PC1 d'accéder à Server1, uniquement pour HTTP (port 80).


`RT1(config-ext-nacl)# deny tcp host 172.31.1.101 host 64.101.255.254 eq 80`

c. Ensuite, entrez la déclaration qui interdit à PC1 d'accéder à Server1, uniquement pour HTTPS (port 443).



`RT1(config-ext-nacl)# deny tcp host 172.31.1.101 host 64.101.255.254 eq 443`

d. Entrez la déclaration qui interdit à PC1 d'accéder à Server2, uniquement pour HTTP.



`RT1(config-ext-nacl)# deny tcp host 172.31.1.101 host 64.103.255.254 eq 80`

e. Entrez la déclaration qui interdit à PC1 d'accéder à Server2, uniquement pour HTTPS.



`RT1(config-ext-nacl)# deny tcp host 172.31.1.101 host 64.103.255.254 eq 443`

---

### **Étape 2 : Refuser à PC2 l'accès aux services FTP sur Server1 et Server2.**

a. Entrez la déclaration qui interdit à PC2 d'accéder à Server1, uniquement pour FTP (port 21).


`RT1(config-ext-nacl)# deny tcp host 172.31.1.102 host 64.101.255.254 eq 21`

b. Entrez la déclaration qui interdit à PC2 d'accéder à Server2, uniquement pour FTP (port 21).



`RT1(config-ext-nacl)# deny tcp host 172.31.1.102 host 64.103.255.254 eq 21`

---

### **Étape 3 : Refuser à PC3 de pinguer Server1 et Server2.**

a. Entrez la déclaration qui interdit l'accès ICMP de PC3 à Server1.



`RT1(config-ext-nacl)# deny icmp host 172.31.1.103 host 64.101.255.254`

b. Entrez la déclaration qui interdit l'accès ICMP de PC3 à Server2.



`RT1(config-ext-nacl)# deny icmp host 172.31.1.103 host 64.103.255.254`

---

### **Étape 4 : Permettre tout autre trafic IP.**

Par défaut, une liste d'accès refuse tout le trafic qui ne correspond à aucune règle de la liste. Entrez la commande pour autoriser tout le trafic qui ne correspond à aucune des déclarations de la liste d'accès configurée.



`RT1(config-ext-nacl)# permit ip any any`

---

### **Étape 5 : Vérifier la configuration de la liste d'accès avant de l'appliquer à une interface.**

Avant d'appliquer une liste d'accès, la configuration doit être vérifiée pour s'assurer qu'il n'y a pas d'erreurs typographiques et que les déclarations sont dans le bon ordre. Pour afficher la configuration actuelle de la liste d'accès, utilisez les commandes `show access-lists` ou `show running-config`.

r

`RT1# show access-lists`

---

### **Partie 2 : Appliquer et vérifier l’ACL étendue**

Le trafic à filtrer provient du réseau 172.31.1.96/27 et est destiné à des réseaux distants. La bonne pratique consiste à appliquer les listes d'accès étendues sur l'interface la plus proche de la source du trafic.

---

**Étape 1 : Appliquer l’ACL sur la bonne interface et dans la bonne direction.**

Appliquez l'ACL sur l'interface G0/0 dans la direction `in`.


`RT1(config-if)# ip access-group LimitedAccess in`

---

**Étape 2 : Tester l’accès pour chaque PC.**

a. Accédez aux sites web de Server1 et Server2 à partir de PC1 en utilisant les protocoles HTTP et HTTPS.

b. Accédez aux serveurs FTP de Server1 et Server2 à partir de PC2.

c. Tentez de pinguer Server1 et Server2 à partir de PC3.






### **Bloc de commandes pour configurer une ACL nommée étendue**

1. **Refuser à PC1 l'accès aux services HTTP et HTTPS sur Server1 et Server2 :**



```
RT1(config-ext-nacl)# deny tcp host 172.31.1.101 host 64.101.255.254 eq 80
RT1(config-ext-nacl)# deny tcp host 172.31.1.101 host 64.101.255.254 eq 443
RT1(config-ext-nacl)# deny tcp host 172.31.1.101 host 64.103.255.254 eq 80
RT1(config-ext-nacl)# deny tcp host 172.31.1.101 host 64.103.255.254 eq 443

```

**Description :** Ces commandes bloquent l'accès HTTP (port 80) et HTTPS (port 443) de PC1 (172.31.1.101) vers Server1 (64.101.255.254) et Server2 (64.103.255.254).


2. Refuser à PC2 l'accès aux services FTP sur Server1 et Server2 :
```
RT1(config-ext-nacl)# deny tcp host 172.31.1.102 host 64.101.255.254 eq 21
RT1(config-ext-nacl)# deny tcp host 172.31.1.102 host 64.103.255.254 eq 21

```
**Description :** Ces commandes bloquent l'accès FTP (port 21) de PC2 (172.31.1.102) vers Server1 et Server2.



3. Refuser à PC3 de pinguer Server1 et Server2 (bloquer ICMP) :

```
RT1(config-ext-nacl)# deny icmp host 172.31.1.103 host 64.101.255.254
RT1(config-ext-nacl)# deny icmp host 172.31.1.103 host 64.103.255.254

```

**Description :** Ces commandes bloquent les requêtes ICMP (ping) de PC3 (172.31.1.103) vers Server1 et Server2.


1. Permettre tout autre trafic :
```
RT1(config-ext-nacl)# permit ip any any

```
**Description :** Cette commande permet à tout autre trafic qui ne correspond pas aux règles précédentes de passer librement.


5. Appliquer l’ACL à l’interface G0/0 dans la direction "in" :

```
RT1(config-if)# ip access-group LimitedAccess in

```

**Description :** Cette commande applique l'ACL nommée "LimitedAccess" à l'interface G0/0 en filtrant le trafic entrant.


6. Vérifier l'ACL configurée :

```
RT1# show access-lists

```

**Description :** Cette commande affiche la configuration actuelle de la liste d'accès pour vérifier que tout est en ordre avant l'application.









```
en
show access-lists
```
affiche les ACL mise en place

remove access-list 11
```
no access-list ID_LIST
```
 remove the ACL
```
no ip access-group ID_GROUP {out|in}
```

create a access-list
```
access-list 1 deny 192.168.11.0 0.0.0.255
```

Apply the ACL by placing it for outbound traffic on the GigabitEthernet 0/0 interface
```
R2(config-if)# ip access-group 1 out
```

```
en
conf t
ip access-list standard File_Server_Restrictions
permit host 192.168.20.4
```

allow FTP packets
```
access-list 100 permit tcp 172.122.34.64 0.0.0.31 host 172.22.34.62 eq ftp
```

ftp connection
```
ftp 172.22.34.62
```

