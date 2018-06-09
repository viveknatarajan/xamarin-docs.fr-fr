---
title: Ajout d’une reconnaissance de mouvement du mouvement Tap
description: Cet article explique comment utiliser l’action d’appuyer pour la détection de clic dans une application de Xamarin.Forms. Appuyez sur la détection est implémentée avec la classe TapGestureRecognizer.
ms.prod: xamarin
ms.assetid: 1D150BAF-4157-49BC-90A0-153323B8EBCF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
ms.openlocfilehash: bbe4ca7a1080459b8aeb33640be5158b15e97715
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35240663"
---
# <a name="adding-a-tap-gesture-gesture-recognizer"></a>Ajout d’une reconnaissance de mouvement du mouvement Tap

_L’action d’appuyer est utilisée pour la détection de drainage et est implémentée avec la classe TapGestureRecognizer._

## <a name="overview"></a>Vue d'ensemble

Pour rendre un élément d’interface utilisateur interactive avec l’action d’appuyer, créer un [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/) d’une instance, de gérer la [ `Tapped` ](https://developer.xamarin.com/api/event/Xamarin.Forms.TapGestureRecognizer.Tapped/) événement et ajoutez la nouvelle reconnaissance de mouvement à le [ `GestureRecognizers` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.GestureRecognizers/) collection sur l’élément d’interface utilisateur. Le code suivant montre d’exemple un `TapGestureRecognizer` attaché à un [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) élément :

```csharp
var tapGestureRecognizer = new TapGestureRecognizer();
tapGestureRecognizer.Tapped += (s, e) => {
    // handle the tap
};
image.GestureRecognizers.Add(tapGestureRecognizer);
```

Par défaut, l’image répond aux clics unique. Définir le [ `NumberOfTapsRequired` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TapGestureRecognizer.NumberOfTapsRequired/) propriété pour attendre un double-clic (ou plus de clics si nécessaire).

```csharp
tapGestureRecognizer.NumberOfTapsRequired = 2; // double-tap
```

Lorsque [ `NumberOfTapsRequired` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TapGestureRecognizer.NumberOfTapsRequired/) a la valeur supérieure à 1, le Gestionnaire d’événements n’est exécuté si les clics ont lieu à une durée donnée (cette période n’est pas configurable). Si les pressions deuxième (ou ultérieures) ne se produisent pas dans ce délai qu’ils sont effectivement ignorés et le nombre de drainage redémarre.

<a name="Using_Xaml" />

## <a name="using-xaml"></a>À l’aide de Xaml

Un module de reconnaissance de mouvement peut être ajouté à un contrôle en Xaml à l’aide des propriétés jointes. La syntaxe pour ajouter un [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/) à une image est indiqué ci-dessous (dans ce cas en définissant un *double appui* événement) :

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

Les applications qui utilisent le modèle Mvvm généralement utilisent `ICommand` plutôt que directement l’écriture des gestionnaires d’événements. Le [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/) permettent de facilement `ICommand` soit en définissant la liaison dans le code :

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

Vous trouverez le code complet pour ce modèle de vue dans l’exemple. Les `Command` détails d’implémentation sont indiquées ci-dessous :

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

## <a name="summary"></a>Récapitulatif

L’action d’appuyer est utilisée pour la détection de drainage et est implémentée avec les [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/) classe. Le nombre de coefficients peut être spécifié pour reconnaître le double-clic (ou triple-clic, ou plusieurs appuie) comportement.


## <a name="related-links"></a>Liens associés

- [TapGesture (exemple)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/TapGesture/)
- [GestureRecognizer](https://developer.xamarin.com/api/type/Xamarin.Forms.GestureRecognizer/)
- [TapGestureRecognizer](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/)
