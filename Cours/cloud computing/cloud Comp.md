
# Cours du 07/10/2024 (Mr Becker)


Le cloud computing (en français, « informatique dans les nuages ») fait référence à **l'utilisation de la mémoire et des capacités de calcul des ordinateurs et des serveurs répartis dans le monde entier et liés par un réseau**. Le cloud c'est aussi un moyen de stockage centraliser ou décentraliser sur le réseau.

# Fournisseur de Cloud 

Les 3 premier mondiale du cloud

AWS : (Amazon web service)

AZURE : (Microsoft)

GOOGLE :


Services Proposer:

IAAS = infrastructure as a Service 

PAAS = Plateform as a Service 

SAAS = Software as a Service  (M365, google, etc... )




Il existe trois type de cloud 
- Le cloud Public
- le cloud sur site (privée) 
- le cloud hybride (50 sur site et 50 sur le cloud) 

![[Pasted image 20241007104017.png]]


# Avantage du cloud computing 

Avantage 
- le cout de l'investissement (car on doit pas acheter le matériel on paye que se qu'on consomme)
- AWS peut réaliser de plus grande économie de mise a l'échelle et en faire des bénéficier ses clients 
- plus besoin de deviner la capaciter car AWS nous montre la capacité nécessaire (pour éviter la surestimation et sous estimation des serveur)
- augmenter la vitesse et l'agilité des serveur (exemple besoin de 100 machin en un click on les a)
- ne plus dépensez d'argent pour le fonctionnement et la maintenance des centre de données 


# Introduction a AWS

un serveur web est un logiciel qui se rend dispo sur internet sous forma normaliser telle que XML ou JSON pour la demande et la réponse de l'API

le service choisi dépend de bcp de facteur telle que les objectif et besoin technologique. Plusieurs outils différents telle que (calculs, stockage, gestion et de gouvernance, sécurité, service de base, service de mise en réseau, service de gestion des couts AWS)

trois moyen d'interagir avec AWS 
- interface WEB (AWS management Console)
- interface de ligne de commande (AWS CLI)
- kits de développement logiciel (SDK)



# Migration vers le Cloud




# Module 2

trois facteur fondamentale des cout avec AWS


payez se que vous utiliser

payer seulement les service utiliser

taris qui est dégressif selon l'utilisation



# Module 6 - Service de calcul AWS

- Amazon EC2 (machine virtuel) infra en tant que service IaaS
- Amazon ECS EKS ECR et AWS fargate (calcule baser sur des conteneur)
- Amazon Lambda (calcul sans serveur)
- AWS Elastic Beantalk 



		section 2


Amazon Elastic compute cloud (Amazon EC2)



Création d'une AMI 
Amazon Machine Image (ami)
modèle utiliser pour crée le EC2 (machine virtuelle ou vm qui exécutée dans AWS cloud)

option de l'image AMI :
quick start : ami Linux et Windows fournies par aws

en gros se sont des machine près installer avec les iso d'installer près a être utiliser avec nos logicielle choisie gain de temps plus de cout

![[Pasted image 20241008092243.png]]




Dénomination et taille des types d'instances 

![[Pasted image 20241008092325.png]]

plusieurs type d'instance en fonction des tache a effectuer 

![[Pasted image 20241008092401.png]]

Choisir l'instance ou on va mettre notre EC2(machine virtuel)
- la bande passante varie en fonction de l'instance
- type de réseaux améliorée 
	- Ena prend jusqua 100Gbits/S
	- interface VF intel 82599 prend 10Gbits/S 





Section 4

- ajout des balise 
- ![[Pasted image 20241008094824.png]]



- paramètre des groupe de sécurité 
	- un groupe de sécurité est un ensemble de règle pare feu qui contrôle le trafic entrant de l'instance 

	- ![[Pasted image 20241008095049.png]]


lancement d'une interface EC2 avec l'interface en ligne de commande 
![[Pasted image 20241008100612.png]]



Analyse du cycle de vie et de fonctionnement d'une EC2

![[Pasted image 20241008100703.png]]


Amazon CloudWatch pour surveiller et faire la supervisons de notre réseau cloud 

![[Pasted image 20241008100848.png]]
