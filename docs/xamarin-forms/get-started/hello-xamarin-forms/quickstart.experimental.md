---
title: Xamarin.Forms - Démarrage rapide
description: Cet article explique comment créer une application Notes multiplateforme simple à l’aide de Xamarin.Forms, pour vous permettre d’entrer une note et de la conserver dans le stockage de l’appareil.
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: E8CF05B1-54B9-428B-8518-D068837BD61E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/21/2018
ms.openlocfilehash: 880fa527d91098cf8c5f8c46cd9c5cce9ec9a489
ms.sourcegitcommit: 744c0a50420bb091fca8b92a84c20e61c741cf9e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/01/2018
ms.locfileid: "52743015"
---
# <a name="xamarinforms-quickstart"></a>Xamarin.Forms - Démarrage rapide

[![Télécharger l’exemple](~/media/shared/download.png) Télécharger l’exemple](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/Notes/SinglePage/)

Ce guide de démarrage rapide explique comment créer une application Notes multiplateforme simple à l’aide de Xamarin.Forms, pour vous permettre d’entrer une note et de la conserver dans le stockage de l’appareil. L’application finale est indiquée ci-dessous :

[![](quickstart-experimental-images/screenshots-sml.png "Application Notes")](quickstart-experimental-images/screenshots.png#lightbox "Application Notes")

::: zone pivot="windows"

## <a name="get-started-with-visual-studio"></a>Bien démarrer avec Visual Studio

1. Dans l’écran **Démarrer**, lancez Visual Studio. La page de démarrage s’ouvre :

    ![](quickstart-experimental-images/vs/start-page.png "Visual Studio")

2. Dans Visual Studio, cliquez sur **Créer un projet...** pour créer un projet :

    ![](quickstart-experimental-images/vs/new-solution.png "Nouveau projet")

3. Dans la boîte de dialogue **Nouveau projet**, cliquez sur **Multiplateforme**, sélectionnez le modèle **Application mobile (Xamarin.Forms)**, définissez le Nom **Notes**, choisissez l’emplacement du projet et cliquez sur le bouton **OK** :

    ![](quickstart-experimental-images/vs/new-project.png "Modèles de projet multiplateforme")

    > [!IMPORTANT]
    > Pour les extraits C# et XAML de ce guide de démarrage rapide, la solution doit se nommer **Notes**. L’utilisation d’un autre nom entraîne des erreurs de build quand vous copiez le code à partir de ce guide de démarrage rapide dans la solution.

4. Dans la boîte de dialogue **Nouvelle application multiplateforme**, cliquez sur **Application vide**, sélectionnez **.NET Standard** comme stratégie de partage de code et cliquez sur le bouton **OK** :

    ![](quickstart-experimental-images/vs/new-app.png "Nouvelle application multiplateforme")

5. Dans l’**Explorateur de solutions**, dans le projet **Notes**, double-cliquez sur **MainPage.xaml** pour l’ouvrir :

    ![](quickstart-experimental-images/vs/open-mainpage-xaml.png "Ouvrir MainPage.xaml")

6. Dans **MainPage.xaml**, supprimez tout le code du modèle et remplacez-le par le code suivant :

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.MainPage">
        <StackLayout Margin="10,35,10,10">
            <Label Text="Notes"
                   HorizontalOptions="Center"
                   FontAttributes="Bold" />
            <Editor x:Name="_editor"
                    Placeholder="Enter your note"
                    HeightRequest="100" />
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                <Button Text="Save"
                        Clicked="OnSaveButtonClicked" />
                <Button Grid.Column="1"
                        Text="Delete"
                        Clicked="OnDeleteButtonClicked"/>
            </Grid>
        </StackLayout>
    </ContentPage>
    ```

    Ce code définit de manière déclarative l’interface utilisateur pour la page, qui comprend un [`Label`](xref:Xamarin.Forms.Label) pour l’affichage du texte, un [`Editor`](xref:Xamarin.Forms.Editor) pour l’entrée de texte et deux instances [`Button`](xref:Xamarin.Forms.Button) qui indiquent à l’application d’enregistrer ou de supprimer un fichier. Les deux instances `Button` sont disposées horizontalement dans une [`Grid`](xref:Xamarin.Forms.Grid), tandis que `Label`, `Editor` et `Grid` sont disposés verticalement dans une [`StackLayout`](xref:Xamarin.Forms.StackLayout).

    Enregistrez les modifications apportées à **MainPage.xaml** en appuyant sur **Ctrl+S** et fermez le fichier.

7. Dans l’**Explorateur de solutions**, dans le projet **Notes**, développez **MainPage.xaml**, puis double-cliquez sur **MainPage.xaml.cs** pour l’ouvrir :

    ![](quickstart-experimental-images/vs/open-mainpage-codebehind.png "Ouvrir MainPage.xaml.cs")

8. Dans **MainPage.xaml.cs**, supprimez tout le code du modèle et remplacez-le par le code suivant :

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;

    namespace Notes
    {
        public partial class MainPage : ContentPage
        {
            string _fileName = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "notes.txt");

            public MainPage()
            {
                InitializeComponent();

                if (File.Exists(_fileName))
                {
                    _editor.Text = File.ReadAllText(_fileName);
                }
            }

            void OnSaveButtonClicked(object sender, EventArgs e)
            {
                File.WriteAllText(_fileName, _editor.Text);
            }

            void OnDeleteButtonClicked(object sender, EventArgs e)
            {
                if (File.Exists(_fileName))
                {
                    File.Delete(_fileName);
                }
                _editor.Text = string.Empty;
            }
        }
    }
    ```

    Ce code définit un champ `_fileName`, qui référence un fichier nommé `notes.txt` qui stocke les données de notes dans le dossier de données d’application locale pour l’application. Quand le constructeur de page est exécuté, le fichier est lu, s’il existe, et affiché dans l’[`Editor`](xref:Xamarin.Forms.Editor). Quand le [`Button`](xref:Xamarin.Forms.Button) **Enregistrer** est enfoncé, le gestionnaire d’événements `OnSaveButtonClicked` est exécuté, ce qui enregistre le contenu de l’`Editor` dans le fichier. Quand le `Button` **Supprimer** est enfoncé, le gestionnaire d’événements `OnDeleteButtonClicked` est exécuté, ce qui supprime le fichier, s’il existe, et supprime tout le texte de l’`Editor`.

    Enregistrez les modifications apportées à **MainPage.xaml.cs** en appuyant sur **Ctrl+S** et fermez le fichier.

### <a name="building-the-quickstart"></a>Génération du guide de démarrage rapide

1. Dans Visual Studio, sélectionnez l’élément de menu **Générer > Générer la solution** (ou appuyez sur F6). La solution est générée et un message de réussite s’affiche dans la barre d’état Visual Studio :

      ![](quickstart-experimental-images/vs/build-succeeded.png "Build réussie")

    En cas d’erreurs, répétez les étapes précédentes et corrigez les erreurs éventuelles jusqu’à ce que la solution soit générée.

2. Dans la barre d’outils de Visual Studio, appuyez sur le bouton **Démarrer** (le bouton triangulaire qui ressemble à un bouton Lire) pour lancer l’application dans l’émulateur Android de votre choix :

    ![](quickstart-experimental-images/vs/android-start.png "Barre d’outils Android de Visual Studio")

    ![](quickstart-experimental-images/vs/notes-android.png "Notes dans l’émulateur Android")

    Entrez une note et appuyez sur le bouton **Enregistrer**.

    > [!NOTE]
    > Vous ne devez exécuter les étapes suivantes que si vous avez un [Mac couplé](~/ios/get-started/installation/windows/connecting-to-mac/index.md) qui répond à la configuration système requise pour le développement Xamarin.Forms.

3. Dans la barre d’outils Visual Studio, cliquez avec le bouton droit sur le projet **Notes.iOS**, puis sélectionnez **Définir comme projet de démarrage**.

      ![](quickstart-experimental-images/vs/set-as-startup-project-ios.png "Définir iOS comme projet de démarrage")

4. Dans la barre d’outils de Visual Studio, appuyez sur le bouton **Démarrer** (le bouton triangulaire qui ressemble à un bouton Lire) pour lancer l’application dans le [simulateur distant iOS](~/tools/ios-simulator/index.md) de votre choix :

    ![](quickstart-experimental-images/vs/ios-start.png "Barre d’outils iOS Visual Studio")

    ![](quickstart-experimental-images/vs/notes-ios.png "Notes dans le simulateur iOS")

    Entrez une note et appuyez sur le bouton **Enregistrer**.

::: zone-end
::: zone pivot="macos"

## <a name="get-started-with-visual-studio-for-mac"></a>Bien démarrer avec Visual Studio pour Mac

1. Lancez Visual Studio pour Mac et, dans la page de démarrage, cliquez sur **Nouveau projet...** pour créer un projet :

    ![](quickstart-experimental-images/vsmac/new-project.png "Nouvelle solution")

2. Dans la boîte de dialogue **Choisir un modèle pour votre nouveau projet**, cliquez sur **Multiplateforme > Application**, sélectionnez le modèle **Application Forms vide**, puis cliquez sur le bouton **Suivant** :

    ![](quickstart-experimental-images/vsmac/choose-template.png "Choisir un modèle")

3. Dans la boîte de dialogue **Configurer une application Forms vide**, nommez la nouvelle application **Notes**, vérifiez que la case d’option **Utiliser .NET Standard** est sélectionnée, puis cliquez sur le bouton **Suivant** :    

    ![](quickstart-experimental-images/vsmac/configure-app.png "Configurer l’application Formulaires")

4. Dans la boîte de dialogue **Configurer votre nouvelle application Forms vide**, laissez **Notes** comme noms de la solution et du projet, choisissez un emplacement approprié pour le projet, puis cliquez sur le bouton **Créer** pour créer le projet :

    ![](quickstart-experimental-images/vsmac/configure-project.png "Configurer le projet Formulaires")

    > [!IMPORTANT]
    > Pour les extraits C# et XAML de ce guide de démarrage rapide, la solution et le projet doivent se nommer **Notes**. L’utilisation d’un autre nom entraîne des erreurs de build quand vous copiez le code à partir de ce guide de démarrage rapide dans le projet.

5. Dans le **Panneau Solutions**, dans le projet **Notes**, double-cliquez sur **MainPage.xaml** pour l’ouvrir :

    ![](quickstart-experimental-images/vsmac/mainpage-xaml.png "MainPage.xaml")

6. Dans **MainPage.xaml**, supprimez tout le code du modèle et remplacez-le par le code suivant :

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.MainPage">
        <StackLayout Margin="10,35,10,10">
            <Label Text="Notes"
                   HorizontalOptions="Center"
                   FontAttributes="Bold" />
            <Editor x:Name="_editor"
                    Placeholder="Enter your note"
                    HeightRequest="100" />
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                <Button Text="Save"
                        Clicked="OnSaveButtonClicked" />
                <Button Grid.Column="1"
                        Text="Delete"
                        Clicked="OnDeleteButtonClicked"/>
            </Grid>
        </StackLayout>
    </ContentPage>
    ```

    Ce code définit de manière déclarative l’interface utilisateur pour la page, qui comprend un [`Label`](xref:Xamarin.Forms.Label) pour l’affichage du texte, un [`Editor`](xref:Xamarin.Forms.Editor) pour l’entrée de texte et deux instances [`Button`](xref:Xamarin.Forms.Button) qui indiquent à l’application d’enregistrer ou de supprimer un fichier. Les deux instances `Button` sont disposées horizontalement dans une [`Grid`](xref:Xamarin.Forms.Grid), tandis que `Label`, `Editor` et `Grid` sont disposés verticalement dans une [`StackLayout`](xref:Xamarin.Forms.StackLayout).

    Enregistrez les modifications apportées à **MainPage.xaml** en choisissant **Fichier > Enregistrer** (ou en appuyant sur **&#8984;+S**) et fermez le fichier.

7. Dans le **Panneau Solutions**, dans le projet **Notes**, développez **MainPage.xaml**, puis double-cliquez sur **MainPage.xaml.cs** pour l’ouvrir :

    ![](quickstart-experimental-images/vsmac/mainpage-xaml-cs.png "MainPage.xaml.cs")

8. Dans **MainPage.xaml.cs**, supprimez tout le code du modèle et remplacez-le par le code suivant :

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;

    namespace Notes
    {
        public partial class MainPage : ContentPage
        {
            string _fileName = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "notes.txt");

            public MainPage()
            {
                InitializeComponent();

                if (File.Exists(_fileName))
                {
                    _editor.Text = File.ReadAllText(_fileName);
                }
            }

            void OnSaveButtonClicked(object sender, EventArgs e)
            {
                File.WriteAllText(_fileName, _editor.Text);
            }

            void OnDeleteButtonClicked(object sender, EventArgs e)
            {
                if (File.Exists(_fileName))
                {
                    File.Delete(_fileName);
                }
                _editor.Text = string.Empty;
            }
        }
    }
    ```

    Ce code définit un champ `_fileName`, qui référence un fichier nommé `notes.txt` qui stocke les données de notes dans le dossier de données d’application locale pour l’application. Quand le constructeur de page est exécuté, le fichier est lu, s’il existe, et affiché dans l’[`Editor`](xref:Xamarin.Forms.Editor). Quand le [`Button`](xref:Xamarin.Forms.Button) **Enregistrer** est enfoncé, le gestionnaire d’événements `OnSaveButtonClicked` est exécuté, ce qui enregistre le contenu de l’`Editor` dans le fichier. Quand le `Button` **Supprimer** est enfoncé, le gestionnaire d’événements `OnDeleteButtonClicked` est exécuté, ce qui supprime le fichier, s’il existe, et supprime tout le texte de l’`Editor`.

    Enregistrez les modifications apportées à **MainPage.xaml.cs** en choisissant **Fichier > Enregistrer** (ou en appuyant sur **&#8984;+S**) et fermez le fichier.

### <a name="building-the-quickstart"></a>Génération du guide de démarrage rapide

1. Dans Visual Studio pour Mac, sélectionnez l’élément de menu **Générer > Générer tout** (ou appuyez sur **&#8984;+B**). Les projets sont générés et un message de réussite s’affiche dans la barre d’outils Visual Studio pour Mac.

      ![](quickstart-experimental-images/vsmac/build-successful.png "Génération réussie")

    En cas d’erreurs, répétez les étapes précédentes et corrigez les erreurs éventuelles jusqu’à ce que les projets soient générés.

2. Dans le **Panneau Solutions**, sélectionnez le projet **Notes.iOS**, cliquez avec le bouton droit et sélectionnez **Définir comme projet de démarrage** :

      ![](quickstart-experimental-images/vsmac/set-startup-project-ios.png "Définir iOS comme projet de démarrage")

3. Dans la barre d’outils Visual Studio pour Mac, appuyez sur le bouton **Démarrer** (le bouton triangulaire qui ressemble à un bouton Lire) pour lancer l’application dans le simulateur iOS de votre choix :

      ![](quickstart-experimental-images/vsmac/start.png "Barre d’outils Visual Studio pour Mac")

      ![](quickstart-experimental-images/vsmac/notes-ios.png "Notes dans le simulateur iOS")

    Entrez une note et appuyez sur le bouton **Enregistrer**.

4. Dans le **Panneau Solutions**, sélectionnez le projet **Notes.Droid**, cliquez avec le bouton droit et sélectionnez **Définir comme projet de démarrage** :

      ![](quickstart-experimental-images/vsmac/set-startup-project-android.png "Définir Android comme projet de démarrage")

5. Dans la barre d’outils Visual Studio pour Mac, appuyez sur le bouton **Démarrer** (le bouton triangulaire qui ressemble à un bouton Lire) pour lancer l’application dans l’émulateur Android de votre choix :

      ![](quickstart-experimental-images/vsmac/notes-android.png "Notes dans l’Émulateur Android")

    Entrez une note et appuyez sur le bouton **Enregistrer**.

::: zone-end

## <a name="related-links"></a>Liens associés

- [Notes (exemple)](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/Notes/SinglePage/)
