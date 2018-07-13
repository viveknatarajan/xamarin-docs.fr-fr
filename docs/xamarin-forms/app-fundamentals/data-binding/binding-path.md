---
title: Chemin de liaison Xamarin.Forms
description: Cet article explique comment utiliser des liaisons de données de Xamarin.Forms pour accéder aux sous-propriétés et membres de la collection avec la propriété de chemin d’accès de la classe de liaison.
ms.prod: xamarin
ms.assetid: 3CF721A5-E157-468B-AD3A-DA0A45E58E8D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: 887a20f1791a190c182e6d179cfabb46c6e0eb48
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998945"
---
# <a name="xamarinforms-binding-path"></a>Chemin de liaison Xamarin.Forms

Dans tous les exemples liaison de données précédentes, le [ `Path` ](xref:Xamarin.Forms.Binding.Path) propriété de la `Binding` classe (ou le [ `Path` ](xref:Xamarin.Forms.Xaml.BindingExtension.Path) propriété de la `Binding` extension de balisage) a été défini pour une propriété unique. Il est en fait possible de définir `Path` à un *sous-propriété* (une propriété d’une propriété), ou à un membre d’une collection.

Par exemple, supposons que votre page contient un `TimePicker`:

```xaml
<TimePicker x:Name="timePicker">
```

Le `Time` propriété de `TimePicker` est de type `TimeSpan`, mais vous voulez peut-être créer une liaison de données qui fait référence à la `TotalSeconds` propriété cela `TimeSpan` valeur. Voici la liaison de données :

```xaml
{Binding Source={x:Reference timePicker},
         Path=Time.TotalSeconds}
```

Le `Time` propriété est de type `TimeSpan`, qui a un `TotalSeconds` propriété. Le `Time` et `TotalSeconds` propriétés sont simplement connectées par un point. Les éléments dans le `Path` chaîne font toujours référence aux propriétés et pas pour les types de ces propriétés.

Exemple et plusieurs autres figurent dans le **Variations de chemin d’accès** page :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:globe="clr-namespace:System.Globalization;assembly=mscorlib"
             x:Class="DataBindingDemos.PathVariationsPage"
             Title="Path Variations"
             x:Name="page">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Label">
                <Setter Property="FontSize" Value="Large" />
                <Setter Property="HorizontalTextAlignment" Value="Center" />
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout Margin="10, 0">
        <TimePicker x:Name="timePicker" />

        <Label Text="{Binding Source={x:Reference timePicker},
                              Path=Time.TotalSeconds,
                              StringFormat='{0} total seconds'}" />

        <Label Text="{Binding Source={x:Reference page},
                              Path=Content.Children.Count,
                              StringFormat='There are {0} children in this StackLayout'}" />

        <Label Text="{Binding Source={x:Static globe:CultureInfo.CurrentCulture},
                              Path=DateTimeFormat.DayNames[3],
                              StringFormat='The middle day of the week is {0}'}" />

        <Label>
            <Label.Text>
                <Binding Path="DateTimeFormat.DayNames[3]"
                         StringFormat="The middle day of the week in France is {0}">
                    <Binding.Source>
                        <globe:CultureInfo>
                            <x:Arguments>
                                <x:String>fr-FR</x:String>
                            </x:Arguments>
                        </globe:CultureInfo>
                    </Binding.Source>
                </Binding>
            </Label.Text>
        </Label>

        <Label Text="{Binding Source={x:Reference page},
                              Path=Content.Children[1].Text.Length,
                              StringFormat='The first Label has {0} characters'}" />
    </StackLayout>
</ContentPage>
```

Dans le deuxième `Label`, la source de liaison est la page elle-même. Le `Content` propriété est de type `StackLayout`, qui a un `Children` propriété de type `IList<View>`, qui a un `Count` propriété qui indique le nombre d’enfants.

## <a name="paths-with-indexers"></a>Chemins d’accès avec les indexeurs

La liaison dans la troisième `Label` dans le **Variations de chemin d’accès** références des pages le [ `CultureInfo` ](xref:System.Globalization.CultureInfo) classe dans le `System.Globalization` espace de noms :

```xaml
<Label Text="{Binding Source={x:Static globe:CultureInfo.CurrentCulture},
                      Path=DateTimeFormat.DayNames[3],
                      StringFormat='The middle day of the week is {0}'}" />
```

La source est définie à la méthode statique `CultureInfo.CurrentCulture` propriété, qui est un objet de type `CultureInfo`. Que classe définit une propriété nommée `DateTimeFormat` de type [ `DateTimeFormatInfo` ](xref:System.Globalization.DateTimeFormatInfo) qui contient un `DayNames` collection. L’index sélectionne le quatrième élément.

La quatrième `Label` fait quelque chose de similaire mais, pour la culture associée France. Le `Source` propriété de la liaison est définie sur `CultureInfo` objet avec un constructeur :

```xaml
<Label>
    <Label.Text>
        <Binding Path="DateTimeFormat.DayNames[3]"
                 StringFormat="The middle day of the week in France is {0}">
            <Binding.Source>
                <globe:CultureInfo>
                    <x:Arguments>
                        <x:String>fr-FR</x:String>
                    </x:Arguments>
                </globe:CultureInfo>
            </Binding.Source>
        </Binding>
    </Label.Text>
</Label>
```

Consultez [en passant les Arguments de constructeur](~/xamarin-forms/xaml/passing-arguments.md#constructor_arguments) pour plus d’informations sur la spécification des arguments de constructeur dans XAML.

Enfin, le dernier exemple est similaire à la seconde, à ceci près qu’elle fait référence à un des enfants de le `StackLayout`:

```xaml
<Label Text="{Binding Source={x:Reference page},
                      Path=Content.Children[1].Text.Length,
                      StringFormat='The first Label has {0} characters'}" />
```

Cet enfant est un `Label`, qui a un `Text` propriété de type `String`, qui a un `Length` propriété. La première `Label` rapports le `TimeSpan` définie le `TimePicker`, de sorte que lorsque ce texte est modifié, la dernière `Label` est également modifiée.

Voici le programme en cours d’exécution sur les trois plateformes :

[![Les Variations de chemin d’accès](binding-path-images/pathvariations-small.png "Variations de chemin d’accès")](binding-path-images/pathvariations-large.png#lightbox "Variations de chemin d’accès")

## <a name="debugging-complex-paths"></a>Débogage des tracés complexes

Les définitions complexes de chemin d’accès peuvent être difficiles à construire : vous devez connaître le type de chaque sous-propriété ou le type d’éléments dans la collection où ajouter correctement la sous-propriété suivante, mais les types eux-mêmes n’apparaissent pas dans le chemin d’accès. Une bonne technique consiste à générer le chemin d’accès de façon incrémentielle et examinez les résultats intermédiaires. Pour ce dernier exemple, vous pouvez commencer par non `Path` définition du tout :

```xaml
<Label Text="{Binding Source={x:Reference page},
                      StringFormat='{0}'}" />
```

Qui affiche le type de la source de liaison, ou `DataBindingDemos.PathVariationsPage`. Vous savez `PathVariationsPage` dérive `ContentPage`, donc il a un `Content` propriété :

```xaml
<Label Text="{Binding Source={x:Reference page},
                      Path=Content,
                      StringFormat='{0}'}" />
```

Le type de la `Content` propriété est désormais révélée être `Xamarin.Forms.StackLayout`. Ajouter le `Children` propriété le `Path` et le type est `Xamarin.Forms.ElementCollection'1[Xamarin.Forms.View]`, qui est une classe interne à Xamarin.Forms, mais évidemment un type de collection. Ajouter un index à qui et le type est `Xamarin.Forms.Label`. Continuer de cette façon.

Xamarin.Forms traite le chemin de liaison, il installe un `PropertyChanged` gestionnaire sur n’importe quel objet dans le chemin d’accès qui implémente le `INotifyPropertyChanged` interface. Par exemple, la liaison finale réagit à une modification dans la première `Label` , car le `Text` les modifications de propriété.

Si une propriété dans le chemin de liaison n’implémente pas `INotifyPropertyChanged`, toutes les modifications à cette propriété seront ignorées. Certaines modifications peut invalider entièrement le chemin de liaison, vous devez utiliser cette technique uniquement lorsque la chaîne de propriétés et les sous-propriétés jamais deviennent non valides.



## <a name="related-links"></a>Liens associés

- [Démonstrations de liaison de données (exemple)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Chapitre de liaison de données à partir du livre de Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
