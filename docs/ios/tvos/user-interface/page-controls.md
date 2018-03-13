---
title: "Utilisation de contrôle de Page"
description: "Cet article décrit la conception et l’utilisation de contrôle de Page à l’intérieur d’une application Xamarin.tvOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 19198D46-7BBE-4D04-9BFA-7D1C5C9F9FC6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: f77eac8179f9e368e767bb4b586ccaa3f93e40a3
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="working-with-page-control"></a>Utilisation de contrôle de Page

_Cet article décrit la conception et l’utilisation de contrôle de Page à l’intérieur d’une application Xamarin.tvOS._

Vous devrez parfois afficher une série de pages ou des images dans votre application Xamarin.tvOS. Un contrôle de Page a été conçu pour montrer clairement quelle page à un utilisateur manque sur le nombre maximal de pages. Un contrôle de Page affiche une série de points par rapport à un thème sombre, oval en arrière-plan. La page en cours affiche un point plein, toutes les autres pages affichent sous forme de points vides. Le contrôle de Page fait correspondre les points de la plupart des externes si elles sont trop nombreuses pour tenir dans la zone d’arrière-plan.

[![](page-controls-images/page01.png "Exemple de contrôle de Page")](page-controls-images/page01.png#lightbox)

Un contrôle de Page dans un élément non interactif conçu pour envoyer des commentaires à l’utilisateur uniquement. Vous devez ajouter d’autres contrôles pour modifier le numéro de page actuel (par exemple, de mouvements ou boutons).

Apple a les suggestions suivantes lors de l’utilisation d’un contrôle de Page :

- **Utilisation de Collections complet uniquement** -Page contrôles fonctionnent mieux dans un environnement de plein écran pour afficher plusieurs pages qui existent dans une collection unique.
- **Limiter le nombre de Pages** -Page contrôles mieux adaptées à dix (10) ou moins de pages et un maximum de vingt (20) pages. Pour plus de vingt pages, envisagez d’utiliser un [vue de Collection](~/ios/tvos/user-interface/collection-views.md) et affiche les pages dans une grille.

<a name="Page-Controls-and-Storyboards" />

## <a name="page-controls-and-storyboards"></a>Contrôles de page et les animations

Utilisation des contrôles de Page dans une application Xamarin.tvOS, le plus simple consiste à ajouter à l’interface utilisateur de l’application d’utiliser le concepteur iOS.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

    
1. Dans le **Solution remplissage**, double-cliquez sur le `Main.storyboard` de fichier et l’ouvre pour modification.
1. Faites glisser un **contrôle de Page** à partir de la **boîte à outils** et déposez-la sur la vue : 

    [![](page-controls-images/page02.png "Un contrôle de Page")](page-controls-images/page02.png#lightbox)
1. Dans le **onglet Widget** de la **propriétés remplissage**, vous pouvez ajuster plusieurs propriétés du contrôle, telles que son **Page en cours** et **nombre de Pages**: 

    [![](page-controls-images/page03.png "L’onglet du Widget")](page-controls-images/page03.png#lightbox)
1. Ensuite, ajoutez les contrôles ou les mouvements à l’affichage pour les déplacer vers le haut et vers l’avant dans la collection de pages.
1. Enfin, affectez **noms** aux contrôles afin que vous pouvez répondre à leur en code c#. Exemple : 

    [![](page-controls-images/page04.png "Nom du contrôle")](page-controls-images/page04.png#lightbox)
1. Enregistrez les modifications apportées.
    

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

    
1. Dans le **l’Explorateur de solutions**, double-cliquez sur le `Main.storyboard` de fichier et l’ouvre pour modification.
1. Faites glisser un **contrôle de Page** à partir de la **boîte à outils** et déposez-la sur la vue : 

    [![](page-controls-images/page02-vs.png "Un contrôle de Page")](page-controls-images/page02-vs.png#lightbox)
1. Dans le **onglet Widget** de la **l’Explorateur de propriétés**, vous pouvez ajuster plusieurs propriétés du contrôle, telles que son **Page actuelle** et **nombre de Pages**: 

    [![](page-controls-images/page03-vs.png "L’onglet du Widget")](page-controls-images/page03-vs.png#lightbox)
1. Ensuite, ajoutez les contrôles ou les mouvements à l’affichage pour les déplacer vers le haut et vers l’avant dans la collection de pages.
1. Enfin, affectez **noms** aux contrôles afin que vous pouvez répondre à leur en code c#. Exemple : 

    [![](page-controls-images/page04-vs.png "Nom du contrôle")](page-controls-images/page04-vs.png#lightbox)
1. Enregistrez les modifications apportées.
    

-----

> [!IMPORTANT]
> **Remarque :** alors qu’il est possible d’attribuer des événements tels que `TouchUpInside` à un élément d’interface utilisateur (par exemple, un UIButton) dans le concepteur iOS, il ne sera jamais appelée car Apple TV n’a pas une fonction tactile d’écran ou de prendre en charge les événements tactiles. Vous devez toujours utiliser le `Primary Action` événement lors de la création de gestionnaires d’événements de tvOS éléments d’interface utilisateur.




Modifier votre vue contrôleur (exemple `ViewController.cs`) et ajoutez le code pour gérer les pages en cours de modification. Exemple :

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

Jetons un œil plus proche à deux propriétés du contrôle. Tout d’abord, pour spécifier le nombre maximal de pages, utilisez les éléments suivants :

```csharp
PageView.Pages = 6;
```

Pour modifier le numéro de page actuel, utilisez le code suivant :

```csharp
PageView.CurrentPage = PageNumber;
```

Le `CurrentPage` propriété est zéro (0) est basée, la première page est égal à zéro et le dernier sera moins le nombre maximal de pages.

Pour plus d’informations sur l’utilisation des plans conceptuels, veuillez consulter notre [Hello, Guide de démarrage rapide de tvOS](~/ios/tvos/get-started/hello-tvos.md). 

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a couvert la conception et l’utilisation de contrôle de Page à l’intérieur d’une application Xamarin.tvOS.



## <a name="related-links"></a>Liens associés

- [Exemples tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS les Guides Interface humaine](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Guide de programmation d’application de tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
