---
title: Mise en page pour les applications de tablette et bureau
description: Cet article explique comment optimiser les présentations d’applications Xamarin.Forms pour les tablettes, par opposition aux téléphones.
ms.prod: xamarin
ms.assetid: D62F472B-4345-4983-8403-659A538B591F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/01/2016
ms.openlocfilehash: 9d1f54fa4753ba2ef44ba9b8b48a84a3ca932c4b
ms.sourcegitcommit: a1a58afea68912c79d16a3f64de9a0c1feb2aeb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55233846"
---
# <a name="layout-for-tablet-and-desktop-apps"></a>Mise en page pour les applications de tablette et bureau

Xamarin.Forms prend en charge tous les types d’appareils disponibles sur les plateformes prises en charge, en plus des téléphones, applications peuvent également s’exécuter :

* iPad,
* Tablettes Android
* Les tablettes Windows et les ordinateurs de bureau (exécutant Windows 10).

Cette page décrit brièvement :

* la prise en charge [types d’appareils](#Device_Types), et
* Comment [optimiser](#optimize) dispositions pour les tablettes et téléphones.

<a name="Device_Types" />

## <a name="device-types"></a>Types d’appareils

Appareils d’écran supérieure sont disponibles pour toutes les plateformes prises en charge par Xamarin.Forms.

### <a name="ipads-ios"></a>iPads (iOS)

Le modèle Xamarin.Forms inclut automatiquement la prise en charge de l’iPad en configurant le **Info.plist > appareils** à **universelle** (ce qui signifie que les iPhone et iPad sont pris en charge).

Pour fournir une expérience agréable de démarrage et garantir la résolution plein écran est utilisée sur tous les appareils, vous devez vous assurer une [écran de lancement de spécifiques à l’iPad](~/ios/app-fundamentals/images-icons/launch-screens.md) (à l’aide d’une table de montage séquentiel) est fourni. Cela garantit que l’application s’affiche correctement sur iPad mini iPad, appareils et iPad Pro.

Avant d’iOS 9 toutes les applications prenait le plein écran sur l’appareil, mais certains iPad peut désormais effectuer [fractionner écran multitâche](~/ios/platform/multitasking.md).
Cela signifie que votre application peut prendre jusqu'à simplement une colonne léger sur le côté de l’écran, 50 % de la largeur de l’écran ou la totalité de l’écran.

[![](tablet-images/ipad-sml.png "iPad partagé écran exemple")](tablet-images/ipad.png#lightbox "iPad exemple d’écran de fractionnement")

Fractionné fonctionnalité signifie que vous devez concevoir votre application pour fonctionner avec aussi peu que 320 pixels de larges, ou autant que 1366 pixels de large.

### <a name="android-tablets"></a>Tablettes Android

L’écosystème Android a une myriade pris en charge de tailles d’écran, à partir de téléphones petits jusqu'à des tablettes volumineux. Xamarin.Forms peut prendre en charge toutes les tailles d’écran, mais comme avec d’autres plateformes vous pouvez souhaiter ajuster votre interface utilisateur pour les appareils plus volumineux.

Lors de la prise en charge de nombreux différentes résolutions d’écran, vous pouvez fournir vos ressources d’image native dans différentes tailles pour optimiser l’expérience utilisateur.
Examinez le [ressources Android](~/android/app-fundamentals/resources-in-android/index.md) documentation (et notamment [crée des ressources pour différentes tailles d’écran](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md)) pour plus d’informations sur la structure de dossiers et des noms de fichiers dans votre application Android projet pour inclure des ressources de l’image optimisée dans votre application.

### <a name="windows-tablets-and-desktops"></a>Ordinateurs de bureau et les tablettes Windows

Pour prendre en charge des tablettes et ordinateurs de bureau exécutant Windows, vous devrez utiliser [prise en charge Windows UWP](~/xamarin-forms/platform/windows/installation/index.md), quelles sont les builds des applications universelles qui s’exécutent sur Windows 10.

Applications qui s’exécutent sur des ordinateurs de bureau et les tablettes Windows peuvent être redimensionnées en outre aux dimensions arbitraires en mode plein écran en cours d’exécution.

[![](tablet-images/splitscreen-sml.png "Windows fractionner écran exemple")](tablet-images/splitscreen.png#lightbox "Windows fractionner l’exemple d’écran")


<a name="optimize" />

## <a name="optimizing-for-tablet-and-desktop"></a>Optimisation pour tablette et bureau

Vous pouvez ajuster votre interface utilisateur de Xamarin.Forms en fonction de si un téléphone ou périphérique de tablette/bureau est utilisé. Cela signifie que vous pouvez optimiser l’expérience utilisateur pour les appareils à grand écran telles que des tablettes et ordinateurs de bureau.


### <a name="deviceidiom"></a>Device.Idiom

Vous pouvez utiliser la [ `Device` ](~/xamarin-forms/platform/device.md) classe pour modifier le comportement de votre application ou l’interface utilisateur. À l’aide de la `Device.Idiom` énumération, vous pouvez

```csharp
if (Device.Idiom == TargetIdiom.Phone)
{
    HeroImage.Source = ImageSource.FromFile("hero.jpg");
} else {
    HeroImage.Source = ImageSource.FromFile("herotablet.jpg");
}
```

Cette approche peut être développée pour apporter des modifications significatives aux dispositions de page individuelle, ou même à restituer les pages complètement différents sur les écrans plus grands.

### <a name="leveraging-masterdetailpage"></a>En tirant parti de MasterDetailPage

Le [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) est idéal pour les écrans plus grands, en particulier sur l’iPad où il utilise le [ `UISplitViewController` ](xref:UIKit.UISplitViewController) pour fournir une expérience iOS native.

Révision [ce billet de blog Xamarin](https://blog.xamarin.com/bringing-xamarin-forms-apps-to-tablets/) pour voir comment vous pouvez adapter votre interface utilisateur afin que les téléphones utilisent une disposition et écrans plus grands peuvent utiliser une autre (avec le `MasterDetailPage`).



## <a name="related-links"></a>Liens associés

- [Blog de Xamarin](https://blog.xamarin.com/bringing-xamarin-forms-apps-to-tablets/)
- [Exemple de MyShoppe](https://github.com/jamesmontemagno/myshoppe)
