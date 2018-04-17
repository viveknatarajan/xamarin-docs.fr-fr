---
title: 'Paramètres de plateforme GTK #'
description: 'Xamarin.Forms prend désormais en charge d’aperçu pour la plateforme GTK #'
ms.prod: xamarin
ms.assetid: 3417FB95-3E4B-47DA-85D0-F34832747236
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/10/2018
ms.openlocfilehash: a601e74cc274fd57bb2be9af3562b3a7290d7047
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="gtk-platform-setup"></a>Paramètres de plateforme GTK #

![Preview](~/media/shared/preview.png)

Xamarin.Forms prend désormais en charge préliminaire pour les applications GTK #. GTK # est une boîte à outils d’interface utilisateur graphique qui lie la boîte à outils GTK + et différentes bibliothèques GNOME, ce qui permet le développement d’applications de graphiques GNONE entièrement natives à l’aide de Mono et .NET. Cet article montre comment ajouter un projet GTK # à une solution Xamarin.Forms.

Avant de démarrer, créez une solution Xamarin.Forms ou utiliser une solution Xamarin.Forms existante, par exemple, [ **GameOfLife**](https://developer.xamarin.com/samples/xamarin-forms/BoxView/GameOfLife/).

> [!NOTE]
> Alors que cet article se concentre sur l’ajout d’une application GTK # pour une solution Xamarin.Forms dans VS2017 et Visual Studio pour Mac, il peut également être effectuée dans [MonoDevelop](http://www.monodevelop.com/) pour Linux.

## <a name="adding-a-gtk-app"></a>Ajout d’une application GTK #

GTK # pour Mac OS et Linux sont installé dans le cadre de [Mono](http://www.mono-project.com/download/stable/). GTK # pour .NET peut être installé sur Windows avec le [Installer GTK #](http://www.mono-project.com/download/stable/#download-win).

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Suivez ces instructions pour ajouter une application GTK # qui s’exécutera sur le bureau Windows :

1. Dans Visual Studio 2017, cliquez sur le nom de la solution dans **l’Explorateur de solutions** et choisissez **Ajouter > Nouveau projet...** .

2. Dans le **nouveau projet** fenêtre, sur la gauche, sélectionnez **Visual C#** et **de bureau Windows classique**. Dans la liste des types de projets, choisissez **bibliothèque de classes (.NET Framework)**et vérifiez que le **Framework** liste déroulante est définie sur un minimum de .NET Framework 4.7.

3. Tapez un nom pour le projet avec un **GTK** extension, par exemple **GameOfLife.GTK**. Cliquez sur le **Parcourir** bouton, sélectionnez le dossier contenant l’autre plate-forme projets, puis appuyez sur **sélectionner le dossier**. Cela place le projet GTK dans le même répertoire que les autres projets dans la solution.

    ![Ajouter un nouveau projet GTK](gtk-images/win/add-new-project.png "ajouter un nouveau projet GTK")

    Appuyez sur la **OK** bouton permettant de créer le projet.

4. Dans le **l’Explorateur de solutions**, cliquez avec le bouton droit sur le nouveau projet GTK et sélectionnez **gérer les Packages NuGet**. Sélectionnez le **Parcourir** , cliquez sur le **inclure la version préliminaire** case à cocher et recherchez **Xamarin.Forms** 3.0 ou version ultérieure.

    ![Sélectionnez le package Xamarin.Forms NuGet](gtk-images/win/select-forms-nuget-package.png "sélectionner le package Xamarin.Forms NuGet")

    Sélectionnez le package, cliquez sur le **installer** bouton.

5. Rechercher maintenant la **Xamarin.Forms.Platform.GTK** package 3.0 ou supérieur.

    ![Sélectionnez le package NuGet de Xamarin.Forms.Platform.GTK](gtk-images/win/select-forms-platform-nuget-package.png "sélectionner le package NuGet de Xamarin.Forms.Platform.GTK")

    Sélectionnez le package, cliquez sur le **installer** bouton.

6. Dans le **l’Explorateur de solutions**, cliquez sur le nom de la solution et sélectionnez **gérer les Packages NuGet pour la Solution**. Sélectionnez le **mise à jour** onglet et la **Xamarin.Forms** package. Sélectionnez tous les projets et les mettre à jour vers la même version de Xamarin.Forms que celles utilisées par le projet GTK.

7. Dans le **l’Explorateur de solutions**, avec le bouton droit sur **références** dans le projet GTK. Dans le **Gestionnaire de références** boîte de dialogue, sélectionnez **projets** à gauche et cocher la case à cocher adjacente au projet .NET Standard, PCL ou partagé :

    ![Fait référence au projet partagé](gtk-images/win/reference-shared-project.png "fait référence au projet partagé")

8. Dans le **Gestionnaire de références** boîte de dialogue, appuyez sur la **Parcourir** bouton et accédez à la **C:\Program Files (x86)\GtkSharp\2.12\lib** dossier et sélectionnez le  **ATK-sharp.dll**, **gdk-sharp.dll**, **glade-sharp.dll**, **glib-sharp.dll**, **gtk-dotnet.dll**, **gtk-sharp.dll** fichiers.

    ![Référencer les bibliothèques GTK #](gtk-images/win/reference-gtk-libraries.png "font référence aux bibliothèques GTK #")

    Appuyez sur la **OK** bouton Ajouter les références.

9. Dans le projet GTK, renommez **Class1.cs** à **Program.cs**.

10. Dans le projet GTK, modifier le **Program.cs** fichier afin qu’il ressemble au code suivant :

    ```csharp
    using System;
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.GTK;

    namespace GameOfLife.GTK
    {
        class MainClass
        {
            [STAThread]
            public static void Main(string[] args)
            {
                Gtk.Application.Init();
                Forms.Init();

                var app = new App();
                var window = new FormsWindow();
                window.LoadApplication(app);
                window.SetApplicationTitle("Game of Life");
                window.Show();

                Gtk.Application.Run();
            }
        }
    }
    ```

    Ce code initialise GTK # et Xamarin.Forms, crée une fenêtre d’application et exécute l’application.

11. Dans le **l’Explorateur de solutions**, cliquez avec le bouton droit sur le projet GTK et sélectionnez **propriétés**.

12. Dans le **propriétés** fenêtre, sélectionnez le **Application** onglet et modifier le **type de sortie** liste déroulante **Application Windows**.

    ![Modifier le type de sortie de projet](gtk-images/win/change-project-output-type.png "modifier le type de sortie de projet")

13. Dans le **l’Explorateur de solutions**, cliquez sur le projet WPF et sélectionnez **définir comme projet de démarrage**. Appuyez sur F5 pour exécuter le programme avec le débogueur Visual Studio sur le bureau Windows :

    ![Jeu GTK # de durée de vie](gtk-images/win/gtk-gameoflife.png "GTK # jeu de durée de vie")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Suivez ces instructions pour ajouter une application GTK # qui s’exécutera sur le bureau Mac :

1. Dans Visual Studio pour Mac, avec le bouton droit sur la solution Xamarin.Forms et choisissez **Ajouter > Ajouter un nouveau projet...** .

2. Dans le **nouveau projet** fenêtre choisissez **autres > .NET > Gtk # 2.0 projet** et appuyez sur **suivant**.

3. Tapez un nom pour le projet avec un **GTK** extension, par exemple **GameOfLife.GTK**, puis appuyez sur **créer**.

4. Dans le **Solution remplissage**, avec le bouton droit sur **Packages > ajouter des Packages en cours...**  pour le GTK le projet, puis ajoutez le package NuGet de version préliminaire Xamarin.Forms 3.0 ou supérieur.

    ![Sélectionnez le package Xamarin.Forms NuGet](gtk-images/mac/select-forms-nuget-package.png "sélectionner le package Xamarin.Forms NuGet")

5. Dans le **Solution remplissage**, avec le bouton droit sur **Packages > ajouter des Packages en cours...**  pour le GTK le projet, puis ajoutez le package NuGet de version préliminaire Xamarin.Forms.Platform.GTK 3.0 ou supérieur.

    ![Sélectionnez le package NuGet de Xamarin.Forms.Platform.GTK](gtk-images/mac/select-forms-platform-nuget-package.png "sélectionner le package NuGet de Xamarin.Forms.Platform.GTK")

6. Mettre à jour les autres projets de plateforme pour utiliser la même version de Xamarin.Forms que celle utilisée par le projet GTK.

7. Dans le **Solution remplissage**, avec le bouton droit sur **références > modifier les références...**  pour le GTK le projet, puis ajoutez une référence au projet Xamarin.Forms (.NET Standard, PCL ou projet partagé).

    ![Fait référence au projet partagé](gtk-images/mac/reference-shared-project.png "fait référence au projet partagé")

8. Modifier la **Program.cs** fichier du projet GTK afin qu’il ressemble au code suivant :

    ```csharp
    using System;
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.GTK;

    namespace GameOfLife.GTK
    {
        class MainClass
        {
            [STAThread]
            public static void Main(string[] args)
            {
                Gtk.Application.Init();
                Forms.Init();

                var app = new App();
                var window = new FormsWindow();
                window.LoadApplication(app);
                window.SetApplicationTitle("Game of Life");
                window.Show();

                Gtk.Application.Run();
            }
        }
    }
    ```

    Ce code initialise GTK # et Xamarin.Forms, crée une fenêtre d’application et exécute l’application.

9. Dans le **Solution remplissage**, cliquez sur le projet GTK et sélectionnez **définir comme projet de démarrage**.

10. Dans Visual Studio pour la barre d’outils du Mac, appuyez sur la **Démarrer** bouton (le bouton triangulaire qui ressemble à un bouton de lecture) pour lancer l’application.

    ![Jeu GTK # de durée de vie](gtk-images/mac/gtk-gameoflife.png "GTK # jeu de durée de vie")

-----

## <a name="next-steps"></a>Étapes suivantes

### <a name="platform-specifics"></a>Caractéristiques de la plateforme

Vous pouvez déterminer quelle plate-forme de votre application Xamarin.Forms est en cours d’exécution à partir de XAML ou du code. Cela vous permet de modifier les caractéristiques de programme lorsqu’il s’exécute sur GTK #. Dans le code, comparez la valeur de `Device.RuntimePlatform` avec la `Device.GTK` constante (ce qui équivaut à la chaîne « GTK »). S’il existe une correspondance, l’application s’exécute sur GTK #.

En XAML, vous pouvez utiliser la `OnPlatform` balise pour sélectionner une valeur de propriété spécifique à la plateforme :

```xaml
<Button.TextColor>
    <OnPlatform x:TypeArguments="Color">
        <On Platform="iOS" Value="White" />
        <On Platform="macOS" Value="White" />
        <On Platform="Android" Value="Black" />
        <On Platform="GTK" Value="Blue" />
    </OnPlatform>
</Button.TextColor>
```

### <a name="application-icon"></a>Icône d’application

Vous pouvez définir l’icône d’application au démarrage :

```csharp
window.SetApplicationIcon("icon.png");
```

### <a name="themes"></a>Thèmes

Il existe une grande variété de thèmes pour GTK #, et ils peuvent être utilisés à partir d’une application Xamarin.Forms :

```csharp
GtkThemes.Init ();
GtkThemes.LoadCustomTheme ("Themes/gtkrc");
```

### <a name="native-forms"></a>Formulaires natifs

Formulaires natifs permet de Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-dérivées des pages à être consommés par les projets natifs, y compris les projets GTK #. Cela peut être accompli en créant une instance de la [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-dérivée de page et de sa conversion en le natif GTK # type à l’aide de la `CreateContainer` méthode d’extension :

```csharp
var settingsView = new SettingsView().CreateContainer();
vbox.PackEnd(settingsView, true, true, 0);
```

Pour plus d’informations sur les formulaires natifs, consultez [Forms natif](~/xamarin-forms/platform/native-forms.md).

## <a name="issues"></a>Problèmes

Il s’agit d’une version préliminaire, vous devez donc vous attendre pas permet à tous les éléments est prêts pour la production. Pour l’état actuel de l’implémentation, consultez [état](https://github.com/jsuarezruiz/forms-gtk-progress/blob/master/Status.md)et pour les problèmes connus actuels, consultez [les problèmes en attente & connus](https://github.com/jsuarezruiz/forms-gtk-progress/blob/master/Issues-Pending.md).
