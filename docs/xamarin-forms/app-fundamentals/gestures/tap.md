---
title: Ajout d’un module de reconnaissance de mouvement tap
description: Cet article explique comment utiliser l’action d’appuyer pour la détection de drainage dans une application Xamarin.Forms. Détection de drainage est implémentée avec la classe TapGestureRecognizer.
ms.prod: xamarin
ms.assetid: 1D150BAF-4157-49BC-90A0-153323B8EBCF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
ms.openlocfilehash: a28afb30770f15861aef06643e7f51070199ea9b
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/18/2018
ms.locfileid: "38994852"
---
# <a name="adding-a-tap-gesture-recognizer"></a>Ajout d’un module de reconnaissance de mouvement tap

_Le geste d’appui est utilisé pour la détection tap et est implémenté avec la classe TapGestureRecognizer._

Pour rendre un élément d’interface utilisateur interactif avec le geste d’appui, créer un [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) d’une instance, de gérer le [ `Tapped` ](xref:Xamarin.Forms.TapGestureRecognizer.Tapped) événement et ajoutez la reconnaissance de mouvement de nouveau à la [ `GestureRecognizers` ](xref:Xamarin.Forms.View.GestureRecognizers) collection sur l’élément d’interface utilisateur. Le code suivant montre l’exemple un `TapGestureRecognizer` attaché à un [ `Image` ](xref:Xamarin.Forms.Image) élément :

```csharp
var tapGestureRecognizer = new TapGestureRecognizer();
tapGestureRecognizer.Tapped += (s, e) => {
    // handle the tap
};
image.GestureRecognizers.Add(tapGestureRecognizer);
```

Par défaut, l’image répondra à pressions uniques. Définir le [ `NumberOfTapsRequired` ](xref:Xamarin.Forms.TapGestureRecognizer.NumberOfTapsRequired) propriété pour attendre un double-clic (ou plus de robinets si nécessaire).

```csharp
tapGestureRecognizer.NumberOfTapsRequired = 2; // double-tap
```

Lorsque [ `NumberOfTapsRequired` ](xref:Xamarin.Forms.TapGestureRecognizer.NumberOfTapsRequired) a la valeur supérieure à 1, le Gestionnaire d’événements est exécuté uniquement si les pressions se produisent au sein d’une durée donnée (cette période n’est pas configurable). Si les pressions deuxième (ou ultérieures) ne se produisent pas dans ce délai qu’ils sont effectivement ignorés et le nombre de drainage redémarre.

<a name="Using_Xaml" />

## <a name="using-xaml"></a>À l’aide de Xaml

Un module de reconnaissance de mouvement peut être ajouté à un contrôle en Xaml à l’aide des propriétés jointes. La syntaxe pour ajouter un [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) à une image est indiqué ci-dessous (dans ce cas en définissant un *appuyer deux fois* événement) :

```xaml
<Image Source="tapped.jpg">
    <Image.GestureRecognizers>
        <TapGestureRecognizer
                Tapped="OnTapGestureRecognizerTapped"
                NumberOfTapsRequired="2" />
  </Image.GestureRecognizers>
</Image>
```

Le code du Gestionnaire d’événements (dans l’exemple) incrémente un compteur et devient l’image à partir de la couleur noire &amp; blanc.

```csharp
void OnTapGestureRecognizerTapped(object sender, EventArgs args)
{
    tapCount++;
    var imageSender = (Image)sender;
    // watch the monkey go from color to black&white!
    if (tapCount % 2 == 0) {
        imageSender.Source = "tapped.jpg";
    } else {
        imageSender.Source = "tapped_bw.jpg";
    }
}
```

## <a name="using-icommand"></a>À l’aide de ICommand

Applications qui utilisent le modèle Model-View-ViewModel (MVVM) généralement utiliser `ICommand` plutôt que directement le câblage des gestionnaires d’événements. Le [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) peut facilement prendre en charge `ICommand` soit en définissant la liaison dans le code :

```csharp
var tapGestureRecognizer = new TapGestureRecognizer();
tapGestureRecognizer.SetBinding (TapGestureRecognizer.CommandProperty, "TapCommand");
image.GestureRecognizers.Add(tapGestureRecognizer);
```

ou, à l’aide de Xaml :

```xaml
<Image Source="tapped.jpg">
    <Image.GestureRecognizers>
        <TapGestureRecognizer
            Command="{Binding TapCommand}"
            CommandParameter="Image1" />
    </Image.GestureRecognizers>
</Image>
```

Vous trouverez le code complet pour ce modèle de vue dans l’exemple. Les informations pertinentes `Command` détails d’implémentation sont présentées ci-dessous :

```csharp
public class TapViewModel : INotifyPropertyChanged
{
    int taps = 0;
    ICommand tapCommand;
    public TapViewModel () {
        // configure the TapCommand with a method
        tapCommand = new Command (OnTapped);
    }
    public ICommand TapCommand {
        get { return tapCommand; }
    }
    void OnTapped (object s)  {
        taps++;
        Debug.WriteLine ("parameter: " + s);
    }
    //region INotifyPropertyChanged code omitted
}
```


## <a name="related-links"></a>Liens associés

- [TapGesture (exemple)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/TapGesture/)
- [GestureRecognizer](xref:Xamarin.Forms.GestureRecognizer)
- [TapGestureRecognizer](xref:Xamarin.Forms.TapGestureRecognizer)
