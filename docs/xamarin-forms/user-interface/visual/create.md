---
title: Créer un convertisseur de Xamarin.Forms Visual
description: Xamarin.Forms Visual permet des convertisseurs être appliquées de façon sélective aux objets VisualElement, sans avoir aux contrôles Xamarin.Forms de sous-classe.
ms.prod: xamarin
ms.assetid: 80BF9C72-AC28-4AAF-9DDD-B60CBDD1CD59
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/05/2019
ms.openlocfilehash: 3c614bcd0044a0aa4a698c830d2f2af5aba6c65a
ms.sourcegitcommit: 00744f754527e5b55154365f89691caaf1c9d929
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/07/2019
ms.locfileid: "57557511"
---
# <a name="xamarinforms-visual"></a>Xamarin.Forms Visual

Xamarin.Forms Visual permet des convertisseurs être appliquées de façon sélective à `VisualElement` objets, sans avoir aux contrôles Xamarin.Forms de sous-classe. Les convertisseurs qui spécifient un `VisualType` dans le cadre de la `ExportRendererAttribute` sera utilisé pour les vues de convertisseur, plutôt que les convertisseurs de valeur par défaut. Au moment de sélection du convertisseur, le `Visual` propriété de la vue est inspectée et incluse dans le processus de sélection de convertisseur.

> [!IMPORTANT]
> Actuellement le `Visual` ne peut pas être modifiée une fois que le contrôle a été rendu, mais cela changera dans une version ultérieure.

Xamarin.Forms inclut une apparence visuelle en fonction de la conception matérielle. Pour plus d’informations, consultez [Xamarin.Forms matériau Visual](material-visual.md).

## <a name="create-a-visual"></a>Créer un élément visuel

Dans votre bibliothèque inter-plateformes, créez un type qui dérive de `IVisual`:

```csharp
public class CustomVisual : IVisual
{
}
```

## <a name="register-the-visual-type"></a>Inscrire le type d’élément visuel

Créer un convertisseur pour la vue requise et inscrire le type d’élément visuel dans le cadre des plateformes `ExportRenderAttribute`:

```csharp
using Xamarin.Forms.Material.Android;

[assembly: ExportRenderer(typeof(ContentPage), typeof(CustomPageRenderer), new[] { typeof(CustomVisual) })]
namespace MyApp.Android
{
    public class CustomPageRenderer : PageRenderer
    {
        ...
    }
}
```

## <a name="consume-the-visual"></a>Utiliser l’élément visuel

Les applications peuvent choisir à l’aide de l’élément visuel personnalisé via le `Visual` propriété :

```xaml
<ContentPage ...
             Visual="Custom">
    ...
</ContentPage>
```

Le code c# équivalent est :

```csharp
ContentPage contentPage = new ContentPage();
contentPage.Visual = new CustomVisual();
```

## <a name="register-a-name-for-a-visual"></a>Inscrire un nom pour un élément visuel

Il peut y avoir des conflits entre les différentes bibliothèques Visual ou peut-être vous souhaitez simplement faire référence à un élément visuel par un autre nom. Dans ce scénario, vous pouvez utiliser un attribut d’assembly pour inscrire un nom différent pour un élément visuel donné :

```csharp
[assembly: Visual("MyMaterialName", typeof(VisualMarker.MaterialVisual))]
```

L’élément visuel personnalisé peut ensuite être consommé via son nom d’inscription :

```xaml
<ContentPage ...
             Visual="MyMaterialName">
    ...
</ContentPage>
````

## <a name="related-links"></a>Liens connexes

- [Xamarin.Forms matériau Visual](material-visual.md)
- [Renderers personnalisés](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
