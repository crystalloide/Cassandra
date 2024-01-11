# Cassandra
Cassandra

[# Pour afficher les workspaces déjà instanciés si besoin de faire du ménage :](https://gitpod.io/workspaces)

# Cluster cassandra "formation" dans un environnement virtualisé "Gitpod" :

Notre cluster de démonstration contient 4 noeuds en tout : 
- 2 noeuds sur le Datacenter 1
- 2 noeuds sur le Datacenter 2

[![Open in Gitpod](https://gitpod.io/button/open-in-gitpod.svg)](https://gitpod.io/#https://github.com/crystalloide/Cassandra
)
## Rappel pour retrouver les environnements éventuellement précédemment instanciés dans Gitpod : https://gitpod.io/workspaces

- Si on était dans une VM classique linux : on récupèrerait les fichiers ainsi : 
git clone https://github.com/crystalloide/cassandra.git


•	Démarrer le cluster :

docker compose up -d

•	Pour lister l'image récupérée  :

docker images

•	Attendez quelques minutes que les conteneurs démarrent



su - user 

## Créer les répertoires : 

sudo rm -Rf ~/cassandra01

sudo rm -Rf ~/cassandra02

sudo rm -Rf ~/cassandra03

sudo rm -Rf ~/cassandra04


sudo rm -Rf ~/cassandra

sudo mkdir -p ~/cassandra

sudo chmod 777 -Rf ~/cassandra

## Rappel : si on cherche des images docker disponibles en ligne dans dockerhub : 

docker search cassandra


## On lance 1 fois un container nommé ici "cassandra" qui monte un répertoire local ~/tmp dans le conteneur sur /tmp : 

sudo chmod 777 -R /home/user/

docker run --name cassandra -d --mount src="$(pwd)",target=/tmp,type=bind  cassandra:4.1

## on se connecte sur le container nommé cassandra en exécution : 

docker exec -it cassandra bash

## Et on copie les fichiers de paramètrage de cassandra qui nous servirons plus tard de modèle : 

cp -r /etc/cassandra /tmp/

## On récupère en local le répertoire /etc/cassandra du container cassandra : 

## ce répertoire contient la configuration du serveur cassandra qui servira de modèle ensuite.

## On ressort du container : 

exit


## On crée maintenant par recopie les répertoires qui serviront de modèle pour les 4 cassandra01/02/03/04 

## (cela nous permettra de les customiser simplement à volonté) :

sudo chmod 777 -Rf ~/cassandra

ls cassandra 

cp -r ~/cassandra ~/cassandra01

cp -r ~/cassandra ~/cassandra02

cp -r ~/cassandra ~/cassandra03

cp -r ~/cassandra ~/cassandra04

sudo chmod 777 -Rf ~/cassandra01

sudo chmod 777 -Rf ~/cassandra02

sudo chmod 777 -Rf ~/cassandra03

sudo chmod 777 -Rf ~/cassandra04

## Et on supprime notre container qui nous a servi juste à récupérer le modèle de départ :

docker ps -a 

docker stop cassandra

docker rm cassandra

docker ps -a 

## Et on supprime notre répertoire modèle :

sudo rm -Rf ~/cassandra


## On va créer le fichier docker-compose pour ensuite créer notre cluster cassandra 

## à 4 noeuds cassandra01/cassandra02/cassandra03/cassandra04 répartis sur 2 Datacenter :

## Exemple d'images docker disponibles de cassandra : 

## docker search cassandra 

## image: registry.axonops.com/axonops-public/axonops-docker/cassandra:4.1

## image: cassandra:4.1




## On notera au passage  dans le fichier docker-compose.yml la façon de vérifier que le container est bien lancé : 
##  méthodes possibles :  
##     test: ["CMD-SHELL", "[ $$(nodetool -h 127.0.0.1 -pw cassandra -u cassandra statusgossip) = running ]"]
## ou, si "nc" est installé dans l'image docker : 
##     test: ["CMD", "nc", "-z", "127.0.0.1", "9044"]



## On lance maintenant le cluster : 
docker-compose up -d

## On contrôle le bon démarrage (plusieurs minutes) : 
docker ps -a

docker logs user_cassandra01_1 

docker logs user_cassandra01_1  | grep 'jump'

docker exec -it user_cassandra01_1  bash



#################################################################################################################

## Une fois le cluster bien démarré, vous aurez : 

#################################################################################################################

docker logs user_cassandra01_1  | grep 'jump'

## Affichage : 

## 	INFO  [main] 2024-01-11 15:39:05,574 StorageService.java:3084 - Node /172.22.0.2:7000 state jump to NORMAL

docker logs user_cassandra02_1  | grep 'jump'

## Affichage : 

## 	INFO  [main] 2024-01-11 15:39:54,752 StorageService.java:3084 - Node /172.22.0.3:7000 state jump to NORMAL

docker logs user_cassandra03_1  | grep 'jump'

## Affichage : 

## 	INFO  [main] 2024-01-11 15:39:22,650 StorageService.java:3084 - Node /172.22.0.4:7000 state jump to NORMAL

docker logs user_cassandra04_1  | grep 'jump'

## Affichage : 

## INFO  [main] 2024-01-11 15:40:00,208 StorageService.java:3084 - Node /172.22.0.5:7000 state jump to NORMAL


docker exec -it user_cassandra01_1 nodetool status 


#################################################################################################################

## On vérifie finalement que le cluster est opérationnel également au sens "service cassandra" :

#################################################################################################################

docker exec -it user_cassandra01_1 nodetool status 

## Affichage : 

## 

	Datacenter: Nord
 
	================
 
	Status=Up/Down
 
	|/ State=Normal/Leaving/Joining/Moving
 
	--  Address     Load        Tokens  Owns (effective)  Host ID                               Rack      
 
	UN  172.22.0.2  109.41 KiB  16      51.7%             d321d3c4-effb-46c4-abaf-3cb9726dcabd  Winterfell
 
	UN  172.22.0.3  109.33 KiB  16      49.2%             71a5ec7f-58c8-474b-8000-5cdb0f4a9886  Winterfell
 


	Datacenter: Terres-de-la-Couronne
 
	=================================
 
	Status=Up/Down
 
	|/ State=Normal/Leaving/Joining/Moving
 
	--  Address     Load        Tokens  Owns (effective)  Host ID                               Rack      
 
	UN  172.22.0.5  104.36 KiB  16      51.1%             6fed90cf-42f8-4ef8-85db-e1829dcc9940  Port-Real 
 
	UN  172.22.0.4  104.37 KiB  16      48.0%             993d2a5e-b6ee-4a67-8dce-223fe8b4413a  Port-Real 





#################################################################################################################

## Fin du lancement du cluster Cassandra 4 noeuds

#################################################################################################################




## Affichons que les conteneurs sont bien en cours d'exécution : 

docker ps -a 

## Pour afficher les logs d'un des conteneurs en cours d'exécution : 

docker logs cassandra-cluster_6_noeuds_2_dc-gitpod-cassandra01-1

# Surveillance d'un cluster Cassandra dans Docker :

nodetool est un programme en ligne de commande qui offre un large choix sur la façon d'examiner le cluster, de comprendre son activité et de le modifier.

nodetool permet d'obtenir des statistiques sur le cluster, de voir les plages de token maintenus par chaque nœud et les diverses tâches de gestion

(ex. : déplacement de données d'un nœud à un autre, la mise hors service de nœuds, la réparation de nœuds, etc.)

## Utilisation de nodetool : On peut utiliser la commande "nodetool status" sur n'importe lequel des nœuds pour voir l'état des nœuds du cluster :

docker exec -it cassandra-cluster_6_noeuds_2_dc-gitpod-cassandra11-1 nodetool status




## Pour vérifier le bon démarrage du service cassandra sur un noeud : 

docker logs cassandra-cluster_6_noeuds_2_dc-gitpod-cassandra22-1 | grep 'jump'

## Affichage des services en cours d'écoute sur les ports 904* :	

netstat -l | grep 904


## La commandes "nodetool info" apporte des informations complémentaires :

docker exec -it cassandra-cluster_6_noeuds_2_dc-gitpod-cassandra01-1 nodetool info

### Affichage :

ID                     : f4539d5f-6e29-417a-b2c2-f59d72101120

Gossip active          : true

Native Transport active: true

Load                   : 75.19 KiB

Generation No          : 1692971004

Uptime (seconds)       : 702

Heap Memory (MB)       : 87.96 / 251.00

Off Heap Memory (MB)   : 0.00

Data Center            : dc1

Rack                   : rack2

Exceptions             : 0

Key Cache              : entries 11, size 984 bytes, capacity 12 MiB, 104 hits, 122 requests, 0.852 recent hit rate, 14400 save period in seconds

Row Cache              : entries 0, size 0 bytes, capacity 0 bytes, 0 hits, 0 requests, NaN recent hit rate, 0 save period in seconds

Counter Cache          : entries 0, size 0 bytes, capacity 6 MiB, 0 hits, 0 requests, NaN recent hit rate, 7200 save period in seconds

Network Cache          : size 8 MiB, overflow size: 0 bytes, capacity 15 MiB

Percent Repaired       : 100.0%

Token                  : (invoke with -T/--tokens to see all 16 tokens)



docker exec -it user_cassandra01_1 bash

cd /opt/cassandra/conf

ls

cat cassandra-env.sh

cat cassandra.yaml

cat cassandra.yaml | grep endpoint_snitch

- Noter :   endpoint_snitch -- Set this to a class that implements
- 
            endpoint_snitch: GossipingPropertyFileSnitch
  
cat cassandra-rackdc.properties 

- Noter !  GossipingPropertyFileSnitch



## To see details of which nodes own which portions of the token ring, use nodetool ring:

docker exec -it cassandra-cluster_6_noeuds_2_dc-gitpod-cassandra01-1 nodetool ring

  

## Utilisons cqlsh :  

docker exec -it cassandra-cluster_6_noeuds_2_dc-gitpod-cassandra01-1 cqlsh

### Affichage : 

Connected to formation at 127.0.0.1:9042

[cqlsh 6.1.0 | Cassandra 4.1.3 | CQL spec 3.4.6 | Native protocol v5]

Use HELP for help.


DESCRIBE KEYSPACES;

### Affichage : 

cqlsh> DESCRIBE KEYSPACES;

system       system_distributed  system_traces  system_virtual_schema

system_auth  system_schema       system_views 

EXIT;

### Affichage : 

cqlsh> EXIT

gitpod /workspace/cassandra-cluster_6_noeuds_2_dc-gitpod-cassandra01-1 (main) $ 


•	Pour arrêter le cluster :

docker compose down

# Have fun!
