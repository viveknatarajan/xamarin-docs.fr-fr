---
title: Prise en main Xamarin.Essentials
description: Xamarin.Essentials fournit une API de multi-plateforme unique qui fonctionne avec n’importe quel iOS, Android ou UWP application qui est accessible à partir du code, quel que soit le mode de création de l’interface utilisateur partagé.
ms.assetid: B2669C48-B659-4854-BD80-FEB0E876F5B9
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: c72c1c66a465075770ce739270cb4b1f2c6fba7a
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353774"
---
# <a name="get-started-with-xamarinessentials"></a>Prise en main Xamarin.Essentials

![Version préliminaire NuGet](~/media/shared/pre-release.png)

Xamarin.Essentials fournit une API de multi-plateforme unique qui fonctionne avec n’importe quel iOS, Android ou UWP application qui est accessible à partir du code, quel que soit le mode de création de l’interface utilisateur partagé.

## <a name="platform-support"></a>Prise en charge de plateforme

Xamarin.Essentials prend en charge les plates-formes et les systèmes d’exploitation suivants :

| Plateforme | Version                                |
| ---------- | -------------------------------------- |
| Android    | 4.4 (API 19) ou une version ultérieure |
| iOS        | 10.0 ou version ultérieure             |
| UWP        | 10.0.16299.0 ou une version ultérieure |

## <a name="installation"></a>Installation

Xamarin.Essentials est disponible sous forme de package NuGet qui peut être ajouté à n’importe quel projet nouveau ou existant à l’aide de Visual Studio.

1. Téléchargez et installez [Visual Studio](http://visualstudio.com) avec la [Visual Studio tools pour Xamarin](~/cross-platform/get-started/installation/index.md).

2. Ouvrez un projet existant ou créer un nouveau projet à l’aide du modèle application vide sous **Visual Studio c#** (Android, iPhone et iPad ou inter-plateformes). **Important**: si l’ajout à un projet UWP Vérifiez la Build 16299 ou une version ultérieure est définie dans les propriétés du projet.

3. Ajouter le **Xamarin.Essentials** package NuGet à chaque projet :

    # <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

    Dans le volet Explorateur de solutions, cliquez avec le bouton droit sur le nom de la solution et sélectionnez **gérer les Packages NuGet**. Recherchez **Xamarin.Essentials** et installez le package dans **tous les** projets, y compris les bibliothèques Android, iOS, UWP et .NET Standard.

    > [!TIP]
    > Vérifier le **inclure la version préliminaire** zone lors de la [ **Xamarin.Essentials** NuGet](https://www.nuget.org/packages/Xamarin.Essentials) est en version préliminaire.

    # <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

    Dans le volet Explorateur de solutions, cliquez avec le bouton droit sur le nom du projet et sélectionnez **Ajouter > ajouter des Packages NuGet...** . Recherchez **Xamarin.Essentials** et installez le package dans **tous les** projets, y compris les bibliothèques Android, iOS et .NET Standard.

    > [!TIP]
    > Vérifier le **afficher les packages de préversion** zone lors de la [ **Xamarin.Essentials** NuGet](https://www.nuget.org/packages/Xamarin.Essentials) est en version préliminaire.

    -----

4. Ajouter une référence à Xamarin.Essentials dans n’importe quelle classe c# pour référencer les API.

    ```csharp
    using Xamarin.Essentials;
    ```

5. Xamarin.Essentials nécessite une configuration spécifique à la plateforme :

    # <a name="androidtabandroid"></a>[Android](#tab/android)

    Xamarin.Essentials prend en charge une version minimale d’Android 4.4, correspondant au niveau d’API 19, mais la version Android cible pour la compilation doit être 8.1, correspondant au niveau de l’API 27. (Dans Visual Studio, ces deux versions sont définies dans la boîte de dialogue Propriétés du projet pour le projet Android, dans l’onglet manifeste Android. Dans Visual Studio pour Mac, elles sont définies dans la boîte de dialogue Options du projet pour le projet Android, dans l’onglet Application Android.)

    Xamarin.Essentials 27.0.2.1 d’installe les bibliothèques Xamarin.Android.Support dont il a besoin. D’autres bibliothèques Xamarin.Android.Support requis par votre application doivent également être mis à jour vers la version 27.0.2.1 à l’aide du Gestionnaire de package NuGet. Toutes les bibliothèques Xamarin.Android.Support utilisés par votre application doit être identique et doit être au moins la version 27.0.2.1. Reportez-vous à la [page Résolution des problèmes](troubleshooting.md) si vous rencontrez des problèmes d’ajout du package Xamarin.Essentials NuGet ou la mise à jour des packages NuGet dans votre solution.

    Dans le projet Android `MainLauncher` ou n’importe quel `Activity` qui est lancée Xamarin.Essentials doivent être initialisés dans le `OnCreate` méthode :

    ```csharp
    Xamarin.Essentials.Platform.Init(this, bundle);
    ```

    Pour gérer les autorisations d’exécution sur Android, Xamarin.Essentials doit recevoir les `OnRequestPermissionsResult`. Ajoutez le code suivant à tous les `Activity` classes :

    ```csharp
    public override void OnRequestPermissionsResult(int requestCode, string[] permissions, [GeneratedEnum] Android.Content.PM.Permission[] grantResults)
    {
        Xamarin.Essentials.Platform.OnRequestPermissionsResult(requestCode, permissions, grantResults);

        base.OnRequestPermissionsResult(requestCode, permissions, grantResults);
    }
    ```

    # <a name="iostabios"></a>[iOS](#tab/ios)

    Aucune configuration supplémentaire n’est requise.

    # <a name="uwptabuwp"></a>[UWP](#tab/uwp)

    Aucune configuration supplémentaire n’est requise.

    -----

6. Suivez le [Xamarin.Essentials guides](index.md) qui permettent de copier et coller des extraits de code pour chaque fonctionnalité.

## <a name="other-resources"></a>Autres ressources

Nous vous recommandons des développeurs qui découvrent Xamarin visite [mise en route avec le développement de Xamarin](~/cross-platform/getting-started/index.md).

Visitez le [Xamarin.Essentials référentiel](http://github.com/xamarin/Essentials) à constate que le code source, ce qui se prépare, exécuter des exemples et fermez le référentiel actuel. Contributions de la Communauté sont les bienvenus !

Parcourez le [documentation de l’API](xref:Xamarin.Essentials) pour toutes les fonctionnalités de Xamarin.Essentials.
