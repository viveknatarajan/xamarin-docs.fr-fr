---
title: Création de Services Android
description: Ce guide décrit les services de Xamarin.Android, qui sont des composants Android qui permettent de travail à faire sans interface utilisateur active. Les services sont très couramment utilisés pour les tâches qui sont effectuées en arrière-plan, par exemple les calculs beaucoup de temps, téléchargement de fichiers, la lecture de musique et ainsi de suite. Il explique les différents scénarios de services sont adaptés à et montre comment les implémenter à la fois pour exécuter des tâches longues en arrière-plan, ainsi que pour fournir une interface pour les appels de procédure distante.
ms.prod: xamarin
ms.assetid: BA371A59-6F7A-F62A-02FC-28253504ACC9
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/19/2018
ms.openlocfilehash: 4ae86ca5fa47169bb5d78eb9d1116e419c23ed6d
ms.sourcegitcommit: 91a4fcb715506e18e8070bc89bf2cb14d079ad32
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/15/2019
ms.locfileid: "59574804"
---
# <a name="creating-android-services"></a>Création de Services Android

_Ce guide décrit les services de Xamarin.Android, qui sont des composants Android qui permettent de travail à faire sans interface utilisateur active. Les services sont très couramment utilisés pour les tâches qui sont effectuées en arrière-plan, par exemple les calculs beaucoup de temps, téléchargement de fichiers, la lecture de musique et ainsi de suite. Il explique les différents scénarios de services sont adaptés à et montre comment les implémenter à la fois pour exécuter des tâches longues en arrière-plan, ainsi que pour fournir une interface pour les appels de procédure distante._

## <a name="android-services-overview"></a>Vue d’ensemble des Services Android

Applications mobiles ne sont pas comme les applications de bureau. Ordinateurs de bureau ont la multitude de ressources, telles que l’écran, la mémoire, espace de stockage et un bloc d’alimentation connectée, n’est pas le cas des appareils mobiles. Ces contraintes forcer des applications mobiles à se comporter différemment. Par exemple, le petit écran sur un appareil mobile signifie généralement qu’une seule application (par exemple, activité) est visible à la fois. Autres activités sont déplacées vers l’arrière-plan et placées dans un état suspendu, où ils ne peuvent pas effectuer des tâches. Toutefois, le fait qu’une application Android soit dans l’arrière-plan ne signifie pas qu’il est impossible pour l’application continuer à travailler. 

Les applications Android sont constituées d’au moins un des quatre composants principaux suivants : _Activités_, _récepteurs de diffusion_, _fournisseurs de contenu_, et _Services_. Les activités sont la pierre angulaire de nombreuses applications Android exceptionnelles, car ils fournissent l’interface utilisateur qui permet à un utilisateur d’interagir avec l’application. Toutefois, lorsqu’il s’agit d’exécution simultanées ou de travail en arrière-plan, les activités ne sont pas toujours le meilleur choix.
 
Le mécanisme principal pour le travail en arrière-plan dans Android est le _service_. Un service Android est un composant qui est conçu pour effectuer un travail sans interface utilisateur. Un service peut télécharger un fichier, écouter de la musique ou appliquer un filtre à une image. Services peuvent également être utilisés pour la communication entre processus (_IPC_) entre les applications Android. Par exemple une application Android peut utiliser le service de lecteur de musique qui provient d’une autre application ou une application peut exposer des données (par exemple, les informations de contact d’une personne) vers d’autres applications via un service. 

Services et leur capacité à exécuter le travail en arrière-plan, sont essentielles à la fourniture d’une interface utilisateur fluides. Toutes les applications Android ont un _thread principal_ (également appelé un _thread d’interface utilisateur_) sur lequel les activités sont exécutées. Pour maintenir la réactivité de l’appareil, Android doit être en mesure de mettre à jour de l’interface utilisateur à une fréquence de 60 images par seconde. Si une application Android trop de travail s’exécute sur le thread principal, puis Android supprimera frames, ce qui entraîne à son tour l’interface utilisateur apparaisse saccadé (également parfois appelé _janky_). Cela signifie que tout travail effectué sur le thread d’interface utilisateur doit se terminer dans l’intervalle de temps entre les deux images, environ 16 millisecondes (1 seconde toutes les 60 images). 

Pour résoudre ce problème, un développeur peut utiliser des threads dans une activité pour exécuter des tâches seraient de bloquer l’interface utilisateur. Toutefois, cela peut entraîner des problèmes. Il est très possible que Android sera détruire et recréer les différentes instances de l’activité. Toutefois, Android sera détruit pas automatiquement les threads, ce qui peut entraîner des fuites de mémoire. Un parfait exemple de ceci est lorsque le [appareil est pivoté](~/android/app-fundamentals/handling-rotation.md) &ndash; Android va tenter de détruire l’instance de l’activité, puis recréez un :

![Lors de l’appareil fait pivoter, destruction de l’instance 1 et 2 d’instance est créée](images/image-01.png)

Il s’agit d’une fuite de mémoire potentielle &ndash; le thread créé par la première instance de l’activité sera toujours en cours d’exécution. Si le thread a une référence à la première instance de l’activité, cela empêchera Android à partir de l’objet de collecte de données incorrectes. Toutefois, la deuxième instance de l’activité est toujours créée (qui à son tour peut créer un nouveau thread). Faire pivoter l’appareil plusieurs fois en succession rapide peut épuiser toute la mémoire RAM et forcer Android pour mettre fin à l’application entière pour récupérer la mémoire.

En règle générale, si le travail à effectuer doit-elle survivre à une activité, puis un service doit être créé pour effectuer ce travail. Toutefois, si le travail n’est applicable dans le contexte d’une activité, puis en créant un thread pour effectuer le travail peut être plus approprié. Par exemple, création d’une miniature pour une photo qui vient d’être ajoutée à une application de galerie photo doit probablement se produire dans un service. Toutefois, un thread peut être plus approprié de lire certains musique entendre uniquement lorsqu’une activité est en premier plan.

Travail en arrière-plan peut être décomposée deux larges classifications :

1. **Tâche en cours d’exécution longue** &ndash; c’est le travail est en cours jusqu'à ce qu’explicitement arrêté. Un exemple d’un _longue tâche_ est une application qui diffuse en continu musique ou qui doivent contrôler les données collectées à partir d’un capteur. Ces tâches doivent s’exécuter même si l’application n’a pas d’interface utilisateur visible.
2. **Tâches périodiques** &ndash; (parfois appelé un _travail_) une tâche périodique est celui qui est relativement courtes (quelques secondes) et est exécuté selon une planification (par exemple, une fois par jour pendant une semaine ou peut-être juste une seule fois dans le 60 secondes). Un exemple de ceci est téléchargement d’un fichier à partir d’internet ou génération d’une miniature d’une image.

Il existe quatre types de services Android :

* **Lié Service** &ndash; A _lié service_ est un service qui a un autre composant (généralement une activité), lié à celui-ci. Un service lié fournit une interface qui permet au service d’interagir entre eux et le composant lié. Une fois qu’il n’y a plus aucun client lié au service, Android va arrêter le service. 

* **`IntentService`** &ndash; Un _`IntentService`_ est une sous-classe spécialisée de la `Service` classe qui simplifie la création de service et l’utilisation. Un `IntentService` est destiné à gérer les appels autonomes individuels. Contrairement à un service, qui peut gérer simultanément plusieurs appels, une `IntentService` ressemble plus un _fonctionne le processeur de la file d’attente_ &ndash; travail est la file d’attente et un `IntentService` traite chaque travail d’un à la fois sur un seul thread de travail. En règle générale, un`IntentService` n’est pas lié à une activité ou un Fragment. 

* **Service démarré** &ndash; A _service démarré_ est un service qui a été démarré par un autre composant Android (par exemple, une activité) et est exécuté en continu en arrière-plan jusqu'à ce que quelque chose indique explicitement la arrêt du service. Contrairement à un service lié, un service démarré n’a pas de tous les clients directement liés à celui-ci. Pour cette raison, il est important de concevoir des services de prise en main afin qu’ils peuvent être redémarrés normalement en fonction des besoins.

* **Service hybride** &ndash; A _service hybride_ est un service qui a les caractéristiques d’un _service démarré_ et un _lié service_. Un service hybride peut être démarré par lorsqu’un composant est lié à il ou elle peut être démarrée par un événement. Un composant client peut ou ne peut pas être lié à ce service hybride. Un service hybride continue de fonctionner jusqu'à ce qu’il est informé explicitement pour arrêter ou jusqu'à ce qu’aucun client plus lié à celui-ci.

Le type de service à utiliser est très dépend des exigences de l’application. En règle générale, un `IntentService` ou un service lié sont suffisantes pour la plupart des tâches que doit effectuer une application Android afin de préférence doit être accordée à un de ces deux types de services. Un `IntentService` est un bon choix pour les tâches « One-Shot », telles que le téléchargement d’un fichier, tandis qu’un service lié serait approprié lorsque les interactions fréquentes avec une activité/Fragment est obligatoire. 

Bien que la plupart des services s’exécutent en arrière-plan, il existe une sous-catégorie spéciale appelée un _service de premier plan_. Il s’agit d’un service donné une priorité plus élevée (comparée à un service normal) pour effectuer un travail de l’utilisateur (par exemple, la lecture de musique). 

Il est également possible d’exécuter un service dans son propre processus sur le même appareil, cela est parfois appelé un _service distant_ ou comme un _out-of-process service_. Cela nécessite davantage d’efforts pour créer, mais peut être utile pour lorsque, par exemple, une application donnée a besoin de partager des fonctionnalités avec d’autres applications et peut, dans certains cas, améliorer l’expérience utilisateur d’une application. 

### <a name="background-execution-limits-in-android-80"></a>Limites de l’exécution en arrière-plan dans Android 8.0

À compter de Android 8.0 (niveau d’API 26), une application Android n’est plus avoir la capacité d’exécuter librement en arrière-plan. En cas de premier plan, une application peut démarrer et exécuter les services sans aucune restriction. Quand une application passe en arrière-plan, Android accorder à l’application un certain laps de temps pour démarrer et utiliser des services. Une fois ce délai passé, l’application ne peut plus démarrer tous les services et tous les services qui ont été démarrées va être interrompues. À ce stade, il n’est pas possible de l’application effectuer des tâches. Android considère qu’une application soit au premier plan si une des conditions suivantes est remplie :

* Il existe une activité visible (démarré ou suspendu).
* L’application a démarré un service de premier plan.
* Une autre application est au premier plan et à l’aide de composants à partir d’une application qui seraient sinon en arrière-plan. Un exemple de ceci est si une Application, qui est au premier plan, est liée à un service fourni par Application B. Application B puis serait également prises en compte dans le premier plan et ne pas arrêté par Android pour en cours en arrière-plan.

Il existe certaines situations où, même si une application est en arrière-plan, Android sera sortir de veille de l’application et d’assouplir ces restrictions pendant quelques minutes, ce qui permet d’effectuer un travail de l’application :
* Une priorité élevée Firebase Cloud Message est reçue par l’application.
* L’application reçoit une diffusion. 
* L’application reçoit et exécute un `PendingIntent` en réponse à une Notification.

Les applications Xamarin.Android existantes peut-être modifier la façon dont ils effectuent le travail en arrière-plan pour éviter les problèmes qui peuvent se produire sur Android 8.0. Voici certaines des méthodes pratiques pour un service Android :

* **Planification du travail à exécuter en arrière-plan à l’aide du Planificateur de travaux Android ou [répartiteur de travail Firebase](~/android/platform/firebase-job-dispatcher.md)**  &ndash; ces deux bibliothèques fournissent une infrastructure pour les applications permet de répartir le travail en arrière-plan dans à _travaux_, une petite unité de travail. Applications peuvent ensuite planifier le travail avec le système d’exploitation, ainsi que certains critères sur lorsque le travail peut s’exécuter.
* **Démarrer le service au premier plan** &ndash; un service de premier plan est utile pour lorsque l’application doit effectuer certaines tâches en arrière-plan et l’utilisateur peut être amené à interagir périodiquement avec cette tâche. Le service de premier plan affiche une notification persistante afin que l’utilisateur est conscient que l’application s’exécute une tâche en arrière-plan et qu’il offre également un moyen pour surveiller ou interagir avec la tâche. Un exemple serait une application podcasting lire un podcast à l’utilisateur ou peut-être téléchargé un épisode du podcast afin qu’il peut être apprécié plus tard. 
* **Utiliser une priorité élevée Firebase Cloud Message (FCM)** &ndash; lorsque Android reçoit une priorité élevée FCM pour une application, il autorise cette application exécuter les services en arrière-plan pendant une courte période de temps. Ce serait une bonne alternative à disposer d’un service d’arrière-plan qui interroge une application en arrière-plan. 
* **Différer le travail pour lorsque l’application s’affiche au premier plan** &ndash; si aucune des solutions précédentes sont viables, alors que les applications doivent développer leur propres permet de suspendre et reprendre le travail lorsque l’application s’affiche au premier plan.

## <a name="related-links"></a>Liens associés

* [Limites de l’exécution en arrière-plan Android Oreo](https://www.youtube.com/watch?v=Pumf_4yjTMc)
