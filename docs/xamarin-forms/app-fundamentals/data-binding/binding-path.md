---
title: Chemin de liaison
description: "Utiliser des liaisons de données pour les membres de collection et les sous-propriétés de l’accès"
ms.topic: article
ms.prod: xamarin
ms.assetid: 3CF721A5-E157-468B-AD3A-DA0A45E58E8D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: fb385a9c7d1dfd01d95691b77122cdbb84d814e5
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="binding-path"></a>Chemin de liaison

Dans tous les exemples liaison de données précédentes, la [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/) propriété de la `Binding` classe (ou le [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.BindingExtension.Path/) propriété de la `Binding` extension de balisage) a été défini pour une seule propriété. Il est en fait possible de définir `Path` à un *sous-propriété* (une propriété d’une propriété) ou à un membre d’une collection.

Par exemple, supposons que votre page contient un `TimePicker`:

```xaml
<TimePicker x:Name="timePicker">
```

Le `Time` propriété du `TimePicker` est de type `TimeSpan`, mais vous pouvez créer une liaison de données qui fait référence à la `TotalSeconds` propriété de ce `TimeSpan` valeur. Voici la liaison de données :

```xaml
{Binding Source={x:Reference timePicker},
         Path=Time.TotalSeconds}
```
         
Le `Time` propriété est de type `TimeSpan`, qui a un `TotalSeconds` propriété. Le `Time` et `TotalSeconds` propriétés sont simplement reliées par un point. Les éléments dans le `Path` chaîne font toujours référence aux propriétés et pas pour les types de ces propriétés.

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

Dans la seconde `Label`, la source de liaison est la page elle-même. Le `Content` propriété est de type `StackLayout`, qui a un `Children` propriété de type `IList<View>`, qui a un `Count` propriété qui indique le nombre d’enfants.

## <a name="paths-with-indexers"></a>Chemins d’accès avec les indexeurs

La liaison dans la troisième `Label` dans les **Variations de chemin d’accès** références des pages le [ `CultureInfo` ](https://developer.xamarin.com/api/type/System.Globalization.CultureInfo/) classe dans le `System.Globalization` espace de noms :

```xaml
<Label Text="{Binding Source={x:Static globe:CultureInfo.CurrentCulture},
                      Path=DateTimeFormat.DayNames[3],
                      StringFormat='The middle day of the week is {0}'}" />
```

La source est définie pour la méthode statique `CultureInfo.CurrentCulture` propriété, qui est un objet de type `CultureInfo`. Que classe définit une propriété nommée `DateTimeFormat` de type [ `DateTimeFormatInfo` ](https://developer.xamarin.com/api/type/System.Globalization.DateTimeFormatInfo/) qui contient un `DayNames` collection. L’index sélectionne le quatrième élément.

La quatrième `Label` fait quelque chose de similaire, mais pour la culture associée France. Le `Source` de la liaison est définie sur `CultureInfo` objet avec un constructeur :

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

Consultez [en passant les Arguments de constructeur](~/xamarin-forms/xaml/passing-arguments.md#constructor_arguments) pour plus d’informations sur la spécification des arguments de constructeur en XAML.

Enfin, le dernier exemple est semblable à la seconde, à ceci près qu’elle fait référence à un des enfants de le `StackLayout`:

```xaml
<Label Text="{Binding Source={x:Reference page},
                      Path=Content.Children[1].Text.Length,
                      StringFormat='The first Label has {0} characters'}" />
```

Cet enfant est un `Label`, qui a un `Text` propriété de type `String`, qui a un `Length` propriété. La première `Label` rapports le `TimeSpan` définie dans le `TimePicker`, de sorte que lorsque ce texte est modifiée, la dernière `Label` est également modifiée.

Voici le programme en cours d’exécution sur les trois plateformes :

[![Variantes de chemin d’accès](binding-path-images/pathvariations-small.png "Variations de chemin d’accès")](binding-path-images/pathvariations-large.png#lightbox "Variations de chemin d’accès")

## <a name="debugging-complex-paths"></a>Débogage des chemins d’accès complexes

Les définitions complexes de chemin d’accès peuvent être difficiles à construire : vous devez connaître le type de chaque propriété sous-répertoire ou le type des éléments dans la collection où ajouter correctement de la sous-propriété suivante, mais les types eux-mêmes n’apparaissent pas dans le chemin d’accès. Une bonne technique consiste à générer le chemin d’accès de façon incrémentielle et examinez les résultats intermédiaires. Pour ce dernier exemple, vous pouvez démarrer sans aucune `Path` la définition de toutes les :

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

Le type de la `Content` propriété est désormais révélée à être `Xamarin.Forms.StackLayout`. Ajouter le `Children` propriété le `Path` et le type est `Xamarin.Forms.ElementCollection'1[Xamarin.Forms.View]`, qui est une classe interne à Xamarin.Forms, mais évidemment d’un type de collection. Ajouter un index à qui et le type est `Xamarin.Forms.Label`. La suite de cette façon.

Xamarin.Forms traite le chemin de liaison, il installe un `PropertyChanged` gestionnaire sur n’importe quel objet dans le chemin d’accès qui implémente le `INotifyPropertyChanged` interface. Par exemple, la liaison finale réagit à une modification de la première `Label` , car le `Text` de propriété est modifiée. 

Si une propriété dans le chemin de liaison n’implémente pas `INotifyPropertyChanged`, toute modification apportée à cette propriété sera ignorée. Certaines modifications rendrait entièrement le chemin de liaison, vous devez utiliser cette technique uniquement lorsque la chaîne de propriétés et les sous-propriétés jamais deviennent non valides.



## <a name="related-links"></a>Liens associés

- [Démonstrations de liaison de données (exemple)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Chapitre de liaison de données à partir du carnet de Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
