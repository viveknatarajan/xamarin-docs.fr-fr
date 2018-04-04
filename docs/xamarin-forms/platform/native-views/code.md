---
title: Vues natives en c#
description: Vues natives dans iOS, Android et UWP peuvent être référencées directement à partir des pages de Xamarin.Forms créés à l’aide de c#. Cet article explique comment ajouter des vues natives à une présentation de Xamarin.Forms créée à l’aide de c# et la substitution de la disposition des affichages personnalisés pour corriger leur utilisation de l’API de mesure.
ms.prod: xamarin
ms.assetid: 230F937C-F914-4B21-8EA1-1A2A9E644769
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: 7979e43cdde60b1b0248118e591c096295847acb
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="native-views-in-c"></a>Vues natives en c#

_Vues natives dans iOS, Android et UWP peuvent être référencées directement à partir des pages de Xamarin.Forms créés à l’aide de c#. Cet article explique comment ajouter des vues natives à une présentation de Xamarin.Forms créée à l’aide de c# et la substitution de la disposition des affichages personnalisés pour corriger leur utilisation de l’API de mesure._

## <a name="overview"></a>Vue d'ensemble

N’importe quel contrôle Xamarin.Forms qui permet de `Content` pour être définie, ou qui a un `Children` collection, peut ajouter des affichages spécifiques à la plateforme. Par exemple, un iOS `UILabel` peuvent être ajoutés directement à la [ `ContentView.Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/) propriété, ou à la [ `StackLayout.Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout%3CT%3E.Children/) collection. Toutefois, notez que cette fonctionnalité requiert l’utilisation de `#if` définit dans les solutions de projet partagé de Xamarin.Forms et n’est pas disponible à partir des solutions de Xamarin.Forms Portable classe bibliothèque (PCL).

Les captures d’écran suivantes illustrent spécifique à la plateforme vues ayant été ajouté à un Xamarin.Forms [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/):

[![](code-images/screenshots-sml.png "StackLayout contenant des affichages spécifiques à la plateforme")](code-images/screenshots.png#lightbox "StackLayout contenant des affichages spécifiques à la plateforme")

La possibilité d’ajouter des affichages spécifiques à la plateforme à une présentation de Xamarin.Forms est activée par les deux méthodes d’extension sur chaque plateforme :

- `Add` : ajoute une vue spécifique à la plateforme pour le [ `Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout%3CT%3E.Children/) collection d’une mise en page.
- `ToView` – prend une vue spécifique à la plateforme et l’inclut comme un Xamarin.Forms [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) qui peut être défini comme le `Content` propriété d’un contrôle.

À l’aide de ces méthodes dans un projet partagé Xamarin.Forms nécessite l’importation de l’espace de noms de Xamarin.Forms spécifique à la plateforme appropriée :

- **iOS** – Xamarin.Forms.Platform.iOS
- **Android** – Xamarin.Forms.Platform.Android
- **Windows Runtime** – Xamarin.Forms.Platform.WinRT
- **Plateforme Windows universelle (UWP)** – Xamarin.Forms.Platform.UWP

## <a name="adding-platform-specific-views-on-each-platform"></a>Ajout d’affichages spécifiques à la plateforme sur chaque plateforme

Les sections suivantes expliquent comment ajouter des affichages spécifiques à la plateforme à une disposition Xamarin.Forms sur chaque plateforme.

### <a name="ios"></a>iOS

L’exemple de code suivant montre comment ajouter un `UILabel` à un [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) et un [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/):

```csharp
var uiLabel = new UILabel {
  MinimumFontSize = 14f,
  Lines = 0,
  LineBreakMode = UILineBreakMode.WordWrap,
  Text = originalText,
};
stackLayout.Children.Add (uiLabel);
contentView.Content = uiLabel.ToView();
```

L’exemple part du principe que la `stackLayout` et `contentView` instances précédemment ont été créées en XAML ou c#.

### <a name="android"></a>Android

L’exemple de code suivant montre comment ajouter un `TextView` à un [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) et un [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/):

```csharp
var textView = new TextView (MainActivity.Instance) { Text = originalText, TextSize = 14 };
stackLayout.Children.Add (textView);
contentView.Content = textView.ToView();
```

L’exemple part du principe que la `stackLayout` et `contentView` instances précédemment ont été créées en XAML ou c#.

### <a name="windows-runtime-and-universal-windows-platform"></a>Windows Runtime et la plateforme Windows universelle

L’exemple de code suivant montre comment ajouter un `TextBlock` à un [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) et un [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/):

```csharp
var textBlock = new TextBlock
{
    Text = originalText,
    FontSize = 14,
    FontFamily = new FontFamily("HelveticaNeue"),
    TextWrapping = TextWrapping.Wrap
};
stackLayout.Children.Add(textBlock);
contentView.Content = textBlock.ToView();
```

L’exemple part du principe que la `stackLayout` et `contentView` instances précédemment ont été créées en XAML ou c#.

## <a name="overriding-platform-measurements-for-custom-views"></a>Substitution des mesures de plate-forme pour des vues personnalisées

Les vues personnalisées sur chaque plateforme implémentent souvent uniquement correctement mesure pour le scénario de mise en page pour laquelle ils ont été conçus. Par exemple, un affichage personnalisé peut ont été conçu uniquement occupe la moitié de la largeur disponible de l’appareil. Toutefois, après avoir partagé avec d’autres utilisateurs, la vue personnalisée peut être requise pour occuper la largeur totale disponible de l’appareil. Par conséquent, il peut être nécessaire de remplacer une implémentation de mesure des vues personnalisées lorsque réutilisée dans une présentation de Xamarin.Forms. La raison pour laquelle le `Add` et `ToView` fournissent des méthodes d’extension remplacements qui permettent aux délégués mesure d’être spécifiés, qui peuvent remplacer la disposition de la vue personnalisée lorsqu’il est ajouté à une présentation de Xamarin.Forms.

Les sections suivantes montrent comment remplacer la disposition des affichages personnalisés, pour corriger leur utilisation de l’API de mesure.

### <a name="ios"></a>iOS

Montre l’exemple de code suit le `CustomControl` (classe), qui hérite de `UILabel`:

```csharp
public class CustomControl : UILabel
{
  public override string Text {
    get { return base.Text; }
    set { base.Text = value.ToUpper (); }
  }

  public override CGSize SizeThatFits (CGSize size)
  {
    return new CGSize (size.Width, 150);
  }
}
```

Une instance de cette vue est ajoutée à un [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), comme illustré dans l’exemple de code suivant :

```csharp
var customControl = new CustomControl {
  MinimumFontSize = 14,
  Lines = 0,
  LineBreakMode = UILineBreakMode.WordWrap,
  Text = "This control has incorrect sizing - there's empty space above and below it."
};
stackLayout.Children.Add (customControl);
```

Toutefois, étant donné que le `CustomControl.SizeThatFits` substitution retourne toujours une hauteur de 150, la vue est affichée avec un espace vide au-dessus et au-dessous d’elle, comme illustré dans la capture d’écran suivante :

![](code-images/ios-bad-measurement.png "iOS CustomControl avec une mauvaise implémentation SizeThatFits")

Une solution à ce problème consiste à fournir un `GetDesiredSizeDelegate` mise en oeuvre, comme illustré dans l’exemple de code suivant :

```csharp
SizeRequest? FixSize (NativeViewWrapperRenderer renderer, double width, double height)
{
  var uiView = renderer.Control;

  if (uiView == null) {
    return null;
  }

  var constraint = new CGSize (width, height);

  // Let the CustomControl determine its size (which will be wrong)
  var badRect = uiView.SizeThatFits (constraint);

  // Use the width and substitute the height
  return new SizeRequest (new Size (badRect.Width, 70));
}
```

Cette méthode utilise la largeur fournie par le `CustomControl.SizeThatFits` (méthode), mais la remplace par la hauteur de 150 pour une hauteur de 70. Lorsque le `CustomControl` instance est ajoutée à la [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), le `FixSize` méthode peut être spécifiée en tant que le `GetDesiredSizeDelegate` pour corriger la mesure incorrecte fournie par le `CustomControl` classe :

```csharp
stackLayout.Children.Add (customControl, FixSize);
```

Cela entraîne l’affichage personnalisé affiché correctement, sans espace vide au-dessus et au-dessous d’elle, comme illustré dans la capture d’écran suivante :

![](code-images/ios-good-measurement.png "iOS CustomControl avec remplacement de GetDesiredSize")

### <a name="android"></a>Android

Montre l’exemple de code suit le `CustomControl` (classe), qui hérite de `TextView`:

```csharp
public class CustomControl : TextView
{
  public CustomControl (Context context) : base (context)
  {
  }

  protected override void OnMeasure (int widthMeasureSpec, int heightMeasureSpec)
  {
    int width = MeasureSpec.GetSize (widthMeasureSpec);

    // Force the width to half of what's been requested.
    // This is deliberately wrong to demonstrate providing an override to fix it with.
    int widthSpec = MeasureSpec.MakeMeasureSpec (width / 2, MeasureSpec.GetMode (widthMeasureSpec));

    base.OnMeasure (widthSpec, heightMeasureSpec);
  }
}
```

Une instance de cette vue est ajoutée à un [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), comme illustré dans l’exemple de code suivant :

```csharp
var customControl = new CustomControl (MainActivity.Instance) {
  Text = "This control has incorrect sizing - it doesn't occupy the available width of the device.",
  TextSize = 14
};
stackLayout.Children.Add (customControl);
```

Toutefois, étant donné que le `CustomControl.OnMeasure` substitution retourne toujours la moitié de la largeur demandée, la vue est affichée occupant uniquement la moitié de la largeur disponible de l’appareil, comme indiqué dans la capture d’écran suivante :

![](code-images/android-bad-measurement.png "Android CustomControl avec mise en œuvre OnMeasure incorrecte")

Une solution à ce problème consiste à fournir un `GetDesiredSizeDelegate` mise en oeuvre, comme illustré dans l’exemple de code suivant :

```csharp
SizeRequest? FixSize (NativeViewWrapperRenderer renderer, int widthConstraint, int heightConstraint)
{
  var nativeView = renderer.Control;

  if ((widthConstraint == 0 && heightConstraint == 0) || nativeView == null) {
    return null;
  }

  int width = Android.Views.View.MeasureSpec.GetSize (widthConstraint);
  int widthSpec = Android.Views.View.MeasureSpec.MakeMeasureSpec (
    width * 2, Android.Views.View.MeasureSpec.GetMode (widthConstraint));
  nativeView.Measure (widthSpec, heightConstraint);
  return new SizeRequest (new Size (nativeView.MeasuredWidth, nativeView.MeasuredHeight));
}
```

Cette méthode utilise la largeur fournie par le `CustomControl.OnMeasure` (méthode), mais il multiplie par deux. Lorsque le `CustomControl` instance est ajoutée à la [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), le `FixSize` méthode peut être spécifiée en tant que le `GetDesiredSizeDelegate` pour corriger la mesure incorrecte fournie par le `CustomControl` classe :

```csharp
stackLayout.Children.Add (customControl, FixSize);
```

Cela entraîne l’affichage personnalisé en cours s’affichent correctement, qui occupent la largeur de l’appareil, comme indiqué dans la capture d’écran suivante :

![](code-images/android-good-measurement.png "CustomControl Android avec le délégué de GetDesiredSize personnalisé")

### <a name="universal-windows-platform"></a>Plateforme Windows universelle

Montre l’exemple de code suit le `CustomControl` (classe), qui hérite de `Panel`:

```csharp
public class CustomControl : Panel
{
  public static readonly DependencyProperty TextProperty =
    DependencyProperty.Register(
      "Text", typeof(string), typeof(CustomControl), new PropertyMetadata(default(string), OnTextPropertyChanged));

  public string Text
  {
    get { return (string)GetValue(TextProperty); }
    set { SetValue(TextProperty, value.ToUpper()); }
  }

  readonly TextBlock textBlock;

  public CustomControl()
  {
    textBlock = new TextBlock
    {
      MinHeight = 0,
      MaxHeight = double.PositiveInfinity,
      MinWidth = 0,
      MaxWidth = double.PositiveInfinity,
      FontSize = 14,
      TextWrapping = TextWrapping.Wrap,
      VerticalAlignment = VerticalAlignment.Center
    };

    Children.Add(textBlock);
  }

  static void OnTextPropertyChanged(DependencyObject dependencyObject, DependencyPropertyChangedEventArgs args)
  {
    ((CustomControl)dependencyObject).textBlock.Text = (string)args.NewValue;
  }

  protected override Size ArrangeOverride(Size finalSize)
  {
      // This is deliberately wrong to demonstrate providing an override to fix it with.
      textBlock.Arrange(new Rect(0, 0, finalSize.Width/2, finalSize.Height));
      return finalSize;
  }

  protected override Size MeasureOverride(Size availableSize)
  {
      textBlock.Measure(availableSize);
      return new Size(textBlock.DesiredSize.Width, textBlock.DesiredSize.Height);
  }
}
```

Une instance de cette vue est ajoutée à un [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), comme illustré dans l’exemple de code suivant :

```csharp
var brokenControl = new CustomControl {
  Text = "This control has incorrect sizing - it doesn't occupy the available width of the device."
};
stackLayout.Children.Add(brokenControl);
```

Toutefois, étant donné que le `CustomControl.ArrangeOverride` substitution retourne toujours la moitié de la largeur demandée, la vue coïncident avec la moitié de la largeur disponible de l’appareil, comme indiqué dans la capture d’écran suivante :

![](code-images/winrt-bad-measurement.png "CustomControl UWP avec mise en œuvre ArrangeOverride incorrect")

Une solution à ce problème consiste à fournir un `ArrangeOverrideDelegate` implémentation, lors de l’ajout de la vue à la [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), comme illustré dans l’exemple de code suivant :

```csharp
stackLayout.Children.Add(fixedControl, arrangeOverrideDelegate: (renderer, finalSize) =>
{
    if (finalSize.Width <= 0 || double.IsInfinity(finalSize.Width))
    {
        return null;
    }
    var frameworkElement = renderer.Control;
    frameworkElement.Arrange(new Rect(0, 0, finalSize.Width * 2, finalSize.Height));
    return finalSize;
});
```

Cette méthode utilise la largeur fournie par le `CustomControl.ArrangeOverride` (méthode), mais il multiplie par deux. Cela entraîne l’affichage personnalisé en cours s’affichent correctement, qui occupent la largeur de l’appareil, comme indiqué dans la capture d’écran suivante :

![](code-images/winrt-good-measurement.png "CustomControl UWP avec ArrangeOverride délégué")

## <a name="summary"></a>Récapitulatif

Cet article a expliqué comment ajouter des vues natives à une présentation de Xamarin.Forms créée à l’aide de c# et la substitution de la disposition des affichages personnalisés pour corriger leur utilisation de l’API de mesure.


## <a name="related-links"></a>Liens associés

- [NativeEmbedding (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/NativeEmbedding/)
- [Formulaires natifs](~/xamarin-forms/platform/native-forms.md)
