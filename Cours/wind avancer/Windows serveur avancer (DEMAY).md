15/10/2024
# DNS (Domain Name Systeme)

il y a un organisme qui gère sa c'est ICANN il gere les top level domain (TLD)

il existe une TLD par pays exemple 
.fr pour la france .it pour italie etc et aussi il y a quelques TLD générales (.com .net .org .mil .biz...)

L'ICANN délègue la gestion de chaque TLD a un organisme (registry) .

en france il y a deux organisme qui gere sa 
- AFNIC tient les registre des .fr
- VeriSign tient le registre des .com/ .net/ .name/ .info/ .biz.

chaque registry peut gérer comme bon lui semble l'attribution des noms de domaine sur sa TLD.

une fois le nom de domaine obtenu il et automatiquement attribuer a une ip

Chaque registry autorise des registrars a vendre des noms de domaine. 
Les registrars ont un role commercial. 


Chaque pays possede un TLD qui correspond a son code pays ISO 

(fr=france it=italie etc etc )
on les appelle ccTLD (contry-code TLD)

il y a 13 serveur racine dans le monde les serveur racine sont les serveur dns la majorité sont au état unis

![[Pasted image 20241015091137.png]]



# Service TCP/IP


permet la correspondance entre un nom de domaine qualifié FQDN (Fully Qualified Domain Name) et une adresse IP

un nom FQDN doit toujours se terminé par un . se qui signifie que on est a la racine


# Primaire et zone secondaire 

Transfert de zone entre serveur maitre (primaire) et un autre serveur (secondaire) chacun ayant autorité sur la zone.

Vis-a-vis d'un client l'un ou l'autre repond en fonction de la vitesse du reseau


# Scenarios possible 

- serveur cache (but: effectuer des requêtes DNS pour se rappeler de la réponse pour la prochaine requête) Avantage :réduction de la bande passante et réduction du temps de latence

- Serveur Primaire(maitre) (but: contenir des enregistrements DNS d'un nom de domaine enregistré) (un ensemble d'enregistrement DNS pour un nom de domaine est appelé une "zone")

- serveur secondaire (esclave) (but : contenir une copie des zones configurées sur le serveur maitre) Avantages : Recommandé sur les réseaux important aussi il assure la disponibilité de la zone DNS si le serveur maitre n'est pas disponible 

- serveur Stub (but : idem que le serveur esclave mais copie uniquement les données du serveur et pas les données de l'hôte)

- enregistrement DNS (but: mapper nom d'hôte / adresse ip et adresse ip /nom d'hote)


![[Pasted image 20241015093449.png]]


# DHCP

Role du DHCP 
permet au serveurs d'attribuer des adresse IP aux ordinateurs et autre périphériques reconnus comme client DHCP

