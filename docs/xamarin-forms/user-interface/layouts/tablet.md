---
title: Mise en page pour les applications de bureau et les Tablet PC
ms.topic: article
ms.prod: xamarin
ms.assetid: D62F472B-4345-4983-8403-659A538B591F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/01/2016
ms.openlocfilehash: f870cda73625197fb15bf19be1cdabbd675124d6
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="layout-for-tablet-and-desktop-apps"></a>Mise en page pour les applications de bureau et les Tablet PC

Xamarin.Forms prend en charge tous les types de périphérique disponibles sur les plateformes prises en charge, en plus de téléphones, applications peuvent également s’exécuter :

* iPad,
* Tablettes Android,
* Tablettes Windows et les ordinateurs de bureau (exécutant Windows 8.1 ou Windows 10).

Cette page décrit brièvement :

* la prise en charge [types d’appareils](#Device_Types), et
* Comment [optimiser](#optimize) dispositions pour les tablettes et téléphones.

<a name="Device_Types" />

## <a name="device-types"></a>Types d’appareils

Périphériques d’écran supérieure sont disponibles pour toutes les plateformes prises en charge par Xamarin.Forms.

### <a name="ipads-ios"></a>iPad (iOS)

Le modèle de Xamarin.Forms inclut automatiquement la prise en charge iPad en configurant le **Info.plist > appareils** à **universel** (ce qui signifie que les iPhone et iPad sont pris en charge).

Pour fournir une expérience de démarrage agréable et garantir la résolution plein écran est utilisée sur tous les appareils, vous devez vous assurer une [écran initial de spécifiques à l’iPad](~/ios/app-fundamentals/images-icons/launch-screens.md) (à l’aide d’un storyboard) est fourni. Cela garantit que l’application est correctement rendue sur iPad mini iPad, appareils et iPad Pro.

Avant d’iOS 9 toutes les applications s’élevait à mode plein écran sur l’appareil, mais certains iPad peut maintenant effectuer [fractionner écran multitâche](~/ios/platform/multitasking.md).
Cela signifie que votre application peut prendre qu’une colonne de fine sur le côté de l’écran, 50 % de la largeur de l’écran ou la totalité de l’écran.

[![](tablet-images/ipad-sml.png "iPad fractionnement écran exemple")](tablet-images/ipad.png#lightbox "iPad exemple d’écran de fractionnement")

Fonctionnalités de l’écran fractionné signifie que vous devez concevoir votre application pour fonctionner avec au minimum de 320 pixels de larges, ou autant que 1366 pixels de large.

### <a name="android-tablets"></a>Tablettes Android

L’écosystème Android a une multitude des tailles d’écran prises en charge, à partir de petits téléphones jusqu'à des tablettes volumineux. Xamarin.Forms peut prendre en charge toutes les tailles d’écran, mais comme avec d’autres plateformes vous pouvez souhaiter ajuster votre interface utilisateur pour les appareils plus volumineux.

Lors de la prise en charge de nombreux différentes résolutions d’écran, vous pouvez fournir vos ressources d’image native dans différentes tailles pour optimiser l’expérience utilisateur.
Examinez le [ressources Android](~/android/app-fundamentals/resources-in-android/index.md) documentation (et notamment [crée des ressources pour différentes tailles d’écran](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md)) pour plus d’informations sur la structure de dossiers et des noms de fichiers dans votre application Android projet à inclure des ressources de l’image optimisée dans votre application.

### <a name="windows-tablets-and-desktops"></a>Ordinateurs de bureau et des tablettes Windows

Pour prendre en charge les tablettes et ordinateurs de bureau Windows en cours d’exécution, vous devez utiliser un des deux types de projet pris en charge :

* [Windows 8.1](~/xamarin-forms/platform/windows/installation/tablet.md) -
  génère des applications spécifiquement pour Windows 8.1 tablettes et ordinateurs de bureau.
* [Prise en charge Windows UWP](~/xamarin-forms/platform/windows/installation/universal.md) -
  génère des applications universelles qui s’exécutent sur les Windows 10 téléphones, tablettes et ordinateurs de bureau.

Applications qui s’exécutent sur des ordinateurs de bureau et des tablettes Windows peuvent être redimensionnées en outre aux dimensions arbitraires en mode plein écran en cours d’exécution.

[![](tablet-images/splitscreen-sml.png "Fenêtres fractionnées divisent l’écran exemple")](tablet-images/splitscreen.png#lightbox "les fenêtres fractionnées divisent l’exemple de l’écran")


<a name="optimize" />

## <a name="optimizing-for-tablet-and-desktop"></a>Optimisation pour les Tablet PC et du bureau

Vous pouvez ajuster votre interface utilisateur de Xamarin.Forms selon si un téléphone ou tablette/bureau périphérique est utilisé. Cela signifie que vous pouvez optimiser l’expérience utilisateur pour les appareils grand écran tels que les tablettes et ordinateurs de bureau.


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

Cette approche peut être développée pour apporter des modifications importantes aux mises en page de la page individuelle, ou même à restituer les pages totalement différentes sur des écrans plus grands.

### <a name="leveraging-masterdetailpage"></a>En tirant parti de MasterDetailPage

Le [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) est idéale pour les écrans plus grands, en particulier sur l’iPad où il utilise le [ `UISplitViewController` ](https://developer.xamarin.com/api/type/UIKit.UISplitViewController/) pour offrir une expérience iOS natif.

Révision [ce billet de blog Xamarin](https://blog.xamarin.com/bringing-xamarin-forms-apps-to-tablets/) pour voir comment vous pouvez adapter votre interface utilisateur afin que les téléphones utilisent une mise en page et d’écrans plus grands peuvent utiliser une autre (avec le `MasterDetailPage`).



## <a name="related-links"></a>Liens associés

- [Xamarin Blog](https://blog.xamarin.com/bringing-xamarin-forms-apps-to-tablets/)
- [Exemple de MyShoppe](https://github.com/jamesmontemagno/myshoppe)
