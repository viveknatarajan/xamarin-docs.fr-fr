---
title: Fourniture de compatibilité avec le Package de prise en charge Android descendante
ms.prod: xamarin
ms.assetid: 7511D2F8-2B4F-4200-C74E-E967153B2E8D
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/12/2017
ms.openlocfilehash: 48ef40ce8560fd9fbb842dde70622d968591ab98
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57666903"
---
# <a name="providing-backwards-compatibility-with-the-android-support-package"></a>Fourniture de compatibilité avec le Package de prise en charge Android descendante

L’utilité des Fragments serait limitée sans des raisons de compatibilité avec des appareils (11 de niveau API) 3.0 préalables à Android vers l’arrière. Pour fournir cette fonctionnalité, Google a introduit le [bibliothèque de prise en charge](https://developer.android.com/sdk/compatibility-library.html) (appelé à l’origine le *bibliothèque de compatibilité Android* quand il a été publié) les backports certaines des API à partir de versions plus récentes de Android aux anciennes versions d’Android. Il est le Package de prise en charge Android qui permet aux appareils exécutant Android 1.6 (API de niveau 4) pour Android 2.3.3. (Niveau d’API 10).

> [!NOTE]
> Uniquement les `ListFragment` et `DialogFragment` sont disponibles via le Package de prise en charge Android. Aucune autre Fragment sous-classes, telles que le `PreferenceFragment,` sont pris en charge dans le Package de prise en charge Android. Ils ne fonctionnent pas dans les 3.0 applications préalables à Android. 


## <a name="adding-the-support-package"></a>Ajout du Package de prise en charge

Le Package de prise en charge Android n’est pas automatiquement ajouté à une application Xamarin.Android. Xamarin fournit le [le package NuGet bibliothèque prend en charge Android v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) pour simplifier l’ajout des prise en charge les bibliothèques à une application Xamarin.Android. Pour inclure les packages de prise en charge dans votre Xamarin.Android application incluent la [v4 de la bibliothèque de prise en charge Android](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) composant dans votre projet Xamarin.Android, comme illustré dans la capture d’écran suivante : 

[![Package v4 de capture d’écran de bibliothèque de prise en charge Android ajouté au projet](providing-backwards-compatibility-images/02-sml.png)](providing-backwards-compatibility-images/02.png#lightbox)

Une fois ces étapes ont été effectuées, il devient possible d’utiliser des Fragments dans les versions antérieures d’Android. Les API de Fragment fonctionnera maintenant même dans les versions antérieures, avec les exceptions suivantes : 

-   **Modifier la Version Android minimale** &ndash; l’application n’a plus besoin de cibler Android 3.0 ou version ultérieure, comme indiqué ci-dessous : 

    [![Capture d’écran de minimale d’Android cible définie sous le manifeste Android](providing-backwards-compatibility-images/03-sml.png)](providing-backwards-compatibility-images/03.png#lightbox)

-   **Étendre FragmentActivity** &ndash; les activités qui hébergent des Fragments doivent désormais hériter `Android.Support.V4.App.FragmentActivity` et non à partir `Android.App.Activity` . 

-   **Mettre à jour les espaces de noms** &ndash; Classes qui héritent de `Android.App.Fragment` doit désormais hériter de `Android.Support.V4.App.Fragment` . Supprimer à l’aide de l’instruction « `using Android.App;` » en haut du code source de fichier et remplacez-le par « `using Android.Support.V4.App` ». 

-   **Utilisez SupportFragmentManager** &ndash; `Android.Support.V4.App.FragmentActivity` expose un `SupportingFragmentManager` propriété qui doit être utilisée pour obtenir une référence à la `FragmentManager` . Exemple : 

```csharp
FragmentTransaction fragmentTx = this.SupportingFragmentManager.BeginTransaction();
DetailsFragment detailsFrag = new DetailsFragment();
fragmentTx.Add(Resource.Id.fragment_container, detailsFrag);
fragmentTx.Commit();
```

Avec ces modifications en place, il sera possible d’exécuter une application basée sur un Fragment sur Android 1.6 ou 2.x ainsi que sur Honeycomb et Ice Cream Sandwich. 


## <a name="related-links"></a>Liens associés

- [Prise en charge Android bibliothèque v4 NuGet](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)
