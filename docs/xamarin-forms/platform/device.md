---
title: Classe de périphérique
ms.prod: xamarin
ms.assetid: 2F304AEC-8612-4833-81E5-B2F3F469B2DF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: 7ba3808e7b8d948d502be3f80b8830e1aaf3b52f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="device-class"></a>Classe de périphérique

Le [ `Device` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device/) classe contient un nombre de propriétés et méthodes pour aider les développeurs de personnaliser la disposition et fonctionnalités sur une base de chaque plateforme.

En plus des méthodes et propriétés pour cibler le code à des types de matériel spécifique et les tailles, les `Device` classe inclut la [BeginInvokeOnMainThread](#Device_BeginInvokeOnMainThread) méthode qui doit être utilisé lors de l’interaction avec l’interface utilisateur des contrôles threads d’arrière-plan.

<a name="providing-platform-values" />

## <a name="providing-platform-specific-values"></a>En fournissant des valeurs spécifiques à la plateforme

Avant de Xamarin.Forms 2.3.4, la plateforme de l’application a été exécutée sur pourrait être obtenue en examinant le [ `Device.OS` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Device.OS/) propriété et en la comparant à la [ `TargetPlatform.iOS` ](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.iOS/), [ `TargetPlatform.Android` ](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.Android/), [ `TargetPlatform.WinPhone` ](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.WinPhone/), et [ `TargetPlatform.Windows` ](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.Windows/) valeurs d’énumération. De même, un de le [ `Device.OnPlatform` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.OnPlatform/p/System.Action/System.Action/System.Action/System.Action/) surcharges peut servir à fournir des valeurs spécifiques à la plateforme à un contrôle.

Toutefois, depuis Xamarin.Forms 2.3.4 ces API est déconseillés et remplacés. Le [ `Device` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device/) classe contient maintenant des constantes de chaîne publique qui identifient des plates-formes – [ `Device.iOS` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.iOS/), [ `Device.Android` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.Android/), [ `Device.WinPhone` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.WinPhone/), [ `Device.WinRT` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.WinRT/), [ `Device.UWP` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.UWP/), et [ `Device.macOS` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.macOS/). De même, la [ `Device.OnPlatform` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.OnPlatform/p/System.Action/System.Action/System.Action/System.Action/) surcharges ont été remplacés par le [ `OnPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnPlatform%3CT%3E/) et [ `On` ](https://developer.xamarin.com/api/type/Xamarin.Forms.On/) API.

En c#, les valeurs spécifiques à la plateforme peuvent être fournis en créant un `switch` instruction sur le [ `Device.RuntimePlatform` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Device.RuntimePlatform/) propriété et en fournissant `case` instructions pour les plateformes requis :

```csharp
double top;
switch (Device.RuntimePlatform)
{
  case Device.iOS:
    top = 20;
    break;
  case Device.Android:
  case Device.WinPhone:
  case Device.UWP:
  default:
    top = 0;
    break;
}
layout.Margin = new Thickness(5, top, 5, 0);
```

Le [ `OnPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnPlatform%3CT%3E/) et [ `On` ](https://developer.xamarin.com/api/type/Xamarin.Forms.On/) classes fournissent les mêmes fonctionnalités en XAML :

```xaml
<StackLayout>
  <StackLayout.Margin>
    <OnPlatform x:TypeArguments="Thickness">
      <On Platform="iOS" Value="0,20,0,0" />
      <On Platform="Android, WinPhone, UWP" Value="0,0,0,0" />
    </OnPlatform>
  </StackLayout.Margin>
  ...
</StackLayout>
```

Le [ `OnPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnPlatform%3CT%3E/) classe est une classe générique et donc qu’il doit être instancié avec un `x:TypeArguments` attribut qui correspond au type cible. Dans le [ `On` ](https://developer.xamarin.com/api/type/Xamarin.Forms.On/) (classe), le [ `Platform` ](https://developer.xamarin.com/api/property/Xamarin.Forms.On.Platform/) attribut peut accepter un seul `string` valeur ou plusieurs virgules `string` valeurs.

> [!IMPORTANT]
> En fournissant un incorrect `Platform` attribut la valeur dans la `On` classe n’entraîne pas une erreur. Au lieu de cela, le code s’exécute sans la valeur spécifique à la plateforme en cours d’application.

<a name="Device_Idiom" />

## <a name="deviceidiom"></a>Device.Idiom

Le `Device.Idiom` peut être utilisé pour modifier la mise en page et les fonctionnalités en fonction de l’appareil de l’application est en cours d’exécution. Le [ `TargetIdiom` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TargetIdiom/) énumération contient les valeurs suivantes :

-  **Téléphone** – iPhone, iPod touch, Windows Phone, les appareils Android plus restrictif que les adresses IP 600 dynamiques ^
-  **Tablet** – iPad, les ordinateurs Windows 8.1, les appareils Android plus larges que les adresses IP 600 dynamiques ^
-  **Bureau** : seuls retournée dans [applications UWP](~/xamarin-forms/platform/windows/installation/universal.md) sur les ordinateurs de bureau Windows 10 (retourne `Phone` sur les appareils Windows mobiles, y compris dans les scénarios de Continuum)
-  **TV** : les appareils Tizen TV
-  **Non pris en charge** – non utilisé

*^ dynamiques n’est pas nécessairement le nombre de pixels physiques*

`Idiom` est particulièrement utile pour créer des dispositions qui tirent parti des écrans plus grands, comme suit :

```csharp
if (Device.Idiom == TargetIdiom.Phone) {
    // layout views vertically
} else {
    // layout views horizontally for a larger display (tablet or desktop)
}
```

<a name="Device_Styles" />

## <a name="devicestyles"></a>Device.Styles

Le [ `Styles` propriété](~/xamarin-forms/user-interface/styles/index.md) contient les définitions de style intégré qui peuvent être appliquées à certains contrôles (tels que `Label`) `Style` propriété. Les styles disponibles sont :

* BodyStyle
* CaptionStyle
* ListItemDetailTextStyle
* ListItemTextStyle
* SubtitleStyle
* TitleStyle

<a name="Device_GetNamedSize" />

## <a name="devicegetnamedsize"></a>Device.GetNamedSize

`GetNamedSize` peut être utilisé lors de la définition [ `FontSize` ](~/xamarin-forms/user-interface/text/fonts.md) en code c# :

```csharp
myLabel.FontSize = Device.GetNamedSize (NamedSize.Small, myLabel);
someLabel.FontSize = Device.OnPlatform (
      24,         // hardcoded size
      Device.GetNamedSize (NamedSize.Medium, someLabel),
      Device.GetNamedSize (NamedSize.Large, someLabel)
);
```

<a name="Device_OpenUri" />

## <a name="deviceopenuri"></a>Device.OpenUri

Le `OpenUri` méthode peut être utilisée pour déclencher des opérations sur la plateforme sous-jacente, comme ouvrir une URL dans le navigateur web natif (**Safari** sur iOS ou **Internet** sur Android).

```csharp
Device.OpenUri(new Uri("https://evolve.xamarin.com/"));
```

Le [WebView exemple](https://github.com/xamarin/xamarin-forms-samples/blob/master/WorkingWithWebview/WorkingWithWebview/WebAppPage.cs) inclut un exemple utilisant `OpenUri` pour ouvrir les URL et également déclencher des appels téléphoniques.

Le [exemple Maps](https://github.com/xamarin/xamarin-forms-samples/blob/master/WorkingWithMaps/WorkingWithMaps/MapAppPage.cs) utilise également `Device.OpenUri` pour afficher les cartes et les itinéraires natif **mappe** applications sur iOS et Android.

<a name="Device_StartTimer" />

## <a name="devicestarttimer"></a>Device.StartTimer

Le `Device` classe a également un `StartTimer` méthode qui fournit un moyen simple pour déclencher les tâches dépendant du temps qui fonctionne dans le code commune de Xamarin.Forms (y compris les PCLs). Passez un `TimeSpan` pour définir l’intervalle et retourner `true` pour conserver le minuteur en cours d’exécution ou `false` pour l’arrêter, après l’appel en cours.

```csharp
Device.StartTimer (new TimeSpan (0, 0, 60), () => {
    // do something every 60 seconds
    return true; // runs again, or false to stop
});
```

Si le code à l’intérieur de la minuterie interagit avec l’interface utilisateur (telles que la définition du texte d’un `Label` ou l’affichage d’une alerte) doit-elle être effectuée à l’intérieur d’un `BeginInvokeOnMainThread` expression (voir ci-dessous).

<a name="Device_BeginInvokeOnMainThread" />

## <a name="devicebegininvokeonmainthread"></a>Device.BeginInvokeOnMainThread

Éléments d’interface utilisateur ne doivent jamais être accessible par les threads d’arrière-plan, tel que le code en cours d’exécution dans une horloge ou un gestionnaire d’achèvement pour les opérations asynchrones, telles que les requêtes de web. N’importe quel code en arrière-plan qui doit mettre à jour l’interface utilisateur doit être encapsulée à l’intérieur de [ `BeginInvokeOnMainThread` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.BeginInvokeOnMainThread/p/System.Action/). C’est l’équivalent de `InvokeOnMainThread` sur iOS, `RunOnUiThread` sur Android, et `Dispatcher.BeginInvoke` sur Windows Phone.

Le code de Xamarin.Forms est :

```csharp
Device.BeginInvokeOnMainThread ( () => {
  // interact with UI elements
});
```

Notez que les méthodes à l’aide de `async/await` n’avez pas besoin d’utiliser `BeginInvokeOnMainThread` s’ils sont exécutés à partir du thread d’interface utilisateur.

## <a name="summary"></a>Récapitulatif

Le Xamarin.Forms `Device` classe permet de contrôler les mises en page et les fonctionnalités sur une base de chaque plateforme - même le code (bibliothèque de classes portables ou les projets partagés) en commun.


## <a name="related-links"></a>Liens associés

- [Exemple de l’appareil](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithDevice/)
- [Exemple de styles](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [Device](https://developer.xamarin.com/api/type/Xamarin.Forms.Device/)
