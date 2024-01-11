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

## Affichage du répertoire courant dans Gitpod : 
pwd

/home/gitpod

- Si on était dans une VM classique linux : on récupèrerait les fichiers ainsi :

git clone https://github.com/crystalloide/cassandra.git

cd cassandra


## Créer les répertoires : 

sudo rm -Rf /home/gitpod/cassandra/cassandra01

sudo rm -Rf /home/gitpod/cassandra/cassandra02

sudo rm -Rf /home/gitpod/cassandra/cassandra03

sudo rm -Rf /home/gitpod/cassandra/cassandra04

sudo rm -Rf /home/gitpod/cassandra/cassandra



sudo rm -Rf /workspace/Cassandra/cassandra/cassandra01

sudo rm -Rf /workspace/Cassandra/cassandra/cassandra02

sudo rm -Rf /workspace/Cassandra/cassandra/cassandra03

sudo rm -Rf /workspace/Cassandra/cassandra/cassandra04

sudo rm -Rf /workspace/Cassandra/cassandra/cassandra



sudo mkdir -p /workspace/Cassandra/cassandra/cassandra

sudo chmod 777 -Rf /workspace/Cassandra/cassandra/cassandra

sudo mkdir -p /home/gitpod/cassandra/cassandra

sudo chmod 777 -Rf /home/gitpod/cassandra/cassandra

sudo chmod 777 -Rf /home/gitpod/cassandra

sudo chmod 777 -Rf /workspace/Cassandra/cassandra


## Rappel : si on cherche des images docker disponibles en ligne dans dockerhub : 

docker search cassandra

## On lance 1 fois un container nommé ici "cassandra" qui monte un répertoire local ~/tmp dans le conteneur sur /tmp : 

docker run --name cassandra -d --mount src="$(pwd)",target=/tmp,type=bind  cassandra:4.1

## on se connecte sur le container nommé cassandra en exécution : 

docker exec -it cassandra bash

## Et on copie les fichiers de paramètrage de cassandra qui nous servirons plus tard de modèle : 

cp -r /etc/cassandra /tmp/

## On vient de récupérer le répertoire /etc/cassandra du container cassandra : 

## Le répertoire /tmp du container correspond également au répertoire "cassandra" de notre workspace)

## et il contient la configuration du serveur cassandra qui nous servira de modèle ensuite.

## On ressort du container : 

exit


## On crée maintenant -par recopie- les répertoires qui serviront de modèle pour les 4 cassandra01/02/03/04 :

ls cassandra

## (cela nous permettra de les customiser simplement à volonté si besoin) :

sudo chmod 777 -Rf /workspace/Cassandra/cassandra/cassandra

ls cassandra 

cp -r cassandra/ cassandra01/

cp -r cassandra/ cassandra02/

cp -r cassandra/ cassandra03/

cp -r cassandra/ cassandra04/

sudo chmod 777 -Rf cassandra01

sudo chmod 777 -Rf cassandra02

sudo chmod 777 -Rf cassandra03

sudo chmod 777 -Rf cassandra04

## Et on supprime notre container qui nous a servi juste à récupérer le modèle de départ :

docker ps -a 

docker stop cassandra

docker rm cassandra

docker ps -a 

## Et on supprime notre répertoire modèle :

sudo rm -Rf cassandra/


## On va créer le fichier docker-compose pour ensuite créer notre cluster cassandra 

## à 4 noeuds cassandra01/cassandra02/cassandra03/cassandra04 répartis sur 2 Datacenter :

## Exemple d'images docker disponibles de cassandra : 

## docker search cassandra 

## image: registry.axonops.com/axonops-public/axonops-docker/cassandra:4.1

## image: cassandra:4.1


## On notera au passage  dans le fichier docker-compose.yml la façon de vérifier que le container est bien lancé : 

## cat docker-compose.yaml 

##  méthodes possibles :  

##     test: ["CMD-SHELL", "[ $$(nodetool -h 127.0.0.1 -pw cassandra -u cassandra statusgossip) = running ]"]

## ou, si "nc" est installé dans l'image docker : 

##     test: ["CMD", "nc", "-z", "127.0.0.1", "9044"]


## Vérifier que le composer.yaml fait reéférence à ces emplacements : /workspace/Cassandra/cassandra/cassandra01   02    03    04 

## On lance maintenant le cluster : 

docker-compose up -d


• Pour lister l'image récupérée  :

docker images

•	Attendez quelques minutes que les conteneurs démarrent


## On contrôle le bon démarrage (plusieurs minutes) : 

docker ps -a

## Affichage : 

CONTAINER ID   IMAGE           COMMAND                  CREATED          STATUS                    PORTS                                                                                    NAMES

c23b99e54a65   cassandra:4.1   "docker-entrypoint.s…"   51 seconds ago   Up 14 seconds (healthy)   7000-7001/tcp, 7199/tcp, 9042/tcp, 9160/tcp, 0.0.0.0:9045->9045/tcp, :::9045->9045/tcp   cassandra-cassandra04-1

2e4b65478df2   cassandra:4.1   "docker-entrypoint.s…"   51 seconds ago   Up 27 seconds (healthy)   7000-7001/tcp, 7199/tcp, 9042/tcp, 9160/tcp, 0.0.0.0:9044->9044/tcp, :::9044->9044/tcp   cassandra-cassandra03-1

b4e6794c7415   cassandra:4.1   "docker-entrypoint.s…"   51 seconds ago   Up 39 seconds (healthy)   7000-7001/tcp, 7199/tcp, 9042/tcp, 9160/tcp, 0.0.0.0:9043->9043/tcp, :::9043->9043/tcp   cassandra-cassandra02-1

06f21e6a9cba   cassandra:4.1   "docker-entrypoint.s…"   51 seconds ago   Up 51 seconds (healthy)   7000-7001/tcp, 7199/tcp, 9160/tcp, 0.0.0.0:9042->9042/tcp, :::9042->9042/tcp             cassandra-cassandra01-1


docker logs cassandra-cassandra01-1 

docker logs cassandra-cassandra01-1  | grep 'jump'

# #On se connecte à un des serveurs cassandra : 

docker exec -it cassandra-cassandra01-1  bash

## On peut lanccer une commande pour voir le statu du cluster : 

nodetool status 

######################
## Une fois le cluster bien démarré, vous aurez : 
######################

docker logs cassandra-cassandra01-1 | grep 'jump'

## Affichage : 

## 	INFO  [main] 2024-01-11 15:39:05,574 StorageService.java:3084 - Node /172.22.0.2:7000 state jump to NORMAL

docker logs cassandra-cassandra02-1  | grep 'jump'

## Affichage : 

## 	INFO  [main] 2024-01-11 15:39:54,752 StorageService.java:3084 - Node /172.22.0.3:7000 state jump to NORMAL

docker logs cassandra-cassandra03-1  | grep 'jump'

## Affichage : 

## 	INFO  [main] 2024-01-11 15:39:22,650 StorageService.java:3084 - Node /172.22.0.4:7000 state jump to NORMAL

docker logs cassandra-cassandra04-1  | grep 'jump'

## Affichage : 

## INFO  [main] 2024-01-11 15:40:00,208 StorageService.java:3084 - Node /172.22.0.5:7000 state jump to NORMAL


docker exec -it cassandra-cassandra01-1 nodetool status 


#################################################################################################################

## On vérifie finalement que le cluster est opérationnel également au sens "service cassandra" :

# Surveillance d'un cluster Cassandra dans Docker :

nodetool est un programme en ligne de commande qui offre un large choix sur la façon d'examiner le cluster, de comprendre son activité et de le modifier.

nodetool permet d'obtenir des statistiques sur le cluster, de voir les plages de token maintenus par chaque nœud et les diverses tâches de gestion

(ex. : déplacement de données d'un nœud à un autre, la mise hors service de nœuds, la réparation de nœuds, etc.)

## Utilisation de nodetool : On peut utiliser la commande "nodetool status" sur n'importe lequel des nœuds pour voir l'état des nœuds du cluster :

#################################################################################################################

docker exec -it cassandra-cassandra01-1 nodetool status 

## Affichage : 

## 

Datacenter: Nord

================

Status=Up/Down

|/ State=Normal/Leaving/Joining/Moving

--  Address     Load        Tokens  Owns (effective)  Host ID                               Rack      

UN  172.18.0.3  104.33 KiB  16      50.5%             ea257e07-3ad3-4971-a0d3-eb5d97bfa07c  Winterfell

UN  172.18.0.2  109.4 KiB   16      49.2%             4742bcf7-15eb-4229-a6e4-b97ad18201b4  Winterfell


Datacenter: Terres-de-la-Couronne

=================================

Status=Up/Down

|/ State=Normal/Leaving/Joining/Moving

--  Address     Load        Tokens  Owns (effective)  Host ID                               Rack      

UN  172.18.0.4  104.34 KiB  16      50.2%             1d591df1-796e-4f03-8628-935d3fc7cb1f  Port-Real 

UN  172.18.0.5  104.35 KiB  16      50.2%             a497f085-ed67-40a0-a3d6-8c9b87b81e64  Port-Real 




#################################################################################################################

## Fin du lancement du cluster Cassandra 4 noeuds

#################################################################################################################



## Affichage des services en cours d'écoute sur les ports 904* :	

sudo apt-get install net-tools

netstat -l | grep 904

tcp        0      0 0.0.0.0:9043            0.0.0.0:*               LISTEN     

tcp        0      0 0.0.0.0:9042            0.0.0.0:*               LISTEN     

tcp        0      0 0.0.0.0:9045            0.0.0.0:*               LISTEN     

tcp        0      0 0.0.0.0:9044            0.0.0.0:*               LISTEN    

tcp6       0      0 [::]:9043               [::]:*                  LISTEN     

tcp6       0      0 [::]:9042               [::]:*                  LISTEN     

tcp6       0      0 [::]:9045               [::]:*                  LISTEN  

tcp6       0      0 [::]:9044               [::]:*                  LISTEN


## La commandes "nodetool info" apporte des informations complémentaires :

docker exec -it cassandra-cassandra01-1 nodetool info

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



docker exec -it cassandra-cassandra01-1 bash

cd /opt/cassandra/conf

ls

cat cassandra-env.sh

cat cassandra.yaml

cat cassandra.yaml | grep endpoint_snitch

- Noter :   endpoint_snitch -- Set this to a class that implements
  
            endpoint_snitch: GossipingPropertyFileSnitch
  
 - Noter le paramètre adapté au déploiement sur plusieurs Datacenters :   GossipingPropertyFileSnitch
 

cat cassandra-rackdc.properties 

 - Noter les informations sur le rack et le DC auquel le noeud est rattaché

## On ressort du container : 

exit
   
## Pour voir les détails sur quel noeud possède quelle partie des tokens, on utilise la commande nodetool ring :

docker exec -it cassandra-cassandra01-1 nodetool ring

Datacenter: Nord

==========

Address          Rack        Status State   Load            Owns                Token          

172.18.0.3       Winterfell  Up     Normal  104.33 KiB      50.45%              -8981644946389424378  

...

...

172.18.0.5       Port-Real   Up     Normal  104.35 KiB      50.19%              8724035788859790933       

172.18.0.5       Port-Real   Up     Normal  104.35 KiB      50.19%              9150322290140920323                 


  Warning: "nodetool ring" is used to output all the tokens of a node.
  
  To view status related info of a node use "nodetool status" instead.


## Utilisons maintenant le client en ligne de commande "cqlsh" :  

docker exec -it cassandra-cassandra01-1 cqlsh

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




•	Pour arrêter le cluster :

docker-compose down


# Have fun!
