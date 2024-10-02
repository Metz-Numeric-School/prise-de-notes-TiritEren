25/09/2024


| savoir faire                                       | savoir etre                       |
| -------------------------------------------------- | --------------------------------- |
| mise a jour des logiciels                          | etre patient                      |
| maintenance equipements informatique               | equillibrer vie pro et vie privée |
| instalation new pc et peripherique                 | etre pedagogue                    |
| lisiting des comptes ad actifs                     | savoir communiquer                |
| former utilisateur                                 | etre autonome                     |
| analyser les besoin de lentreprise                 | etre vigilant                     |
| conseiller les utilisateur sur les bonnes pratique | etre diplomate                    |
| verifier le respect l'ANSSI des mots de passe      | apporter des solution             |
| A2F                                                | etre calme et reactif             |
| inventer des materiaux                             | s'adapter a tous les cas          |
| respecter le budget                                | savoir travailler en equipe       |
| verifier les backup                                | avoir une tenue adaptée           |
| gestion des droit                                  |                                   |
| mettre a jour les docu et procedure                |                                   |
| controle et gestion des licences                   |                                   |
| centraliser l'achat des equipements                |                                   |



Glpi 
	-Gerer tous les parcs
	-faire un inventaire
	-creer des tickets 
		-creer plusieurs catégorie de tickets 
			-tickets pour le service Compta ou RH 
			-tickets poour le service IT



pour changer ladresse ip en  ligne de commande sous linux 
	- fichier ou est stocker l'infomation de linterface reseau 
- /etc/network/interfaces
	-  pour modifier ladresse ip en static 
		-iface eth0 inet static
			adresse 10.10.10.100
			netmask 255.255.255.0
			getaway 10.10.10.2
				dns-nameserveur 8.8.8.8    4.4.4.4

	et on modifie le dns --> /etc/reslov.conf
						nameserver 10.10.10.2



# La gestion de la maintenance 

si un pc HS on remplace plus de réparation 


Il y a plusieurs niveau de maintenance il en existe 5

Niveau 1 action simple 
Niveau 2 opération mineur
niveau 3 opération de diagnostic 
niveau 4 travaux important de maintenance 
niveau 5 travaux de rénovation et de réparation 


# Les types de maintenance 

Maintenance préventive systématique
maintenance préventive conditionnel 
maintenance corrective qui sont consécutive a une défaillance puis il y a la maintenance préventive qui sont destinée a réduire la probabilité d'une défaillance 


sauvegarde 321 
3 sauvegarde 
sur 2 support différents 
2 sauvegarde sur site
1 sauvegarde en externe (hors réseau)


# Le cout d'une panne

une panne a deux type de cout un cout directs lie a la réparation
- temps passer au cout de la main d'œuvre des techniciens 
- intervention éventuelle d'expert externes 
- pièces de rechange 

ainsi que des couts indirects :
- pièces rebutées a cause de la panne 
- reprise ou retouche de ces pièces 
- remplacement des rebutées 
- perte de productivité durant l'arrêt 


# Les critères de choix

il faut faire un choix sur quelle équipement il faut faire des maintenance exemple si on sou traitre une imprimante si il tombe en panne on sen occupe pas car il est en location 

# L'appréciation du risque 

- économie : perte d'exploitation et pénalité de retard pour un fournisseur 
- qualitative l'arrêt et le redémarrage ont une incidence sur la qualité des produits sans la détérioration soit directement visible
- environnementale le retraitement d'effluents (fumées fluides...) ne peut se faire durant la panne

se poser la question si le risque et 
- risque inacceptable :supprimer les causes de panne et supprimer les effets des pannes 
- risque acceptable :prévention inutile
- risque acceptable si limité choisir entre maintenance préventive systématique et maintenance conditionnelle 

PCA : plan de continuer de l'activité 

PCI : plan de continuité de l'informatique 



