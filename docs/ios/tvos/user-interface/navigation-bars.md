---
title: Utilisation des barres de Navigation dans Xamarin de tvOS
description: Ce document décrit comment utiliser des barres de navigation dans une application tvOS générées avec Xamarin. Il aborde la configuration des barres de navigation dans une table de montage séquentiel et répondre aux événements à partir de ces boutons.
ms.prod: xamarin
ms.assetid: 74E396B7-87F0-46F7-BC6C-827DB8884C97
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 81e6cfe1e532bcfa7616e35adb28b314587bafc8
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50106947"
---
# <a name="working-with-tvos-navigation-bars-in-xamarin"></a>Utilisation des barres de Navigation dans Xamarin de tvOS

Barres de navigation peuvent être ajoutés en haut des vues pour afficher le titre et des boutons de barre de Navigation facultatifs. Ils sont généralement utilisés lorsque l’utilisateur a accédé à partir d’une page principale, comme une vue de Table, la Collection ou le Menu pour un sous-affichage présentant les détails de l’élément sélectionné.

[![](navigation-bars-images/navbar01.png "Exemple de barre de Navigation")](navigation-bars-images/navbar01.png#lightbox)

En outre, au titre (qui est affiché dans le centre), les barres de Navigation peut contenir un ou plusieurs boutons de barre de Navigation (`UIBarButtonItem`) sur les côtés gauche et droit de la barre.

> [!IMPORTANT]
> Barres de navigation sont totalement transparents par défaut. Veillez à garantir que le contenu de la barre de Navigation reste accessible en lecture sur le contenu situé en dessous. Par exemple, lorsque dans une vue de Table ou de la Collection du défilement du contenu dans cette section.

<a name="Navigation-Bars-and-Storyboards" />

## <a name="navigation-bars-and-storyboards"></a>Barres de navigation et les tables de montage séquentiel

Pour travailler avec des barres de Navigation dans une application Xamarin.tvOS, le plus simple consiste à ajouter à l’interface utilisateur de l’application à l’aide du concepteur iOS.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

1. Dans le **panneau solutions**, double-cliquez sur `Main.storyboard` et ouvrez-le pour le modifier.
1. Faites glisser un **barre de Navigation** à partir de la **boîte à outils** et déposez-la sur la vue en haut de l’écran : 

    [![](navigation-bars-images/navbar02.png "Une barre de Navigation")](navigation-bars-images/navbar02.png#lightbox)
1. Double-cliquez sur le **barre de Navigation** sélectionner pour **élément de Navigation**. Dans le **Widget** onglet de la **panneau Propriétés**, vous pouvez définir le **titre**: 

    [![](navigation-bars-images/navbar03.png "Définir le titre")](navigation-bars-images/navbar03.png#lightbox)
1. Ensuite, vous pouvez ajouter un ou plusieurs **éléments de bouton de barre de** à chaque extrémité de la barre : 

    [![](navigation-bars-images/navbar04.png "Un élément de bouton de la barre")](navigation-bars-images/navbar04.png#lightbox)
1. AutoEventWireup Enfin, le **éléments de bouton de barre de** à des Actions dans le **événements** onglet de la **l’Explorateur de propriétés**: 

    [![](navigation-bars-images/navbar05.png "Une barre d’Action du bouton d’élément")](navigation-bars-images/navbar05.png#lightbox)
1. Enregistrez les modifications apportées.


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)


1. Dans le **l’Explorateur de solutions**, double-cliquez sur `Main.storyboard` et ouvrez-le pour le modifier.
1. Faites glisser un **barre de Navigation** à partir de la **boîte à outils** et déposez-la sur la vue en haut de l’écran : 

    [![](navigation-bars-images/navbar02-vs.png "Une barre de Navigation")](navigation-bars-images/navbar02-vs.png#lightbox)
1. Double-cliquez sur le **barre de Navigation** sélectionner pour **élément de Navigation**. Dans le **Widget** onglet de la **l’Explorateur de propriétés**, vous pouvez définir le **titre**: 

    [![](navigation-bars-images/navbar03-vs.png "Définir le titre")](navigation-bars-images/navbar03-vs.png#lightbox)
1. Ensuite, vous pouvez ajouter un ou plusieurs **éléments de bouton de barre de** à chaque extrémité de la barre : 

    [![](navigation-bars-images/navbar04-vs.png "Une barre des éléments de bouton")](navigation-bars-images/navbar04-vs.png#lightbox)
1. AutoEventWireup Enfin, le **éléments de bouton de barre de** à des Actions dans le **événements** onglet de la **l’Explorateur de propriétés**: 

    [![](navigation-bars-images/navbar05-vs.png "Une barre de bouton élément Actions")](navigation-bars-images/navbar05-vs.png#lightbox)
1. Enregistrez les modifications apportées.


-----

> [!IMPORTANT]
> Bien qu’il soit possible d’attribuer des événements tels que `TouchUpInside` à un élément d’interface utilisateur (par exemple, un UIButton) dans le concepteur iOS, il ne sera jamais appelée car Apple TV n’a pas une pression tactile écran ou de prendre en charge les événements tactiles. Vous devez toujours utiliser le `Primary Action` événement lors de la création des éléments d’interface utilisateur de gestionnaires d’événements pour tvOS.

Le code suivant donne un exemple de gestionnaires d’événements sur trois BarButtonItems différents : `ShowFirstHotel`, `ShowSecondHotel`, et `ShowThirdHotel`. Lorsque vous cliquez sur chaque élément, l’image d’arrière-plan `HotelImage` est modifiée. Il est modifié dans le contrôleur d’affichage (exemple `ViewController.cs`) fichier :

```csharp
using System;
using Foundation;
using UIKit;

namespace MySingleView
{
    public partial class ViewController : UIViewController
    {
        #region Constructors
        public ViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();
            // Perform any additional setup after loading the view, typically from a nib.
        }

        public override void DidReceiveMemoryWarning ()
        {
            base.DidReceiveMemoryWarning ();
            // Release any cached data, images, etc that aren't in use.
        }
        #endregion

        #region Custom Actions
        partial void ShowFirstHotel (UIBarButtonItem sender) {
            // Change background image
            HotelImage.Image = UIImage.FromFile("Motel01.jpg");
        }

        partial void ShowSecondHotel (UIBarButtonItem sender) {
            // Change background image
            HotelImage.Image = UIImage.FromFile("Motel02.jpg");
        }

        partial void ShowThirdHotel (UIBarButtonItem sender) {
            // Change background image
            HotelImage.Image = UIImage.FromFile("Motel03.jpg");
        }
        #endregion
    }
}
```

Aussi longtemps que d’un bouton `Enabled` propriété est `true` et il n’est pas couvert par un autre contrôle ou une vue, elle peut être rendue de l’élément actif à l’aide de l’instance distante de Siri.

Pour plus d’informations sur l’utilisation des tables de montage séquentiel, consultez notre [Hello, tvOS Guide de démarrage rapide](~/ios/tvos/get-started/hello-tvos.md). 

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a présenté la conception et l’utilisation des barres de Navigation à l’intérieur d’une application Xamarin.tvOS.



## <a name="related-links"></a>Liens associés

- [Exemples tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS Human Interface Guides](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Guide de programmation d’application pour tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
