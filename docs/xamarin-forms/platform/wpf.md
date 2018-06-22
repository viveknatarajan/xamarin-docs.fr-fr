---
title: Programme d’installation WPF plateforme
description: Xamarin.Forms prend désormais en charge d’aperçu pour la plateforme WPF
ms.prod: xamarin
ms.assetid: 650723F2-4279-4B7B-B0A1-D7F8FF26BF1E
ms.technology: xamarin-forms
ms.custom: xamu-video
author: charlespetzold
ms.author: chape
ms.date: 04/05/2018
ms.openlocfilehash: 416e33f131c6e1ef144608f98964fd8372f454f8
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33799345"
---
# <a name="wpf-platform-setup"></a>Programme d’installation WPF plateforme

![Preview](~/media/shared/preview.png)

Xamarin.Forms prend désormais en charge de l’aperçu de Windows Presentation Foundation (WPF). Cet article explique comment ajouter un projet WPF pour une solution Xamarin.Forms.

Avant de commencer, créez une solution Xamarin.Forms dans Visual Studio 2017 ou utiliser une solution Xamarin.Forms existante, par exemple, [ **BoxViewClock**](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BoxViewClock/). Vous pouvez uniquement ajouter des applications WPF à une solution Xamarin.Forms dans Windows.

## <a name="add-a-wpf-project-to-a-xamarinforms-app-with-xamarinuniversity"></a>Ajouter un projet WPF pour une application Xamarin.Forms avec Xamarin.University

> [!VIDEO https://youtube.com/embed/Fy9N6OSxK64]

**Xamarin.Forms 3.0 WPF prend en charge, par [Xamarin University](https://university.xamarin.com/)**

## <a name="adding-a-wpf-app"></a>Ajout d’une application WPF

Suivez ces instructions pour ajouter une application WPF qui s’exécutent sur les ordinateurs de bureau Windows 7, 8 et 10 :

1. Dans Visual Studio 2017, cliquez sur le nom de la solution dans le **l’Explorateur de solutions** et choisissez **Ajouter > Nouveau projet...** .

2. Dans le **nouveau projet** fenêtre, sur la gauche, sélectionnez **Visual C#** et **de bureau Windows classique**. Dans la liste des types de projets, choisissez **l’application WPF (.NET Framework)**. 

3. Tapez un nom pour le projet avec un **WPF** extension, par exemple, **BoxViewClock.WPF**. Cliquez sur le **Parcourir** bouton, sélectionnez le **BoxViewClock** dossier, puis appuyez sur **sélectionner le dossier**. Cela place le projet WPF dans le même répertoire que les autres projets dans la solution.

    ![Ajouter un nouveau projet WPF](wpf-images/add-new-project.png "ajouter un nouveau projet WPF")

    Appuyez sur OK pour créer le projet.

4. Dans le **l’Explorateur de solutions**, à droite cliquez sur le nouveau **BoxViewClock.WPF** de projet et sélectionnez **gérer les Packages NuGet**. Sélectionnez le **Parcourir** , cliquez sur le **inclure la version préliminaire** case à cocher et recherchez **Xamarin.Forms**.

    ![Sélectionnez le package NuGet](wpf-images/select-nuget-package.png "sélectionner le package NuGet")

    Cliquez sur ce package et cliquez sur le **installer** bouton.

5. Rechercher maintenant **Xamarin.Forms.Platform.WPF** empaqueter et d’installer un également. Assurez-vous que le package est à partir de Microsoft !

6. Cliquez avec le bouton droit sur le nom de la solution dans le **l’Explorateur de solutions** et sélectionnez **gérer les Packages NuGet pour la Solution**. Sélectionnez le **mise à jour** onglet et la **Xamarin.Forms** package. Sélectionnez tous les projets et les mettre à jour vers la même version de Xamarin.Forms :

    ![Mettre à jour le package NuGet](wpf-images/update-nuget-package.png "mettre à jour le package NuGet") 

7. Dans le projet WPF, cliquez sur **références**. Dans le **Gestionnaire de références** boîte de dialogue, sélectionnez **projets** à gauche et cocher la case à cocher adjacente à la **BoxViewClock** projet :

    ![Fait référence au projet partagé](wpf-images/reference-shared-project.png "fait référence au projet partagé")

8. Modifier la **MainWindow.xaml** fichier du projet WPF. Dans le `Window` , ajoutez une déclaration d’espace de noms XML pour le **Xamarin.Forms.Platform.WPF** assembly et l’espace de noms :

    ```xaml
    xmlns:wpf="clr-namespace:Xamarin.Forms.Platform.WPF;assembly=Xamarin.Forms.Platform.WPF"
    ```

    Modifiez à présent la `Window` à la balise `wpf:FormsApplicationPage`. Modifier la `Title` affectant le nom de votre application, par exemple, **BoxViewClock**. Le fichier XAML complet doit ressembler à ceci :

    ```xaml
    <wpf:FormsApplicationPage x:Class="BoxViewClock.WPF.MainWindow"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:wpf="clr-namespace:Xamarin.Forms.Platform.WPF;assembly=Xamarin.Forms.Platform.WPF"
            xmlns:local="clr-namespace:BoxViewClock.WPF"
            mc:Ignorable="d"
            Title="BoxViewClock" Height="450" Width="800">
        <Grid>
        
        </Grid>
    </wpf:FormsApplicationPage>
    ```

9. Modifier la **MainWindow.xaml.cs** fichier du projet WPF. Ajoutez deux nouvelles `using` directives :

    ```csharp
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.WPF;
    ```

    Modifiez la classe de base de `MainWindow` de `Window` à `FormsApplicationPage`. Suivant le `InitializeComponent` appeler, ajoutez les deux instructions suivantes :

    ```csharp
    Forms.Init();
    LoadApplication(new BoxViewClock.App());
    ```
    
    À l’exception des commentaires et inutilisés `using` directives, le texte complet **MainWindows.xaml.cs** fichier doit ressembler à ceci :

    ```csharp
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.WPF;

    namespace BoxViewClock.WPF
    {
        public partial class MainWindow : FormsApplicationPage
        {
            public MainWindow()
            {
                InitializeComponent();

                Forms.Init();
                LoadApplication(new BoxViewClock.App());
            }
        }
    }
    ```

10. Cliquez sur le projet WPF dans le **l’Explorateur de solutions** et sélectionnez **définir comme projet de démarrage**. Appuyez sur F5 pour exécuter le programme avec le débogueur Visual Studio sur le bureau Windows :

    ![WPF BoxView horloge](wpf-images/wpf-boxviewclock.png "WPF BoxView horloge" )

## <a name="next-steps"></a>Étapes suivantes

### <a name="platform-specifics"></a>Caractéristiques de la plateforme

Vous pouvez déterminer quelle plate-forme de votre application Xamarin.Forms est en cours d’exécution à partir de code ou XAML. Cela vous permet de modifier les caractéristiques de programme lorsqu’il s’exécute sur WPF. Dans le code, comparez la valeur de `Device.RuntimePlatform` avec la `Device.WPF` constante (ce qui équivaut à la chaîne « WPF »). S’il existe une correspondance, l’application s’exécute sur WPF.

En XAML, vous pouvez utiliser la `OnPlatform` balise pour sélectionner une valeur de propriété spécifique à la plateforme :

```xaml
<Button.TextColor>
    <OnPlatform x:TypeArguments="Color">
        <On Platform="iOS" Value="White" />
        <On Platform="macOS" Value="White" />
        <On Platform="Android" Value="Black" />
        <On Platform="WPF" Value="Blue" />
    </OnPlatform>
</Button.TextColor>
```

### <a name="window-size"></a>Taille de la fenêtre

Vous pouvez ajuster la taille initiale de la fenêtre dans WPF **MainWindow.xaml** fichier :

```xaml
Title="BoxViewClock" Height="450" Width="800"
```

## <a name="issues"></a>Problèmes

Il s’agit d’une version préliminaire, vous devez donc vous attendre pas permet à tous les éléments est prêts pour la production. Pas de tous les packages NuGet Xamarin.Forms sont prêts pour le WPF, et certaines fonctionnalités peuvent être entièrement ne fonctionne pas.

