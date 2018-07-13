---
title: Xamarin.Forms multi-écran - Démarrage rapide
description: Cet article montre comment étendre l’application Phoneword en ajoutant un second écran de manière à suivre l’historique des appels pour l’application.
ms.prod: quickstart
ms.assetid: 255d93b9-518c-4e5d-a9cd-4dd8a7945a7f
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/13/2018
ms.openlocfilehash: a4e27f1810a16b5d13838d2e2c1067950586fab3
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996179"
---
# <a name="xamarinforms-multiscreen-quickstart"></a>Xamarin.Forms multi-écran - Démarrage rapide

Ce guide de démarrage rapide montre comment étendre l’application Phoneword en ajoutant un second écran de manière à suivre l’historique des appels pour l’application. L’application finale est indiquée ci-dessous :

[![](quickstart-images/intro-app-examples-sml.png "Application Phoneword")](quickstart-images/intro-app-examples.png#lightbox "Application Phoneword")

Étendez l’application Phoneword comme suit :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Lancez Visual Studio. Dans la page de démarrage, cliquez sur **Ouvrir un projet...** puis, dans la boîte de dialogue **Ouvrir un projet**, sélectionnez le fichier solution pour le projet Phoneword :

    ![](quickstart-images/vs/open-solution.png "Ouvrir un projet")

2. Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le projet **Phoneword**, puis sélectionnez **Ajouter > Nouvel élément...**  :

    ![](quickstart-images/vs/add-new-item.png "Ajouter un nouvel élément")

3. Dans la boîte de dialogue **Ajouter un nouvel élément**, sélectionnez **Éléments Visual C# > Xamarin.Forms > Page de contenu**, nommez le nouvel élément **CallHistoryPage**, puis cliquez sur le bouton **Ajouter**. Vous ajoutez ainsi une page nommée **CallHistoryPage** au projet :

    ![](quickstart-images/vs/add-callhistorypage-class.png "Modèles de projet Xamarin.Forms")

4. Dans **CallHistoryPage.xaml**, supprimez tout le code du modèle et remplacez-le par le code suivant. Ce code définit de manière déclarative l’interface utilisateur de la page :

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                       xmlns:local="clr-namespace:Phoneword;assembly=Phoneword"
                       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                       x:Class="Phoneword.CallHistoryPage"
                       Title="Call History">
        <ContentPage.Padding>
            <OnPlatform x:TypeArguments="Thickness">
                <On Platform="iOS" Value="20, 40, 20, 20" />
                <On Platform="Android, UWP" Value="20" />
            </OnPlatform>
        </ContentPage.Padding>
        <StackLayout>
          <ListView ItemsSource="{x:Static local:App.PhoneNumbers}" />
        </StackLayout>
    </ContentPage>
    ```

    Enregistrez les modifications apportées à **CallHistoryPage.xaml** en appuyant sur **Ctrl+S** et fermez le fichier.

5. Dans l’**Explorateur de solutions**, double-cliquez sur le fichier **App.xaml.cs** dans le projet **Phoneword** partagé pour l’ouvrir :

    ![](quickstart-images/vs/open-app-class.png "Ouvrir App.xaml.cs")

6. Dans **App.xaml.cs**, importez l’espace de noms `System.Collections.Generic`, ajoutez la déclaration de la propriété `PhoneNumbers`, initialisez la propriété dans le constructeur `App` et initialisez la propriété [`MainPage`](xref:Xamarin.Forms.Application.MainPage) en [`NavigationPage`](xref:Xamarin.Forms.NavigationPage). La collection `PhoneNumbers` permet de stocker une liste de tous les numéros de téléphone convertis appelés par l’application :

    ```csharp
    using System.Collections.Generic;
    using Xamarin.Forms;
    using Xamarin.Forms.Xaml;

    [assembly: XamlCompilation(XamlCompilationOptions.Compile)]
    namespace Phoneword
    {
        public partial class App : Application
        {
            public static IList<string> PhoneNumbers { get; set; }

            public App()
            {
                InitializeComponent();
                PhoneNumbers = new List<string>();
                MainPage = new NavigationPage(new MainPage());
            }
            ...
        }
    }
    ```

    Enregistrez les modifications apportées à **App.xaml.cs** en appuyant sur **Ctrl+S** et fermez le fichier.

7. Dans l’**Explorateur de solutions**, double-cliquez sur le fichier **MainPage.xaml** dans le projet **Phoneword** partagé pour l’ouvrir :

    ![](quickstart-images/vs/open-mainpage-xaml.png "Ouvrir MainPage.xaml")

8. Dans **MainPage.xaml**, ajoutez un contrôle [`Button`](xref:Xamarin.Forms.Button) à la fin du contrôle [`StackLayout`](xref:Xamarin.Forms.StackLayout). Le bouton sera utilisé pour accéder à la page de l’historique des appels :

    ```xaml
    <StackLayout VerticalOptions="FillAndExpand"
                 HorizontalOptions="FillAndExpand"
                 Orientation="Vertical"
                 Spacing="15">
      ...
      <Button x:Name="callButton" Text="Call" IsEnabled="false" Clicked="OnCall" />
      <Button x:Name="callHistoryButton" Text="Call History" IsEnabled="false"
              Clicked="OnCallHistory" />
    </StackLayout>
    ```

    Enregistrez les modifications apportées à **MainPage.xaml** en appuyant sur **Ctrl+S** et fermez le fichier.

9. Dans l’**Explorateur de solutions**, double-cliquez sur **MainPage.xaml.cs** pour l’ouvrir :

    ![](quickstart-images/vs/open-mainpage-codebehind.png "Ouvrir MainPage.xaml.cs")

10. Dans **MainPage.xaml.cs**, ajoutez la méthode de gestionnaire d’événements `OnCallHistory` et modifiez la méthode de gestionnaire d’événements `OnCall` pour ajouter le numéro de téléphone converti à la collection `App.PhoneNumbers` et activer l’élément `callHistoryButton`, à condition que la variable `dialer` n’ait pas la valeur `null` :

    ```csharp
    using System;
    using Xamarin.Forms;

    namespace Phoneword
    {
        public partial class MainPage : ContentPage
        {
            ...

            async void OnCall(object sender, EventArgs e)
            {
                ...
                if (dialer != null) {
                    App.PhoneNumbers.Add (translatedNumber);
                    callHistoryButton.IsEnabled = true;
                    dialer.Dial (translatedNumber);
                }
                ...
            }

            async void OnCallHistory(object sender, EventArgs e)
            {
                await Navigation.PushAsync (new CallHistoryPage ());
            }
        }
    }
    ```

    Enregistrez les modifications apportées à **MainPage.xaml.cs** en appuyant sur **Ctrl+S** et fermez le fichier.

11. Dans Visual Studio, sélectionnez l’élément de menu **Générer > Générer la solution** (ou appuyez sur **Ctrl+Maj+B**). L’application est générée et un message de réussite s’affiche dans la barre d’état Visual Studio :

    ![](quickstart-images/vs/build-successful.png "Génération réussie")

    En cas d’erreurs, répétez les étapes précédentes et corrigez les erreurs éventuelles jusqu’à ce que l’application soit générée.

12. Dans la barre d’outils Visual Studio, appuyez sur le bouton **Démarrer** (le bouton triangulaire qui ressemble à un bouton Lire) pour lancer l’application :

    ![](quickstart-images/vs/start.png "Barre d’outils Visual Studio")
    ![](quickstart-images/vs/phone-result-uwp.png "Application Phoneword UWP")

13. Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le projet **Phoneword.Droid**, puis sélectionnez **Définir comme projet de démarrage**.
14. Dans la barre d’outils de Visual Studio, appuyez sur le bouton **Démarrer** (le bouton triangulaire qui ressemble à un bouton Lire) pour lancer l’application dans un émulateur Android.
15. Si vous possédez un appareil iOS et répondez à la configuration requise Mac pour le développement Xamarin.Forms, utilisez une technique similaire pour déployer l’application sur l’appareil iOS. Vous pouvez aussi déployer l’application sur le [simulateur distant iOS](~/tools/ios-simulator.md).

    Remarque : Les appels téléphoniques ne sont pas pris en charge sur tous les simulateurs.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1. Lancez Visual Studio pour Mac. Dans la page de démarrage, cliquez sur **Ouvrir...** puis, dans la boîte de dialogue, sélectionnez le fichier solution pour le projet Phoneword :

    ![](quickstart-images/xs/open-solution.png "Ouvrir une solution")

2. Dans le **Panneau Solutions**, sélectionnez le projet **Phoneword**, cliquez avec le bouton droit et sélectionnez **Ajouter > Nouveau fichier...**  :

    ![](quickstart-images/xs/add-new-file.png "Ajouter un nouveau fichier")

3. Dans la boîte de dialogue **Nouveau fichier**, sélectionnez **Formulaires > ContentPage XAML Formulaires**, nommez le nouveau fichier **CallHistoryPage**, puis cliquez sur le bouton **Nouveau**. Vous ajoutez ainsi une page nommée **CallHistoryPage** au projet :

    ![](quickstart-images/xs/add-callhistorypage-class.png "Ajouter une ContentPage Formulaires")

4. Dans le **Panneau Solutions**, double-cliquez sur **CallHistoryPage.xaml** pour l’ouvrir :

    ![](quickstart-images/xs/open-callhistorypage-xaml.png "Ouvrir CallHistoryPage.xaml")

5. Dans **CallHistoryPage.xaml**, supprimez tout le code du modèle et remplacez-le par le code suivant. Ce code définit de manière déclarative l’interface utilisateur de la page :

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                       xmlns:local="clr-namespace:Phoneword;assembly=Phoneword"
                       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                       x:Class="Phoneword.CallHistoryPage"
                       Title="Call History">
        <ContentPage.Padding>
            <OnPlatform x:TypeArguments="Thickness">
                <On Platform="iOS" Value="20, 40, 20, 20" />
                <On Platform="Android, UWP" Value="20" />
            </OnPlatform>
        </ContentPage.Padding>
        <StackLayout>
          <ListView ItemsSource="{x:Static local:App.PhoneNumbers}" />
        </StackLayout>
    </ContentPage>      
    ```

    Enregistrez les modifications apportées à **CallHistoryPage.xaml** en choisissant **Fichier > Enregistrer** (ou en appuyant sur **&#8984;+S**) et fermez le fichier.

6. Dans le **Panneau Solutions**, double-cliquez sur **App.xaml.cs** pour l’ouvrir :

    ![](quickstart-images/xs/open-app-class.png "Ouvrir App.xaml.cs")

7. Dans **App.xaml.cs**, importez l’espace de noms `System.Collections.Generic`, ajoutez la déclaration de la propriété `PhoneNumbers`, initialisez la propriété dans le constructeur `App` et initialisez la propriété [`MainPage`](xref:Xamarin.Forms.Application.MainPage) en [`NavigationPage`](xref:Xamarin.Forms.NavigationPage). La collection `PhoneNumbers` permet de stocker une liste de tous les numéros de téléphone convertis appelés par l’application :

    ```csharp
    using System.Collections.Generic;
    using Xamarin.Forms;
    using Xamarin.Forms.Xaml;

    [assembly: XamlCompilation(XamlCompilationOptions.Compile)]
    namespace Phoneword
    {
        public partial class App : Application
        {
            public static IList<string> PhoneNumbers { get; set; }

            public App()
            {
                InitializeComponent();
                PhoneNumbers = new List<string>();
                MainPage = new NavigationPage(new MainPage());
            }
            ...
        }
    }
    ```

    Enregistrez les modifications apportées à **App.xaml.cs** en choisissant **Fichier > Enregistrer** (ou en appuyant sur **&#8984;+S**) et fermez le fichier.

8. Dans le **Panneau Solutions**, double-cliquez sur **MainPage.xaml** pour l’ouvrir :

    ![](quickstart-images/xs/open-mainpage-xaml.png "Ouvrir MainPage.xaml")

9. Dans **MainPage.xaml**, ajoutez un contrôle [`Button`](xref:Xamarin.Forms.Button) à la fin du contrôle [`StackLayout`](xref:Xamarin.Forms.StackLayout). Le bouton sera utilisé pour accéder à la page de l’historique des appels :

    ```xaml
    <StackLayout VerticalOptions="FillAndExpand"
                 HorizontalOptions="FillAndExpand"
                 Orientation="Vertical"
                 Spacing="15">
      ...
      <Button x:Name="callButton" Text="Call" IsEnabled="false" Clicked="OnCall" />
      <Button x:Name="callHistoryButton" Text="Call History" IsEnabled="false"
              Clicked="OnCallHistory" />
    </StackLayout>
    ```

    Enregistrez les modifications apportées à **MainPage.xaml** en choisissant **Fichier > Enregistrer** (ou en appuyant sur **&#8984;+S**) et fermez le fichier.

10. Dans le **Panneau Solutions**, double-cliquez sur **MainPage.xaml.cs** pour l’ouvrir :

    ![](quickstart-images/xs/open-mainpage-codebehind.png "Ouvrir MainPage.xaml.cs")

11. Dans **MainPage.xaml.cs**, ajoutez la méthode de gestionnaire d’événements `OnCallHistory` et modifiez la méthode de gestionnaire d’événements `OnCall` pour ajouter le numéro de téléphone converti à la collection `App.PhoneNumbers` et activer l’élément `callHistoryButton`, à condition que la variable `dialer` n’ait pas la valeur `null` :

    ```csharp
    using System;
    using Xamarin.Forms;

    namespace Phoneword
    {
        public partial class MainPage : ContentPage
        {
            ...

            async void OnCall(object sender, EventArgs e)
            {
                ...
                if (dialer != null) {
                    App.PhoneNumbers.Add (translatedNumber);
                    callHistoryButton.IsEnabled = true;
                    dialer.Dial (translatedNumber);
                }
                ...
            }

            async void OnCallHistory(object sender, EventArgs e)
            {
                await Navigation.PushAsync (new CallHistoryPage ());
            }
        }
    }
    ```

    Enregistrez les modifications apportées à **MainPage.xaml.cs** en choisissant **Fichier > Enregistrer** (ou en appuyant sur **&#8984;+S**) et fermez le fichier.

12. Dans Visual Studio pour Mac, sélectionnez l’élément de menu **Générer > Générer tout** (ou appuyez sur **&#8984;+B**). L’application est générée et un message de réussite s’affiche dans la barre d’outils Visual Studio pour Mac :

    ![](quickstart-images/xs/build-successful.png "Génération réussie")

    En cas d’erreurs, répétez les étapes précédentes et corrigez les erreurs éventuelles jusqu’à ce que l’application soit générée.

13. Dans la barre d’outils Visual Studio pour Mac, appuyez sur le bouton **Démarrer** (le bouton triangulaire qui ressemble à un bouton Lire) pour lancer l’application dans le simulateur iOS :

    ![](quickstart-images/xs/start.png "Barre d’outils Visual Studio pour Mac")
    ![](quickstart-images/xs/phone-result-ios.png "Simulateur iOS")

    Remarque : Les appels téléphoniques ne sont pas pris en charge dans le simulateur iOS.

14. Dans le **Panneau Solutions**, sélectionnez le projet **Phoneword.Droid**, cliquez avec le bouton droit et sélectionnez **Définir comme projet de démarrage** :

    ![](quickstart-images/xs/set-startup-project.png "Définir comme projet de démarrage")

15. Dans la barre d’outils Visual Studio pour Mac, appuyez sur le bouton **Démarrer** (le bouton triangulaire qui ressemble à un bouton Lire) pour lancer l’application dans un émulateur Android :

    ![](quickstart-images/xs/phone-result-android.png "Émulateur Android")

    Remarque : Les appels téléphoniques ne sont pas pris en charge dans les émulateurs Android.

-----

Félicitations ! Vous avez terminé une application Xamarin.Forms multi-écran. La [rubrique suivante](~/xamarin-forms/get-started/hello-xamarin-forms-multiscreen/deepdive.md) de ce guide passe en revue les étapes qui ont été effectuées dans cette procédure pas à pas afin de comprendre la navigation entre les pages et la liaison de données avec Xamarin.Forms.


## <a name="related-links"></a>Liens associés

- [Phoneword (exemple)](https://developer.xamarin.com/samples/xamarin-forms/Phoneword/)
- [PhonewordMultiscreen (exemple)](https://developer.xamarin.com/samples/xamarin-forms/PhonewordMultiscreen/)
