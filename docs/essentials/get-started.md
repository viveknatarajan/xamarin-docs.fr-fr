---
title: Bien démarrer avec Xamarin.Essentials
description: Xamarin.Essentials fournit une API inter-plateformes unique qui fonctionne avec n’importe quelle application Android, iOS ou UWP qui est accessible à partir du code partagé, quel que soit le mode de création de l’interface utilisateur.
ms.assetid: B2669C48-B659-4854-BD80-FEB0E876F5B9
author: jamesmontemagno
ms.author: jamont
ms.custom: video
ms.date: 08/08/2018
ms.openlocfilehash: 78b7235d8c9e45c2179b1cca2827f45fe6edd8b2
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50675105"
---
# <a name="get-started-with-xamarinessentials"></a>Bien démarrer avec Xamarin.Essentials

![Préversion NuGet](~/media/shared/pre-release.png)

Xamarin.Essentials fournit une API inter-plateformes unique qui fonctionne avec n’importe quelle application Android, iOS ou UWP qui est accessible à partir du code partagé, quel que soit le mode de création de l’interface utilisateur.

## <a name="platform-support"></a>Prise en charge de plateforme

Xamarin.Essentials prend en charge les plateformes et les systèmes d’exploitation suivants :

| Plateforme | Version |
| --- | --- |
| Android | 4.4 (API 19) ou version ultérieure |
| iOS |10.0 ou version ultérieure |
| UWP | 10.0.16299.0 ou version ultérieure |

## <a name="installation"></a>Installation

Xamarin.Essentials est disponible sous forme de package NuGet qui peut être ajouté à n’importe quel projet nouveau ou existant à l’aide de Visual Studio.

1. Téléchargez et installez [Visual Studio](http://visualstudio.com) avec [Visual Studio Tools pour Xamarin](~/cross-platform/get-started/installation/index.md).

2. Ouvrez un projet existant ou créez un projet à l’aide du modèle d’application vide sous **Visual Studio C#**  (Android, iPhone et iPad ou inter-plateformes). **Important** : en cas d’ajout à un projet UWP, vérifiez que la build 16299 ou une version ultérieure est définie dans les propriétés du projet.

3. Ajouter le package NuGet **Xamarin.Essentials** à chaque projet :

    # <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

    Dans le panneau de l’Explorateur de solutions, cliquez avec le bouton droit sur le nom de la solution et sélectionnez **Gérer les packages NuGet**. Recherchez **Xamarin.Essentials** et installez le package dans **Tous** les projets, notamment les bibliothèques Android, iOS, UWP et .NET Standard.

    > [!TIP]
    > Cochez la case **Inclure la préversion** lorsque le NuGet [**Xamarin.Essentials** ](https://www.nuget.org/packages/Xamarin.Essentials)est en préversion.

    # <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

    Dans le panneau de l’Explorateur de solutions, cliquez avec le bouton droit sur le nom du projet et sélectionnez **Ajouter > Ajouter des packages NuGet...**. Recherchez **Xamarin.Essentials** et installez le package dans **Tous** les projets, notamment les bibliothèques Android, iOS et .NET Standard.

    > [!TIP]
    > Cochez la case **Afficher les packages en préversion** lorsque le NuGet [**Xamarin.Essentials** ](https://www.nuget.org/packages/Xamarin.Essentials)est en préversion.

    -----

4. Ajoutez une référence à Xamarin.Essentials dans n’importe quelle classe C# pour référencer les API.

    ```csharp
    using Xamarin.Essentials;
    ```

5. Xamarin.Essentials nécessite une configuration spécifique à la plateforme :

    # <a name="androidtabandroid"></a>[Android](#tab/android)

    Xamarin.Essentials prend en charge une version minimale d’Android 4.4, correspondant au niveau d’API 19, mais la version Android cible pour la compilation doit être 8.1, correspondant au niveau de l’API 27. (Dans Visual Studio, ces deux versions sont définies dans la boîte de dialogue Propriétés du projet pour le projet Android, dans l’onglet Manifeste Android. (Dans Visual Studio pour Mac, elles sont définies dans la boîte de dialogue Options du projet pour le projet Android, dans l’onglet Application Android.) 

    Xamarin.Essentials installe la version 27.0.2.1 des bibliothèques Xamarin.Android.Support dont il a besoin. Les autres bibliothèques Xamarin.Android.Support requises par votre application doivent également être mises à jour vers la version 27.0.2.1 à l’aide du Gestionnaire de package NuGet. Toutes les bibliothèques Xamarin.Android.Support utilisées par votre application doivent être identiques, et doivent avoir au moins la version 27.0.2.1. Consultez la [page Résolution des problèmes](troubleshooting.md) si vous rencontrez des problèmes d’ajout du package NuGet Xamarin.Essentials ou de mise à jour des packages NuGet dans votre solution.

    Dans `MainLauncher` pour le projet Android ou n’importe quel `Activity` qui est lancé Xamarin.Essentials doit être initialisé dans la méthode `OnCreate` :

    ```csharp
    protected override void OnCreate(Bundle savedInstanceState) {
        //...
        base.OnCreate(savedInstanceState);
        Xamarin.Essentials.Platform.Init(this, savedInstanceState); // add this line to your code
        //...
    ```

    Pour gérer les autorisations d’exécution sur Android, Xamarin.Essentials doit recevoir `OnRequestPermissionsResult`. Ajoutez le code suivant à toutes les classes `Activity` :

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

6. Suivez les [guides Xamarin.Essentials](index.md) qui vous permettent de copier et de coller des extraits de code pour chaque fonctionnalité.

## <a name="xamarinessentials---cross-platform-apis-for-mobile-apps-video"></a>Xamarin.Essentials - APIs inter-plateformes pour Mobile Apps (vidéo)

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Snack-Pack-XamarinEssentials-Cross-Platform-APIs-for-Mobile-Apps/player]

## <a name="other-resources"></a>Autres ressources

Nous recommandons aux développeurs qui découvrent Xamarin de consulter [Bien démarrer avec le développement de Xamarin](~/cross-platform/getting-started/index.md).

Consultez le [référentiel GitHub Xamarin.Essentials](http://github.com/xamarin/Essentials) pour afficher le code source actuel, ce qui se prépare, des exemples d’exécution et pour cloner le référentiel. Les contributions de la communauté sont les bienvenues !

Parcourez la [documentation de l’API](xref:Xamarin.Essentials) pour découvrir toutes les fonctionnalités de Xamarin.Essentials.
