---
title: Fragments
description: Android 3.0 a introduit des Fragments, en montrant comment prendre en charge des conceptions plus flexibles pour les nombreux différentes tailles d’écran trouvés sur les téléphones et tablettes. Cet article décrit comment utiliser des Fragments pour développer des applications Xamarin.Android et également prendre en charge les Fragments sur les appareils (11 de niveau API) 3.0 préalables à Android.
ms.prod: xamarin
ms.assetid: 1AFB4242-A337-F8E0-83D9-B8D850D7F384
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/15/2018
ms.openlocfilehash: bc4441c7ee0c36af990297bad1b0c2f0e77123f3
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50113285"
---
# <a name="fragments"></a>Fragments

_Android 3.0 a introduit des Fragments, en montrant comment prendre en charge des conceptions plus flexibles pour les nombreux différentes tailles d’écran trouvés sur les téléphones et tablettes. Cet article décrit comment utiliser des Fragments pour développer des applications Xamarin.Android et également prendre en charge les Fragments sur les appareils (11 de niveau API) 3.0 préalables à Android._

## <a name="fragments-overview"></a>Vue d’ensemble des fragments

Les tailles d’écran plus grands trouvés sur la plupart des tablettes ajouté une couche supplémentaire de complexité au développement Android, une disposition conçue pour le petit écran ne fonctionne pas nécessairement également pour les écrans plus grande et vice versa. Pour réduire le nombre de complications Cela introduit, Android 3.0 a ajouté deux nouvelles fonctionnalités, *Fragments* et *Packages de prise en charge*.

Fragments peuvent être considérés en tant que modules d’interface utilisateur. Ils permettent au développeur de diviser l’interface utilisateur en parties isolés et réutilisables qui peuvent être exécutés dans des activités distinctes. Au moment de l’exécution, les activités elles-mêmes décidera les Fragments à utiliser.

Packages de prise en charge ont été appelées à l’origine *compatibilité bibliothèques* et autorisé de Fragments pour être utilisée sur les appareils qui exécutent des versions d’Android antérieure Android 3.0 (API niveau 11).

Par exemple, l’image ci-dessous montre comment une application utilise des Fragments sur différents facteurs de forme de périphérique.

[![Diagramme de l’utilisation des fragments dans les tablettes et téléphones](images/00.png)](images/00.png#lightbox)

*Le fragment A* contient une liste, tandis que *Fragment B* contient les détails d’un élément sélectionné dans cette liste. Lorsque l’application est exécutée sur une tablette, il peut afficher les deux Fragments sur la même activité. Lors de la même application est exécutée sur un téléphone (avec sa taille d’écran plus petit), les Fragments sont hébergés dans les deux activités distinctes. Le fragment A et B du Fragment sont les mêmes sur les deux facteurs de forme, mais les activités qui les hébergent sont différentes.

Pour aider à une activité de coordonner et de gérer tous ces Fragments, Android a introduit une nouvelle classe appelée le *FragmentManager*. Chaque activité possède sa propre instance d’un `FragmentManager` pour l’ajout, suppression et recherche hébergé Fragments. Le diagramme suivant illustre la relation entre les Fragments et des activités :

[![Diagramme illustrant les relations entre l’activité, le Gestionnaire de Fragment et Fragments](images/01.png)](images/01.png#lightbox)

Dans certains salutations, Fragments peuvent être considérés en tant que contrôles composites ou en tant qu’activités de mini. Ils regroupent des éléments d’interface utilisateur dans des modules réutilisables qui peuvent ensuite servir indépendamment par les développeurs dans les activités. Un Fragment n’est pas une hiérarchie d’affichage, tout comme une activité, mais, contrairement à une activité, il peut être partagé entre les écrans. Vues de la différence des Fragments de Fragments ont leur propre cycle de vie ; les vues ne peuvent pas.

Bien que l’activité est un ordinateur hôte à un ou plusieurs Fragments, il n’est pas directement conscient des Fragments eux-mêmes. De même, les Fragments ne sont pas directement conscient des autres Fragments dans l’activité d’hébergement. Toutefois, les activités et les Fragments connaissent la `FragmentManager` dans leur activité. À l’aide de la `FragmentManager`, il est possible pour une activité ou un Fragment obtenir une référence à une instance spécifique d’un Fragment et appelez ensuite les méthodes sur cette instance. De cette façon, l’activité ou les Fragments peuvent communiquer et interagir avec les autres Fragments.

Ce guide contient une couverture complète sur l’utilisation de Fragments, y compris :

-   **Créer des Fragments** – comment créer un Fragment de base et les méthodes clés qui doivent être implémentées.
-   **Gestion et les Transactions du fragment** – comment manipuler les Fragments en cours d’exécution.
-   **Package de prise en charge Android** : comment utiliser les bibliothèques qui permettent des Fragments à utiliser dans les versions antérieures d’Android.


## <a name="requirements"></a>Configuration requise

Fragments sont disponibles dans le Kit de développement logiciel Android en commençant par le niveau d’API 11 (Android 3.0), comme illustré dans la capture d’écran suivante :

[![En sélectionnant le niveau d’API dans le Gestionnaire de kit de développement logiciel Android.](images/02.png)](images/02.png#lightbox)

Les fragments sont disponibles dans Xamarin.Android 4.0 et versions ultérieures. Une application Xamarin.Android doit cibler au moins le niveau API 11 (Android 3.0) ou une version ultérieure pour pouvoir utiliser des Fragments. Le Framework cible peut être défini dans le projet propriétés comme indiqué ci-dessous :

[![Définition du niveau de l’API du Framework cible dans les Options du projet](images/03-sml.png)](images/03.png#lightbox)

Il est possible d’utiliser des Fragments dans les versions antérieures d’Android à l’aide du Package de prise en charge Android et Xamarin.Android 4.2 ou version ultérieure. Comment effectuer cette opération est couvert plus en détail dans les documents de cette section.


## <a name="related-links"></a>Liens associés

- [Galerie Honeycomb (exemple)](https://developer.xamarin.com/samples/monodroid/HoneycombGallery)
- [Fragments](http://developer.android.com/guide/topics/fundamentals/fragments.html)
- [Package de support](http://developer.android.com/sdk/compatibility-library.html)
- [Séminaire Web MOTODEV : Présentation de Fragments](http://motodev.adobeconnect.com/p9h1aqk3ttn/)
