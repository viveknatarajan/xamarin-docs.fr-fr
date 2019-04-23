---
ms.openlocfilehash: a7a5d51b8931aa9dffd2c16b4ead51371390f033
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61372931"
---
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Lancez Visual Studio et créez une application Xamarin.Forms vide nommée **ButtonTutorial**. Assurez-vous que l’application utilise bien .NET Standard comme mécanisme de code partagé.

    > [!IMPORTANT]
    > Pour les extraits C# et XAML de ce didacticiel, la solution doit se nommer **ButtonTutorial**. L’utilisation d’un autre nom entraîne des erreurs de build quand vous copiez le code à partir de ce didacticiel dans la solution.

    Pour plus d’informations sur la bibliothèque .NET Standard créée, consultez la rubrique [Anatomie d’une application Xamarin.Forms](~/get-started/first-app/index.md) dans l’article [Immersion dans Xamarin.Forms - démarrage rapide](~/get-started/first-app/index.md).

1. Dans **l’Explorateur de solutions**, dans le projet **ButtonTutorial**, double-cliquez sur **MainPage.xaml** pour l’ouvrir. Puis, dans **MainPage.xaml**, supprimez tout le code du modèle et remplacez-le par le code suivant :

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>    
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="ButtonTutorial.MainPage">
       <StackLayout Margin="20,35,20,20">
           <Button Text="Click me" />
       </StackLayout>
    </ContentPage>
    ```

    Ce code définit de manière déclarative l’interface utilisateur de la page, qui comprend un élément [`Button`](xref:Xamarin.Forms.Button) dans [`StackLayout`](xref:Xamarin.Forms.StackLayout). La propriété [`Button.Text`](xref:Xamarin.Forms.Button.Text) spécifie le texte qui apparaît dans l’élément `Button`.

1. Dans la barre d’outils Visual Studio, appuyez sur le bouton **Démarrer** (le bouton triangulaire qui ressemble à un bouton de lecture) pour lancer l’application à l’intérieur du simulateur iOS distant ou de l’émulateur Android de votre choix :

    [![Capture d’écran d’un bouton sur iOS et Android](../images/create-button.png "Bouton contenant du texte")](../images/create-button-large.png#lightbox "Bouton contenant du texte")

    Notez que, par défaut, un élément [`Button`](xref:Xamarin.Forms.Button) tend à occuper tout l’espace qui lui est accordé ; dans ce cas, la largeur totale de son parent (l’élément [`StackLayout`](xref:Xamarin.Forms.StackLayout)).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1. Lancez Visual Studio pour Mac et créez une application Xamarin.Forms vide nommée **ButtonTutorial**. Assurez-vous que l’application utilise bien .NET Standard comme mécanisme de code partagé.

    > [!IMPORTANT]
    > Pour les extraits C# et XAML de ce didacticiel, la solution doit se nommer **ButtonTutorial**. L’utilisation d’un autre nom entraîne des erreurs de build quand vous copiez le code à partir de ce didacticiel dans la solution.

    Pour plus d’informations sur la bibliothèque .NET Standard créée, consultez la rubrique [Anatomie d’une application Xamarin.Forms](~/get-started/first-app/index.md) dans l’article [Immersion dans Xamarin.Forms - démarrage rapide](~/get-started/first-app/index.md).

1. Dans le **Panneau Solutions**, dans le projet **ButtonTutorial**, double-cliquez sur **MainPage.xaml** pour l’ouvrir. Puis, dans **MainPage.xaml**, supprimez tout le code du modèle et remplacez-le par le code suivant :

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="ButtonTutorial.MainPage">
       <StackLayout Margin="20,35,20,20">
           <Button Text="Click me" />
       </StackLayout>
    </ContentPage>
    ```

    Ce code définit de manière déclarative l’interface utilisateur de la page, qui comprend un élément [`Button`](xref:Xamarin.Forms.Button) dans [`StackLayout`](xref:Xamarin.Forms.StackLayout). La propriété [`Button.Text`](xref:Xamarin.Forms.Button.Text) spécifie le texte qui apparaît dans l’élément `Button`.

1. Dans la barre d’outils Visual Studio pour Mac, appuyez sur le bouton **Démarrer** (le bouton triangulaire qui ressemble à un bouton de lecture) pour lancer l’application dans le simulateur iOS ou dans l’émulateur Android de votre choix :

    [![Capture d’écran d’un bouton sur iOS et Android](../images/create-button.png "Bouton contenant du texte")](../images/create-button-large.png#lightbox "Bouton contenant du texte")

    Notez que, par défaut, un élément [`Button`](xref:Xamarin.Forms.Button) tend à occuper tout l’espace qui lui est accordé ; dans ce cas, la largeur totale de son parent (l’élément [`StackLayout`](xref:Xamarin.Forms.StackLayout)).
