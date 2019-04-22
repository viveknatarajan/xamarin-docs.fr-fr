---
title: Utiliser des données de conception avec le Générateur d’aperçu XAML
description: Cet article explique comment utiliser des données au moment du design pour afficher les mises en page comportant beaucoup de données dans le Générateur d’aperçu XAML sans exécuter votre application.
ms.prod: xamarin
ms.assetid: 0F608019-5951-4BE6-80E0-9EEE1733D642
ms.technology: xamarin-forms
author: maddyleger1
ms.author: maleger
ms.date: 03/27/2019
ms.openlocfilehash: 0ff9f8b5ee6f9468650b6535745706bee8f96536
ms.sourcegitcommit: 3489c281c9eb5ada2cddf32d73370943342a1082
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58866356"
---
# <a name="use-design-time-data-with-the-xaml-previewer"></a>Utiliser des données de conception avec le Générateur d’aperçu XAML

_Certaines dispositions sont difficiles à visualiser les données. Utilisez ces conseils pour tirer le meilleur parti de l’aperçu de vos pages comportant beaucoup de données dans le Générateur d’aperçu XAML._

## <a name="design-time-data-basics"></a>Concevoir des bases de données de temps

Données de conception sont fausses données que vous définissez pour faciliter vos contrôles visualiser dans le Générateur d’aperçu XAML. Pour commencer, ajoutez les lignes de code suivantes à l’en-tête de votre page XAML :

```csharp
xmlns:d="http://xamarin.com/schemas/2014/forms/design"
xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
mc:Ignorable="d"
```

Après avoir ajouté les espaces de noms, vous pouvez placer `d:` devant n’importe quel attribut ou d’un contrôle à afficher dans le Générateur d’aperçu XAML. Éléments avec `d:` ne sont pas visibles lors de l’exécution.

Par exemple, vous pouvez ajouter le texte à une étiquette qui est généralement liée aux données.

```csharp
<Label Text={Binding Name} d:Text="Name" />
```

[![Concevoir des données d’heure avec le texte dans une étiquette](xaml-previewer-images/designtimedata-label-sm.png "conception heure des données avec le texte une étiquette")](xaml-previewer-images/designtimedata-label-lg.png#lightbox)

 Dans cet exemple, sans `d:Text`, le Générateur d’aperçu XAML afficherait rien pour l’étiquette. Au lieu de cela, il affiche « Name » où l’étiquette a des données réelles lors de l’exécution.

Vous pouvez utiliser `d:` avec n’importe quel attribut pour un contrôle Xamarin.Forms, telles que les couleurs, les tailles de police et l’espacement. Vous pouvez même l’ajouter au contrôle lui-même :

```csharp
<d:Button Text="Design Time Button" />
```

[![Concevoir des données d’heure avec un contrôle bouton](xaml-previewer-images/designtimedata-controls-sm.png "Design des données de temps avec un contrôle de bouton")](xaml-previewer-images/designtimedata-controls-lg.png#lightbox)

Dans cet exemple, le bouton apparaît uniquement au moment du design. Utilisez cette méthode pour placer un espace réservé dans un [contrôle personnalisé non pris en charge par le Générateur d’aperçu XAML](render-custom-controls.md).

## <a name="preview-images-at-design-time"></a>Images d’aperçu au moment du design

Vous pouvez définir un moment du design pour les images qui sont liées à la page ou chargée dynamiquement dans Source. Dans votre projet Android, ajoutez l’image que vous souhaitez afficher dans le Générateur d’aperçu XAML pour le **ressources > Drawable** dossier. Dans votre projet iOS, ajoutez l’image à la **ressources** dossier. Vous pouvez ensuite afficher cette image dans le Générateur d’aperçu XAML au moment du design :

```csharp
<Image Source={Binding ProfilePicture} d:Source="DesignTimePicture.jpg" />
```
[![Concevoir des données de temps avec des images](xaml-previewer-images/designtimedata-image-sm.png "données chronologique avec iamges de conception")](xaml-previewer-images/designtimedata-image-lg.png#lightbox)

## <a name="design-time-data-for-listviews"></a>Données de temps de conception de ListView

ListView est largement utilisé pour afficher des données dans une application mobile. Toutefois, ils sont difficiles à visualiser les données réelles. Pour utiliser les données de conception avec eux, vous devez créer un tableau de temps de conception à utiliser comme un ItemsSource. Le Générateur d’aperçu XAML présente les nouveautés dans ce tableau dans votre ListView au moment du design.

```csharp
<StackLayout>
    <ListView ItemsSource="{Binding Items}">
        <d:ListView.ItemsSource>
            <x:Array Type="{x:Type x:String}">
                <x:String>Item One</x:String>
                <x:String>Item Two</x:String>
                <x:String>Item Three</x:String>
            </x:Array>
        </d:ListView.ItemsSource>
        <ListView.ItemTemplate>
            <DataTemplate>
                <TextCell Text="{Binding ItemName}"
                          d:Text="{Binding .}" />
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</StackLayout>
```

[![Concevoir des données chronologiques présentant un ListView](xaml-previewer-images/designtimedata-itemssource-sm.png "concevoir des données chronologiques présentant un ListView")](xaml-previewer-images/designtimedata-itemssource-lg.png#lightbox)

Cet exemple affiche un ListView de trois TextCells dans le Générateur d’aperçu XAML. Vous pouvez modifier `x:String` à un modèle de données existant dans votre projet.

Reportez-vous à [application de Hanselman.Forms de James Montemagno](https://github.com/jamesmontemagno/Hanselman.Forms/blob/vnext/src/Hanselman/Views/Podcasts/PodcastDetailsPage.xaml#L36-L57) pour obtenir un exemple plus complexe.


## <a name="alternative-hardcode-a-static-viewmodel"></a>Alternative : Coder en dur un ViewModel statique

Si vous ne souhaitez pas ajouter des données au moment du design à des contrôles individuels, vous pouvez configurer un magasin de données fictives pour lier à votre page. Reportez-vous à de James Montemagno [billet de blog sur l’ajout de données au moment du design](http://motzcod.es/post/143702671962/xamarinforms-xaml-previewer-design-time-data) pour voir comment lier à un ViewModel statique dans XAML.

## <a name="troubleshooting"></a>Résolution des problèmes

### <a name="requirements"></a>Configuration requise

Données de conception nécessitent une version minimale de Xamarin.Forms 3.6.

### <a name="intellisense-shows-squiggly-lines-under-my-design-time-data"></a>IntelliSense affiche des lignes ondulées sous Mes données de conception

Il existe un problème connu et sera résolu dans une prochaine version de Visual Studio. Le projet sera toujours généré sans erreur.

### <a name="the-xaml-previewer-stopped-working"></a>Le Générateur d’aperçu XAML a cessé de fonctionner

Essayez de fermer et rouvrir le fichier XAML, nettoyer et régénérer votre projet.
