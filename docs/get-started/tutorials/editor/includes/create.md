---
ms.openlocfilehash: ce3e0f18d299d2bdf8d9bd81c467d45924d0d2bc
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61373371"
---
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Lancez Visual Studio et créez une application Xamarin.Forms vide nommée **EditorTutorial**. Assurez-vous que l’application utilise bien .NET Standard comme mécanisme de code partagé.

    > [!IMPORTANT]
    > Pour les extraits C# et XAML de ce didacticiel, la solution doit se nommer **EditorTutorial**. L’utilisation d’un autre nom entraîne des erreurs de build quand vous copiez le code à partir de ce didacticiel dans la solution.

    Pour plus d’informations sur la bibliothèque .NET Standard créée, consultez la rubrique [Anatomie d’une application Xamarin.Forms](~/get-started/first-app/index.md) dans l’article [Immersion dans Xamarin.Forms - démarrage rapide](~/get-started/first-app/index.md).

1. Dans **l’Explorateur de solutions**, dans le projet **EditorTutorial**, double-cliquez sur **MainPage.xaml** pour l’ouvrir. Puis, dans **MainPage.xaml**, supprimez tout le code du modèle et remplacez-le par le code suivant :

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="EditorTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Editor Placeholder="Enter multi-line text here"
                    HeightRequest="200" />
        </StackLayout>
    </ContentPage>
    ```

    Ce code définit de manière déclarative l’interface utilisateur de la page, qui comprend un élément [`Editor`](xref:Xamarin.Forms.Editor) dans [`StackLayout`](xref:Xamarin.Forms.StackLayout). La propriété [`Editor.Placeholder`](xref:Xamarin.Forms.Editor.Placeholder) spécifie le texte d’espace réservé qui apparaît lorsque l’élément `Editor` s’affiche pour la première fois. Par ailleurs, la propriété [`HeightRequest`](xref:Xamarin.Forms.VisualElement) spécifie la hauteur de l’élément `Editor` en unités indépendantes de l’appareil.

1. Dans la barre d’outils Visual Studio, appuyez sur le bouton **Démarrer** (le bouton triangulaire qui ressemble à un bouton de lecture) pour lancer l’application à l’intérieur du simulateur iOS distant ou de l’émulateur Android de votre choix :

    [![Capture d’écran d’un éditeur sur iOS et Android](../images/create-editor.png "Éditeur contenant un texte d’espace réservé")](../images/create-editor-large.png#lightbox "Éditeur contenant un texte d’espace réservé")

    > [!NOTE]
    > Android indique la hauteur de l’élément [`Editor`](xref:Xamarin.Forms.Editor), mais ce n’est pas le cas d’iOS.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1. Lancez Visual Studio pour Mac et créez une application Xamarin.Forms vide nommée **EditorTutorial**. Assurez-vous que l’application utilise bien .NET Standard comme mécanisme de code partagé.

    > [!IMPORTANT]
    > Pour les extraits C# et XAML de ce didacticiel, la solution doit se nommer **EditorTutorial**. L’utilisation d’un autre nom entraîne des erreurs de build quand vous copiez le code à partir de ce didacticiel dans la solution.

    Pour plus d’informations sur la bibliothèque .NET Standard créée, consultez la rubrique [Anatomie d’une application Xamarin.Forms](~/get-started/first-app/index.md) dans l’article [Immersion dans Xamarin.Forms - démarrage rapide](~/get-started/first-app/index.md).

1. Dans **Panneau Solutions**, dans le projet **EditorTutorial**, double-cliquez sur **MainPage.xaml** pour l’ouvrir. Puis, dans **MainPage.xaml**, supprimez tout le code du modèle et remplacez-le par le code suivant :

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="EditorTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Editor Placeholder="Enter multi-line text here"
                    HeightRequest="200" />
        </StackLayout>
    </ContentPage>
    ```

    Ce code définit de manière déclarative l’interface utilisateur de la page, qui comprend un élément [`Editor`](xref:Xamarin.Forms.Editor) dans [`StackLayout`](xref:Xamarin.Forms.StackLayout). La propriété [`Editor.Placeholder`](xref:Xamarin.Forms.Editor.Placeholder) spécifie le texte d’espace réservé qui apparaît lorsque l’élément `Editor` s’affiche pour la première fois. Par ailleurs, la propriété [`HeightRequest`](xref:Xamarin.Forms.VisualElement) spécifie la hauteur de l’élément `Editor` en unités indépendantes de l’appareil.

1. Dans la barre d’outils Visual Studio pour Mac, appuyez sur le bouton **Démarrer** (le bouton triangulaire qui ressemble à un bouton de lecture) pour lancer l’application dans le simulateur iOS ou dans l’émulateur Android de votre choix :

    [![Capture d’écran d’un éditeur sur iOS et Android](../images/create-editor.png "Éditeur contenant un texte d’espace réservé")](../images/create-editor-large.png#lightbox "Éditeur contenant un texte d’espace réservé")

    > [!NOTE]
    > Android indique la hauteur de l’élément [`Editor`](xref:Xamarin.Forms.Editor), mais ce n’est pas le cas d’iOS.
