---
title: Solutions de secours de liaison Xamarin.Forms
description: Cet article explique comment rendre les liaisons plus robuste en définissant les valeurs de secours qui seront utilisés si la liaison échoue.
ms.prod: xamarin
ms.assetid: 637ACD9D-3E5D-4014-86DE-A77D1FEF238A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/16/2018
ms.openlocfilehash: fa375720730630065609e328b343e16578c6f1df
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50131190"
---
# <a name="xamarinforms-binding-fallbacks"></a>Solutions de secours de liaison Xamarin.Forms

Liaisons de données échouent parfois, car la source de liaison ne peut pas être résolue, ou parce que la liaison réussit mais retourne un `null` valeur. Bien que ces scénarios peuvent être gérés avec les convertisseurs de valeur, ou tout autre code supplémentaire, des liaisons de données peuvent devenir plus robustes en définissant les valeurs de secours à utiliser si le processus de liaison échoue. Cela est possible en définissant le [ `FallbackValue` ](xref:Xamarin.Forms.BindingBase.FallbackValue) et [ `TargetNullValue` ](xref:Xamarin.Forms.BindingBase.TargetNullValue) propriétés dans une expression de liaison. Étant donné que ces propriétés se trouvent dans le [ `BindingBase` ](xref:Xamarin.Forms.BindingBase) (classe), elles peuvent servir avec des liaisons, des liaisons compilées et avec le `Binding` extension de balisage.

> [!NOTE]
> Utilisation de la [ `FallbackValue` ](xref:Xamarin.Forms.BindingBase.FallbackValue) et [ `TargetNullValue` ](xref:Xamarin.Forms.BindingBase.TargetNullValue) des propriétés dans une expression de liaison est facultatif.

## <a name="defining-a-fallback-value"></a>Définition d’une valeur de secours

Le [ `FallbackValue` ](xref:Xamarin.Forms.BindingBase.FallbackValue) propriété permet à une valeur à définir de secours qui sera utilisée lors de la liaison *source* ne peut pas être résolu. Un scénario courant pour la définition de cette propriété est lors de la liaison aux propriétés de la source qui ne peuvent pas exister sur tous les objets dans une collection de types hétérogènes liée.

Le **MonkeyDetail** page illustre le paramètre de la [ `FallbackValue` ](xref:Xamarin.Forms.BindingBase.FallbackValue) propriété :

```xaml
<Label Text="{Binding Population, FallbackValue='Population size unknown'}"
       ... />   
```

La liaison sur le [ `Label` ](xref:Xamarin.Forms.Label) définit un [ `FallbackValue` ](xref:Xamarin.Forms.BindingBase.FallbackValue) valeur sera définie sur la cible si la source de liaison ne peut pas être résolue. Par conséquent, la valeur définie par le `FallbackValue` propriété s’affiche si le `Population` propriété n’existe pas sur l’objet lié. Notez qu’ici le `FallbackValue` valeur de propriété est délimité par des guillemets simples (apostrophe) caractères.

Au lieu de la définition [ `FallbackValue` ](xref:Xamarin.Forms.BindingBase.FallbackValue) inline de valeurs de propriété, il est recommandé de les définir en tant que ressources dans un [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary). L’avantage de cette approche est que ces valeurs sont définies une seule fois dans un emplacement unique et sont plus facilement localisables. Les ressources peuvent ensuite être récupérés à l’aide de la `StaticResource` extension de balisage :

```xaml
<Label Text="{Binding Population, FallbackValue={StaticResource populationUnknown}}"
       ... />  
```

> [!NOTE]
> Il n’est pas possible de définir le `FallbackValue` propriété avec une expression de liaison.

Voici le programme en cours d’exécution sur les trois plateformes :

![Liaison de FallbackValue](binding-fallbacks-images/bindingunavailable-detail-cropped.png "FallbackValue liaison")

Lorsque le `FallbackValue` propriété n’est pas définie dans une expression de liaison et le chemin de liaison ou la partie du chemin d’accès n’est pas résolu, [ `BindableProperty.DefaultValue` ](xref:Xamarin.Forms.BindableProperty.DefaultValue) est défini sur la cible. Toutefois, lorsque le `FallbackValue` propriété est définie et le chemin de liaison ou la partie du chemin d’accès n’est pas résolu, la valeur de la `FallbackValue` propriété valeur est définie sur la cible. Par conséquent, sur le **MonkeyDetail** page le [ `Label` ](xref:Xamarin.Forms.Label) affiche « Inconnu de taille de remplissage », car l’objet lié n’a pas un `Population` propriété.

> [!IMPORTANT]
> Un convertisseur de valeur définie n’est pas exécuté dans une expression de liaison lors de la [ `FallbackValue` ](xref:Xamarin.Forms.BindingBase.FallbackValue) propriété est définie.

## <a name="defining-a-null-replacement-value"></a>Définition d’une valeur de remplacement de valeur null

Le [ `TargetNullValue` ](xref:Xamarin.Forms.BindingBase.TargetNullValue) propriété permet de définir une valeur de remplacement qui sera utilisée lors de la liaison *source* est résolu, mais la valeur est `null`. Un scénario courant pour la définition de cette propriété est lors de la liaison aux propriétés de la source qui peuvent être `null` dans une collection liée.

Le **singes** page illustre le paramètre de la [ `TargetNullValue` ](xref:Xamarin.Forms.BindingBase.TargetNullValue) propriété :

```xaml
<ListView ItemsSource="{Binding Monkeys}"
          ...>
    <ListView.ItemTemplate>
        <DataTemplate>
            <ViewCell>
                <Grid>
                    ...
                    <Image Source="{Binding ImageUrl, TargetNullValue='https://upload.wikimedia.org/wikipedia/commons/2/20/Point_d_interrogation.jpg'}"
                           ... />
                    ...
                    <Label Text="{Binding Location, TargetNullValue='Location unknown'}"
                           ... />
                </Grid>
            </ViewCell>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

Les liaisons sur le [ `Image` ](xref:Xamarin.Forms.Image) et [ `Label` ](xref:Xamarin.Forms.Label) définissent tous deux [ `TargetNullValue` ](xref:Xamarin.Forms.BindingBase.TargetNullValue) les valeurs qui seront appliqués si le chemin de liaison retourne `null`. Par conséquent, les valeurs définies par le `TargetNullValue` propriétés seront affichées pour tous les objets dans la collection où la `ImageUrl` et `Location` propriétés ne sont pas définies. Notez qu’ici le `TargetNullValue` les valeurs de propriété sont délimitées par des caractères de guillemets simples (apostrophe).

Au lieu de la définition [ `TargetNullValue` ](xref:Xamarin.Forms.BindingBase.TargetNullValue) inline de valeurs de propriété, il est recommandé de les définir en tant que ressources dans un [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary). L’avantage de cette approche est que ces valeurs sont définies une seule fois dans un emplacement unique et sont plus facilement localisables. Les ressources peuvent ensuite être récupérés à l’aide de la `StaticResource` extension de balisage :

```xaml
<Image Source="{Binding ImageUrl, TargetNullValue={StaticResource fallbackImageUrl}}"
       ... />
<Label Text="{Binding Location, TargetNullValue={StaticResource locationUnknown}}"
       ... />
```

> [!NOTE]
> Il n’est pas possible de définir le `TargetNullValue` propriété avec une expression de liaison.

Voici le programme en cours d’exécution sur les trois plateformes :

[![Liaison de TargetNullValue](binding-fallbacks-images/bindingunavailable-small.png "TargetNullValue liaison")](binding-fallbacks-images/bindingunavailable-large.png#lightbox "TargetNullValue liaison")

Lorsque le `TargetNullValue` propriété n’est pas définie dans une expression de liaison, une valeur de la source de `null` sera convertie si un convertisseur de valeurs est défini, la mise en forme dans un `StringFormat` est défini, et le résultat est ensuite défini sur la cible. Toutefois, lorsque le `TargetNullValue` propriété est définie, une valeur de la source de `null` sera convertie si un convertisseur de valeurs est défini, et s’il s’agit toujours `null` après la conversion, la valeur de la `TargetNullValue` propriété est définie sur la cible.

> [!IMPORTANT]
> Mise en forme de chaîne n’est pas appliqué dans une expression de liaison lorsque la `TargetNullValue` propriété est définie.

## <a name="related-links"></a>Liens associés

- [Démonstrations de liaison de données (exemple)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
