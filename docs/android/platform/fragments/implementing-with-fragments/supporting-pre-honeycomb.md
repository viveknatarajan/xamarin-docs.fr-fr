---
title: "Prise en charge Android nid préalable à l’aide de Packages de prise en charge"
ms.topic: article
ms.prod: xamarin
ms.assetid: DACD0C14-5DDF-7BDE-6844-80550D301307
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: 242adc94f04b3bda40238370ca924c1807a0c787
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="supporting-pre-honeycomb-android-using-support-packages"></a>Prise en charge Android nid préalable à l’aide de Packages de prise en charge

Le *Package de prise en charge Android* se compose de bibliothèques ce nouveau port certains de la nouvelle API &ndash; tels que des fragments &ndash; aux anciennes versions d’Android. Par conséquent, en ajoutant le Package de prise en charge Android, nous pouvons exécuter notre application sur les appareils Android 2.3, comme indiqué dans les écrans suivants :

![Capture d’écran procédure pas à pas de fragments](supporting-pre-honeycomb-images/00.png)

![Capture d’écran de détails de l’activité](supporting-pre-honeycomb-images/01.png)


## <a name="adding-the-support-package"></a>Ajout du Package de prise en charge

Le Package de prise en charge Android n’est pas automatiquement ajouté à une application Xamarin.Android. Xamarin fournit le [package de bibliothèque de prise en charge Android v4 NuGet](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) pour simplifier l’ajout des prise en charge des bibliothèques à une application Xamarin.Android.
Pour inclure les packages de prise en charge dans votre application inclure de Xamarin.Android le [bibliothèque de prise en charge Android v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) composant dans votre projet Xamarin.Android, comme illustré dans la capture d’écran suivante :

![Ajout du package v4 de bibliothèque de prise en charge Android](supporting-pre-honeycomb-images/02.png)

Une fois que le package a été ajouté, modifier le framework cible pour Android 2.2 ou version ultérieure :

![Capture d’écran de modification du niveau de l’API du Framework cible](supporting-pre-honeycomb-images/03.png)

En outre, assurez-vous que la version Android minimale cible le même niveau d’API :

![Capture d’écran de définition de la version minimale Android](supporting-pre-honeycomb-images/04.png)



### <a name="change-mainactivity-to-derive-from-fragmentactivity"></a>Modifier MainActivity dériver FragmentActivity

Toute activité qui utilise des fragments doit hériter de `Xamarin.Support.V4.App.FragmentActivity`. Cette classe est une étape nécessaire pour le Package de prise en charge, car elle permet de fragments pour être hébergé par les activités, quelle que soit la version d’Android. `MainActivity` requiert qu’une petite modification, il doit hériter maintenant `Android.Support.V4.App.FragmentActivity`:

```csharp
[Activity(Label = "Fragments Walkthrough", MainLauncher = true, Icon = "@drawable/launcher")]
public class MainActivity : Android.Support.V4.App.FragmentActivity
{
   protected override void OnCreate(Bundle bundle)
   {
       base.OnCreate(bundle);
       SetContentView(Resource.Layout.activity_main);
   }
}
```


### <a name="change-detailsactivity-to-derive-from-fragmentactivity"></a>Modifier DetailsActivity dériver FragmentActivity

`DetailsActivity` doit également être modifiés dans une `Activity` à un `FragmentActivity`. En tant que `FragmentManager` est compatible avec les versions d’Android nid préliminaire, le Package de prise en charge Android inclut une classe wrapper, `SupportFragmentManager`, qui fournit une compatibilité descendante. Chaque `FragmentActivity` a un `SupportFragmentManager` propriété, et `DetailsActivity` est modifiée pour utiliser le `SupportFragmentManager` au lieu du `FragmentManager`:

```csharp
[Activity(Label = "Details Activity")]
public class DetailsActivity : Android.Support.V4.App.FragmentActivity
{
   protected override void OnCreate(Bundle bundle)
   {
       base.OnCreate(bundle);
       var index = Intent.Extras.GetInt("index", 0);
       var details = DetailsFragment.NewInstance(index);
       var fragmentTransaction = SupportFragmentManager.BeginTransaction(); // Notice the change from FragmentManager to SupportFragmentManager
       fragmentTransaction.Add(Android.Resource.Id.Content, details);
       fragmentTransaction.Commit();
   }
}
```

Après avoir terminé ces modifications, nous disposons désormais d’une application qui peut s’exécuter sur Android 1.6 et versions ultérieures, et qui utilise des fragments d’adapter notre interface utilisateur à la taille de notre appareil cible.


## <a name="related-links"></a>Liens associés

- [Android prend en charge la bibliothèque v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4)
