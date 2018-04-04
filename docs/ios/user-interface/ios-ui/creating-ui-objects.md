---
title: Création d’objets d’Interface utilisateur
ms.prod: xamarin
ms.assetid: 4D6B136C-744A-4936-8655-A77E62BA7A60
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 5229beb7a882049daf58d3a3e62da6fed25a1f48
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="creating-user-interface-objects"></a>Création d’objets d’Interface utilisateur

Apple regroupe connexes de fonctionnalités dans « infrastructures », qui sont équivalentes aux espaces de noms Xamarin.iOS. `UIKit` est l’espace de noms qui contient tous les contrôles d’interface utilisateur pour iOS.

Chaque fois que votre code doit faire référence à un contrôle d’interface utilisateur, tel qu’une étiquette ou un bouton, pensez à inclure les éléments suivants à l’aide d’instruction :

```csharp
using UIKit;
```


Tous les contrôles décrits dans ce chapitre sont dans l’espace de noms UIKit, et chaque nom de classe du contrôle utilisateur a le `UI` préfixe.

Vous pouvez modifier les contrôles d’interface utilisateur et des dispositions de trois façons :

-  **[Xamarin iOS concepteur](~/ios/user-interface/designer/index.md)**  : Concepteur de disposition prédéfinie utilisez Xamarin pour concevoir des écrans. Double-cliquez sur storyboard ou fichiers XIB modifier avec le concepteur intégré.
-  **Xcode Interface Builder** : faites glisser des contrôles sur vos dispositions d’écran avec le Générateur de l’Interface. Ouvrez le plan conceptuel ou XIB dans Xcode en double-cliquant sur le fichier dans le **Solution remplissage** et en choisissant **ouvrir avec > Xcode Interface Builder**.
-  **À l’aide de c#** – contrôles peuvent également être par programmation construites avec le code et ajoutés à la hiérarchie de la vue.

Nouveaux fichiers de plan conceptuel et XIB peuvent être ajoutés en cliquant sur un projet iOS et en choisissant **Ajouter > nouveau fichier...** .

Quelle que soit la méthode que vous utilisez, les propriétés de contrôle et les événements peuvent être manipulés avec c# dans votre logique d’application.

## <a name="using-xamarin-ios-designer"></a>À l’aide de Xamarin iOS Concepteur

Pour commencer à créer votre interface utilisateur dans le concepteur iOS, double-cliquez sur un fichier d’animation. Faire glisser les contrôles sur l’aire de conception à partir de la **boîte à outils** comme illustré ci-dessous :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

 [![](creating-ui-objects-images/image2b.png "Remplissage de la boîte à outils")](creating-ui-objects-images/image2b.png#lightbox)
 
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

 [![](creating-ui-objects-images/image2b-vs.png "Remplissage de la boîte à outils - Visual Studio")](creating-ui-objects-images/image2b.png#lightbox)
 
-----

Quand un contrôle est sélectionné sur l’aire de conception du **propriétés remplissage** affichent les attributs de ce contrôle. Le **Widget > identité > nom** champ, qui est chargé dans la capture d’écran ci-dessous, est utilisé en tant que le *prise* nom. Voici comment vous pouvez référencer le contrôle en c# :

 [![](creating-ui-objects-images/image3b.png "Remplissage de Widget de propriétés")](creating-ui-objects-images/image3b.png#lightbox)

Pour approfondir l’aide du concepteur iOS, reportez-vous à la [présentation dans le concepteur iOS](~/ios/user-interface/designer/introduction.md) guide.

## <a name="using-xcode-interface-builder"></a>À l’aide de Xcode Interface Builder

Si vous n’êtes pas familiarisé avec l’utilisation du générateur Interface, reportez-vous à Apple [Interface Générateur](https://developer.apple.com/xcode/interface-builder/) documents.

Pour ouvrir un plan conceptuel dans Xcode, droit pour accéder au menu contextuel pour le fichier d’animation et choisissez Ouvrir avec le **Xcode Interface Builder**:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

 [![](creating-ui-objects-images/imagexcode.png "Menu contextuel de table de montage séquentiel : Xcode")](creating-ui-objects-images/imagexcode.png#lightbox)
 
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](creating-ui-objects-images/imagexcode-vs.png "Menu contextuel de table de montage séquentiel : Xcode")](creating-ui-objects-images/imagexcode-vs.png#lightbox)

-----

Faire glisser les contrôles sur l’aire de conception à partir de la **bibliothèque d’objets** illustré ci-dessous :

 [![](creating-ui-objects-images/image5a.png "Bibliothèque d’objets de Xcode")](creating-ui-objects-images/image5a.png#lightbox)

Lorsque vous concevez votre interface utilisateur avec le Générateur de l’Interface que vous devez créer un **prise** pour chaque contrôle que vous voulez référencer dans c#. Cela est effectué en activant le **Assistant éditeur** à l’aide du centre de **éditeur** bouton sur le bouton de barre d’outils Xcode :

 [![](creating-ui-objects-images/image6a.png "Bouton de l’éditeur de l’Assistant")](creating-ui-objects-images/image6a.png#lightbox)

Cliquez sur un objet d’interface utilisateur ; puis **faites glisser le contrôle** dans le fichier .h. Pour ** contrôle glisser **, maintenez enfoncée la touche CTRL puis cliquez sur l’objet d’interface utilisateur que vous créez la sortie (ou action) pour. Maintenez la touche CTRL enfoncée pendant que vous faites glisser dans le fichier d’en-tête. Terminer en faisant glisser sous le `@interface` définition. Une ligne bleue doit apparaître avec une légende insérer la sortie ou une Collection de sortie, comme illustré dans la capture d’écran ci-dessous.

Lorsque vous relâchez la, cliquez sur, vous devrez fournir un nom pour la sortie, qui sera utilisé pour créer une propriété c# qui peut être référencée dans le code :

 [![](creating-ui-objects-images/image8a.png "Création d’une prise de courant")](creating-ui-objects-images/image8a.png#lightbox)

Pour plus d’informations sur comment le Générateur de Xcode Interface s’intègre avec Visual Studio pour Mac, reportez-vous à la [génération de Code Xib](~/ios/internals/xib-code-generation.md#generated) document.

##  <a name="using-c"></a>À l’aide de c#

Si vous décidez de créer par programmation un objet d’interface utilisateur à l’aide de c# (dans une vue ou d’un contrôleur de vue, par exemple), procédez comme suit :

-  Déclarez un champ de niveau classe pour l’objet d’interface utilisateur. Créer le contrôle lui-même dans une seule fois, `ViewDidLoad` par exemple. L’objet peut ensuite être référencé dans les méthodes de cycle de vie du contrôleur de vue (par exemple).
`ViewWillAppear`).
-  Créer un `CGRect` qui définit le cadre du contrôle (ses coordonnées X et Y sur l’écran, ainsi que sa largeur et sa hauteur). Vous devez vous assurer que vous avez un `using CoreGraphics` cette directive.
-  Appelez le constructeur pour créer et affecter le contrôle.
-  Définir des propriétés ou des gestionnaires d’événements.
-  Appelez `Add()` pour ajouter le contrôle à la hiérarchie.

Voici un exemple simple de création d’un `UILabel` dans un contrôleur de la vue à l’aide de c# :

```csharp
UILabel label1;
public override void ViewDidLoad () {
    base.ViewDidLoad ();
    var frame = new CGRect(10, 10, 300, 30);
    label1 = new UILabel(frame);
    label1.Text = "New Label";
    View.Add (label1);
}
```

<a name="partial_classes" />

## <a name="using-c-and-storyboards"></a>À l’aide de c# et les animations

Lors de l’affichage des contrôleurs sont ajoutés à l’aire de conception, deux c# fichiers correspondants sont créés dans le projet. Dans cet exemple, `ControlsViewController.cs` et `ControlsViewController.designer.cs` ont été créés automatiquement :

 [![](creating-ui-objects-images/image9b.png "Classe partielle de ViewController")](creating-ui-objects-images/image9b.png#lightbox)

Le `MainViewController.cs` fichier est conçu pour *votre code*. C’est là la `View` du cycle de vie des méthodes telles que `ViewDidLoad` et `ViewWillAppear` sont implémentées et où vous pouvez ajouter vos propres propriétés, les champs et les méthodes.

Le `ControlsViewController.designer.cs` est le code généré qui contient une classe partielle. Lorsque vous nommez un contrôle de la conception de surface dans Visual Studio pour Mac, ou créer une sortie ou une action dans Xcode, une propriété correspondante ou une méthode partielle, est ajouté dans le fichier de concepteur (designer.cs). Le code suivant montre un exemple de code généré pour les deux boutons et une vue de texte, où un des boutons a également un `TouchUpInside` événement.

Ces éléments de la classe partielle permettent à votre code référencer les contrôles et répondre aux actions qui sont déclarées sur l’aire de conception :

```csharp
[Register ("ControlsViewController")]
    partial class ControlsViewController
    {
        [Outlet]
        [GeneratedCodeAttribute ("iOS Designer", "1.0")]
        UIKit.UIButton Button1 { get; set; }

        [Outlet]
        [GeneratedCodeAttribute ("iOS Designer", "1.0")]
        UIKit.UIButton Button2 { get; set; }

        [Outlet]
        [GeneratedCodeAttribute ("iOS Designer", "1.0")]
        UIKit.UITextField textfield1 { get; set; }

        [Action ("button2_TouchUpInside:")]
        [GeneratedCodeAttribute ("iOS Designer", "1.0")]
        partial void button2_TouchUpInside (UIButton sender);

        void ReleaseDesignerOutlets ()
        {
            if (Button1 != null) {
                Button1.Dispose ();
                Button1 = null;
            }
            if (Button2 != null) {
                Button2.Dispose ();
                Button2 = null;
            }
            if (textfield1 != null) {
                textfield1.Dispose ();
                textfield1 = null;
            }
        }
    }
}
```

Le `designer.cs` fichier ne doit pas être modifié manuellement, l’IDE (Visual Studio pour Mac ou Visual Studio) est chargé de maintenir synchronisés avec le plan conceptuel.

Lorsque des objets d’interface utilisateur sont ajoutés par programme à un `View` ou `ViewController`, instancier et de gérer les références d’objet vous-même et par conséquent, aucun fichier de concepteur est nécessaire.



## <a name="related-links"></a>Liens associés

- [Contrôles (exemple)](https://developer.xamarin.com/samples/Controls/)
