---
title: Fournissant vers l’arrière de la compatibilité avec le Package de prise en charge Android
ms.prod: xamarin
ms.assetid: 7511D2F8-2B4F-4200-C74E-E967153B2E8D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/12/2017
ms.openlocfilehash: 07aec6f5fb66d4efcc114f92f0fb85d5b5b99c6f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="providing-backwards-compatibility-with-the-android-support-package"></a>Fournissant vers l’arrière de la compatibilité avec le Package de prise en charge Android

L’utilité des Fragments serait limitée sans vers l’arrière sur la compatibilité avec des périphériques (API niveau 11) 3.0 préalable Android. Pour fournir cette fonctionnalité, Google a introduit le [bibliothèque de prise en charge](http://developer.android.com/sdk/compatibility-library.html) (appelée à l’origine le *Android bibliothèque de compatibilité* lorsqu’elle a été publiée) qui backports certaines API à partir de versions plus récentes de Android aux anciennes versions d’Android. Il est le Package de prise en charge Android qui permet aux périphériques exécutant Android 2.3.3 1.6 Android (API niveau 4). (API niveau 10).

> [!NOTE]
> Uniquement les `ListFragment` et `DialogFragment` sont disponibles via le Package de prise en charge Android. Aucune des autres fragments sous-classes, telles que le `PreferenceFragment,` sont pris en charge dans le Package de prise en charge Android. Elles ne fonctionneront pas dans les 3.0 applications préalable Android. 


## <a name="adding-the-support-package"></a>Ajout du Package de prise en charge

Le Package de prise en charge Android n’est pas automatiquement ajouté à une application Xamarin.Android. Xamarin fournit le [package de bibliothèque de prise en charge Android v4 NuGet](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) pour simplifier l’ajout des prise en charge des bibliothèques à une application Xamarin.Android. Pour inclure les packages de prise en charge dans votre application inclure de Xamarin.Android le [bibliothèque de prise en charge Android v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) composant dans votre projet Xamarin.Android, comme illustré dans la capture d’écran suivante : 

[![Package v4 de capture d’écran de bibliothèque de prise en charge Android ajouté au projet](providing-backwards-compatibility-images/02-sml.png)](providing-backwards-compatibility-images/02.png#lightbox)

Après ont effectué ces étapes, il devient possible d’utiliser des Fragments dans les versions antérieures d’Android. Les API de Fragment fonctionnera désormais même dans les versions antérieures, avec les exceptions suivantes : 

-   **Modifier la Version Android minimale** &ndash; l’application n’a plus besoin de cibler Android version 3.0 ou ultérieure, comme indiqué ci-dessous : 

    [![Capture d’écran de Minimum Android cible défini dans le manifeste Android](providing-backwards-compatibility-images/03-sml.png)](providing-backwards-compatibility-images/03.png#lightbox)

-   **Étendre FragmentActivity** &ndash; les activités qui hébergent des Fragments doit maintenant hériter `Android.Support.V4.App.FragmentActivity` et non à partir `Android.App.Activity` . 

-   **Mettre à jour des espaces de noms** &ndash; Classes qui héritent de `Android.App.Fragment` doit maintenant hériter `Android.Support.V4.App.Fragment` . Supprimer à l’aide de l’instruction « `using Android.App;` » en haut du code source de fichier et remplacez-le par « `using Android.Support.V4.App` ». 

-   **Utilisez SupportFragmentManager** &ndash; `Android.Support.V4.App.FragmentActivity` expose un `SupportingFragmentManager` propriété qui doit être utilisée pour obtenir une référence à la `FragmentManager` . Par exemple : 

```csharp
FragmentTransaction fragmentTx = this.SupportingFragmentManager.BeginTransaction();
DetailsFragment detailsFrag = new DetailsFragment();
fragmentTx.Add(Resource.Id.fragment_container, detailsFrag);
fragmentTx.Commit();
```

Avec ces modifications en place, il sera possible d’exécuter une application basée sur un Fragment sur Android 1.6 ou 2.x ainsi que sur nid et Sandwich de glace. 


## <a name="related-links"></a>Liens associés

- [Prise en charge Android bibliothèque v4 NuGet](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)
