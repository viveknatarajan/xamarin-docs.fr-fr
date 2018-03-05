---
title: Passage des Arguments en XAML
description: "Cet article décrit à l’aide d’attributs XAML qui peuvent être utilisés pour passer des arguments aux constructeurs par défaut, pour appeler des méthodes de fabrique et pour spécifier le type d’argument générique."
ms.topic: article
ms.prod: xamarin
ms.assetid: 8F3B267F-499E-4D79-9193-FCA99F199519
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 10/25/2016
ms.openlocfilehash: a30dd9b33466ac6907322f8c6b586c012452a44f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="passing-arguments-in-xaml"></a>Passage des Arguments en XAML

_Cet article décrit à l’aide d’attributs XAML qui peuvent être utilisés pour passer des arguments aux constructeurs par défaut, pour appeler des méthodes de fabrique et pour spécifier le type d’argument générique._

## <a name="overview"></a>Vue d'ensemble

Il est souvent nécessaire instancier des objets avec des constructeurs qui nécessitent des arguments, ou en appelant une méthode statique de la création. Cela peut être obtenue en XAML à l’aide de la `x:Arguments` et `x:FactoryMethod` attributs :

- Le `x:Arguments` attribut est utilisé pour spécifier des arguments de constructeur pour un constructeur par défaut, ou pour une déclaration d’objet de fabrique (méthode). Pour plus d’informations, consultez [en passant les Arguments de constructeur](#constructor_arguments).
- Le `x:FactoryMethod` attribut est utilisé pour spécifier une méthode de fabrique qui peut être utilisée pour initialiser un objet. Pour plus d’informations, consultez [appelant des méthodes de fabrique](#factory_methods).

En outre, le `x:TypeArguments` attribut peut être utilisé pour spécifier des arguments de type générique au constructeur d’un type générique. Pour plus d’informations, consultez [en spécifiant un Argument de Type générique](#generic_type_arguments).

<a name="constructor_arguments" />

## <a name="passing-constructor-arguments"></a>Passage des Arguments de constructeur

Arguments peuvent être passés à un constructeur personnalisé à l’aide de la `x:Arguments` attribut. Chaque argument du constructeur doit être placé dans un élément XML qui représente le type de l’argument. Xamarin.Forms prend en charge les éléments suivants pour les types de base :

- `x:Object`
- `x:Boolean`
- `x:Byte`
- `x:Int16`
- `x:Int32`
- `x:Int64`
- `x:Single`
- `x:Double`
- `x:Decimal`
- `x:Char`
- `x:String`
- `x:TimeSpan`
- `x:Array`
- `x:DateTime`

L’exemple de code suivant montre comment utiliser le `x:Arguments` attribut avec trois [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) constructeurs :

```xaml
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color>
      <x:Arguments>
        <x:Double>0.9</x:Double>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color>
      <x:Arguments>
        <x:Double>0.25</x:Double>
        <x:Double>0.5</x:Double>
        <x:Double>0.75</x:Double>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color>
      <x:Arguments>
        <x:Double>0.8</x:Double>
        <x:Double>0.5</x:Double>
        <x:Double>0.2</x:Double>
        <x:Double>0.5</x:Double>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
```

Le nombre d’éléments dans le `x:Arguments` balise et les types de ces éléments, doivent correspondre à l’un de le [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) constructeurs. Le `Color` [constructeur](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Color.Color/p/System.Double/) avec un seul paramètre requiert en nuances de gris comprise entre 0 (noir) à 1 (blanc). Le `Color` [constructeur](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Color.Color/p/System.Double/System.Double/System.Double/) avec trois paramètres requiert une valeur de rouge, verte et bleue comprises entre 0 et 1. Le `Color` [constructeur](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Color.Color/p/System.Double/System.Double/System.Double/System.Double/) avec quatre paramètres ajoute un canal alpha en tant que quatrième paramètre.

Les captures d’écran suivantes affichent le résultat de l’appel de chaque [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) constructeur avec les valeurs d’argument spécifiée :

![](passing-arguments-images/passing-arguments.png "BoxView.Color spécifié avec un x : Arguments")

<a name="factory_methods" />

## <a name="calling-factory-methods"></a>Méthodes de fabrique

Méthodes de fabrique qui peuvent être appelées dans XAML en spécifiant la méthode nom à l’aide de la `x:FactoryMethod` attribut et ses arguments à l’aide de la `x:Arguments` attribut. Une méthode de fabrique est un `public static` méthode qui retourne des objets ou des valeurs du même type que la classe ou structure qui définit les méthodes.

Le [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) structure définit un nombre de méthodes de fabrique et l’exemple de code suivant illustre l’appel trois d'entre eux :

```xaml
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color x:FactoryMethod="FromRgba">
      <x:Arguments>
        <x:Int32>192</x:Int32>
        <x:Int32>75</x:Int32>
        <x:Int32>150</x:Int32>                      
        <x:Int32>128</x:Int32>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color x:FactoryMethod="FromHsla">
      <x:Arguments>
        <x:Double>0.23</x:Double>
        <x:Double>0.42</x:Double>
        <x:Double>0.69</x:Double>
        <x:Double>0.7</x:Double>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color x:FactoryMethod="FromHex">
      <x:Arguments>
        <x:String>#FF048B9A</x:String>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
```

Le nombre d’éléments dans le `x:Arguments` balise et les types de ces éléments, doivent correspondre les arguments de la méthode de fabrique appelée. Le [ `FromRgba` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromRgba/p/System.Int32/System.Int32/System.Int32/System.Int32/) méthode de fabrique requiert quatre [ `Int32` ](https://developer.xamarin.com/api/type/System.Int32/) paramètres, qui représentent les valeurs de rouges, verts, bleus et alphanumériques, compris entre 0 et 255. Le [ `FromHsla` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromHsla/p/System.Double/System.Double/System.Double/System.Double/) méthode de fabrique requiert quatre [ `Double` ](https://developer.xamarin.com/api/type/System.Double/) paramètres, qui représentent les valeurs de teinte, saturation, luminosité et alphanumériques, entre 0 et 1 respectivement. Le [ `FromHex` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromHex/p/System.String/) requiert de la méthode de fabrique un [ `String` ](https://developer.xamarin.com/api/type/System.String/) qui représente la valeur hexadécimale (A) couleur RVB.

Les captures d’écran suivantes affichent le résultat de l’appel de chaque [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) méthode de fabrique avec les valeurs de l’argument spécifié :

![](passing-arguments-images/factory-methods.png "BoxView.Color spécifié avec x : FactoryMethod et x : Arguments")

<a name="generic_type_arguments" />

## <a name="specifying-a-generic-type-argument"></a>Spécification d’un Argument de Type générique

Arguments de type générique pour le constructeur d’un type générique peuvent être spécifiés à l’aide de la `x:TypeArguments` d’attribut, comme illustré dans l’exemple de code suivant :

```xaml
<ContentPage ...>
  <StackLayout>
    <StackLayout.Margin>
      <OnPlatform x:TypeArguments="Thickness">
        <On Platform="iOS" Value="0,20,0,0" />
        <On Platform="Android" Value="5, 10" />
        <On Platform="WinPhone, Windows" Value="10" />
      </OnPlatform>
    </StackLayout.Margin>
  </StackLayout>
</ContentPage>
```

Le [ `OnPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnPlatform%3CT%3E/) classe est une classe générique et doit être instancié avec un `x:TypeArguments` attribut qui correspond au type cible. Dans le [ `On` ](https://developer.xamarin.com/api/type/Xamarin.Forms.On/) (classe), le [ `Platform` ](https://developer.xamarin.com/api/property/Xamarin.Forms.On.Platform/) attribut peut accepter un seul `string` valeur ou plusieurs virgules `string` valeurs. Dans cet exemple, le [ `StackLayout.Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) est définie sur une plateforme spécifique [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/).

## <a name="summary"></a>Récapitulatif

Cet article ne présente à l’aide d’attributs XAML qui peuvent être utilisés pour passer des arguments aux constructeurs par défaut, pour appeler des méthodes de fabrique et pour spécifier le type d’argument générique.


## <a name="related-links"></a>Liens associés

- [Espaces de noms XAML](~/xamarin-forms/xaml/namespaces.md)
- [Passer des Arguments de constructeur (exemple)](https://developer.xamarin.com/samples/xamarin-forms/xaml/passingconstructorarguments/)
- [Méthodes de fabrique (exemple)](https://developer.xamarin.com/samples/xamarin-forms/xaml/callingfactorymethods/)
