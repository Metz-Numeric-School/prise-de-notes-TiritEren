
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
11. redémarrer l'ordi le réseau devrait être bon