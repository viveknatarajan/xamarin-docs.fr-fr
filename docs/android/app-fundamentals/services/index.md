---
title: "Création de Services Android"
description: "Ce guide décrit les services de Xamarin.Android, qui sont des composants Android qui permettent de travail à faire sans interface utilisateur actif. Les services sont fréquemment utilisées pour les tâches qui sont exécutées en arrière-plan, telles que des calculs beaucoup de temps, téléchargement de fichiers, audio et ainsi de suite. Il explique les différents scénarios de services sont adaptés à et montre comment les implémenter à la fois pour effectuer des tâches d’arrière-plan longues, ainsi que pour fournir une interface pour les appels de procédure distante."
ms.topic: article
ms.prod: xamarin
ms.assetid: BA371A59-6F7A-F62A-02FC-28253504ACC9
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 77a3db2b36a0fb423ac2481fbde80c1a732cac4b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="creating-android-services"></a>Création de Services Android

_Ce guide décrit les services de Xamarin.Android, qui sont des composants Android qui permettent de travail à faire sans interface utilisateur actif. Les services sont fréquemment utilisées pour les tâches qui sont exécutées en arrière-plan, telles que des calculs beaucoup de temps, téléchargement de fichiers, audio et ainsi de suite. Il explique les différents scénarios de services sont adaptés à et montre comment les implémenter à la fois pour effectuer des tâches d’arrière-plan longues, ainsi que pour fournir une interface pour les appels de procédure distante._

## <a name="android-services-overview"></a>Vue d’ensemble des Services Android

Les applications mobiles ne sont pas tels que des applications de bureau. Ordinateurs de bureau ont multitude de ressources, telles que l’espace d’écran, la mémoire, espace de stockage et un bloc d’alimentation connectés, les appareils mobiles ne sont pas. Ces contraintes forcer des applications mobiles pour se comporter différemment. Par exemple, l’écran de petite taille sur un appareil mobile signifie généralement que qu’une seule application (par exemple, d’activité) est visible à la fois. Autres activités sont déplacées vers l’arrière-plan et placées dans un état suspendu, où ils ne peuvent pas effectuer des tâches. Toutefois, parce qu’une application Android est dans l’arrière-plan ne signifie pas qu’il est impossible pour l’application continuer à travailler. 

Les applications Android sont composées d’au moins quatre composants principaux suivants : _activités_, _récepteurs de diffusion_, _fournisseurs de contenu_et _Services_. Les activités sont la pierre angulaire de nombreuses applications Android excellentes car ils fournissent l’interface utilisateur qui permet à un utilisateur d’interagir avec l’application. Toutefois, lorsqu’il s’agit d’exécution simultanées ou de travail d’arrière-plan, les activités ne sont pas toujours le meilleur choix.
 
Le mécanisme principal pour le travail d’arrière-plan dans Android est le _service_. Un service Android est un composant qui est conçu pour effectuer certaines tâches sans interface utilisateur. Un service peut télécharger un fichier, la musique ou appliquer un filtre à une image. Services peuvent également être utilisés pour la communication entre processus (_IPC_) entre les applications Android. Par exemple une application Android peut utiliser le service de lecteur de musique provient d’une autre application ou une application peut exposer des données (telles que les informations de contact d’une personne) vers d’autres applications via un service. 

Services et leur capacité à effectuer du travail en arrière-plan, sont cruciaux pour fournir une interface utilisateur fluides. Toutes les applications Android ont un _thread principal_ (également appelé un _thread d’interface utilisateur_) sur lequel les activités sont exécutées. Pour maintenir la réactivité de l’appareil, Android doit être en mesure de mettre à jour l’interface utilisateur au taux de 60 frames par seconde. Si une application Android s’exécute à la quantité de travail sur le thread principal, puis Android entraînera l’abandon de frames, ce qui entraîne à son tour l’interface utilisateur apparaisse saccadé (parfois appelé _janky_). Cela signifie que tout travail effectué sur le thread d’interface utilisateur doit se terminer dans l’intervalle de temps entre deux images, environ 16 millisecondes (1 seconde toutes les 60 images). 

Pour résoudre ce problème, un développeur peut utiliser des threads dans une activité à exécuter des tâches sont de bloquer l’interface utilisateur. Toutefois, cela peut provoquer des problèmes. Il est tout à fait possible que Android détruira et recréer les différentes instances de l’activité. Toutefois, Android ne détruira pas automatiquement les threads, ce qui peut entraîner des fuites de mémoire. Un exemple de cela est que lorsque la [appareil passe](~/android/app-fundamentals/handling-rotation.md) &ndash; Android va tenter de détruire l’instance de l’activité, puis recréez un :

![Lorsque le périphérique pivote, destruction de l’instance 1 et 2 Création](images/image-01.png)

Il s’agit d’une fuite de mémoire &ndash; thread créé par la première instance de l’activité est toujours en cours d’exécution. Si le thread a une référence à la première instance de l’activité, cela empêchera Android à partir de l’objet de collecte de données incorrectes. Toutefois, la deuxième instance de l’activité est toujours créée (qui à son tour peut créer un nouveau thread). Faire pivoter le périphérique plusieurs fois de suite, vous pouvez épuiser toute la mémoire RAM et forcer Android pour mettre fin à l’application entière pour libérer la mémoire.

En règle générale, si le travail à effectuer doit survivent à une activité, puis un service doit être créé pour effectuer ce travail. Toutefois, si le travail n’est applicable dans le contexte d’une activité, puis en créant un thread pour effectuer le travail peut être plus approprié. Par exemple, création d’une miniature pour une photo qui vient d’être ajoutée à une application de la galerie photo doit probablement se produire dans un service. Toutefois, un thread peut être plus approprié à certaines musique entendre uniquement lors d’une activité est au premier plan.

Travail d’arrière-plan peut être répartie en deux catégories générales :

1. **Tâche en cours d’exécution** &ndash; il s’agit d’un travail qui est en cours jusqu'à ce qu’arrêté de manière explicite. Un exemple d’un _longue tâche_ est une application qui diffuse en continu musique ou qui doivent contrôler les données collectées à partir d’un capteur. Ces tâches doivent s’exécuter même si l’application n’a pas d’interface utilisateur visible.
2. **Tâches périodiques** &ndash; (parfois appelé un _travail_) une tâche périodique est celui qui est relativement courtes (quelques secondes) et est exécuté selon une planification (autrement dit, une fois par jour pendant une semaine ou peut-être juste une seule fois dans le 60 secondes). Un exemple est téléchargement d’un fichier à partir d’internet ou générer une miniature pour une image.

Il existe quatre différents types de services Android :

* **Lié Service** &ndash; A _lié service_ est un service qui a un autre composant (généralement une activité), qui lui est associée. Un service lié fournit une interface qui permet au service d’interagir entre eux et le composant lié. Une fois qu’il n’y a plus aucun client lié au service, Android va arrêter le service.

* **`IntentService`** &ndash; Un  _`IntentService`_  est une sous-classe spécialisée de la `Service` classe qui simplifie la création de services et d’utilisation. Un `IntentService` est destinée à traiter des appels autonomes individuels. Contrairement à un service, ce qui peut gérer simultanément plusieurs appels, un `IntentService` ressemble davantage un _processeur de la file d’attente de travail_ &ndash; travail est la file d’attente et un `IntentService` traite chaque travail d’un à la fois sur un thread de travail unique. En règle générale, une`IntentService` n’est pas lié à une activité ou un Fragment. 

* **Démarrage du Service de** &ndash; A _service démarré_ est un service qui a été démarré par un autre composant Android (par exemple, une activité) et l’exécution en continu en arrière-plan jusqu'à ce qu’un élément indique explicitement la arrêt du service. Contrairement à un service lié, un service démarré n’a pas de tous les clients directement liés à celui-ci. Pour cette raison, il est important de concevoir les services démarrés afin qu’ils peuvent être redémarrés normalement si nécessaire.

* **Service hybride** &ndash; A _service hybride_ est un service qui a les caractéristiques d’un _service démarré_ et un _lié service_. Un service hybride peut être démarré à lorsqu’un composant est lié à il ou elle peut être démarrée par un événement. Un composant client peut ou ne peut pas être lié à ce service hybride. Un service hybride continue de fonctionner jusqu'à ce qu’elle est explicitement dit à arrêter, ou jusqu'à ce qu’aucun client plus lié à celui-ci.

Le type de service à utiliser est très dépend des exigences de l’application. En règle générale, un `IntentService` ou un service lié sont suffisantes pour une application Android doit effectuer, la plupart des tâches afin de préférence doit être accordée à un de ces deux types de services. Un `IntentService` est un bon choix pour les tâches « One-Shot », telles que le téléchargement d’un fichier, tandis qu’un service lié est approprié lorsque fréquentes interactions avec un Fragment ou l’activité est obligatoire. 

La plupart des services s’exécutent en arrière-plan, mais il existe une sous-catégorie spéciale appelée un _service de premier plan_. Il s’agit d’un service qui reçoit une priorité plus élevée (comparée à un service normal) d’effectuer un travail de l’utilisateur (par exemple, la lecture de la musique). 

Il est également possible d’exécuter un service dans son propre processus sur le même périphérique, il est parfois appelé un _service distant_ ou comme une _out-of-process service_. Cela nécessite davantage d’efforts pour créer, mais peut être utile pour lorsqu’une application doit partager des fonctionnalités avec d’autres applications et peut, dans certains cas, améliorer l’expérience utilisateur d’une application. 

Chacun de ces services possède ses propres caractéristiques et les comportements et donc est abordée plus en détail dans les guides de leurs propres.
