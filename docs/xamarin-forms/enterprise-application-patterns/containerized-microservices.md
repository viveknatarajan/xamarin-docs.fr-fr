---
title: Microservices conteneurisés
description: Ce chapitre explique comment utiliser des conteneurs et microservices pour générer agile et évolutive et les applications cloud modernes fiable.
ms.prod: xamarin
ms.assetid: 5872ad92-04e0-4f1a-9691-79d5602f5683
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 33be84bc17f72c8b70d117a0742b001f1f763d3d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61300706"
---
# <a name="containerized-microservices"></a>Microservices conteneurisés

Développement d’applications client-serveur a entraîné le focus sur la création d’applications à plusieurs niveaux qui utilisent des technologies spécifiques dans chaque niveau. Ces applications sont souvent appelées *monolithique* applications et sont empaquetés sur du matériel préalable à l’échelle pour les charges maximales. Les principaux inconvénients de cette approche de développement sont le couplage étroit entre les composants de chaque niveau, que les composants individuels ne peuvent pas être facilement mis à l’échelle et le coût des tests. Une mise à jour simple peut avoir des effets imprévus sur le reste de la couche, et par conséquent, une modification apportée à un composant d’application nécessite sa totalité de la couche d’être réanalysée et redéployé.

En particulier concernant à l’ère du cloud, est que les composants individuels ne peut pas être facilement mis à l’échelle. Une application monolithique contient des fonctionnalités spécifiques à un domaine et est généralement divisée en couches fonctionnelles telles que le stockage de données, logique métier et front-end. Une application monolithique est à l’échelle par le clonage de l’application entière sur plusieurs ordinateurs, comme illustré dans la Figure 8-1.

![](containerized-microservices-images/monolithicapp.png "Approche de mise à l’échelle une application monolithique")

**Figure 8-1**: Approche de mise à l’échelle une application monolithique

## <a name="microservices"></a>Microservices

Les Microservices offrent une approche différente pour le développement d’applications et de déploiement, une approche qui convient pour l’agilité, mise à l’échelle et les exigences de fiabilité des applications cloud modernes. Une application de microservices est décomposée en composants indépendants qui fonctionnent ensemble pour fournir des fonctionnalités globales de l’application. Le microservice de terme met l’accent sur que les applications doivent être composées de services suffisamment petits pour refléter les problèmes indépendants, afin que chaque microservice implémente une fonction unique. En outre, chaque microservice a contrats bien définis afin que les autres microservices peuvent communiquer et partager des données avec elle. Des exemples classiques de microservices incluent les paniers d’achat, le traitement de stock, acheter des sous-systèmes et le traitement des paiements.

Microservices pouvez faire évoluer indépendamment, par rapport à des applications monolithiques géants à l’échelle ensemble. Cela signifie qu’une zone fonctionnelle spécifique, ce qui requiert plus le traitement d’alimentation de bande passante réseau pour prendre en charge à la demande, peut être montée au lieu d’inutilement l’évolution horizontale autres zones de l’application. Figure 8-2 illustre cette approche, où les microservices sont déployés et mis à l’échelle indépendamment, créer des instances de services sur plusieurs ordinateurs.

![](containerized-microservices-images/microservicesapp.png "Approche de Microservices application mise à l’échelle")

**Figure 8-2**: Approche de Microservices application mise à l’échelle

Montée Microservice peut être quasi instantanée, autoriser une application à s’adapter à l’évolution des charges. Par exemple, un seul microservice dans les fonctionnalités accessibles sur le web d’une application peut être le seul microservice dans l’application qui a besoin de faire évoluer pour gérer le trafic entrant supplémentaire.

Le modèle classique pour l’évolutivité des applications consiste à avoir un niveau d’équilibrage de charge, sans état avec un magasin de données externe partagée pour stocker les données persistantes. Microservices avec état gérer leurs propres données persistantes, généralement le stockage local sur les serveurs sur lesquels ils sont placés, afin d’éviter la surcharge du réseau de l’accès et la complexité d’entre les services operations. Cela permet le traitement le plus rapide possible des données et peut éliminer la nécessité de systèmes de mise en cache. En outre, les microservices avec état évolutifs partitionner généralement des données entre leurs instances, pour gérer la taille et le transfert le débit des données au-delà de laquelle un seul serveur peut prendre en charge.

Microservices prennent également en charge les mises à jour indépendantes. Ce couplage faible entre microservices fournit une évolution des applications rapides et fiables. Leur nature indépendante, distribuée prend en charge les mises à jour propagées, où seul un sous-ensemble des instances d’un seul microservice met à jour à un moment donné. Par conséquent, si un problème est détecté, une mise à jour bogué peut être restaurée, avant de mettre à jour de toutes les instances avec le code défectueux ou la configuration. De même, microservices utilisent généralement les versions de schéma, afin que les clients voient une version cohérente lorsque des mises à jour sont appliquées, quel que soit le microservice instance est en cours de communication.

Par conséquent, les applications de microservices ont des nombreux avantages sur les applications monolithiques :

-   Chaque microservice est relativement faible, facile à gérer et faire évoluer.
-   Chaque microservice peut être développé et déployé indépendamment des autres services.
-   Chaque microservice peut être montée indépendamment. Par exemple, un service de catalogue ou service de panier d’achat peut doivent être montés en charge plus d’un service commandes. Par conséquent, l’infrastructure résultante sera plus efficacement consomment des ressources lors de la montée en charge.
-   Chaque microservice isole les problèmes. Par exemple, s’il existe un problème dans un service il affecte uniquement ce service. Les autres services peuvent continuer à gérer les demandes.
-   Chaque microservice peut utiliser les dernières technologies. Microservices étant autonome et exécution côte à côte, les dernières technologies et infrastructures peuvent être utilisés, au lieu de devoir utiliser une infrastructure plus anciennes qui peut-être être utilisée par une application monolithique.

Toutefois, une solution basée sur des microservices présente également des inconvénients potentiels :

-   Choisir comment partitionner une application en microservices peut être difficile, car chaque microservice doit être entièrement autonome, bout, y compris la responsabilité de ses sources de données.
-   Les développeurs doivent implémenter la communication interservice, ce qui augmente la complexité et la latence à l’application.
-   Les transactions atomiques entre plusieurs microservices généralement ne sont pas possibles. Par conséquent, les besoins de l’entreprise doivent accepter la cohérence entre les microservices.
-   Dans la production, il existe une complexité opérationnelle dans le déploiement et gestion d’un système compromis de nombreux services indépendants.
-   Communication directe de client à microservice peut rendre difficile de refactoriser les contrats de microservices. Par exemple, au fil du temps comment le système est partitionné en services devrez peut-être modifier. Un seul service peut fractionner en deux ou plusieurs services, et deux services peuvent fusionner. Lorsque les clients communiquent directement avec les microservices, ce travail de refactorisation peut empêcher la compatibilité avec les applications clientes.

## <a name="containerization"></a>Mise en conteneur

Mise en conteneur est une approche du développement de logiciels dans lequel une application et son ensemble par version de dépendances, ainsi que sa configuration de l’environnement extraits sous forme de fichiers manifeste de déploiement, sont regroupés comme une image de conteneur, testée en tant qu’unité, et déployé sur un système d’exploitation hôte.

Un conteneur est un isolé, ressource contrôlée et portable environnement d’exploitation, où une application peut s’exécuter sans toucher les ressources d’autres conteneurs, ou l’hôte. Par conséquent, un conteneur ressemble et agit comme un ordinateur physique nouvellement installé ou d’une machine virtuelle.

Il existe de nombreuses similitudes entre les conteneurs et les machines virtuelles, comme illustré dans la Figure 8-3.

![](containerized-microservices-images/containersvsvirtualmachines.png "Approche de Microservices application mise à l’échelle")

**Figure 8-3**: Comparaison des machines virtuelles et conteneurs

Un conteneur exécute un système d’exploitation, a un système de fichiers et sont accessibles via un réseau comme s’il s’agissait d’un ordinateur physique ou virtuel. Toutefois, la technologie et les concepts utilisés par les conteneurs sont très différents à partir de machines virtuelles. Les machines virtuelles incluent les applications, les dépendances requises et un système d’exploitation complet. Les conteneurs incluent l’application et ses dépendances, mais partagent le système d’exploitation avec d’autres conteneurs en cours d’exécution en tant que processus isolés sur le système d’exploitation hôte (hormis les conteneurs Hyper-V qui s’exécutent à l’intérieur d’une machine virtuelle spéciale par conteneur). Par conséquent, les conteneurs partagent des ressources et nécessitent généralement moins de ressources que les machines virtuelles.

L’avantage d’une approche de développement et le déploiement orienté sur le conteneur est qu’il élimine la plupart des problèmes qui surviennent à partir des configurations d’environnement incohérent et les problèmes qui les accompagnent. En outre, conteneurs autorisent les fonctionnalités de montée en charge rapide des applications par instanciation des conteneurs en fonction des besoins.

Les concepts clés lors de la création et utilisation de conteneurs sont :

-   Hôte de conteneur : L’ordinateur physique ou virtuel configuré pour héberger des conteneurs. L’hôte de conteneur exécute un ou plusieurs conteneurs.
-   Image de conteneur : Une image se compose d’une union de systèmes de fichiers en couches empilés les uns sur les autres et constitue la base d’un conteneur. Une image n’a pas d’état et ne change jamais tel qu’il est déployé dans des environnements différents.
-   Conteneur : Un conteneur est une instance d’exécution d’une image.
-   Image de système d’exploitation de conteneur : Conteneurs sont déployés à partir d’images. L’image de système d’exploitation de conteneur est la première couche dans potentiellement de nombreuses couches d’images qui constituent un conteneur. Un système d’exploitation de conteneur est immuable et ne peut pas être modifié.
-   Référentiel du conteneur : Chaque fois qu’une image de conteneur est créée, l’image et ses dépendances sont stockés dans un référentiel local. Ces images peuvent être réutilisées plusieurs fois sur l’hôte de conteneur. Les images de conteneur peuvent également être stockées dans un registre public ou privé, tel que [Docker Hub](https://hub.docker.com/), afin qu’ils peuvent être utilisés sur des hôtes de conteneur différent.

Les entreprises adoptent conteneurs lors de l’implémentation de microservices et applications basées sur, Docker est devenu l’implémentation de conteneur standard qui a été adoptée par la plupart des plateformes logicielles et les fournisseurs de cloud.

L’application de référence eShopOnContainers utilise Docker pour héberger les quatre microservices en conteneur de serveur principal, comme illustré dans la Figure 8-4.

![](containerized-microservices-images/microservicesarchitecture.png "microservices de backend d’application de référence eShopOnContainers")

**Figure 8-4**: microservices de backend d’application de référence eShopOnContainers

L’architecture des services back-end de l’application de référence est décomposé en plusieurs sous-systèmes autonomes sous la forme de microservices de collaboration et de conteneurs. Chaque microservice fournit une zone de fonctionnalité : un service d’identité, un service de catalogue, un classement service et un service de panier d’achat.

Chaque microservice possède sa propre base de données, ce qui lui permet d’être entièrement découplées des autres microservices. Le cas échéant, la cohérence entre les bases de données à partir de différents microservices est obtenue à l’aide d’événements de niveau application. Pour plus d’informations, consultez [Communication entre Microservices](#communication_between_microservices).

Pour plus d’informations sur l’application de référence, consultez [.NET Microservices : Architecture pour les applications .NET en conteneurs](https://aka.ms/microservicesebook).

<a name="communication_between_client_and_microservices" />

## <a name="communication-between-client-and-microservices"></a>Communication entre Client et de Microservices

L’application mobile eShopOnContainers communique avec les microservices de backend en conteneur à l’aide *directe de client à microservice* communication, ce qui est illustrée à la Figure 8-5.

![](containerized-microservices-images/directclienttomicroservicecommunication.png "Approche de Microservices application mise à l’échelle")

**Figure 8-5**: Communication directe de client à microservice

Avec la communication client à microservice directe, l’application mobile fait des demandes à chaque microservice directement par le biais de son point de terminaison public avec un autre port TCP par microservice. En production, le point de terminaison serait généralement mappé à équilibrage de charge du microservice, qui distribue les demandes entre les instances disponibles.

> [!TIP]
> Envisagez d’utiliser la communication de la passerelle API. Communication directe de client à microservice peut présenter des inconvénients lors de la création d’un microservice volumineux et complexes en fonction des applications, mais il est largement suffisante pour une petite application. Lors de conception d’un microservice grand application avec des dizaines de microservices, envisagez d’utiliser la communication de la passerelle API. Pour plus d’informations, consultez [.NET Microservices : Architecture pour les applications .NET en conteneurs](https://aka.ms/microservicesebook).

<a name="communication_between_microservices" />

## <a name="communication-between-microservices"></a>Communication entre Microservices

Une application de microservices en est un système distribué, potentiellement en cours d’exécution sur plusieurs ordinateurs. Chaque instance de service est généralement un processus. Par conséquent, les services doivent interagir en utilisant un protocole de communication entre processus, tels que HTTP, TCP, Advanced Message Queuing Protocol (AMQP) ou des protocoles binaires, selon la nature de chaque service.

Les deux approches courantes pour la communication de microservice-microservice sont communication REST basé sur HTTP lors de l’interrogation de données et une messagerie asynchrone légère lors de la communication des mises à jour entre plusieurs microservices.

Messagerie en pilotée par événements communication asynchrone est essentiel lors de la propagation des modifications entre plusieurs microservices. Avec cette approche, un microservice publie un événement lorsque quelque chose notable se produit, par exemple, lorsqu’il met à jour une entité métier. Autres microservices peuvent s’abonner à ces événements. Ensuite, lorsqu’un microservice reçoit un événement, il met à jour ses propres entités commerciales, qui peuvent à son tour entraîner d’autres événements qui est publiés. Cette publication-abonnement fonctionnalité est généralement obtenue avec un bus d’événements.

Un bus d’événements permet la communication entre les microservices, sans nécessiter que les composants à être explicitement prenant en charge les unes des autres, comme indiqué dans la Figure 8-6 de publication / abonnement.

![](containerized-microservices-images/eventbus.png "Publication-abonnement avec un bus d’événements")

**Figure 8-6 :** Publication-abonnement avec un bus d’événements

Du point de vue de l’application, le bus d’événements est simplement un publier-s’abonner exposée via une interface de canal. Toutefois, la façon dont le bus d’événements est implémenté peut varier. Par exemple, une implémentation de bus d’événements peut utiliser RabbitMQ, Azure Service Bus ou autres bus de service tels que NServiceBus et MassTransit. Figure 8-7 illustre comment un bus d’événements est utilisé dans l’application de référence eShopOnContainers.

![](containerized-microservices-images/microservicesarchitecturewitheventbus.png "Communication asynchrone pilotée par événements dans l’application de référence")

**Figure 8-7 :** Communication asynchrone pilotée par événements dans l’application de référence

Le bus d’événements eShopOnContainers, implémenté à l’aide de RabbitMQ, fournit un-à-plusieurs asynchrone de publication-abonnement fonctionnalités. Cela signifie qu’après la publication d’un événement, il peut y avoir plusieurs abonnés à l’écoute pour le même événement. Figure 8-9 illustre cette relation.

![](containerized-microservices-images/eventdrivencommunication.png "Communication un-à-plusieurs")

**Figure 8-9**: Communication un-à-plusieurs

Cette approche de communication un-à-plusieurs utilise des événements pour implémenter des transactions commerciales qui s’étendent sur plusieurs services, en garantissant la cohérence entre les services. Une transaction éventuelle cohérente se compose d’une série d’étapes distribuées. Par conséquent, lorsque le microservice de profil de l’utilisateur reçoit la commande UpdateUser, il met à jour les détails de l’utilisateur dans sa base de données et publie l’événement UserUpdated sur le bus d’événements. Le microservice de panier d’achat et le microservice de passation êtes inscrit pour recevoir cet événement et en réponse à jour leurs informations de l’acheteur dans leurs bases de données.

> [!NOTE]
> Le bus d’événements eShopOnContainers, implémenté à l’aide de RabbitMQ, est destiné à être utilisée uniquement comme une preuve de concept. Pour les systèmes de production, les implémentations de bus d’événements autre doivent être considérées.

Pour plus d’informations sur l’implémentation de bus d’événements, consultez [.NET Microservices : Architecture pour les applications .NET en conteneurs](https://aka.ms/microservicesebook).

## <a name="summary"></a>Récapitulatif

Les Microservices offrent une approche de développement d’applications et de déploiement qui est adaptée aux exigences d’agilité, mise à l’échelle et la fiabilité des applications cloud modernes. Un des principaux avantages de microservices est qu’elles peuvent être montées indépendamment, ce qui signifie qu’une zone fonctionnelle spécifique peut être augmenté ou nécessitant plus le traitement d’alimentation de bande passante réseau pour prendre en charge à la demande, sans augmenter inutilement des zones de l’application qui ne rencontre pas d’augmentation de la demande.

Un conteneur est un isolé, ressource contrôlée et portable environnement d’exploitation, où une application peut s’exécuter sans toucher les ressources d’autres conteneurs, ou l’hôte. Les entreprises adoptent conteneurs lors de l’implémentation de microservices et applications basées sur, Docker est devenu l’implémentation de conteneur standard qui a été adoptée par la plupart des plateformes logicielles et les fournisseurs de cloud.


## <a name="related-links"></a>Liens associés

- [Téléchargez le livre électronique (PDF de 2 Mo)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (sample)](https://github.com/dotnet-architecture/eShopOnContainers)
