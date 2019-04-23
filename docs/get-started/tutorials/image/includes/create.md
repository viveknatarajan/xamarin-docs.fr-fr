---
ms.openlocfilehash: 550828327eb6b72c1c9712e54e6e36c9e30bd1f0
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61384484"
---
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Lancez Visual Studio et créez une application Xamarin.Forms vide nommée **ImageTutorial**. Assurez-vous que l’application utilise bien .NET Standard comme mécanisme de code partagé.

    > [!IMPORTANT]
    > Pour les extraits C# et XAML de ce didacticiel, la solution doit se nommer **ImageTutorial**. L’utilisation d’un autre nom entraîne des erreurs de build quand vous copiez le code à partir de ce didacticiel dans la solution.

    Pour plus d’informations sur la bibliothèque .NET Standard créée, consultez la rubrique [Anatomie d’une application Xamarin.Forms](~/get-started/first-app/index.md) dans l’article [Immersion dans Xamarin.Forms - démarrage rapide](~/get-started/first-app/index.md).

1. Dans **l’Explorateur de solutions**, dans le projet **ImageTutorial**, double-cliquez sur **MainPage.xaml** pour l’ouvrir. Puis, dans **MainPage.xaml**, supprimez tout le code du modèle et remplacez-le par le code suivant :

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="ImageTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Image Source="http://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg"
                   HeightRequest="300" />
        </StackLayout>
    </ContentPage>
    ```

    Ce code définit de manière déclarative l’interface utilisateur de la page, qui comprend un élément [`Image`](xref:Xamarin.Forms.Image) dans [`StackLayout`](xref:Xamarin.Forms.StackLayout). La propriété [`Image.Source`](xref:Xamarin.Forms.Image.Source) spécifie l’image à afficher via un URI. La propriété [`Image.Source`](xref:Xamarin.Forms.Image.Source) est de type [`ImageSource`](xref:Xamarin.Forms.ImageSource). Les images peuvent ainsi provenir de fichiers, d’URI ou de ressources. Pour plus d’informations, consultez la rubrique [Affichage des images](~/xamarin-forms/user-interface/images.md#displaying-images) du guide [Images dans Xamarin.Forms](~/xamarin-forms/user-interface/images.md).

    La propriété [`HeightRequest`](xref:Xamarin.Forms.VisualElement) spécifie la hauteur de l’élément `Image` en unités indépendantes de l’appareil.

    > [!NOTE]
    > Il n’est pas nécessaire de définir la propriété [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) dans cet exemple. En effet, par défaut, l’élément [`Image`](xref:Xamarin.Forms.Image) conserve les proportions de l’image.

1. Dans la barre d’outils Visual Studio, appuyez sur le bouton **Démarrer** (le bouton triangulaire qui ressemble à un bouton de lecture) pour lancer l’application à l’intérieur du simulateur iOS distant ou de l’émulateur Android de votre choix :

    [![Capture d’écran d’une image sur iOS et Android](../images/create-image.png "Vue d’une image affichant une image")](../images/create-image-large.png#lightbox "Vue d’une image affichant une image")

    > [!NOTE]
    > La vue [`Image`](xref:Xamarin.Forms.Image) met automatiquement en cache les images téléchargées pendant 24 heures. Pour plus d’informations, consultez la rubrique [Mise en cache des images téléchargées](~/xamarin-forms/user-interface/images.md#downloaded-image-caching) du guide [Images dans Xamarin.Forms](~/xamarin-forms/user-interface/images.md).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1. Lancez Visual Studio pour Mac et créez une application Xamarin.Forms vide nommée **ImageTutorial**. Assurez-vous que l’application utilise bien .NET Standard comme mécanisme de code partagé.

    > [!IMPORTANT]
    > Pour les extraits C# et XAML de ce didacticiel, la solution doit se nommer **ImageTutorial**. L’utilisation d’un autre nom entraîne des erreurs de build quand vous copiez le code à partir de ce didacticiel dans la solution.

    Pour plus d’informations sur la bibliothèque .NET Standard créée, consultez la rubrique [Anatomie d’une application Xamarin.Forms](~/get-started/first-app/index.md) dans l’article [Immersion dans Xamarin.Forms - démarrage rapide](~/get-started/first-app/index.md).

1. Dans **Panneau Solutions**, dans le projet **ImageTutorial**, double-cliquez sur **MainPage.xaml** pour l’ouvrir. Puis, dans **MainPage.xaml**, supprimez tout le code du modèle et remplacez-le par le code suivant :

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="ImageTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Image Source="http://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg"
                   HeightRequest="300" />
        </StackLayout>
    </ContentPage>
    ```

    Ce code définit de manière déclarative l’interface utilisateur de la page, qui comprend un élément [`Image`](xref:Xamarin.Forms.Image) dans [`StackLayout`](xref:Xamarin.Forms.StackLayout). La propriété [`Image.Source`](xref:Xamarin.Forms.Image.Source) spécifie l’image à afficher via un URI. La propriété [`Image.Source`](xref:Xamarin.Forms.Image.Source) est de type [`ImageSource`](xref:Xamarin.Forms.ImageSource). Les images peuvent ainsi provenir de fichiers, d’URI ou de ressources. Pour plus d’informations, consultez la rubrique [Affichage des images](~/xamarin-forms/user-interface/images.md#displaying-images) du guide [Images dans Xamarin.Forms](~/xamarin-forms/user-interface/images.md).

    La propriété [`HeightRequest`](xref:Xamarin.Forms.VisualElement) spécifie la hauteur de l’élément `Image` en unités indépendantes de l’appareil.

    > [!NOTE]
    > Il n’est pas nécessaire de définir la propriété [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) dans cet exemple. En effet, par défaut, l’élément [`Image`](xref:Xamarin.Forms.Image) conserve les proportions de l’image.

1. Dans la barre d’outils Visual Studio pour Mac, appuyez sur le bouton **Démarrer** (le bouton triangulaire qui ressemble à un bouton de lecture) pour lancer l’application dans le simulateur iOS ou dans l’émulateur Android de votre choix :

    [![Capture d’écran d’une image sur iOS et Android](../images/create-image.png "Vue d’une image affichant une image")](../images/create-image-large.png#lightbox "Vue d’une image affichant une image")

    > [!NOTE]
    > La vue [`Image`](xref:Xamarin.Forms.Image) met automatiquement en cache les images téléchargées pendant 24 heures. Pour plus d’informations, consultez la rubrique [Mise en cache des images téléchargées](~/xamarin-forms/user-interface/images.md#downloaded-image-caching) du guide [Images dans Xamarin.Forms](~/xamarin-forms/user-interface/images.md).
