---
title: Xamarin.Forms à l’aide de Visual Basic.NET
description: Modèle de projet de bibliothèque de classes portable Xamarin.Forms peut être modifié pour utiliser Visual Basic pour l’assembly principal, efficacement ce qui vous permet de créer des applications mobiles multiplateforme à l’aide de VB.NET.
ms.prod: xamarin
ms.assetid: da4b4ba9-9205-47dc-8bae-23272ede2c50
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: f397cf595a9ae151c5f105341733b2c57023fe99
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61282260"
---
# <a name="xamarinforms-using-visual-basicnet"></a>Xamarin.Forms à l’aide de Visual Basic.NET

Xamarin ne prend pas en charge Visual Basic directement, suivez les instructions de cette page pour créer une solution Xamarin.Forms PCL de c# et puis remplacer le projet de bibliothèque de classes portable de code commun avec Visual Basic.

[![](xamarin-forms-images/hero-sml.png "Créer une solution Xamarin.Forms PCL et remplacez ensuite le projet de bibliothèque de classes portable de code commun avec Visual Basic")](xamarin-forms-images/hero.png#lightbox)

> [!NOTE]
> Vous devez utiliser Visual Studio sur Windows programmation avec Visual Basic.

## <a name="xamarinforms-with-visual-basic-walkthrough"></a>Xamarin.Forms avec la procédure pas à pas de Visual Basic

Suivez ces étapes pour créer un projet Xamarin.Forms simple qui utilise Visual Basic :

1. Créer un nouveau *Xamarin.Forms c#* solution qui utilise les bibliothèques de classes portables (PCL).
Accédez à **fichier > Nouveau projet** et dans le **nouveau projet** fenêtre Naviguer vers **installé > Modèles > Visual C# > multiplateforme** puis choisissez **Cross Platform App (Xamarin.Forms ou natif) > Xamarin.Forms**.

2. Avec le bouton droit sur la solution et **Ajouter > Nouveau projet**.

3. Choisissez le **Visual Basic > bibliothèque de classes (Portable)** type de projet :

   [![](xamarin-forms-images/add-vb-2-sml.png "Ajouter le nouveau projet de bibliothèque de classes portable")](xamarin-forms-images/add-vb-2.png#lightbox)

4. Sélectionnez les plateformes, comme illustré pour configurer le profil PCL (veillez à inclure Xamarin.iOS et Xamarin.Android) :

   ![](xamarin-forms-images/add-vb-3-sml.png "Choisissez les plateformes pour prendre en charge")

5. Avec le bouton droit sur le projet Visual Basic et choisissez **propriétés**, puis modifiez le **par défaut d’espace de noms** pour correspondre à l’existant c# des projets :

   ![](xamarin-forms-images/add-vb-4s-sml.png "Vérifiez l’espace de noms racine Visual Basic correspond à l’application Xamarin.Forms")

6. Avec le bouton droit sur le nouveau projet Visual Basic et choisissez **gérer les Packages Nuget**, puis installez **Xamarin.Forms** et fermez la fenêtre du Gestionnaire de package.

   [![](xamarin-forms-images/add-vb-4-sml.png "Formulaires et fermer la fenêtre du Gestionnaire de package")](xamarin-forms-images/add-vb-4.png#lightbox)

7. Renommez la valeur par défaut **Class1** fichier *et* classe `App`:

   [![](xamarin-forms-images/add-vb-5-sml.png "Renommez le fichier Class1 par défaut et la classe application")](xamarin-forms-images/add-vb-5.png#lightbox)

8. Collez le code suivant dans le **App.vb** fichier, qui deviendra le point de départ de votre application Xamarin.Forms. N’oubliez pas d’inclure `Imports Xamarin.Forms` et ajoutez `Inherits Application` à la classe :

    ```vb 
    Imports Xamarin.Forms

    Public Class App
        Inherits Application

        Public Sub New()
            Dim label = New Label With {.HoriztonalTextAlignment = TextAlignment.Center,
                                        .FontSize = Device.GetNamedSize(NamedSize.Medium, GetType(Label)),
                                        .Text = "Welcome to Xamarin.Forms with Visual Basic.NET"}

            Dim stack = New StackLayout With {
                .VerticalOptions = LayoutOptions.Center
            }
            stack.Children.Add(label)

            Dim page = New ContentPage
            page.Content = stack
            MainPage = page

        End Sub

    End Class
    ```

9. Nous devons à présent pointer les projets iOS et Android sur le nouveau projet Visual Basic.
Avec le bouton droit sur le **références** nœud dans les projets iOS et Android pour ouvrir le **Gestionnaire de références**. Cycle d’annuler la C# bibliothèque portable et cochez la bibliothèque portable VB (n’oubliez pas, cela pour à la fois les projets iOS et Android).

   [![](xamarin-forms-images/add-vb-8-sml.png "Supprimer l’ancienne référence de projet, ajouter une référence de Visual Basic")](xamarin-forms-images/add-vb-8.png#lightbox)

10. Supprimer le projet c# portable. Ajouter un nouveau **.vb** fichiers à générer hors de votre application Xamarin.Forms. Un modèle pour les nouveaux `ContentPage`s en Visual Basic est indiqué ci-dessous :

    ```vb
    Imports Xamarin.Forms

    Public Class Page2
    Inherits ContentPage

        Public Sub New()
            Dim label = New Label With {.HoriztonalTextAlignment = TextAlignment.Center,
                                        .FontSize = Device.GetNamedSize(NamedSize.Medium, GetType(Label)),
                                        .Text = "Visual Basic ContentPage"}

            Dim stack = New StackLayout With {
                .VerticalOptions = LayoutOptions.Center
            }
            stack.Children.Add(label)

            Content = stack
        End Sub
    End Class
    ```

## <a name="limitations-of-visual-basic-in-xamarinforms"></a>Limitations de Visual Basic dans Xamarin.Forms

Comme indiqué sur la [Portable Visual Basic.NET page](~/cross-platform/platform/visual-basic/index.md), Xamarin ne prend pas en charge le langage Visual Basic. Cela signifie qu’il existe certaines limitations sur où vous pouvez utiliser Visual Basic :

 - Renderers personnalisés ne peuvent pas être écrits en Visual Basic, il doivent être écrit en c# dans les projets de plateforme native.

 - Les implémentations de Service de dépendance ne peut pas être écrite en Visual Basic, il doivent être écrit en c# dans les projets de plateforme native.

 - Les pages XAML ne peut pas être inclus dans le projet Visual Basic : le Générateur de code-behind peut uniquement générer c#. Il est possible d’inclure XAML dans une distincte, référencée, C# bibliothèque de classes portable et utiliser la liaison de données pour remplir les fichiers XAML via des modèles de Visual Basic (un exemple de ce qui est inclus dans le [exemple](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/XamarinFormsVB/XamlPages)).

 - Xamarin ne prend pas en charge le langage Visual Basic.NET.

## <a name="related-links"></a>Liens associés

- [XamarinFormsVB (sample)](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/XamarinFormsVB)
- [Le développement multiplateforme avec le .NET Framework](https://docs.microsoft.com/dotnet/standard/cross-platform/)
