---
ms.openlocfilehash: c826ee87c006b05322af8c9312bdf3120df8b357
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61375361"
---
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Dans **MainPage.xaml**, modifiez la déclaration [`Grid`](xref:Xamarin.Forms.Grid) pour définir des colonnes et des lignes, et pour y placer du contenu sur toutes ces colonnes et lignes :

    ```xaml
    <Grid Margin="20,35,20,20">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="0.5*" />
            <ColumnDefinition Width="0.5*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="50" />
            <RowDefinition Height="30" />
            <RowDefinition Height="30" />
        </Grid.RowDefinitions>
        <Label Grid.ColumnSpan="2"
               Text="This text uses the ColumnSpan property to span both columns." />
        <Label Grid.Row="1"
               Grid.RowSpan="2"
               Text="This text uses the RowSpan property to span two rows." />
    </Grid>
    ```

    Ce code définit des colonnes et des lignes pour [`Grid`](xref:Xamarin.Forms.Grid) et positionne les instances [`Label`](xref:Xamarin.Forms.Label) dans des colonnes et des lignes spécifiques. La première `Label` définit la propriété jointe [`ColumnSpan`](xref:Xamarin.Forms.Grid.ColumnSpanProperty) afin que son texte s’étende sur plusieurs colonnes. La propriété `ColumnSpan` est définie sur 2, qui représente le nombre de colonnes qu’occupera `Label`. La deuxième `Label` définit la propriété jointe [`RowSpan`](xref:Xamarin.Forms.Grid.RowSpanProperty) afin que son texte s’étende sur plusieurs lignes. La propriété `RowSpan` est définie sur 2, qui représente le nombre de lignes qu’occupera `Label`.

1. Dans la barre d’outils Visual Studio, appuyez sur le bouton **Démarrer** (le bouton triangulaire qui ressemble à un bouton de lecture) pour lancer l’application à l’intérieur du simulateur iOS distant ou de l’émulateur Android de votre choix :

    [![Capture d’écran d’une Grid dont le contenu s’étend sur plusieurs colonnes et lignes, sur iOS et Android](../images/span-columns-rows.png "Grid avec du contenu sur plusieurs colonnes et lignes")](../images/span-columns-rows-large.png#lightbox "Grid avec du contenu sur plusieurs colonnes et lignes")

    Pour plus d’informations sur l’extension de contenu sur plusieurs colonnes et des lignes, consultez la rubrique [Étendues](~/xamarin-forms/user-interface/layouts/grid.md#spans) dans le guide [Xamarin.Forms Grid](~/xamarin-forms/user-interface/layouts/grid.md).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1. Dans **MainPage.xaml**, modifiez la déclaration [`Grid`](xref:Xamarin.Forms.Grid) pour définir des colonnes et des lignes, et pour y placer du contenu sur toutes ces colonnes et lignes :

    ```xaml
    <Grid Margin="20,35,20,20">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="0.5*" />
            <ColumnDefinition Width="0.5*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="50" />
            <RowDefinition Height="30" />
            <RowDefinition Height="30" />
        </Grid.RowDefinitions>
        <Label Grid.ColumnSpan="2"
               Text="This text uses the ColumnSpan property to span both columns." />
        <Label Grid.Row="1"
               Grid.RowSpan="2"
               Text="This text uses the RowSpan property to span two rows." />
    </Grid>
    ```

    Ce code définit des colonnes et des lignes pour [`Grid`](xref:Xamarin.Forms.Grid) et positionne les instances [`Label`](xref:Xamarin.Forms.Label) dans des colonnes et des lignes spécifiques. La première `Label` définit la propriété jointe [`ColumnSpan`](xref:Xamarin.Forms.Grid.ColumnSpanProperty) afin que son texte s’étende sur plusieurs colonnes. La propriété `ColumnSpan` est définie sur 2, qui représente le nombre de colonnes qu’occupera `Label`. La deuxième `Label` définit la propriété jointe [`RowSpan`](xref:Xamarin.Forms.Grid.RowSpanProperty) afin que son texte s’étende sur plusieurs lignes. La propriété `RowSpan` est définie sur 2, qui représente le nombre de lignes qu’occupera `Label`.

1. Dans la barre d’outils Visual Studio pour Mac, appuyez sur le bouton **Démarrer** (le bouton triangulaire qui ressemble à un bouton de lecture) pour lancer l’application dans le simulateur iOS ou dans l’émulateur Android de votre choix :

    [![Capture d’écran d’une Grid dont le contenu s’étend sur plusieurs colonnes et lignes, sur iOS et Android](../images/span-columns-rows.png "Grid avec du contenu sur plusieurs colonnes et lignes")](../images/span-columns-rows-large.png#lightbox "Grid avec du contenu sur plusieurs colonnes et lignes")

    Pour plus d’informations sur l’extension de contenu sur plusieurs colonnes et des lignes, consultez la rubrique [Étendues](~/xamarin-forms/user-interface/layouts/grid.md#spans) dans le guide [Xamarin.Forms Grid](~/xamarin-forms/user-interface/layouts/grid.md).
