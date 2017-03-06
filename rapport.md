---
author: Charles Langlois & François Poitras
title: TP2
---


Couplage et cohésion
--------------------
Le système est séparé en quatres parties(paquets):

* Le paquet "UserInterface" contient les interfaces utilisateurs,
  pour administrateurs et pour clients, pour chaque sous-système(train, bateau, avion).
  le couplage au reste du système est faible, et est limitée à une référence à l'interface
  système(voir paquet "System"), et potentiellement un certain nombre de références/dépendences
  aux classes du paquet "Utilities". Le paquet n'est pas complètement développée, puisque cela serait
  hors de la portée du projet, mais on peut supposer que la cohésion des classes de ce module devrait
  être élevé, puisqu'elles n'auraient comme responsabilité que de communiquer les requêtes d'un client ou administrateur
  au système interne(par le biais d'appel à l'interface système).

* Le paquet "Utilities" contient les classes pouvant être utilisé par tout le système, 
  et qui ne dépendent pas du reste du système. Ceci inclus les classes d'énumération
  utilisés par exemple pour spécifier la classe(section) d'un siège ou d'une cabine,
  la disposition d'un siège dans une rangé(avion et train seulement), etc.
  Le couplage avec le reste du système est donc nul, et le couplage interne très faible(limité à des liens d'héritages).
  La cohésion des classes est très élevée: ces classes ont chacun un but précis et très limité en portée, offrant des
  fonctionalité relativement simple au reste du système.

* Le paquet "System" contient toutes les classes implémentant le fonctionnement interne(back-end) du système,
  ainsi que les classes implémentant une interface(API) au système(i.e. les classes héritant de la classe `System`).
  Le couplage interne est relativement élevé, nécéssaire pour implémenter la logique du système, ainsi que pour offrir
  des accès directs et rapides aux différentes composantes du système.
  La cohésion des classes de ce paquet est pour la plus part élevée, sauf pour les classes d'interfaces système.
  Les classes "FlightSystem", "CruiseSystem", "TrainSystem" et leur parent "System" offre des méthodes publiques
  permettant d'effectuer toutes les opérations qu'un utilisateur du système peut vouloir effectuer, incluant 
  des modifications du système et des requêtes sur l'état du système.
  Les classes des modules "Flight", "Train" et "Cruise" ne serve qu'à contenir l'information pertinentes
  au système, et ne contiennent que des méthodes permettant d'accéder ou de modifier cette information(lorsqu'appropriée).
  Les classes du module "Transaction" servent à contenir l'information relatif aux transactions,
  ainsi que des opérations pour effectuer ces transactions.
    
* Le paquet "Database Interface" contient les classes permettant d'offrir une interface à une base de donnée servant de stockage
  persistant. Ce paquet à un faible couplage au reste du système, mais une référence aux classes de ce paquet est gardée par 
  les interfaces systèmes du paquet "System". Les détails d'implémentation de ce paquet sont hors de la portée de ce projet,
  mais on peut supposer que la cohésion de ces classes serait élevé: elles ne doivent offrir que la capacité de lire et d'écrire
  dans une base de donnée. Un couplage implicite indirect peut être nécessaire, puisque ces interfaces peuvent avoir à connaître 
  la structure et le fonctionnement du système interne pour éviter de forcer le reste du système à fournir des méthodes servant 
  à structurer l'information pour une base de donnée(ce qui diminurait la cohésion du système interne).
  

  
Fardeau
-------

* Paquet "User Interface": le fardeau de ces classes est plutôt élevé puisqu'elles contiennent une référence aux interfaces
  systèmes du paquet "System", qui elles même ont un fardeau élevé. Par contre, ces classes ne dépendent que de l'api
  du système interne, qui doit être stable. Des changements de l'implémentation du backend ne devraient donc pas affecter ce paquet.

* Paquet "Utilities" : ce paquet peut être considéré comme faisant partie des classes fondamentales(fardeau nul).

* Paquet "System" :   Les classes de ce paquet on un fardeau plus élevé, puisqu'il y a plus de couplage interne(entre les classes du paquet),
  nécessaire pour implémenter la logique du système, et un accès direct et rapide aux composantes du système(e.g. liens bidirectionnelles).
  De plus, les classes offrant une interface publique à cette partie du système doivent nécessairement dépendre(par composition) de toutes
  les autres composantes du système(i.e. ces classes ont la responsabilité de construire les composantes du système et de garder
  des références vers les composantes nécessaires au fonctionnement du système et pouvant être accèdés par les utilisateurs).

* Paquet "Database Interface" : fardeau faible puisque faible couplage interne, et dépend uniquement des classes du paquet "Utilities".



Principes de conception
-------------------------

* Liskov Substitutability: 

* 
