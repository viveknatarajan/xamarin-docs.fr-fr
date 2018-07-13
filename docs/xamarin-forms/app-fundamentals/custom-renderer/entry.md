---
title: Personnalisation d’une entrée
description: Le contrôle d’entrée de Xamarin.Forms permet à une seule ligne de texte à modifier. Cet article montre comment créer un convertisseur personnalisé pour le contrôle d’entrée, en permettant aux développeurs de substituer le rendu natif par défaut avec leur propres personnalisation spécifique à la plateforme.
ms.prod: xamarin
ms.assetid: 7B5DD10D-0411-424F-88D8-8A474DF16D8D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 30326b8d52f39268015bdcbee1b84b9d9e5516b9
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998958"
---
# <a name="customizing-an-entry"></a>Personnalisation d’une entrée

_Le contrôle d’entrée de Xamarin.Forms permet à une seule ligne de texte à modifier. Cet article montre comment créer un convertisseur personnalisé pour le contrôle d’entrée, en permettant aux développeurs de substituer le rendu natif par défaut avec leur propres personnalisation spécifique à la plateforme._

Chaque contrôle Xamarin.Forms a un convertisseur qui accompagne cet article pour chaque plateforme qui crée une instance d’un contrôle natif. Quand un [ `Entry` ](xref:Xamarin.Forms.Entry) contrôle est restitué par une application Xamarin.Forms, dans iOS le `EntryRenderer` classe est instanciée, ce qui réduit instancie native `UITextField` contrôle. Sur la plateforme Android, le `EntryRenderer` classe instancie un `EditText` contrôle. Sur la plateforme de Windows universelle (UWP), le `EntryRenderer` classe instancie un `TextBox` contrôle. Pour plus d’informations sur les classes de contrôle natif correspondant aux contrôles Xamarin.Forms et le convertisseur, consultez [convertisseur des Classes de Base et des contrôles natifs](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Le diagramme suivant illustre la relation entre la [ `Entry` ](xref:Xamarin.Forms.Entry) contrôle et les contrôles natifs correspondants qui l’implémentent :

![](entry-images/entry-classes.png "Relation entre le contrôle d’entrée et l’implémentation des contrôles natifs")

Le processus de rendu peut être exploitée pour implémenter les personnalisations spécifiques à la plateforme en créant un convertisseur personnalisé pour le [ `Entry` ](xref:Xamarin.Forms.Entry) contrôle sur chaque plateforme. Le processus pour effectuer cette opération est la suivante :

1. [Créer](#Creating_the_Custom_Entry_Control) un contrôle personnalisé Xamarin.Forms.
1. [Consommer](#Consuming_the_Custom_Control) le contrôle personnalisé à partir de Xamarin.Forms.
1. [Créer](#Creating_the_Custom_Renderer_on_each_Platform) le convertisseur personnalisé pour le contrôle sur chaque plateforme.

Chaque élément maintenant nous reviendrons à son tour, pour implémenter un [ `Entry` ](xref:Xamarin.Forms.Entry) contrôle qui a une couleur d’arrière-plan différente sur chaque plateforme.

<a name="Creating_the_Custom_Entry_Control" />

## <a name="creating-the-custom-entry-control"></a>Création du contrôle d’entrée personnalisée

Personnalisé [ `Entry` ](xref:Xamarin.Forms.Entry) contrôle peut être créé en sous-classant la `Entry` contrôler, comme indiqué dans l’exemple de code suivant :

```csharp
public class MyEntry : Entry
{
}
```

Le `MyEntry` contrôle est créé dans le projet de bibliothèque .NET Standard et est simplement un [ `Entry` ](xref:Xamarin.Forms.Entry) contrôle. Personnalisation du contrôle est effectuée dans le convertisseur personnalisé, par conséquent, aucune implémentation supplémentaire n’est requise dans le `MyEntry` contrôle.

<a name="Consuming_the_Custom_Control" />

## <a name="consuming-the-custom-control"></a>Utilisation du contrôle personnalisé

Le `MyEntry` contrôle peut être référencé dans le XAML dans le projet de bibliothèque .NET Standard en déclarant un espace de noms pour son emplacement et en utilisant le préfixe d’espace de noms sur l’élément de contrôle. Le code suivant montre l’exemple comment la `MyEntry` contrôle peut être consommé par une page XAML :

```xaml
<ContentPage ...
    xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
    ...>
    ...
    <local:MyEntry Text="In Shared Code" />
    ...
</ContentPage>
```

Le `local` préfixe d’espace de noms peut être n’importe quel nom. Toutefois, le `clr-namespace` et `assembly` valeurs doivent correspondre les détails du contrôle personnalisé. Une fois que l’espace de noms est déclaré que le préfixe est utilisé pour référencer le contrôle personnalisé.

Le code suivant montre l’exemple comment la `MyEntry` contrôle peut être consommé par une page c# :

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

Ce code instancie un nouveau [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) objet affiche un [ `Label` ](xref:Xamarin.Forms.Label) et `MyEntry` contrôle centré à la fois verticalement et horizontalement sur la page.

Un convertisseur personnalisé peut maintenant être ajouté à chaque projet d’application pour personnaliser l’apparence du contrôle sur chaque plateforme.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Création du convertisseur personnalisé sur chaque plateforme

Le processus de création de la classe de convertisseur personnalisé est comme suit :

1. Créer une sous-classe de la `EntryRenderer` classe qui restitue le contrôle natif.
1. Remplacer le `OnElementChanged` méthode qui restitue la logique de contrôle et d’écriture native pour personnaliser le contrôle. Cette méthode est appelée lorsque le contrôle de Xamarin.Forms correspondant est créé.
1. Ajouter un `ExportRenderer` d’attribut à la classe de convertisseur personnalisé pour spécifier qu’il sera être utilisé pour restituer le contrôle Xamarin.Forms. Cet attribut est utilisé pour inscrire le renderer personnalisé avec Xamarin.Forms.

> [!NOTE]
> Il est facultatif pour fournir un convertisseur personnalisé dans chaque projet de plateforme. Si un convertisseur personnalisé n’est pas inscrit, le convertisseur par défaut pour la classe de base du contrôle sera être utilisé.

Le diagramme suivant illustre les responsabilités de chaque projet dans l’exemple d’application, ainsi que les relations entre eux :

![](entry-images/solution-structure.png "Responsabilités de projet de convertisseur MyEntry personnalisé")

Le `MyEntry` contrôle est restitué par spécifiques à la plateforme `MyEntryRenderer` classes qui dérivent de la `EntryRenderer` classe pour chaque plateforme. Ainsi, chaque `MyEntry` contrôler le rendu avec une couleur d’arrière-plan spécifique à la plateforme, comme indiqué dans les captures d’écran suivante :

![](entry-images/screenshots.png "Contrôle MyEntry sur chaque plateforme")

Le `EntryRenderer` classe expose le `OnElementChanged` (méthode), qui est appelé lorsque le contrôle Xamarin.Forms est créé pour restituer le contrôle natif correspondant. Cette méthode prend un `ElementChangedEventArgs` paramètre contienne `OldElement` et `NewElement` propriétés. Ces propriétés représentent l’élément Xamarin.Forms qui le convertisseur *a été* associée et l’élément Xamarin.Forms qui le convertisseur *est* attaché, respectivement. Dans l’exemple d’application le `OldElement` propriété sera `null` et `NewElement` propriété contiendra une référence à la `MyEntry` contrôle.

Une version substituée de la `OnElementChanged` méthode dans la `MyEntryRenderer` classe est l’endroit où effectuer la personnalisation du contrôle natif. Une référence typée au contrôle natif utilisé sur la plateforme sont accessibles via le `Control` propriété. En outre, une référence au contrôle Xamarin.Forms qui est rendu peut être obtenue via la `Element` propriété, bien qu’il n’est pas utilisé dans l’exemple d’application.

Chaque classe de convertisseur personnalisé est décorée avec un `ExportRenderer` attribut qui inscrit le convertisseur avec Xamarin.Forms. L’attribut accepte deux paramètres : le nom de type du contrôle Xamarin.Forms en cours d’affichage et le nom de type du convertisseur personnalisé. Le `assembly` préfixe à l’attribut spécifie que l’attribut s’applique à la totalité de l’assembly.

Les sections suivantes décrivent l’implémentation de chaque spécifiques à la plateforme `MyEntryRenderer` classe de convertisseur personnalisé.

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

L’appel à la classe de base `OnElementChanged` méthode instancie un iOS `UITextField` contrôle, avec une référence au contrôle assignée pour le convertisseur `Control` propriété. La couleur d’arrière-plan est ensuite définie sur violet clair avec le `UIColor.FromRGB` (méthode).

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

L’appel à la classe de base `OnElementChanged` méthode instancie un Android `EditText` contrôle, avec une référence au contrôle assignée pour le convertisseur `Control` propriété. La couleur d’arrière-plan est ensuite définie sur vert clair avec le `Control.SetBackgroundColor` (méthode).

### <a name="creating-the-custom-renderer-on-uwp"></a>Création du convertisseur personnalisé sur UWP

L’exemple de code suivant montre le convertisseur personnalisé pour UWP :

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

L’appel à la classe de base `OnElementChanged` méthode instancie un `TextBox` contrôle, avec une référence au contrôle assignée pour le convertisseur `Control` propriété. La couleur d’arrière-plan est ensuite définie sur cyan en créant un `SolidColorBrush` instance.

## <a name="summary"></a>Récapitulatif

Cet article a montré comment créer un convertisseur de contrôle personnalisé pour le Xamarin.Forms [ `Entry` ](xref:Xamarin.Forms.Entry) (contrôle), permettant aux développeurs de substituer le rendu natif par défaut avec leur propre rendu spécifiques à la plateforme. Renderers personnalisés fournissent une approche puissante pour personnaliser l’apparence des contrôles Xamarin.Forms. Elles peuvent servir pour les styles de petites modifications ou sophistiquée disposition spécifique à la plateforme et personnalisation du comportement.


## <a name="related-links"></a>Liens associés

- [CustomRendererEntry (exemple)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/entry/)
