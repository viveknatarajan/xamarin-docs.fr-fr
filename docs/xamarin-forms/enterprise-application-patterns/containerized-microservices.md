---
title: Microservices en conteneur
ms.prod: xamarin
ms.assetid: 5872ad92-04e0-4f1a-9691-79d5602f5683
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 461a1310ff430c16e49fa0ed6037a77b1302f769
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="containerized-microservices"></a>Microservices en conteneur

Développement d’applications client-serveur a entraîné l’accent sur la création d’applications à plusieurs niveaux qui utilisent des technologies spécifiques dans chaque couche. Ces applications sont souvent appelées *monolithique* applications et sont empaquetés sur du matériel préalable à l’échelle de charges de pointe. Les inconvénients majeurs de cette approche de développement sont le couplage étroit entre les composants au sein de chaque couche, que les composants individuels ne peuvent pas être facilement mis à l’échelle et le coût de test. Une mise à jour simple peut avoir des conséquences inattendues sur le reste de la couche, et par conséquent, une modification apportée à un composant d’application nécessite son niveau d’ensemble d’être réanalysée et redéployé.

En particulier le concernant dans la durée du cloud, est que les composants individuels ne peuvent pas être facilement mis à l’échelle. Une application monolithique contient des fonctionnalités spécifiques à un domaine et est généralement divisée par couches fonctionnelles telles que le serveur frontal, de logique métier et de stockage de données. Une application monolithique est monté en charge par le clonage de l’application entière sur plusieurs ordinateurs, comme illustré dans la Figure 8-1.

![](containerized-microservices-images/monolithicapp.png "Approche de mise à l’échelle une application monolithique")

**Figure 8-1**: application monolithique approche de mise à l’échelle

## <a name="microservices"></a>Microservices

Microservices offrent une approche différente pour le développement d’applications et de déploiement, une approche qui convient à l’agilité, l’échelle et les exigences de fiabilité des applications cloud modernes. Une application de microservices est décomposée en composants indépendants qui fonctionnent ensemble pour fournir des fonctionnalités globales de l’application. Le terme microservice met en évidence que les applications doivent être composées de services assez petits pour refléter les préoccupations indépendantes, afin que chaque microservice implémente une fonction unique. En outre, chaque microservice a contrats bien définis afin que les autres microservices peuvent communiquer et partager des données avec lui. Exemples classiques de microservices incluent paniers d’achat, le traitement d’inventaire, les sous-systèmes et le traitement du paiement d’achat.

Microservices pouvez montée en puissance parallèle indépendamment, par rapport aux applications monolithiques géants susceptibles d’évoluer ensemble. Cela signifie qu’un domaine fonctionnel spécifique, ce qui nécessite plus le traitement d’alimentation de bande passante réseau pour prendre en charge à la demande, mise à l’échelle au lieu d’inutilement l’évolution horizontale autres zones de l’application. Figure 8-2 illustre cette approche, où microservices sont déployés et mis à l’échelle indépendamment, créer des instances de services sur plusieurs ordinateurs.

![](containerized-microservices-images/microservicesapp.png "Approche de mise à l’échelle Microservices application")

**Figure 8-2**: application Microservices approche de mise à l’échelle

Microservice montée en puissance parallèle peut être quasie, permettant à une application pour s’adapter à l’évolution des charges. Par exemple, un microservice unique dans la fonctionnalité web côté d’une application peut être la seule microservice dans l’application qui a besoin de montée en puissance parallèle pour gérer le trafic entrant supplémentaire.

Le modèle classique pour l’extensibilité de l’application est d’avoir un niveau de charge équilibrée, sans état, avec une banque de données externe partagée pour stocker les données persistantes. Avec état microservices gérer leurs propres données persistantes, généralement le stockage localement sur les serveurs sur lesquels ils sont placés, afin d’éviter la surcharge du réseau d’accès et la complexité entre les services operations. Cela permet le traitement le plus rapide possible de données et peut éliminer la nécessité pour les systèmes de mise en cache. En outre, évolutives microservices avec état partitionner généralement des données entre les instances, pour gérer le débit de taille et de transfert de données au delà duquel un serveur unique peut prendre en charge.

Microservices prennent également en charge les mises à jour indépendamment. Ce couplage faible entre microservices fournit une évolution de l’application rapide et fiable. Leur nature indépendante et distribuée, prend en charge les mises à jour propagées, où seul un sous-ensemble des instances d’un seul microservice mettra à jour à un moment donné. Par conséquent, si un problème est détecté, une mise à jour bogué peut être restaurée, avant de mettre à jour de toutes les instances avec la configuration ou un code défectueux. De même, microservices utilisent en général, contrôle de version de schéma, afin que les clients voient une version cohérente lorsque les mises à jour sont appliquées, quel que soit le microservice instance est en cours de communication.

Par conséquent, les applications de microservice présentent de nombreux avantages sur les applications monolithiques :

-   Chaque microservice est relativement faible, facile à gérer et faire évoluer.
-   Chaque microservice peut être développé et déployé indépendamment des autres services.
-   Chaque microservice peut être monté en charge indépendamment. Par exemple, un service de catalogue ou service de panier d’achat peut doivent être montés en plus d’un service de tri. Par conséquent, l’infrastructure obtenue sera plus efficacement consomment des ressources lors de la montée en puissance parallèle.
-   Chaque microservice isole les problèmes. Par exemple, s’il existe un problème dans un service il affecte uniquement ce service. Les autres services peuvent continuer à traiter les demandes.
-   Chaque microservice pouvez utiliser les technologies les plus récentes. Microservices étant autonome et exécution côte à côte, les dernières technologies et infrastructures peuvent être utilisés, au lieu de devoir utiliser une infrastructure plus anciens qui peut-être être utilisée par une application monolithique.

Toutefois, une solution en fonction de microservice a également des inconvénients potentiels :

-   Choisir comment partitionner une application en microservices peut être difficile, car chaque microservice doit être complètement autonome, de bout en bout, y compris la responsabilité de ses sources de données.
-   Les développeurs doivent implémenter la communication interservices, qui ajoute la complexité et la latence à l’application.
-   Les transactions atomiques entre plusieurs microservices généralement ne sont pas possibles. Par conséquent, les besoins de l’entreprise doivent adopter cohérence éventuelle entre microservices.
-   En production, il existe une complexité opérationnelle dans le déploiement et la gestion d’un système compromis de nombreux services indépendants.
-   Communication client à microservice directe peut rendre difficile à refactoriser les contrats de microservices. Par exemple, au fil du temps comment le système est partitionné en services devrez peut-être modifier. Un seul service peut fractionner en deux ou plusieurs services, et fusionnez des deux services. Lorsque les clients communiquent directement avec microservices, ce travail de refactorisation peut empêcher la compatibilité avec les applications clientes.

## <a name="containerization"></a>Mise en conteneur

CONTENEURISATION des données sont une approche du développement de logiciels dans lequel une application et son contrôle de version du jeu de dépendances, ainsi que sa configuration de l’environnement abstraits en tant que fichiers manifeste de déploiement, sont regroupés comme une image de conteneur, testée en tant qu’unité, et déployé sur un système d’exploitation hôte.

Un conteneur est un isolé, ressources, portable et contrôlé par environnement d’exploitation, où une application peut s’exécuter sans toucher les ressources d’autres conteneurs, ou l’ordinateur hôte. Par conséquent, un conteneur de recherche et agit comme un ordinateur physique nouvellement installé ou d’une machine virtuelle.

Il existe de nombreuses similitudes entre les conteneurs et les ordinateurs virtuels, comme illustré dans la Figure 8-3.

![](containerized-microservices-images/containersvsvirtualmachines.png "Approche de mise à l’échelle Microservices application")

**Figure 8-3**: comparaison des machines virtuelles et conteneurs

Un conteneur exécute un système d’exploitation, a un système de fichiers et est accessible via un réseau comme s’il s’agissait d’un ordinateur physique ou virtuel. Toutefois, la technologie et les concepts utilisés par les conteneurs sont très différents des machines virtuelles. Machines virtuelles incluent les applications, les dépendances requises et système d’exploitation invité complète. Les conteneurs incluent l’application et ses dépendances, mais partagent le système d’exploitation avec d’autres conteneurs en cours d’exécution en tant que processus isolés sur le système d’exploitation (à l’exception de conteneurs Hyper-V qui s’exécutent à l’intérieur d’un ordinateur virtuel spécial par conteneur). Par conséquent, les conteneurs partagent des ressources et nécessitent généralement moins de ressources que les machines virtuelles.

L’avantage d’une approche de développement et de déploiement orienté sur le conteneur est qu’elle élimine la plupart des problèmes qui surviennent à partir des paramètres d’environnement incohérent et les problèmes qui sont fournis avec les. En outre, conteneurs autorisent la fonctionnalité de montée en puissance parallèle d’application rapide par instanciation des conteneurs selon les besoins.

Les concepts clés lors de la création et l’utilisation de conteneurs sont :

-   Hôte de conteneur : L’ordinateur physique ou virtuel configuré pour les conteneurs de l’hôte. L’hôte de conteneur exécute un ou plusieurs conteneurs.
-   Image de conteneur : Image se compose d’une union de systèmes de fichiers en couche empilées les unes sur les autres et constitue la base d’un conteneur. Une image n’a pas d’état et ne change jamais tel qu’il est déployé vers d’autres environnements.
-   Conteneur : Un conteneur est une instance d’exécution d’une image.
-   Image de système d’exploitation de conteneur : Conteneurs sont déployés à partir d’images. L’image de système d’exploitation de conteneur est la première couche potentiellement plusieurs couches d’images qui constituent un conteneur. Un système d’exploitation de conteneur est immuable et ne peut pas être modifié.
-   Référentiel de conteneurs : Chaque fois qu’une image de conteneur est créée, l’image et ses dépendances sont stockés dans un référentiel local. Ces images peuvent être réutilisées plusieurs fois sur l’hôte de conteneur. Les images de conteneur peuvent également être stockées dans un registre public ou privé, tel que [Hub d’ancrage](https://hub.docker.com/), afin qu’ils peuvent être utilisés sur tous les hôtes de conteneurs différents.

Les entreprises adoptent plus conteneurs lorsque microservice de mise en œuvre des applications et Docker est devenu l’implémentation standard de conteneur qui a été adoptée par la plupart des plateformes logicielles et les fournisseurs de cloud.

L’application de référence eShopOnContainers utilise Docker pour héberger les quatre microservices principal en conteneur, comme illustré dans la Figure 8-4.

![](containerized-microservices-images/microservicesarchitecture.png "eShopOnContainers référence application back-end microservices")

**Figure 8-4**: eShopOnContainers référence application back-end microservices

L’architecture des services principaux de l’application de référence est décomposé en plusieurs sous-systèmes autonomes sous la forme de microservices de collaboration et de conteneurs. Chaque microservice fournit une zone de fonctionnalité : un service d’identité, un service de catalogue, un tri service et un service de panier d’achat.

Chaque microservice possède sa propre base de données, ce qui permet de découpler complètement à partir de l’autre microservices. Le cas échéant, la cohérence entre les bases de données à partir de différents microservices est obtenue à l’aide d’événements de niveau application. Pour plus d’informations, consultez [Communication entre Microservices](#communication_between_microservices).

Pour plus d’informations sur l’application de référence, consultez [.NET Microservices : Architecture pour les Applications .NET en conteneur](https://aka.ms/microservicesebook).

<a name="communication_between_client_and_microservices" />

## <a name="communication-between-client-and-microservices"></a>Communication entre le Client et Microservices

L’application mobile eShopOnContainers communique avec l’à l’aide d’en conteneur principal microservices *directe du client à microservice* communication, ce qui est indiquée dans la Figure 8-5.

![](containerized-microservices-images/directclienttomicroservicecommunication.png "Approche de mise à l’échelle Microservices application")

**Figure 8-5**: client pour microservice communication directe

Communication du client à microservice direct, l’application mobile fait des demandes à chaque microservice directement par le biais de son point de terminaison public avec un autre port TCP par microservice. En production, le point de terminaison serait généralement mappé à équilibrage de charge de la microservice, qui répartit les demandes entre les instances disponibles.

> [!TIP]
> Envisagez d’utiliser la communication de passerelle API. Communication client à microservice directe peut avoir des inconvénients lors de la création un microservice important et complexe basée sur des applications, mais il est suffisante pour une petite application. Lors de la conception d’une grande microservice application avec des dizaines de microservices, envisagez d’utiliser la communication de passerelle API. Pour plus d’informations, consultez [.NET Microservices : Architecture pour les Applications .NET en conteneur](https://aka.ms/microservicesebook).

<a name="communication_between_microservices" />

## <a name="communication-between-microservices"></a>Communication entre Microservices

Une application en fonction de microservices est un système distribué, éventuellement en cours d’exécution sur plusieurs ordinateurs. Chaque instance de service est généralement un processus. Par conséquent, les services doivent interagissent à l’aide d’un protocole de communication entre processus, tels que HTTP, TCP, Advanced Message Queuing Protocol (AMQP) ou des protocoles binaires, selon la nature de chaque service.

Les deux approches courantes pour la communication de microservice à microservice sont que HTTP en fonction de communication de REST lors de l’interrogation pour les données et la messagerie asynchrone léger lors de la communication des mises à jour sur plusieurs microservices.

En fonction pilotée par événements de communication asynchrone est essentielle lors de la propagation des modifications sur plusieurs microservices. Avec cette approche, un microservice publie un événement lorsqu’un élément remarquables se produit, par exemple, lorsqu’il met à jour une entité commerciale. Autres microservices s’abonner à ces événements. Ensuite, lorsqu’un microservice reçoit un événement, il met à jour son propre entités commerciales, à son tour susceptible d’entraîner à davantage d’événements en cours de publication. Cette publication-abonnement fonctionnalités s’effectue généralement avec un bus d’événements.

Un bus d’événements permet la communication entre microservices, sans nécessiter de composants pour être explicitement prenant en charge les unes des autres, comme illustré dans la Figure 8-6 de publication / abonnement.

![](containerized-microservices-images/eventbus.png "Publication-abonnement avec un bus d’événements")

**Figure 8-6 :** publication / abonnement avec un bus d’événements

Du point de vue de l’application, le bus d’événements est simplement une publication-abonnement exposée via une interface de canal. Toutefois, l’implémentation du bus d’événements peut varier. Par exemple, une implémentation de bus d’événements peut utiliser RabbitMQ, Azure Service Bus ou autres bus de service telles que NServiceBus et MassTransit. Figure 8-7 montre comment un bus d’événements est utilisé dans l’application de référence eShopOnContainers.

![](containerized-microservices-images/microservicesarchitecturewitheventbus.png "Communication asynchrone basé sur l’événement dans l’application de référence")

**Figure 8-7 :** une communication asynchrone basé sur l’événement dans l’application de référence

Le bus d’événements eShopOnContainers, implémenté à l’aide de RabbitMQ, fournit un-à-plusieurs asynchrone de publication / abonnement fonctionnalité. Cela signifie qu’après la publication d’un événement, il peut y avoir plusieurs abonnés à l’écoute pour le même événement. Figure 8-9 illustre cette relation.

![](containerized-microservices-images/eventdrivencommunication.png "Communication de type un-à-plusieurs")

**Figure 8-9**: un-à-plusieurs communication

Cette approche de communication de type un-à-plusieurs utilise des événements pour implémenter des transactions commerciales qui s’étendent sur plusieurs services, assurer la cohérence éventuelle entre les services. Une transaction éventuelle cohérent se compose d’une série d’étapes distribuées. Par conséquent, lorsque le profil utilisateur microservice reçoit la commande UpdateUser, il met à jour les détails de l’utilisateur dans sa base de données et publie l’événement UserUpdated pour le bus d’événements. Le microservice du panier d’achat et le tri microservice sont abonnés pour recevoir cet événement et réponse mettre à jour leurs informations de l’acheteur dans leurs bases de données.

> [!NOTE]
> Le bus d’événements eShopOnContainers, implémenté à l’aide de RabbitMQ, est destiné à être utilisée uniquement comme une preuve de concept. Pour les systèmes de production, les implémentations de bus d’événements autre doivent être envisagées.

Pour plus d’informations sur l’implémentation de bus d’événements, consultez [.NET Microservices : Architecture pour les Applications .NET en conteneur](https://aka.ms/microservicesebook).

## <a name="summary"></a>Récapitulatif

Microservices offrent une approche pour le développement d’applications et de déploiement qui convient aux exigences de flexibilité, l’échelle et la fiabilité des applications cloud modernes. Un des principaux avantages de microservices est qu’ils peuvent être montés en indépendamment, ce qui signifie qu’un domaine fonctionnel spécifique peut être monté en nécessitant plus le traitement d’alimentation de bande passante réseau pour prendre en charge à la demande, sans mise à échelle inutilement des zones de l’application qui ne rencontre pas d’augmentation de la demande.

Un conteneur est un isolé, ressources, portable et contrôlé par environnement d’exploitation, où une application peut s’exécuter sans toucher les ressources d’autres conteneurs, ou l’ordinateur hôte. Les entreprises adoptent plus conteneurs lorsque microservice de mise en œuvre des applications et Docker est devenu l’implémentation standard de conteneur qui a été adoptée par la plupart des plateformes logicielles et les fournisseurs de cloud.


## <a name="related-links"></a>Liens associés

- [Téléchargez le livre électronique (PDF 2 Mo)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (sample)](https://github.com/dotnet-architecture/eShopOnContainers)
