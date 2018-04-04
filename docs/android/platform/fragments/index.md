---
title: Fragments
description: Android 3.0 introduit des Fragments, en montrant comment prendre en charge des modèles plus flexibles pour les nombreux différentes tailles d’écran trouvés sur les téléphones et tablettes. Cet article couvre l’utilisation de Fragments de développer des applications de Xamarin.Android et également la prise en charge des Fragments sur Android des périphériques (API niveau 11) 3.0.
ms.prod: xamarin
ms.assetid: 1AFB4242-A337-F8E0-83D9-B8D850D7F384
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/15/2018
ms.openlocfilehash: 08c2edb3acc15518c7d5a69f227fb9ef819887be
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="fragments"></a>Fragments

_Android 3.0 introduit des Fragments, en montrant comment prendre en charge des modèles plus flexibles pour les nombreux différentes tailles d’écran trouvés sur les téléphones et tablettes. Cet article couvre l’utilisation de Fragments de développer des applications de Xamarin.Android et également la prise en charge des Fragments sur Android des périphériques (API niveau 11) 3.0._

## <a name="fragments-overview"></a>Vue d’ensemble des fragments

Les tailles d’écran supérieure trouvés sur la plupart des tablettes ajouté une couche supplémentaire de complexité au développement Android : une mise en page pour le petit écran ne fonctionne pas nécessairement également pour les écrans plus grand et vice versa. Pour réduire le nombre de complications Cela introduit, Android 3.0 a ajouté deux nouvelles fonctionnalités, *Fragments* et *prise en charge de Packages*.

Fragments peuvent être considérés comme des modules d’interface utilisateur. Ils permettent aux développeurs de diviser l’interface utilisateur en parties isolés et réutilisables qui peuvent être exécutés dans des activités distinctes. Au moment de l’exécution, les activités elles-mêmes décidera les Fragments à utiliser.

Prise en charge des Packages ont été appelées à l’origine *compatibilité bibliothèques* et autorisée de Fragments pour être utilisée sur les appareils qui exécutent des versions d’Android avant la version 3.0 Android (API niveau 11).

Par exemple, l’image ci-dessous montre comment une application utilise des Fragments sur différents facteurs de forme de périphérique.

[![Diagramme de l’utilisation des fragments dans les tablettes et téléphones](images/00.png)](images/00.png#lightbox)

*Fragment A* contient une liste, tandis que *Fragment B* contient les détails d’un élément sélectionné dans cette liste. Lorsque l’application est exécutée sur une tablette, il peut afficher les deux Fragments sur la même activité. Lors de la même application est exécutée sur un combiné (avec sa taille minimale de l’écran), les Fragments sont hébergés dans les deux activités distinctes. Fragment A et B du Fragment sont les mêmes sur les deux facteurs de forme, mais les activités qui les hébergent sont différentes.

Pour une activité coordonner et gérer tous ces Fragments, Android introduit une nouvelle classe appelée le *FragmentManager*. Chaque activité possède sa propre instance d’un `FragmentManager` pour l’ajout, suppression et recherche hébergé Fragments. Le diagramme suivant illustre la relation entre les Fragments et des activités :

[![Diagramme illustrant les relations entre les activités, le Gestionnaire de Fragment et Fragments](images/01.png)](images/01.png#lightbox)

Certains ce qui est, Fragments peuvent être considérés comme contrôles composites, ou en tant qu’activités de mini. Ils regroupement des éléments d’interface utilisateur dans des modules réutilisables qui peuvent ensuite être utilisées de façon indépendante par les développeurs dans les activités. Un Fragment dispose d’une hiérarchie de la vue, tout comme une activité, mais, contrairement à une activité, il peut être partagé entre les écrans. Vues de la différence des Fragments de Fragments ont leur propre cycle de vie ; les vues ne le font pas.

Alors que l’activité est un ordinateur hôte à un ou plusieurs Fragments, il n’est pas directement connaissance des Fragments eux-mêmes. De même, les Fragments ne sont pas directement prenant en charge d’autres Fragments dans l’activité d’hébergement. Toutefois, les Fragments et les activités sont prenant en charge de la `FragmentManager` dans leur activité. À l’aide de la `FragmentManager`, il est possible pour une activité ou d’un Fragment obtenir une référence à une instance spécifique d’un Fragment, puis appelez des méthodes sur cette instance. De cette façon, l’activité ou les Fragments peuvent communiquer et interagir avec d’autres Fragments.

Ce guide contient une couverture complète sur l’utilisation de Fragments, y compris :

-   **Création de Fragments** : comment créer un Fragment de base et les méthodes qui doivent être implémentées.
-   **Gestion et les Transactions de fragment** – comment manipuler les Fragments en cours d’exécution.
-   **Package de prise en charge Android** : comment faire pour utiliser les bibliothèques qui autorisent les Fragments à utiliser sur des versions antérieures d’Android.


## <a name="requirements"></a>Spécifications

Fragments sont disponibles dans le SDK Android, en commençant par le niveau de l’API 11 (Android version 3.0), comme indiqué dans la capture d’écran suivante :

[![En sélectionnant le niveau de l’API dans le Gestionnaire de kit de développement logiciel Android.](images/02.png)](images/02.png#lightbox)

Les fragments sont disponibles dans Xamarin.Android 4.0 et versions ultérieures. Une application de Xamarin.Android doit cibler au moins le niveau API 11 (Android 3.0) ou une version ultérieure pour pouvoir utiliser des Fragments. Le Framework cible peut être défini dans le projet de propriétés comme indiqué ci-dessous :

[![Définition du niveau de l’API de l’infrastructure cible dans les Options de projet](images/03-sml.png)](images/03.png#lightbox)

Il est possible d’utiliser des Fragments dans les versions antérieures d’Android à l’aide du Package de prise en charge Android et Xamarin.Android 4.2 ou version ultérieure. Comment effectuer cette opération est couvert plus en détail dans les documents de cette section.


## <a name="related-links"></a>Liens associés

- [La galerie de nid (exemple)](https://developer.xamarin.com/samples/monodroid/HoneycombGallery)
- [Fragments](http://developer.android.com/guide/topics/fundamentals/fragments.html)
- [Package de prise en charge](http://developer.android.com/sdk/compatibility-library.html)
- [Séminaire Web MOTODEV : Présentation de Fragments](http://motodev.adobeconnect.com/p9h1aqk3ttn/)
