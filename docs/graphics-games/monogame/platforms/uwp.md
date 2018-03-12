---
title: "Création d’un projet MonoGame UWP"
description: "MonoGame peut être utilisé pour créer des jeux et des applications pour la plateforme Windows universelle, ciblant que plusieurs appareils avec un code base et un ensemble de contenu."
ms.topic: article
ms.prod: xamarin
ms.assetid: C6B99E44-00C1-4139-A1B7-FCFBE8749AB1
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: d8f805d8a3fcadd9c2a6758f1dc5592c03fe3ed4
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/12/2018
---
# <a name="creating-a-monogame-uwp-project"></a>Création d’un projet MonoGame UWP

_MonoGame peut être utilisé pour créer des jeux et des applications pour la plateforme Windows universelle, ciblant que plusieurs appareils avec un code base et un ensemble de contenu._

Cette procédure pas à pas couvre la création du projet MonoGame Universal Windows Platform (UWP) et chargement du contenu. Les applications UWP peuvent s’exécuter sur tous les appareils Windows 10, y compris les ordinateurs de bureau, tablettes, téléphones Windows Phone et Xbox One.

Cette procédure pas à pas crée un projet vide qui affiche un *bleu vif bleu* en arrière-plan (la couleur d’arrière-plan traditionnelle des applications XNA).


# <a name="requirements"></a>Configuration requise

Développement d’applications MonoGame UWP nécessite :

 - Système d’exploitation Windows 10
 - N’importe quelle version de Visual Studio 2015
 - Outils de développement Windows 10
 - Appareil de paramètre pour le mode développeur
- [3.5 MonoGame pour Visual Studio](http://www.monogame.net/2016/03/17/monogame-3-5/) ou une version ultérieure

Pour plus d’informations, consultez ce [page sur la configuration pour le développement Windows 10 UWP](https://msdn.microsoft.com/en-us/windows/uwp/get-started/get-set-up).

Jeux Xbox One peuvent être développés sur du matériel Xbox One de vente au détail. Logiciel supplémentaire est requis sur l’ordinateur de développement et de la Xbox One. Pour plus d’informations sur la configuration d’une Xbox One pour le développement de jeux, consultez cette page sur [configuration d’une Xbox One](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/index).


# <a name="creating-an-empty-template"></a>Création d’un modèle vide

Une fois toutes les ressources requises ont été installés et le mode développeur a été activé sur l’ordinateur Windows 10, nous pouvons créer un nouveau projet MonoGame à l’aide de Visual Studio en procédant comme suit :

1. Sélectionnez **fichier** > **nouveau** > **projet...**
1. Sélectionnez le **installé** > **modèles** > **Visual C#** > **MonoGame** catégorie : 

    ![](uwp-images/image1.png "Catégorie de MonoGame")

1. Sélectionnez le **MonoGame Windows 10 universelles projet** option : 

    ![](uwp-images/image2.png "Sélectionnez l’option de projet d’application universelle MonoGame Windows 10")

1. Entrez un nom pour le nouveau projet, cliquez sur **OK**.
Si Visual Studio affiche toutes les erreurs après avoir cliqué sur OK, vérifiez que les outils Windows 10 sont installés et que l’appareil est en mode développeur. 

Une fois que Visual Studio a terminé la création du modèle, nous pouvons l’exécuter pour afficher le projet vide en cours d’exécution :

![](uwp-images/image3.png "Une fois que Visual Studio a terminé la création du modèle, exécutez-le pour voir le projet vide en cours d’exécution")

Les nombres dans les coins fournissent des informations de diagnostic. Ces informations peuvent être supprimées en supprimant le code dans `App.xaml.cs` dans les `DEBUG` bloquer la `OnLaunched` méthode :


```csharp
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
#if DEBUG
    if (System.Diagnostics.Debugger.IsAttached)
    {
        this.DebugSettings.EnableFrameRateCounter = true;
    }
#endif
    ...
```

# <a name="running-on-xbox-one"></a>En cours d’exécution sur Xbox un

Les projets UWP peuvent déployer sur un appareil Windows 10 à partir du même projet. Après avoir configuré l’ordinateur de développement Windows 10 et Xbox One, les applications UWP peuvent être déployées en passant de la cible à l’ordinateur distant et en entrant adresse IP de la Xbox One :

![](uwp-images/remote.png "Les applications UWP peuvent être déployées en passant la cible à l’ordinateur distant et en entrant l’adresse IP de ceux Xbox")

Sur une Xbox, la bordure blanche représente la zone de non sécurisé pour les téléviseurs. Pour plus d’informations, consultez la [section de la zone de sécurité](#Safe_Area_on_Xbox_One).

![](uwp-images/safearea.png "Sur Xbox One, la bordure blanche représente la zone non sécurisé pour les téléviseurs")

## <a name="safe-area-on-xbox-one"></a>Zone de sans échec sur Xbox un

Développement de jeux pour les consoles requiert la prise en compte de la zone sécurisée, ce qui est une zone dans le centre de l’écran qui doit contenir tous les éléments critiques visuels (par exemple, l’interface utilisateur ou HUD). La zone en dehors de la zone de sans échec n’est pas garantie pour être visibles sur tous les téléviseurs, éléments visuels placés dans cette zone peuvent être partiellement ou totalement invisibles sur certains écrans.

Le modèle MonoGame pour Xbox One considère que la zone sécurisée et rend sous la forme d’une bordure blanche. Cette zone est également répercutée lors de l’exécution dans du jeu `Window.ClientBounds` la propriété comme indiqué dans cette image de la fenêtre Espion dans Visual Studio. Notez que la hauteur des limites du client est 1016, en dépit de la résolution d’écran de 1920 x 1080 pixels :

![](uwp-images/clientbounds.png "Notez que la hauteur des limites du client est 1016, en dépit de la résolution d’écran de 1920 x 1080 pixels")


# <a name="referencing-content-in-uwp-projects"></a>Référencement du contenu dans les projets UWP

Contenu dans les projets MonoGame peut être référencé directement à partir de fichiers ou via la [Pipeline de contenu MonoGame](~/graphics-games/cocossharp/content-pipeline/index.md). Petits projets jeu peuvent bénéficier de la simplicité de chargement à partir du fichier. Projets de grande taille l’avantage d’utiliser le pipeline de contenu pour optimiser le contenu pour réduire la taille et le temps de chargement. Contrairement à XNA sur la Xbox 360, la `System.IO.File` classe n’est disponible sur les applications UWP d’une Xbox.

Pour plus d’informations sur le chargement de contenu à l’aide du pipeline de contenu, consultez la [contenu Pipeline Guide](~/graphics-games/cocossharp/content-pipeline/index.md). 


## <a name="loading-content-from-file"></a>Chargement de contenu à partir du fichier

Contrairement à iOS et Android, les projets UWP peuvent référencer des fichiers par rapport à l’exécutable. Jeux simples peuvent d’effectuer cet charge le contenu technique sans avoir besoin de modifier et générer le projet de pipeline de contenu.

Pour charger un `Texture2D` à partir du fichier :

1. Dans le dossier de contenu dans le projet UWP, ajoutez un fichier .png. Ajout de contenu vers le dossier de contenu est une convention de XNA et MonoGame.
1. Avec le bouton droit sur le fichier PNG nouvellement ajouté, puis sélectionnez Propriétés.
1. Modifier la **copier dans le répertoire de sortie** à **Copier si plus récent**.
1. Ajoutez le code suivant à la méthode d’initialisation de votre jeu pour charger un `Texture2D`:

    ```csharp
    Texture2D texture;
    using (var stream = System.IO.File.OpenRead("Content/YourPngName.png"))
    {
        texture = Texture2D.FromStream(graphics.GraphicsDevice, stream);
    }
    ```

Pour plus d’informations sur l’utilisation d’un `Texture2D`, consultez la [Introduction au guide de MonoGame](~/graphics-games/monogame/introduction/index.md).


# <a name="summary"></a>Récapitulatif

Ce guide explique comment créer un nouveau projet UWP et considérations relatives à la plateforme Windows universelle spécifiques lors du chargement de fichiers. Les développeurs qui souhaitent créer des jeux UWP complètes en savoir plus sur MonoGame dans le [Introduction au Guide de MonoGame](~/graphics-games/monogame/introduction/index.md).