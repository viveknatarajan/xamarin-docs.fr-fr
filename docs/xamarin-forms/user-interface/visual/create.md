---
title: Créer un convertisseur de Xamarin.Forms Visual
description: Xamarin.Forms Visual permet des convertisseurs être appliquées de façon sélective aux objets VisualElement, sans avoir aux vues Xamarin.Forms de sous-classe.
ms.prod: xamarin
ms.assetid: 80BF9C72-AC28-4AAF-9DDD-B60CBDD1CD59
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/12/2019
ms.openlocfilehash: 1bd56d09932c97508dd0a05fbc0eb2bad3af3f0e
ms.sourcegitcommit: 97dca3face7c4ad5555dfaca88f5b45a70ca556d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/14/2019
ms.locfileid: "57972583"
---
# <a name="create-a-xamarinforms-visual-renderer"></a>Créer un convertisseur de Xamarin.Forms Visual

[![Télécharger l’exemple](~/media/shared/download.png) Télécharger l’exemple](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VisualDemos/)

Xamarin.Forms Visual permet des convertisseurs être créées et appliquées de façon sélective à [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) objets, sans avoir aux vues Xamarin.Forms de sous-classe. Un convertisseur qui spécifie un `IVisual` type, dans le cadre de son `ExportRendererAttribute`, sera utilisé pour restituer participent aux vues, plutôt que le convertisseur par défaut. Au moment de sélection du convertisseur, le `Visual` propriété de la vue est inspectée et incluse dans le processus de sélection de convertisseur.

> [!IMPORTANT]
> Actuellement le [ `Visual` ](xref:Xamarin.Forms.VisualElement.Visual) propriété ne peut pas être modifiée après la vue a été restituée, mais cela changera dans une version ultérieure.

Le processus de création et utilisation d’un convertisseur de Xamarin.Forms Visual est :

1. Créer des convertisseurs de plateforme pour la vue requise. Pour plus d’informations, consultez [créer des convertisseurs](#create-platfomr-renderers).
1. Créer un type qui dérive de `IVisual`. Pour plus d’informations, consultez [créer un type IVisual](#create-an-ivisual-type).
1. Inscrire le `IVisual` type dans le cadre de la `ExportRendererAttribute` qui décore les convertisseurs. Pour plus d’informations, consultez [inscrire le type IVisual](#register-the-ivisual-type).
1. Utiliser le convertisseur Visual en définissant le [ `Visual` ](xref:Xamarin.Forms.VisualElement.Visual) propriété sur la vue à le `IVisual` nom. Pour plus d’informations, consultez [consommer le convertisseur Visual](#consume-the-visual-renderer).
1. [facultatif] Inscrire un nom pour le `IVisual` type. Pour plus d’informations, consultez [inscrire un nom pour le type IVisual](#register-a-name-for-the-ivisual-type).

## <a name="create-platform-renderers"></a>Créer des convertisseurs de plateforme

Pour plus d’informations sur la création d’une classe de convertisseur, consultez [Renderers personnalisés](~/xamarin-forms/app-fundamentals/custom-renderer/index.md). Toutefois, notez qu’un convertisseur de Xamarin.Forms Visual est appliqué à une vue sans devoir sous-classe de la vue.

Les classes de convertisseur décrites ici implémentent un [ `Button` ](xref:Xamarin.Forms.Button) qui affiche son texte avec une ombre.

### <a name="ios"></a>iOS

L’exemple de code suivant montre le convertisseur de bouton pour iOS :

```csharp
public class CustomButtonRenderer : ButtonRenderer
{
    protected override void OnElementChanged(ElementChangedEventArgs<Button> e)
    {
        base.OnElementChanged(e);

        if (e.OldElement != null)
        {
            // Cleanup
        }

        if (e.NewElement != null)
        {
            Control.TitleShadowOffset = new CoreGraphics.CGSize(1, 1);
            Control.SetTitleShadowColor(Color.Black.ToUIColor(), UIKit.UIControlState.Normal);
        }
    }
}
```

### <a name="android"></a>Android

L’exemple de code suivant montre le convertisseur de bouton pour Android :

```csharp
public class CustomButtonRenderer : Xamarin.Forms.Platform.Android.AppCompat.ButtonRenderer
{
    public CustomButtonRenderer(Context context) : base(context)
    {
    }

    protected override void OnElementChanged(ElementChangedEventArgs<Button> e)
    {
        base.OnElementChanged(e);

        if (e.OldElement != null)
        {
            // Cleanup
        }

        if (e.NewElement != null)
        {
            Control.SetShadowLayer(5, 3, 3, Color.Black.ToAndroid());
        }
    }
}
```

## <a name="create-an-ivisual-type"></a>Créer un type de IVisual

Dans votre bibliothèque multiplateforme, créez un type qui dérive de `IVisual`:

```csharp
public class CustomVisual : IVisual
{
}
```

Le `CustomVisual` type peut ensuite être inscrits sur les classes de convertisseur, autorisant [ `Button` ](xref:Xamarin.Forms.Button) objets à choisissent d’utiliser les convertisseurs.

## <a name="register-the-ivisual-type"></a>Inscrire le type IVisual

Dans les projets de plateforme, décorer les classes de convertisseur avec le `ExportRendererAttribute`:

```csharp
[assembly: ExportRenderer(typeof(Xamarin.Forms.Button), typeof(CustomButtonRenderer), new[] { typeof(CustomVisual) })]
public class CustomButtonRenderer : ButtonRenderer
{
    protected override void OnElementChanged(ElementChangedEventArgs<Button> e)
    {
        ...
    }
}
```

Dans cet exemple, le `ExportRendererAttribute` Spécifie que le `CustomButtonRenderer` classe permet de restituer la consommation [ `Button` ](xref:Xamarin.Forms.Button) objets, avec le `IVisual` type inscrit comme troisième argument. Un convertisseur qui spécifie un `IVisual` type, dans le cadre de son `ExportRendererAttribute`, sera utilisé pour restituer participent aux vues, plutôt que le convertisseur par défaut.

## <a name="consume-the-visual-renderer"></a>Utiliser le convertisseur Visual

Un [ `Button` ](xref:Xamarin.Forms.Button) objet permettre opter pour l’utilisation des classes de convertisseur en définissant son [ `Visual` ](xref:Xamarin.Forms.VisualElement.Visual) propriété `Custom`:

```xaml
<Button Visual="Custom"
        Text="CUSTOM BUTTON"
        BackgroundColor="{StaticResource PrimaryColor}"
        TextColor="{StaticResource SecondaryTextColor}"
        HorizontalOptions="FillAndExpand" />
```

> [!NOTE]
> Dans XAML, un convertisseur de type n’est plus nécessaire d’inclure le suffixe « Visual » dans le [ `Visual` ](xref:Xamarin.Forms.VisualElement.Visual) valeur de propriété. Toutefois, le nom de type complet peut également être spécifié.

Le code c# équivalent est :

```csharp
Button button = new Button { Text = "CUSTOM BUTTON", ... };
button.Visual = new CustomVisual();
```

Au moment de sélection du convertisseur, le [ `Visual` ](xref:Xamarin.Forms.VisualElement.Visual) propriété de la [ `Button` ](xref:Xamarin.Forms.Button) sont inspectées et inclus dans le processus de sélection de convertisseur. Si un convertisseur n’est pas localisé, le convertisseur par défaut Xamarin.Forms sera utilisé.

Les captures d’écran suivantes montrent le rendu [ `Button` ](xref:Xamarin.Forms.Button), qui affiche son texte avec une ombre :

[![Capture d’écran du bouton personnalisé avec le texte de clichés instantanés, sur iOS et Android](material-visual-images/custom-button.png "bouton avec le texte de l’ombre")](material-visual-images/custom-button-large.png#lightbox)

## <a name="register-a-name-for-the-ivisual-type"></a>Inscrire un nom pour le type de IVisual

Le [ `VisualAttribute` ](xref:Xamarin.Forms.VisualAttribute) peut être utilisé pour inscrire éventuellement un nom différent pour le `IVisual` type. Cette approche peut être utilisée pour résoudre les conflits d’affectation de noms entre les différentes bibliothèques Visual ou dans les situations où vous souhaitez simplement faire référence à un élément visuel par un nom différent de son nom de type.

Le [ `VisualAttribute` ](xref:Xamarin.Forms.VisualAttribute) doivent être définies au niveau de l’assembly dans la bibliothèque multiplateforme, ou dans le projet de plateforme :

```csharp
[assembly: Visual("MyVisual", typeof(CustomVisual))]
```

Le `IVisual` type permettre ensuite être consommé via son nom d’inscription :

```xaml
<Button Visual="MyVisual"
        ... />
```

> [!NOTE]
> Lors de l’utilisation d’un élément visuel via son nom d’inscription, n’importe quel suffixe « Visual » doit être inclus.

## <a name="related-links"></a>Liens connexes

- [MATÉRIAU Visual (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VisualDemos/)
- [Xamarin.Forms matériau Visual](material-visual.md)
- [Renderers personnalisés](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
