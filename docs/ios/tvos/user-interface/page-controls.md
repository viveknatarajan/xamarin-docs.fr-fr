---
title: Utilisation de contrôles de Page dans Xamarin de tvOS
description: Ce document décrit comment utiliser des contrôles de page de tvOS dans une application conçue avec Xamarin. Il fournit une description globale des contrôles de page, explique comment les définir dans les tables de montage séquentiel et examine comment répondre aux événements de changement de page.
ms.prod: xamarin
ms.assetid: 19198D46-7BBE-4D04-9BFA-7D1C5C9F9FC6
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 173bc7713b5b8c330d4d4c5863bef24be8bdcb52
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61179616"
---
# <a name="working-with-tvos-page-controls-in-xamarin"></a>Utilisation de contrôles de Page dans Xamarin de tvOS

Parfois, vous devrez peut-être afficher une série de pages ou des images dans votre application Xamarin.tvOS. Un contrôle de Page a été conçu pour montrer clairement quelle page un utilisateur manque sur le nombre maximal de pages. Un contrôle de Page affiche une série de points par rapport à une lumière, oval en forme en arrière-plan. La page en cours affiche un point plein, toutes les autres pages affichent sous forme de points vides. Le contrôle de Page fait correspondre les points de la plupart des externes s’il existe trop nombreuses pour s’ajuster à sa zone d’arrière-plan.

[![](page-controls-images/page01.png "Exemple de contrôle de Page")](page-controls-images/page01.png#lightbox)

Un contrôle de Page dans un élément non interactif conçu pour envoyer des commentaires à l’utilisateur uniquement. Vous devez ajouter d’autres contrôles pour modifier le numéro de page actuel (par exemple, les mouvements ou les boutons).

Apple a les suggestions suivantes lors de l’utilisation d’un contrôle de Page :

- **Utilisation de Collections complète uniquement** -contrôles de Page fonctionnent le mieux dans un environnement de plein écran pour afficher plusieurs pages qui existent dans une collection unique.
- **Limiter le nombre de Pages** -contrôles de Page mieux adaptées à dix (10) ou moins de pages et un maximum de pages vingt (20). Pour plus de 20 pages, envisagez d’utiliser un [vue de Collection](~/ios/tvos/user-interface/collection-views.md) et affiche les pages dans une grille.

<a name="Page-Controls-and-Storyboards" />

## <a name="page-controls-and-storyboards"></a>Contrôles de page et les tables de montage séquentiel

Pour travailler avec des contrôles de Page dans une application Xamarin.tvOS le plus simple consiste à les ajouter à l’interface utilisateur de l’application à l’aide du concepteur iOS.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

    
1. Dans le **panneau solutions**, double-cliquez sur le `Main.storyboard` de fichier et l’ouvrir pour modification.
1. Faites glisser un **contrôle de Page** à partir de la **boîte à outils** et déposez-la sur la vue : 

    [![](page-controls-images/page02.png "Un contrôle de Page")](page-controls-images/page02.png#lightbox)
1. Dans le **onglet Widget** de la **panneau Propriétés**, vous pouvez ajuster plusieurs propriétés du contrôle de Page, telles que son **Page actuelle** et **# de Pages**: 

    [![](page-controls-images/page03.png "L’onglet de Widget")](page-controls-images/page03.png#lightbox)
1. Ensuite, ajoutez des contrôles ou des mouvements à l’affichage pour les déplacer vers le haut et en arrière dans la collection de pages.
1. Enfin, attribuez **noms** aux contrôles afin que vous pouvez y répondre dans C# code. Exemple : 

    [![](page-controls-images/page04.png "Nommez le contrôle")](page-controls-images/page04.png#lightbox)
1. Enregistrez les modifications apportées.
    

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

    
1. Dans le **l’Explorateur de solutions**, double-cliquez sur le `Main.storyboard` de fichier et l’ouvrir pour modification.
1. Faites glisser un **contrôle de Page** à partir de la **boîte à outils** et déposez-la sur la vue : 

    [![](page-controls-images/page02-vs.png "Un contrôle de Page")](page-controls-images/page02-vs.png#lightbox)
1. Dans le **onglet Widget** de la **l’Explorateur de propriétés**, vous pouvez ajuster plusieurs propriétés du contrôle de Page, telles que son **Page actuelle** et **# de Pages**: 

    [![](page-controls-images/page03-vs.png "L’onglet de Widget")](page-controls-images/page03-vs.png#lightbox)
1. Ensuite, ajoutez des contrôles ou des mouvements à l’affichage pour les déplacer vers le haut et en arrière dans la collection de pages.
1. Enfin, attribuez **noms** aux contrôles afin que vous pouvez y répondre dans C# code. Exemple : 

    [![](page-controls-images/page04-vs.png "Nommez le contrôle")](page-controls-images/page04-vs.png#lightbox)
1. Enregistrez les modifications apportées.
    

-----

> [!IMPORTANT]
> Bien qu’il soit possible d’attribuer des événements tels que `TouchUpInside` à un élément d’interface utilisateur (par exemple, un UIButton) dans le concepteur iOS, il ne sera jamais appelée car Apple TV n’a pas une pression tactile écran ou de prendre en charge les événements tactiles. Vous devez toujours utiliser le `Primary Action` événement lors de la création des éléments d’interface utilisateur de gestionnaires d’événements pour tvOS.

Modifier votre contrôleur d’affichage (exemple `ViewController.cs`) et ajoutez le code pour gérer les pages en cours de modification. Exemple :

```csharp
using System;
using Foundation;
using UIKit;

namespace MySingleView
{
    public partial class ViewController : UIViewController
    {
        #region Computed Properties
        public nint PageNumber { get; set; } = 0;
        #endregion

        #region Constructors
        public ViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Initialize
            PageView.Pages = 6;
            ShowCat ();
        }

        public override void DidReceiveMemoryWarning ()
        {
            base.DidReceiveMemoryWarning ();
            // Release any cached data, images, etc that aren't in use.
        }
        #endregion

        #region Custom Actions
        partial void NextCat (UIBarButtonItem sender) {

            // Display next Cat
            if (++PageNumber > 5) {
                PageNumber = 5;
            }
            ShowCat();

        }

        partial void PreviousCat (UIBarButtonItem sender) {
            // Display previous cat
            if (--PageNumber < 0) {
                PageNumber = 0;
            }
            ShowCat();
        }
        #endregion

        #region Private Methods
        private void ShowCat() {

            // Adjust UI
            PreviousButton.Enabled = (PageNumber > 0);
            NextButton.Enabled = (PageNumber < 5);
            PageView.CurrentPage = PageNumber;

            // Display new cat
            CatView.Image = UIImage.FromFile(string.Format("Cat{0:00}.jpg",PageNumber+1));
        }
        #endregion
    }
}
```

Examinons plus en détail sur deux propriétés du contrôle de Page. Tout d’abord, pour spécifier le nombre maximal de pages, utilisez les éléments suivants :

```csharp
PageView.Pages = 6;
```

Pour modifier le numéro de page actuel, utilisez le code suivant :

```csharp
PageView.CurrentPage = PageNumber;
```

Le `CurrentPage` propriété est zéro (0) est basée, la première page sera égal à zéro et le dernier sera 1 moins le nombre maximal de pages.

Pour plus d’informations sur l’utilisation des tables de montage séquentiel, consultez notre [Hello, tvOS Guide de démarrage rapide](~/ios/tvos/get-started/hello-tvos.md). 

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a présenté la conception et l’utilisation de contrôle de Page à l’intérieur d’une application Xamarin.tvOS.



## <a name="related-links"></a>Liens associés

- [Exemples tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS Human Interface Guides](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Guide de programmation d’application pour tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
