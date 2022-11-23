# TestLink : Outil de Management des plans de test

TestLink est un outil opensource de management de plan de test, il vous permet de mieux documenté votre plan de test lors d'une campagne de test permetant de validé un produit (Logiciel, VNF....).

#### Fonctionnalités de l'outil :

    Gestion des utilisateur et des roles
    Gestion des campages de test
    Gestion des cas de test
    Gestion des resultats de test

Les resultats d'exécution de test peuvent également etre saisie de facon automatique dans testlink
via des api(RPC). il existe un pluging Jenkins qui s'interface avec testlink et permettant a Jenkins de fournir  via cette api les resultats de test exécuté.

a chaque nouvelles fonctionnalité du logiciel, de la VNF ou de facon globale du produit a testé, vous pouvez revenir modifier votre plan de test dans testlink pour créer les nouveaux cas de test afin de tester les nouvelles fonctionnalité.

## Déploiement de Testlink
Dans ce document nous montrons comment nous déployons testLink sous Kubernetes, l'un des objectifs étant de consolider la notions d'objets Kubernetes et de mieux comprendre les déployement avec kubernetes.

Pour notre déploiement, nous avons définit nos services et objets kubernetes qui seront créer de
facon séparé. Cela nous permet de mieurx comprendre notre déploiement.

Comme objets créer pour ce déploiement :
       - Namespace
       - service ClusterIp
       - service Nodeport
       - secret
        -deployment (mariadb et testlink)

vous trouverez ici tous les fichier YAML anterinant notre déploiement

l'ordre de déploiement est celui définit pat la liste d'objet plus haut

#### Commande de déploiement : 
        kubctl apply app-testlink-namespace.yml
        kubctl apply app-testlink-secret.yml
        kubctl apply serice-clusterip.yml
        kubctl apply serice-nodeport.yml
        kubctl apply mariadb-deployment.yml
        kubctl apply testlink-deployment.yml
        

nous avons créer un objet de type "namespace" qui représente le namespace dans lequel nous
allons effectué notre depeloiement. Les namespace ici étant des objets Kubernetes qui permettent d'isolé les environnements ou les déploiement sur un meme serveur.

Nous avons créer un  service de type "cluster IP" qui nous permet d'exposé notre application testlink aux utilisateurs externe.

Nous avons un service de type "nodeIP", qui permet aux services testlink(frontend) et mariadb(backend) de pouvoir communiquer ensemble.

Nous avons deux objets de types "deployment" : 
Le premier objet de type deployment "testlink-deployent" qui permet de déployer l'outil de gestion des plans de tests 

Le second objet de type deployment "mariadb" qui permet de déployer le service de base de donnée (mariadb) qui sera utilisé.

Nous avons également un objet de type "secret" qui nous permet de creer les secrets qui seront utilisé pour se connecté la bases de donnée mariadb et a l'outil testlink.

##

### Déploiement de testLink avec Docker-compose

Dans ce repertoire vous trouverez égament le fichier docker-compose.yml permettant également de
déployer testlink.
dans se fichier nous avons définit deux service :
le premier represente l'outil testlink (le frontend)
et le second représente la base de donné avec laquelle elle va fonctionné (backend).
noté bien que l'instruction depend_on avec la valeur mariadb permet de lier les deux services.
l'outil docker-compose déployera la base de donnée avant de déployer testlink.

creer un dossier puis copier et coller le fichier docker-compose.yml dans le repertoire précédant
puis utilisé la commande : #docker-compose up -d pour lancer le déploiement
pour avoir les logs :  #docker-compose logs
pour avoir les composants ou services déployer avce docker-compose : #docker-compose ps
![image](https://user-images.githubusercontent.com/27947973/203508250-52189064-05e3-41fd-b9ce-00d7df061785.png)

[Creer un projet sous testLink](./projet.md) \
[Créer des utilisateurs et leurs attribuer des roles](./user.md) \
[Créer un  plan de test dans un projet pour la validation ou pour une campagne de test](./testplan.md) \
[Créer des cas de tests](./testcase.md) \
[Saisie automatique des résultats de test via api](./pluging.md) \
[Créer des cas de tests](./testcase.md)

