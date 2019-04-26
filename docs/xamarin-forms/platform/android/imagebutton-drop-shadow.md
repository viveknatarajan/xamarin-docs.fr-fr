---
title: Ombres ImageButton sur Android
description: Caractéristiques de la plateforme vous autorisons à utiliser les fonctionnalités qui est disponible uniquement sur une plateforme spécifique, sans avoir à implémenter des convertisseurs personnalisés ou des effets. Cet article explique comment utiliser l’Android spécifique à la plateforme qui permet une ombre sur un ImageButton.
ms.prod: xamarin
ms.assetid: D3604D87-9F9F-4FE2-8B10-DF3B143C0734
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: 56415aff251149aee01c2e2eb7e335e157180962
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61293783"
---
# <a name="imagebutton-drop-shadows-on-android"></a>Ombres ImageButton sur Android

[![Télécharger l’exemple](~/media/shared/download.png) Télécharger l’exemple](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

Cette spécifiques à la plateforme Android est utilisée pour activer une ombre sur un `ImageButton`. Elle est consommée dans XAML en définissant le `ImageButton.IsShadowEnabled` propriété pouvant être liée `true`, ainsi que d’un nombre de propriétés pouvant être liées facultatifs supplémentaires qui contrôlent l’ombre :

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
       <ImageButton ...
                    Source="XamarinLogo.png"
                    BackgroundColor="GhostWhite"
                    android:ImageButton.IsShadowEnabled="true"
                    android:ImageButton.ShadowColor="Gray"
                    android:ImageButton.ShadowRadius="12">
            <android:ImageButton.ShadowOffset>
                <Size>
                    <x:Arguments>
                        <x:Double>10</x:Double>
                        <x:Double>10</x:Double>
                    </x:Arguments>
                </Size>
            </android:ImageButton.ShadowOffset>
        </ImageButton>
        ...
    </StackLayout>
</ContentPage>
```

Vous pouvez également, il peut être consommé à partir de C# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

var imageButton = new Xamarin.Forms.ImageButton { Source = "XamarinLogo.png", BackgroundColor = Color.GhostWhite, ... };
imageButton.On<Android>()
           .SetIsShadowEnabled(true)
           .SetShadowColor(Color.Gray)
           .SetShadowOffset(new Size(10, 10))
           .SetShadowRadius(12);
```

> [!IMPORTANT]
> Une ombre est dessinée dans le cadre de la `ImageButton` en arrière-plan et l’arrière-plan est dessiné uniquement si le `BackgroundColor` propriété est définie. Par conséquent, une ombre portée n’est pas dessinée si le `ImageButton.BackgroundColor` propriété n’est pas définie.

Le `ImageButton.On<Android>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur Android. Le `ImageButton.SetIsShadowEnabled` (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) espace de noms, est utilisé pour contrôler si une ombre portée est activée sur le `ImageButton`. En outre, les méthodes suivantes peuvent être appelées pour contrôler l’ombre :

- `SetShadowColor` : définit la couleur de l’ombre. La couleur par défaut est [ `Color.Default` ](xref:Xamarin.Forms.Color.Default*).
- `SetShadowOffset` : définit le décalage de l’ombre. Le décalage modifie le sens de l’ombre est castée et spécifié comme un [ `Size` ](xref:Xamarin.Forms.Size) valeur. Le `Size` valeurs de la structure sont exprimées en unités indépendantes du périphérique, avec la première valeur en cours de la distance vers la gauche (valeur négative) ou la droite (valeur positive) et la deuxième valeur en cours de la distance ci-dessus (négatif) ou en dessous (valeur positive) . La valeur par défaut de cette propriété est (0.0, 0.0), ce qui entraîne le cliché instantané est castée autour de chaque côté de la `ImageButton`.
- `SetShadowRadius`: définit le rayon de flou utilisé pour restituer l’ombre portée. La valeur de rayon par défaut est 10.0.

> [!NOTE]
> L’état d’une ombre portée peut être interrogé en appelant le `GetIsShadowEnabled`, `GetShadowColor`, `GetShadowOffset`, et `GetShadowRadius` méthodes.

Le résultat est qu’une ombre portée peut être activée sur un `ImageButton`:

![](imagebutton-drop-shadow-images/imagebutton-drop-shadow.png "ImageButton avec ombre portée")

## <a name="related-links"></a>Liens connexes

- [PlatformSpecifics (exemple)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [Création de caractéristiques de la plateforme](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific.AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
