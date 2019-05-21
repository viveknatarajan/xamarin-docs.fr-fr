---
title: Grand titres des pages sur iOS
description: Caractéristiques de la plateforme vous autorisons à utiliser les fonctionnalités qui est disponible uniquement sur une plateforme spécifique, sans avoir à implémenter des convertisseurs personnalisés ou des effets. Cet article explique comment utiliser la plateforme spécifique à iOS qui affiche le titre de la page sous la forme d’un titre de grande taille dans la barre de navigation d’un NavigationPage.
ms.prod: xamarin
ms.assetid: 45FD9145-8319-452C-9AE6-624431A4D43C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 01c5e4f449a1aed84a73b0284ba15e0c03deeed7
ms.sourcegitcommit: b23a107b0fe3d2f814ae35b52a5855b6ce2a3513
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65925781"
---
# <a name="large-page-titles-on-ios"></a>Grand titres des pages sur iOS

[![Télécharger l’exemple](~/media/shared/download.png) Télécharger l’exemple](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)

Cette spécifiques à la plateforme iOS est utilisé pour afficher le titre de page comme un titre de grande taille dans la barre de navigation d’un [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage), des appareils qui utilisent iOS 11 ou version ultérieure. Un titre volumineux est aligné à gauche et utilise une police plus grande et passe à un titre standard comme l’utilisateur commence à faire défiler un contenu, afin que l’écran est utilisé efficacement. Cependant, en mode paysage, le titre renvoie au centre de la barre de navigation pour optimiser la disposition du contenu. Elle est consommée dans XAML en définissant le `NavigationPage.PrefersLargeTitles` propriété jointe un `boolean` valeur :

```xaml
<NavigationPage xmlns="http://xamarin.com/schemas/2014/forms"
                xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                ...
                ios:NavigationPage.PrefersLargeTitles="true">
  ...
</NavigationPage>
```

Il peut également être consommé à partir de C# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var navigationPage = new Xamarin.Forms.NavigationPage(new iOSLargeTitlePageCS());
navigationPage.On<iOS>().SetPrefersLargeTitles(true);
```

Le `NavigationPage.On<iOS>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur iOS. Le `NavigationPage.SetPrefersLargeTitle` (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) espace de noms, détermine si les grands titres sont activées.

Condition que les grands titres sont activés sur le [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage), toutes les pages de la pile de navigation affichera les grands titres. Ce comportement peut être substitué dans les pages en définissant le `Page.LargeTitleDisplay` propriété attachée à une valeur de la `LargeTitleDisplayMode` énumération :

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             Title="Large Title"
             ios:Page.LargeTitleDisplay="Never">
  ...
</ContentPage>
```

Vous pouvez également le comportement de la page peut être substitué à partir de C# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

public class iOSLargeTitlePageCS : ContentPage
{
    public iOSLargeTitlePageCS(ICommand restore)
    {
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Never);
        ...
    }
    ...
}
```

Le `Page.On<iOS>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur iOS. Le `Page.SetLargeTitleDisplay` (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) espace de noms, contrôle le comportement de titre volumineux sur le [ `Page` ](xref:Xamarin.Forms.Page), avec le `LargeTitleDisplayMode` énumération en fournissant trois possible valeurs :

- `Always` – forcer la barre de navigation et la police taille à utiliser le grand format.
- `Automatic` – Utilisez le même style (petit ou grand) en tant que l’élément précédent dans la pile de navigation.
- `Never` – forcer l’utilisation de la barre de navigation standard, petit format.

En outre, le `SetLargeTitleDisplay` méthode peut être utilisée pour activer/désactiver les valeurs d’énumération en appelant le `LargeTitleDisplay` (méthode), qui retourne actuel `LargeTitleDisplayMode`:

```csharp
switch (On<iOS>().LargeTitleDisplay())
{
    case LargeTitleDisplayMode.Always:
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Automatic);
        break;
    case LargeTitleDisplayMode.Automatic:
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Never);
        break;
    case LargeTitleDisplayMode.Never:
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Always);
        break;
}
```

Le résultat qui est spécifié `LargeTitleDisplayMode` est appliqué à la [ `Page` ](xref:Xamarin.Forms.Page), qui contrôle le comportement de titre volumineux :

![](page-large-title-images/large-title.png "Spécifique à la plateforme effet de flou")

## <a name="related-links"></a>Liens connexes

- [PlatformSpecifics (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)
- [Création de caractéristiques de la plateforme](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
