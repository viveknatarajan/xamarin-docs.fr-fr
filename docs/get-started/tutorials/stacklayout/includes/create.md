---
ms.openlocfilehash: a7b23e47269ecca4d36a344ce3589443a4a85e31
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61382507"
---
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Lancez Visual Studio et créez une application Xamarin.Forms vide nommée **StackLayoutTutorial**. Assurez-vous que l’application utilise bien .NET Standard comme mécanisme de code partagé.

    > [!IMPORTANT]
    > Pour les extraits C# et XAML de ce didacticiel, la solution doit se nommer **StackLayoutTutorial**. L’utilisation d’un autre nom entraîne des erreurs de build quand vous copiez le code à partir de ce didacticiel dans la solution.

    Pour plus d’informations sur la bibliothèque .NET Standard créée, consultez la rubrique [Anatomie d’une application Xamarin.Forms](~/get-started/first-app/index.md) dans l’article [Immersion dans Xamarin.Forms - démarrage rapide](~/get-started/first-app/index.md).

1. Dans **l’Explorateur de solutions**, dans le projet **StackLayoutTutorial**, double-cliquez sur **MainPage.xaml** pour l’ouvrir. Puis, dans **MainPage.xaml**, supprimez tout le code du modèle et remplacez-le par le code suivant :

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="StackLayoutTutorial.MainPage">
        <StackLayout Margin="20,35,20,25">
            <Label Text="The StackLayout has its Margin property set, to control the rendering position of the StackLayout." />
            <Label Text="The Padding property can be set to specify the distance between the StackLayout and its children." />
            <Label Text="The Spacing property can be set to specify the distance between views in the StackLayout." />
        </StackLayout>
    </ContentPage>
    ```

    Ce code définit de manière déclarative l’interface utilisateur de la page, qui comprend trois instances [`Label`](xref:Xamarin.Forms.Label) dans un [`StackLayout`](xref:Xamarin.Forms.StackLayout). [`StackLayout`](xref:Xamarin.Forms.StackLayout) positionne ses affichages enfants (les instances `Label`) sur une seule ligne, laquelle est orientée verticalement par défaut. En outre, la propriété [`Margin`](xref:Xamarin.Forms.View.Margin) indique la position de rendu de `StackLayout` dans [`ContentPage`](xref:Xamarin.Forms.ContentPage).

    > [!NOTE]
    > Outre la propriété [`Margin`](xref:Xamarin.Forms.View.Margin), les propriétés [`Padding`](xref:Xamarin.Forms.Layout.Padding) et [`Spacing`](xref:Xamarin.Forms.StackLayout.Spacing) peuvent également être définies sur [`StackLayout`](xref:Xamarin.Forms.StackLayout). La valeur de propriété [`Padding`](xref:Xamarin.Forms.Layout.Padding) spécifie la distance entre les affichages dans `StackLayout`et la valeur de propriété [`Spacing`](xref:Xamarin.Forms.StackLayout.Spacing) spécifie la quantité d’espace entre chaque élément enfant dans `StackLayout`. Pour plus d’informations, consultez [Marge et remplissage](~/xamarin-forms/user-interface/layouts/margin-and-padding.md).

1. Dans la barre d’outils Visual Studio, appuyez sur le bouton **Démarrer** (le bouton triangulaire qui ressemble à un bouton de lecture) pour lancer l’application à l’intérieur du simulateur iOS distant ou de l’émulateur Android de votre choix :

    [![Capture d’écran d’affichages enfants dans un StackLayout, sur iOS et Android](../images/create-stacklayout.png "StackLayout contenant des instances d’étiquette")](../images/create-stacklayout-large.png#lightbox "StackLayout contenant des instances d’étiquette")

    Pour plus d’informations sur [`StackLayout`](xref:Xamarin.Forms.StackLayout), consultez [Xamarin.Forms StackLayout](~/xamarin-forms/user-interface/layouts/stack-layout.md).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1. Lancez Visual Studio pour Mac et créez une application Xamarin.Forms vide nommée **StackLayoutTutorial**. Assurez-vous que l’application utilise bien .NET Standard comme mécanisme de code partagé.

    > [!IMPORTANT]
    > Pour les extraits C# et XAML de ce didacticiel, la solution doit se nommer **StackLayoutTutorial**. L’utilisation d’un autre nom entraîne des erreurs de build quand vous copiez le code à partir de ce didacticiel dans la solution.

    Pour plus d’informations sur la bibliothèque .NET Standard créée, consultez la rubrique [Anatomie d’une application Xamarin.Forms](~/get-started/first-app/index.md) dans l’article [Immersion dans Xamarin.Forms - démarrage rapide](~/get-started/first-app/index.md).

1. Dans **Panneau Solutions**, dans le projet **StackLayoutTutorial**, double-cliquez sur **MainPage.xaml** pour l’ouvrir. Puis, dans **MainPage.xaml**, supprimez tout le code du modèle et remplacez-le par le code suivant :

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="StackLayoutTutorial.MainPage">
        <StackLayout Margin="20,35,20,25">
            <Label Text="The StackLayout has its Margin property set, to control the rendering position of the StackLayout." />
            <Label Text="The Padding property can be set to specify the distance between the StackLayout and its children." />
            <Label Text="The Spacing property can be set to specify the distance between views in the StackLayout." />
        </StackLayout>
    </ContentPage>
    ```

    Ce code définit de manière déclarative l’interface utilisateur de la page, qui comprend trois instances [`Label`](xref:Xamarin.Forms.Label) dans un [`StackLayout`](xref:Xamarin.Forms.StackLayout). [`StackLayout`](xref:Xamarin.Forms.StackLayout) positionne ses affichages enfants (les instances `Label`) sur une seule ligne, laquelle est orientée verticalement par défaut. En outre, la propriété [`Margin`](xref:Xamarin.Forms.View.Margin) indique la position de rendu de `StackLayout` dans [`ContentPage`](xref:Xamarin.Forms.ContentPage).

    > [!NOTE]
    > Outre la propriété [`Margin`](xref:Xamarin.Forms.View.Margin), les propriétés [`Padding`](xref:Xamarin.Forms.Layout.Padding) et [`Spacing`](xref:Xamarin.Forms.StackLayout.Spacing) peuvent également être définies sur [`StackLayout`](xref:Xamarin.Forms.StackLayout). La valeur de propriété [`Padding`](xref:Xamarin.Forms.Layout.Padding) spécifie la distance entre les affichages dans `StackLayout`et la valeur de propriété [`Spacing`](xref:Xamarin.Forms.StackLayout.Spacing) spécifie la quantité d’espace entre chaque élément enfant dans `StackLayout`. Pour plus d’informations, consultez [Marge et remplissage](~/xamarin-forms/user-interface/layouts/margin-and-padding.md).

1. Dans la barre d’outils Visual Studio pour Mac, appuyez sur le bouton **Démarrer** (le bouton triangulaire qui ressemble à un bouton de lecture) pour lancer l’application dans le simulateur iOS ou dans l’émulateur Android de votre choix :

    [![Capture d’écran d’affichages enfants dans un StackLayout, sur iOS et Android](../images/create-stacklayout.png "StackLayout contenant des instances d’étiquette")](../images/create-stacklayout-large.png#lightbox "StackLayout contenant des instances d’étiquette")

    Pour plus d’informations sur [`StackLayout`](xref:Xamarin.Forms.StackLayout), consultez [Xamarin.Forms StackLayout](~/xamarin-forms/user-interface/layouts/stack-layout.md).
