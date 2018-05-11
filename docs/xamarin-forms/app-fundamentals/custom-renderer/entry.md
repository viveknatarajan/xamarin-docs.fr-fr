---
title: Personnalisation d’une entrée
description: Le contrôle d’entrée de Xamarin.Forms permet une seule ligne de texte à modifier. Cet article explique comment créer un convertisseur personnalisé pour le contrôle d’entrée, en permettant aux développeurs de substituer le rendu natif par défaut avec leur propres personnalisation spécifiques à la plateforme.
ms.prod: xamarin
ms.assetid: 7B5DD10D-0411-424F-88D8-8A474DF16D8D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: c93681c3bfd8de8d813cbe98a7ac28b3ee8b74fc
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="customizing-an-entry"></a>Personnalisation d’une entrée

_Le contrôle d’entrée de Xamarin.Forms permet une seule ligne de texte à modifier. Cet article explique comment créer un convertisseur personnalisé pour le contrôle d’entrée, en permettant aux développeurs de substituer le rendu natif par défaut avec leur propres personnalisation spécifiques à la plateforme._

Chaque contrôle Xamarin.Forms a un convertisseur qui l’accompagne pour chaque plateforme qui crée une instance d’un contrôle natif. Lorsqu’un [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) contrôle est restitué par une application de Xamarin.Forms, dans iOS le `EntryRenderer` classe est instanciée, ce qui réduit instancie natif `UITextField` contrôle. Sur la plateforme Android, le `EntryRenderer` classe instancie un `EditText` contrôle. Sur la plate-forme de Windows universelle (UWP), le `EntryRenderer` classe instancie un `TextBox` contrôle. Pour plus d’informations sur les classes de contrôle natif correspondant aux contrôles de Xamarin.Forms et le convertisseur, consultez [convertisseur des Classes de Base et des contrôles natifs](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Le diagramme suivant illustre la relation entre la [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) contrôle et les contrôles natives correspondantes qui l’implémentent :

![](entry-images/entry-classes.png "Relation entre le contrôle d’entrée et l’implémentation de contrôles natifs")

Le processus de rendu peut être exploitée pour implémenter les personnalisations spécifiques à une plateforme en créant un convertisseur personnalisé pour le [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) contrôle sur chaque plateforme. Le processus pour cette opération est la suivante :

1. [Créer](#Creating_the_Custom_Entry_Control) un contrôle personnalisé de Xamarin.Forms.
1. [Consommer](#Consuming_the_Custom_Control) le contrôle personnalisé à partir de Xamarin.Forms.
1. [Créer](#Creating_the_Custom_Renderer_on_each_Platform) le convertisseur personnalisé pour le contrôle sur chaque plateforme.

Chaque élément présent nous reviendrons à son tour, pour implémenter un [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) contrôle qui a une couleur d’arrière-plan différente sur chaque plateforme.

<a name="Creating_the_Custom_Entry_Control" />

## <a name="creating-the-custom-entry-control"></a>Création du contrôle d’entrée personnalisée

Personnalisé [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) contrôle peut être créé en sous-classant le `Entry` contrôler, comme indiqué dans l’exemple de code suivant :

```csharp
public class MyEntry : Entry
{
}
```

Le `MyEntry` contrôle est créé dans le projet de bibliothèque .NET Standard et est simplement un [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) contrôle. Personnalisation du contrôle est effectuée dans le convertisseur personnalisé, aucune implémentation supplémentaire est requis dans le `MyEntry` contrôle.

<a name="Consuming_the_Custom_Control" />

## <a name="consuming-the-custom-control"></a>Utilisation du contrôle personnalisé

Le `MyEntry` contrôle qui peut être référencée en XAML dans le projet de bibliothèque .NET Standard en déclarant un espace de noms pour son emplacement et en utilisant le préfixe d’espace de noms sur l’élément de contrôle. Le code suivant exemple montre comment la `MyEntry` contrôle peut être consommée par une page XAML :

```xaml
<ContentPage ...
    xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
    ...>
    ...
    <local:MyEntry Text="In Shared Code" />
    ...
</ContentPage>
```

Le `local` préfixe d’espace de noms peut être n’importe quel nom. Toutefois, le `clr-namespace` et `assembly` valeurs doivent correspondre les détails du contrôle personnalisé. Une fois que l’espace de noms est déclaré le préfixe est utilisé pour référencer le contrôle personnalisé.

Le code suivant montre d’exemple comment la `MyEntry` contrôle peut être consommée par une page c# :

```csharp
public class MainPage : ContentPage
{
  public MainPage ()
  {
    Content = new StackLayout {
      Children = {
        new Label {
          Text = "Hello, Custom Renderer !",
        },
        new MyEntry {
          Text = "In Shared Code",
        }
      },
      VerticalOptions = LayoutOptions.CenterAndExpand,
      HorizontalOptions = LayoutOptions.CenterAndExpand,
    };
  }
}
```

Ce code instancie une nouvelle [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) objet affiche un [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) et `MyEntry` contrôle centré à la fois verticalement et horizontalement sur la page.

Un convertisseur personnalisé peut maintenant être ajouté à chaque projet d’application pour personnaliser l’apparence du contrôle sur chaque plateforme.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Création du convertisseur personnalisé sur chaque plateforme

Le processus de création de la classe de convertisseur personnalisé est comme suit :

1. Créer une sous-classe de la `EntryRenderer` classe qui restitue le contrôle natif.
1. Remplacer la `OnElementChanged` méthode qui restitue la logique de contrôle et d’écriture native pour personnaliser le contrôle. Cette méthode est appelée lorsque le contrôle de Xamarin.Forms correspondant est créé.
1. Ajouter un `ExportRenderer` d’attribut à la classe de convertisseur personnalisé pour spécifier qu’il sera utilisé pour restituer le contrôle de Xamarin.Forms. Cet attribut est utilisé pour inscrire le convertisseur personnalisé avec Xamarin.Forms.

> [!NOTE]
> Il est facultatif pour fournir un convertisseur personnalisé dans chaque projet de plateforme. Si un convertisseur personnalisé n’est pas inscrit, puis le convertisseur par défaut pour la classe de base du contrôle sera utilisé.

Le diagramme suivant illustre les responsabilités de chaque projet dans l’exemple d’application, ainsi que les relations entre eux :

![](entry-images/solution-structure.png "Responsabilités de projet convertisseur MyEntry personnalisé")

Le `MyEntry` contrôle est restitué par spécifique à la plateforme `MyEntryRenderer` classes qui dérivent de la `EntryRenderer` classe pour chaque plateforme. Ainsi, chaque `MyEntry` contrôler le rendu avec une couleur d’arrière-plan de la plateforme spécifique, comme indiqué dans les captures d’écran suivants :

![](entry-images/screenshots.png "Contrôle MyEntry sur chaque plateforme")

Le `EntryRenderer` classe expose le `OnElementChanged` (méthode), qui est appelé lorsque le contrôle de Xamarin.Forms est créé pour restituer le contrôle natif correspondant. Cette méthode prend un `ElementChangedEventArgs` paramètre contienne `OldElement` et `NewElement` propriétés. Ces propriétés représentent l’élément Xamarin.Forms que le convertisseur *a été* associée et l’élément Xamarin.Forms que le convertisseur *est* attaché, respectivement. Dans l’exemple d’application le `OldElement` propriété sera `null` et `NewElement` propriété contient une référence à la `MyEntry` contrôle.

Une version substituée de la `OnElementChanged` méthode dans la `MyEntryRenderer` classe est l’endroit où effectuer la personnalisation du contrôle natif. Une référence typée au contrôle natif utilisé sur la plateforme est accessible via la `Control` propriété. En outre, une référence au contrôle qui est rendue Xamarin.Forms peut être obtenue via le `Element` propriété, bien qu’il n’est pas utilisé dans l’exemple d’application.

Chaque classe de convertisseur personnalisé est décorée avec un `ExportRenderer` attribut qui inscrit le convertisseur avec Xamarin.Forms. L’attribut accepte deux paramètres : le nom de type du contrôle Xamarin.Forms rendu et le nom de type du convertisseur personnalisé. Le `assembly` préfixe à l’attribut spécifie que l’attribut s’applique à la totalité de l’assembly.

Les sections suivantes traitent de l’implémentation de chaque spécifique à la plateforme `MyEntryRenderer` classe de convertisseur personnalisé.

### <a name="creating-the-custom-renderer-on-ios"></a>Création du convertisseur personnalisé sur iOS

L’exemple de code suivant montre le convertisseur personnalisé pour la plateforme iOS :

```csharp
using Xamarin.Forms.Platform.iOS;

[assembly: ExportRenderer (typeof(MyEntry), typeof(MyEntryRenderer))]
namespace CustomRenderer.iOS
{
    public class MyEntryRenderer : EntryRenderer
    {
        protected override void OnElementChanged (ElementChangedEventArgs<Entry> e)
        {
            base.OnElementChanged (e);

            if (Control != null) {
                // do whatever you want to the UITextField here!
                Control.BackgroundColor = UIColor.FromRGB (204, 153, 255);
                Control.BorderStyle = UITextBorderStyle.Line;
            }
        }
    }
}
```

L’appel à la classe de base `OnElementChanged` méthode instancie un iOS `UITextField` contrôle, avec une référence au contrôle assignée au convertisseur `Control` propriété. La couleur d’arrière-plan est ensuite affectée à marron avec la `UIColor.FromRGB` (méthode).

### <a name="creating-the-custom-renderer-on-android"></a>Création du convertisseur personnalisé sur Android

L’exemple de code suivant montre le convertisseur personnalisé pour la plateforme Android :

```csharp
using Xamarin.Forms.Platform.Android;

[assembly: ExportRenderer(typeof(MyEntry), typeof(MyEntryRenderer))]
namespace CustomRenderer.Android
{
    class MyEntryRenderer : EntryRenderer
    {
        public MyEntryRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(ElementChangedEventArgs<Entry> e)
        {
            base.OnElementChanged(e);

            if (Control != null)
            {
                Control.SetBackgroundColor(global::Android.Graphics.Color.LightGreen);
            }
        }
    }
}
```

L’appel à la classe de base `OnElementChanged` méthode instancie un Android `EditText` contrôle, avec une référence au contrôle assignée au convertisseur `Control` propriété. La couleur d’arrière-plan est ensuite affectée à vert clair avec le `Control.SetBackgroundColor` (méthode).

### <a name="creating-the-custom-renderer-on-uwp"></a>Création du convertisseur personnalisé sur la plateforme Windows universelle

L’exemple de code suivant montre le convertisseur personnalisé pour la plateforme Windows universelle :

```csharp
[assembly: ExportRenderer(typeof(MyEntry), typeof(MyEntryRenderer))]
namespace CustomRenderer.UWP
{
    public class MyEntryRenderer : EntryRenderer
    {
        protected override void OnElementChanged(ElementChangedEventArgs<Entry> e)
        {
            base.OnElementChanged(e);

            if (Control != null)
            {
                Control.Background = new SolidColorBrush(Colors.Cyan);
            }
        }
    }
}
```

L’appel à la classe de base `OnElementChanged` méthode instancie un `TextBox` contrôle, avec une référence au contrôle assignée au convertisseur `Control` propriété. La couleur d’arrière-plan a la valeur cyan puis en créant un `SolidColorBrush` instance.

## <a name="summary"></a>Récapitulatif

Cet article a montré comment créer un contrôle personnalisé de convertisseur pour la Xamarin.Forms [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) (contrôle), permettant aux développeurs de substituer le rendu natif par défaut avec leur propre rendu spécifique à la plateforme. Les convertisseurs personnalisés fournissent une approche puissante pour personnaliser l’apparence des contrôles de Xamarin.Forms. Ils peuvent être utilisés pour les modifications de conception de styles de petite ou sophistiquée spécifique à la plateforme disposition et personnalisation du comportement.


## <a name="related-links"></a>Liens associés

- [CustomRendererEntry (exemple)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/entry/)
