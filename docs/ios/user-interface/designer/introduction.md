---
title: principes fondamentaux du Concepteur d’iOS
description: Ce guide présente le Concepteur de Xamarin pour iOS. Il montre comment utiliser le concepteur iOS à organiser visuellement les contrôles, comment accéder à ces contrôles dans le code et comment modifier les propriétés.
ms.prod: xamarin
ms.assetid: E7045E41-0DEF-416B-BCDB-52502350F61C
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 01/31/2018
ms.openlocfilehash: 2c6409410ecba7df8559c07c3231f5e493c98897
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61080179"
---
# <a name="ios-designer-basics"></a>principes fondamentaux du Concepteur d’iOS

_Ce guide présente le Concepteur de Xamarin pour iOS. Il montre comment utiliser le concepteur iOS à organiser visuellement les contrôles, comment accéder à ces contrôles dans le code et comment modifier les propriétés._

Le Concepteur de Xamarin pour iOS est un concepteur d’interface visuelle similaire à Interface Builder de Xcode et le concepteur Android. Certaines de ses nombreuses fonctionnalités incluent l’intégration transparente avec Visual Studio pour Windows et Mac, faites glisser-déplacer, une interface pour la définition des gestionnaires d’événements et la capacité d’afficher des contrôles personnalisés.

## <a name="requirements"></a>Spécifications

Le concepteur iOS est disponible dans Visual Studio pour Mac et Visual Studio 2017 et version ultérieure sur Windows. Dans Visual Studio pour Windows, le concepteur iOS nécessite une connexion à un hôte de build Mac configuré correctement, même si Xcode n’a pas besoin d’exécuter.

Ce guide suppose que vous connaissez le contenu couvert dans le [guides de mise en route](~/ios/get-started/index.md).

<a name="how-it-works" />

## <a name="how-the-ios-designer-works"></a>Fonctionnement du concepteur iOS

Cette section décrit comment le concepteur iOS facilite la création d’une interface utilisateur et de se connecter au code.

Le concepteur iOS permet aux développeurs de concevoir visuellement d’interface utilisateur d’une application. Comme indiqué dans le [Introduction aux Storyboards](~/ios/user-interface/storyboards/index.md) guide, une table de montage séquentiel décrit les écrans (contrôleurs d’affichage) qui composent une application, les éléments d’interface (vues) placés sur ces contrôleurs d’affichage et de flux de navigation globale de l’application . 

Un contrôleur d’affichage comporte deux parties : une représentation visuelle dans le concepteur iOS et une classe c# associée :

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

[![Un contrôleur d’affichage dans le concepteur iOS](introduction-images/1-storyboardwithviewcontroller-vsmac.png "un contrôleur d’affichage dans le concepteur iOS")](introduction-images/1-storyboardwithviewcontroller-vsmac-large.png#lightbox)

[![Le code pour un contrôleur d’affichage](introduction-images/2-viewcontrollercode-vsmac.png "le code pour un contrôleur d’affichage")](introduction-images/2-viewcontrollercode-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Un contrôleur d’affichage dans le concepteur iOS](introduction-images/1-storyboardwithviewcontroller-vs.png "un contrôleur d’affichage dans le concepteur iOS")](introduction-images/1-storyboardwithviewcontroller-vs-large.png#lightbox)

[![Le code pour un contrôleur d’affichage](introduction-images/2-viewcontrollercode-vs.png "le code pour un contrôleur d’affichage")](introduction-images/2-viewcontrollercode-vs-large.png#lightbox)

-----

Dans son état par défaut, un contrôleur d’affichage ne fournit pas toutes les fonctionnalités ; Il doit être rempli avec des contrôles. Ces contrôles sont placés dans la vue du contrôleur d’affichage, la zone rectangulaire qui contient l’ensemble du contenu de l’écran. La plupart des contrôleurs d’affichage contient des contrôles courants tels que des boutons, des étiquettes et des champs de texte, comme illustré dans la capture d’écran suivante, qui montre un contrôleur d’affichage qui contient un bouton : 

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

[![Un contrôleur d’affichage qui contient un bouton](introduction-images/3-viewcontrollerwithbutton-vsmac.png "un contrôleur d’affichage qui contient un bouton")](introduction-images/3-viewcontrollerwithbutton-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Un contrôleur d’affichage qui contient un bouton](introduction-images/3-viewcontrollerwithbutton-vs.png "un contrôleur d’affichage qui contient un bouton")](introduction-images/3-viewcontrollerwithbutton-vs-large.png#lightbox)

-----

Certains contrôles, tels que des étiquettes contenant du texte statique, peuvent être ajoutées pour le contrôleur d’affichage et y toucher. Toutefois, plus souvent à pas, les contrôles doivent être personnalisés par programmation. Par exemple, le bouton ajouté ci-dessus doit faire quelque chose lorsque l’utilisateur appuie dessus, un gestionnaire d’événements doit être ajouté dans le code.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

Pour accéder et manipuler le bouton dans le code, il doit avoir un identificateur unique. Fournir un identificateur unique en sélectionnant le bouton, en ouvrant le **panneau Propriétés**et en définissant son **nom** champ à une valeur telle que « SubmitButton » :

[![Définition de nom d’un bouton dans le panneau Propriétés](introduction-images/4-settingbuttonname-vsmac.png "définissant les nom d’un bouton dans le panneau Propriétés")](introduction-images/4-settingbuttonname-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Pour accéder et manipuler le bouton dans le code, il doit avoir un identificateur unique. Fournir un identificateur unique en sélectionnant le bouton, en ouvrant le **fenêtre Propriétés**et en définissant son **nom** champ à une valeur telle que « SubmitButton » :

[![Définition de nom d’un bouton dans la fenêtre Propriétés](introduction-images/4-settingbuttonname-vs.png "définissant les nom d’un bouton dans la fenêtre de propriétés")](introduction-images/4-settingbuttonname-vs-large.png#lightbox)

-----

Maintenant que le bouton a un nom, il est accessible dans le code. Mais comment cela fonctionne-t-il ?

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

Dans le **panneau solutions**, la navigation vers **ViewController.cs** et en cliquant sur l’indicateur de divulgation révèle que le contrôleur d’affichage `ViewController` étendues de définition de classe deux fichiers, chacun d’eux contient un [classe partielle](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods) définition :

[![Les deux fichiers qui composent la classe ViewController : ViewController.cs et ViewController.designer.cs](introduction-images/5-twoviewcontrollerfiles-vsmac.png "les deux fichiers qui composent la classe ViewController : ViewController.cs et ViewController.designer.cs")](introduction-images/5-twoviewcontrollerfiles-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Dans le **l’Explorateur de solutions**, la navigation vers **ViewController.cs** et en cliquant sur l’indicateur de divulgation révèle que le contrôleur d’affichage `ViewController` définition de classe s’étend sur deux fichiers, chacun des qui contient un [classe partielle](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods) définition :

[![Les deux fichiers qui composent la classe ViewController : ViewController.cs et ViewController.designer.cs](introduction-images/5-twoviewcontrollerfiles-vs.png "les deux fichiers qui composent la classe ViewController : ViewController.cs et ViewController.designer.cs")](introduction-images/5-twoviewcontrollerfiles-vs-large.png#lightbox)

-----

- **ViewController.cs** doit être rempli avec un code personnalisé associé à la `ViewController` classe. Dans ce fichier, le `ViewController` classe peut répondre à iOS diverses méthodes de cycle de vie de contrôleur de vue, personnaliser l’interface utilisateur et répondre aux entrées telles qu’appuie sur le bouton de l’utilisateur.

- **ViewController.designer.cs** est un fichier généré, créé par le concepteur pour mapper l’interface construits visuellement au code iOS. Dans la mesure où les modifications apportées à ce fichier seront remplacées, il ne doit pas être modifié. Les déclarations de propriété dans ce fichier rendent possible pour le code dans le `ViewController` classe à l’accès, par **nom**, détermine l’ensemble dans le concepteur iOS. Ouverture **ViewController.designer.cs** révèle le code suivant :

```csharp
namespace Designer
{
    [Register ("ViewController")]
    partial class ViewController
    {
        [Outlet]
        [GeneratedCode ("iOS Designer", "1.0")]
        UIKit.UIButton SubmitButton { get; set; }

        void ReleaseDesignerOutlets ()
        {
            if (SubmitButton != null) {
                SubmitButton.Dispose ();
                SubmitButton = null;
            }
        }
    }
}
```

Le `SubmitButton` déclaration de propriété connecte à l’ensemble du `ViewController` classe - pas uniquement le **ViewController.designer.cs** fichier – au bouton défini dans la table de montage séquentiel. Dans la mesure où **ViewController.cs** définit la partie de la `ViewController` (classe), il a accès à `SubmitButton`.

La capture d’écran suivante illustre que IntelliSense reconnaît désormais les `SubmitButton` référencer dans **ViewController.cs**:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

[![IntelliSense reconnaissant la référence SubmitButton](introduction-images/6-submitbuttonintellisense-vsmac.png "IntelliSense reconnaissant la référence SubmitButton")](introduction-images/6-submitbuttonintellisense-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![IntelliSense reconnaissant la référence SubmitButton](introduction-images/6-submitbuttonintellisense-vs.png "IntelliSense reconnaissant la référence SubmitButton")](introduction-images/6-submitbuttonintellisense-vs-large.png#lightbox)

-----

Cette section a montré comment créer un bouton dans le concepteur iOS et accéder à ce bouton dans le code.

Le reste de ce document fournit une vue d’ensemble supplémentaire du concepteur iOS.

## <a name="ios-designer-basics"></a>principes fondamentaux du Concepteur d’iOS

Cette section présente les différentes parties du concepteur iOS et propose une présentation de ses fonctionnalités.

### <a name="launching-the-ios-designer"></a>Lancement du concepteur iOS

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

Les projets Xamarin.iOS créés avec Visual Studio pour Mac incluent un storyboard. Pour afficher le contenu d’un storyboard, double-cliquez sur le fichier .storyboard dans le **panneau solutions**:

[![Un storyboard ouvrir dans le concepteur iOS](introduction-images/7-storyboardopen-vsmac.png "un storyboard ouvrir dans le concepteur iOS")](introduction-images/7-storyboardopen-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

La plupart des projets Xamarin.iOS créées avec Visual Studio incluent un storyboard. Pour afficher le contenu d’un storyboard, double-cliquez sur le fichier .storyboard dans le **l’Explorateur de solutions**:

[![Un storyboard ouvrir dans le concepteur iOS](introduction-images/7-storyboardopen-vs.png "un storyboard ouvrir dans le concepteur iOS")](introduction-images/7-storyboardopen-vs-large.png#lightbox)

-----

<a name="iOS_Designer_features"/>

### <a name="ios-designer-features"></a>fonctionnalités du Concepteur d’iOS

Le concepteur iOS a six sections principales :

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

[![Sections du concepteur iOS](introduction-images/8-sixpartsofiosdesigner-vsmac.png "Sections du concepteur iOS")](introduction-images/8-sixpartsofiosdesigner-vsmac-large.png#lightbox)

1. **Aire de conception** : espace de travail principal du concepteur iOS. Indiqué dans la zone de document, il permet la construction visuelle d’interfaces utilisateur.
2. **Barre d’outils de contraintes** – permet de basculer entre le mode édition et mode de modification de contrainte, deux façons différentes pour positionner des éléments dans une interface utilisateur de frame.
3. **Boîte à outils** : répertorie les contrôleurs, objets, contrôles, des vues de données, modules de reconnaissance de mouvement, windows et barres qui peut être glissé sur l’aire de conception et ajouté à une interface utilisateur.
4. **Panneau Propriétés** – affiche les propriétés du contrôle sélectionné, y compris l’identité, les styles visuels, l’accessibilité, la disposition et comportement.
5. **Structure du document** – affiche l’arborescence des contrôles qui composent la disposition de l’interface en cours de modification. En cliquant sur un élément dans l’arborescence de le sélectionner dans le concepteur iOS et afficher ses propriétés dans le **panneau Propriétés**. Cela est pratique pour la sélection d’un contrôle spécifique dans une interface utilisateur profondément imbriquées.
6. **En bas de barre d’outils** – contient des options pour modifier la façon dont le concepteur iOS affiche le fichier .storyboard ou .xib, y compris les appareils, l’orientation et zoom.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Sections du concepteur iOS](introduction-images/8-sixpartsofiosdesigner-vs.png "Sections du concepteur iOS")](introduction-images/8-sixpartsofiosdesigner-vs-large.png#lightbox)

1. **Aire de conception** : espace de travail principal du concepteur iOS. Indiqué dans la zone de document, il permet la construction visuelle d’interfaces utilisateur.
2. **Barre d’outils de contraintes** – permet de basculer entre le mode édition et mode de modification de contrainte, deux façons différentes pour positionner des éléments dans une interface utilisateur de frame.
3. **Boîte à outils** : répertorie les contrôleurs, objets, contrôles, des vues de données, modules de reconnaissance de mouvement, windows et barres qui peut être glissé sur l’aire de conception et ajouté à une interface utilisateur.
4. **Fenêtre Propriétés** – affiche les propriétés du contrôle sélectionné, y compris l’identité, les styles visuels, l’accessibilité, la disposition et comportement.
5. **Structure du document** – affiche l’arborescence des contrôles qui composent la disposition de l’interface en cours de modification. En cliquant sur un élément dans l’arborescence de le sélectionner dans le concepteur iOS et afficher ses propriétés dans le **fenêtre Propriétés**. Cela est pratique pour la sélection d’un contrôle spécifique dans une interface utilisateur profondément imbriquées.
6. **En bas de barre d’outils** – contient des options pour modifier la façon dont le concepteur iOS affiche le fichier .storyboard ou .xib, y compris les appareils, l’orientation et zoom.

-----

### <a name="design-workflow"></a>Concevoir des flux de travail

#### <a name="adding-a-control-to-the-interface"></a>Ajout d’un contrôle à l’interface

Pour ajouter un contrôle à une interface, faites-le glisser à partir de la **boîte à outils** et déposez-la sur l’aire de conception. Lorsque vous ajoutez ou un contrôle de positionnement, instructions verticales et horizontales mettre en surbrillance les positions de disposition couramment utilisé comme centre vertical, horizontal center et les marges :

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)
 
![Sur l’aire de conception, les instructions mettre en surbrillance les positions de disposition couramment utilisés](introduction-images/9-layoutguides-vsmac.png "sur l’aire de conception, les instructions mettre en surbrillance les positions de disposition couramment utilisés")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Sur l’aire de conception, les instructions mettre en surbrillance les positions de disposition couramment utilisés](introduction-images/9-layoutguides-vs.png "sur l’aire de conception, les instructions mettre en surbrillance les positions de disposition couramment utilisés")

-----

La ligne bleue en pointillés dans l’exemple ci-dessus fournit des indications alignement visual Centre horizontal pour faciliter l’emplacement du bouton.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

#### <a name="context-menu-commands"></a>Commandes de menu contextuel

Un menu contextuel est disponible à la fois sur l’aire de conception et le **structure du Document**. Ce menu fournit des commandes pour le contrôle sélectionné et son parent, ce qui est utile lorsque vous travaillez avec des vues dans une hiérarchie imbriquée :

[![Le menu contextuel sur l’aire de conception](introduction-images/10-contextmenudesignsurface-vsmac.png "le menu contextuel sur l’aire de conception")](introduction-images/10-contextmenudesignsurface-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

-----

### <a name="constraints-toolbar"></a>Barre d’outils de contraintes

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)
 
[![La barre d’outils de contraintes](introduction-images/11-constraintstoolbar-vsmac.png "la barre d’outils de contraintes")](introduction-images/11-constraintstoolbar-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![La barre d’outils de contraintes](introduction-images/11-constraintstoolbar-vs.png "la barre d’outils de contraintes")](introduction-images/11-constraintstoolbar-vs-large.png#lightbox)

-----

La barre d’outils de contraintes a été mis à jour et maintenant se compose de deux contrôles : le mode d’édition de frame / contrainte modification activer/désactiver le mode et les contraintes de mise à jour / mise à jour de bouton de trames.

#### <a name="frame-editing-mode--constraint-editing-mode-toggle"></a>Mode d’édition de frame / activer/désactiver le mode de modification de contrainte

Dans les versions précédentes du concepteur iOS, en cliquant sur une vue déjà sélectionné sur l’aire de conception basculé entre le mode édition et mode de modification de contrainte de frame. À présent, un contrôle bouton bascule dans la barre d’outils de contraintes bascule entre ces modes d’édition.

- Mode de modification de frame :

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

![Frame de modification de bouton mode](introduction-images/12a-frameeditingmode-vsmac.png "bouton en mode d’édition de Frame")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Frame de modification de bouton mode](introduction-images/12a-frameeditingmode-vs.png "bouton en mode d’édition de Frame")

-----

- Mode de modification de contrainte :

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

![Bouton du mode de modification de contrainte](introduction-images/12b-constrainteditingmode-vsmac.png "bouton mode édition de contrainte")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Bouton du mode de modification de contrainte](introduction-images/12b-constrainteditingmode-vs.png "bouton mode édition de contrainte")

-----

#### <a name="update-constraints--update-frames-button"></a>Contraintes de mise à jour / mise à jour de bouton cadres

Les contraintes de mise à jour / mise à jour des frames réside de bouton à droite de l’image en mode d’édition / activer/désactiver le mode de modification de contrainte.

- Dans le cadre de mode d’édition, cliquez sur ce bouton ajuste les trames de tous les éléments sélectionnés pour correspondre à leurs contraintes.
- En mode de modification de contrainte, ce bouton s’ajuste les contraintes de tous les éléments sélectionnés pour correspondre à leurs images.

### <a name="bottom-toolbar"></a>Barre d’outils inférieure

La barre d’outils inférieure offre un moyen pour sélectionner l’appareil, l’orientation et zoom permet d’afficher un fichier storyboard ou .xib dans le concepteur iOS :

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

[![La barre d’outils inférieure, permet de sélectionner un appareil et l’orientation de l’aire de conception](introduction-images/13-bottomtoolbar-vsmac.png "la barre d’outils inférieure, permet de sélectionner un appareil et l’orientation de l’aire de conception")](introduction-images/13-bottomtoolbar-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![La barre d’outils inférieure, permet de sélectionner un appareil et l’orientation de l’aire de conception](introduction-images/13-bottomtoolbar-vs.png "la barre d’outils inférieure, permet de sélectionner un appareil et l’orientation de l’aire de conception")](introduction-images/13-bottomtoolbar-vs-large.png#lightbox)

-----

#### <a name="device-and-orientation"></a>Appareil et l’orientation

Développée, la barre d’outils du bas affiche tous les appareils, orientations et/ou adaptations applicables au document actif. En cliquant sur les modifie l’affichage sur l’aire de conception. 

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

[![La barre d’outils inférieure, développé pour afficher les appareils et les orientations](introduction-images/14-bottomtoolbarexpanded-vsmac.png "la barre d’outils inférieure, développé pour afficher les appareils et des orientations")](introduction-images/14-bottomtoolbarexpanded-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![La barre d’outils inférieure, développé pour afficher les appareils et les orientations](introduction-images/14-bottomtoolbarexpanded-vs.png "la barre d’outils inférieure, développé pour afficher les appareils et des orientations")](introduction-images/14-bottomtoolbarexpanded-vs-large.png#lightbox)

-----

Notez que la sélection d’un appareil et l’orientation modifie uniquement comment le concepteur iOS affiche un aperçu de la conception. Quelle que soit la sélection actuelle, qui vient d’être ajouté les contraintes sont appliquées dans l’ensemble des appareils et des orientations, sauf si le **modifier les caractéristiques** bouton a été utilisé pour spécifier dans le cas contraire.

Lorsque [classes de taille](~/ios/user-interface/storyboards/unified-storyboards.md#size-classes) sont [activé](~/ios/user-interface/storyboards/unified-storyboards.md#enabling-size-classes), le **modifier les caractéristiques** bouton s’affiche dans la barre d’outils du bas développé.  En cliquant sur le **modifier les caractéristiques** bouton affiche des options pour la création d’une variante d’interface basé sur la classe de taille représentée par l’appareil sélectionné et l’orientation. Prenons les exemples suivants :

- Si **système d’exploitation iPhone** / **Portrait**, est sélectionné, le menu fournit les options pour créer une variante de l’interface pour la largeur compacte, catégorie de taille de hauteur normale. 
- Si **iPad Pro 9,7"** / **paysage** / **plein écran** est sélectionnée, le menu fournit les options pour créer une variante de l’interface pour la largeur de régulière, de la catégorie de taille de hauteur normale.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

[![La barre d’outils inférieure utilisé pour faire varier d’une interface par catégorie de taille](introduction-images/15-edittraitsbutton-vsmac.png "la barre d’outils inférieure utilisé pour faire varier d’une interface par catégorie de taille")](introduction-images/15-edittraitsbutton-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![La barre d’outils inférieure utilisé pour faire varier d’une interface par catégorie de taille](introduction-images/15-edittraitsbutton-vs.png "la barre d’outils inférieure utilisé pour faire varier d’une interface par catégorie de taille")](introduction-images/15-edittraitsbutton-vs-large.png#lightbox)

-----

#### <a name="zoom-controls"></a>Contrôles de zoom

L’aire de conception prend en charge le zoom par le biais de plusieurs contrôles :

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)
 
![Les contrôles de zoom dans la barre d’outils inférieure](introduction-images/16-zoomcontrols-vsmac.png "les contrôles de zoom dans la barre d’outils inférieure")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Les contrôles de zoom dans la barre d’outils inférieure](introduction-images/16-zoomcontrols-vs.png "les contrôles de zoom dans la barre d’outils inférieure")

-----

Les contrôles sont les suivantes :

1. Zoom pour ajuster
2. Zoom arrière
3. Zoom avant
4. Taille réelle (la taille en pixels de 1:1)

Ces contrôles ajustent le zoom sur l’aire de conception. Elles n’affectent pas l’interface utilisateur de l’application lors de l’exécution.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

### <a name="properties-pad"></a>Panneau Propriétés

Utilisez le **panneau Propriétés** pour modifier l’identité, les styles visuels, l’accessibilité et le comportement d’un contrôle. La capture d’écran suivante illustre le **panneau Propriétés** options pour un bouton :

[![Le panneau Propriétés pour un bouton](introduction-images/17-buttonpropertiespad-vsmac.png "le panneau Propriétés pour un bouton")](introduction-images/17-buttonpropertiespad-vsmac-large.png#lightbox)
#### <a name="properties-pad-sections"></a>Sections du panneau Propriétés

Le **panneau Propriétés** contient trois sections :

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

### <a name="properties-window"></a>Propriétés (fenêtre)

Utilisez le **fenêtre Propriétés** pour modifier l’identité, les styles visuels, l’accessibilité et le comportement d’un contrôle. La capture d’écran suivante illustre le **fenêtre Propriétés** options pour un bouton :

[![La fenêtre Propriétés pour un bouton](introduction-images/17-buttonpropertieswindow-vs.png "la fenêtre de propriétés pour un bouton")](introduction-images/17-buttonpropertieswindow-vs-large.png#lightbox)

#### <a name="properties-window-sections"></a>Sections de fenêtre de propriétés

Le **fenêtre Propriétés** contient trois sections :

-----

1.  **Widget** – propriétés principales du contrôle, telles que le nom, les classes, les propriétés de style, etc. Propriétés de gestion du contenu du contrôle sont généralement placées ici.
2.  **Disposition** – propriétés qui effectuer le suivi de la position et la taille du contrôle, y compris les contraintes et les cadres, sont répertoriées ici.
3.  **Événements** : événements et gestionnaires d’événements spécifiés ici. Utile pour gérer les événements tactiles, appuyez sur, faites glisser, etc. Événements peuvent également être gérés directement dans le code.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

#### <a name="editing-properties-in-the-properties-pad"></a>Modification des propriétés dans le panneau Propriétés

En plus de la modification visuelle sur l’aire de conception, le concepteur iOS prend en charge la modification des propriétés dans le **panneau Propriétés**. La modification de propriétés disponibles en fonction du contrôle sélectionné, comme illustré par les captures d’écran ci-dessous :

[![Bouton Propriétés](introduction-images/18a-buttonpropertiespad-vsmac.png "bouton Propriétés")](introduction-images/18a-buttonpropertiespad-vsmac-large.png#lightbox)

[![Afficher les propriétés du contrôleur](introduction-images/18b-viewcontrollerpropertiespad-vsmac.png "afficher les propriétés du contrôleur")](introduction-images/18b-viewcontrollerpropertiespad-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

#### <a name="editing-properties-in-the-properties-window"></a>Modification des propriétés dans la fenêtre Propriétés

En plus de la modification visuelle sur l’aire de conception, le concepteur iOS prend en charge la modification des propriétés dans le **fenêtre Propriétés**. La modification de propriétés disponibles en fonction du contrôle sélectionné, comme illustré par les captures d’écran ci-dessous :

[![Bouton Propriétés](introduction-images/18a-buttonpropertieswindow-vs.png "bouton Propriétés")](introduction-images/18a-buttonpropertieswindow-vs-large.png#lightbox)

[![Afficher les propriétés du contrôleur](introduction-images/18b-viewcontrollerpropertieswindow-vs.png "afficher les propriétés du contrôleur")](introduction-images/18b-viewcontrollerpropertieswindow-vs-large.png#lightbox)

-----

> [!IMPORTANT]
> La section de l’identité de l’affiche désormais panneau Propriétés un **Module** champ. Il est nécessaire pour remplir cette section uniquement lorsqu’il interagit avec les classes Swift. Il permet d’entrer un nom de module pour les classes Swift, qui sont à espace de noms.

#### <a name="default-values"></a>Valeurs par défaut

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

De nombreuses propriétés dans le **panneau Propriétés** afficher aucune valeur ou une valeur par défaut. Toutefois, le code de l’application peut toujours modifier ces valeurs. Le **panneau Propriétés** n’affiche pas les valeurs définies dans le code.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

De nombreuses propriétés dans le **fenêtre Propriétés** afficher aucune valeur ou une valeur par défaut. Toutefois, le code de l’application peut toujours modifier ces valeurs. Le **fenêtre Propriétés** n’affiche pas les valeurs définies dans le code.

-----

#### <a name="event-handlers"></a>Gestionnaires d’événements

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

Pour spécifier des gestionnaires d’événements personnalisés pour divers événements, utilisez le **événements** onglet de la **panneau Propriétés**. Par exemple, dans la capture d’écran ci-dessous, un `HandleClick` méthode gère le bouton **Touch dans** événement :

[![Le panneau Propriétés, avec un gestionnaire d’événements pour un bouton](introduction-images/19-buttonpropertiespadevents-vsmac.png "le panneau Propriétés, avec un gestionnaire d’événements pour un bouton")](introduction-images/19-buttonpropertiespadevents-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Pour spécifier des gestionnaires d’événements personnalisés pour divers événements, utilisez le **événements** onglet de la **fenêtre Propriétés**. Par exemple, dans la capture d’écran ci-dessous, un `HandleClick` méthode gère le bouton **Touch dans** événement :

[![La fenêtre Propriétés, avec un gestionnaire d’événements pour un bouton](introduction-images/19-buttonpropertieswindowevents-vs.png "la fenêtre Propriétés, avec un gestionnaire d’événements pour un bouton")](introduction-images/19-buttonpropertieswindowevents-vs-large.png#lightbox)

-----

Une fois qu’un gestionnaire d’événements a été spécifié, une méthode portant le même nom doit être ajoutée à la classe de contrôleur d’affichage correspondant. Sinon, un `unrecognized selector` exception se produit lorsque l’utilisateur appuie sur le bouton :

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

[![Une exception non reconnu sélecteur](introduction-images/20-unrecognizedselector-vsmac.png "une exception de sélecteur non reconnu")](introduction-images/20-unrecognizedselector-vsmac-large.png#lightbox)

Notez que, après un gestionnaire d’événements a été spécifié dans le **panneau Propriétés**, le concepteur iOS sera immédiatement ouvrir le fichier de code correspondant et offrir à insérer la déclaration de méthode. 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Une exception non reconnu sélecteur](introduction-images/20-unrecognizedselector-vs.png "une exception de sélecteur non reconnu")](introduction-images/20-unrecognizedselector-vs-large.png#lightbox)

-----

Pour obtenir un exemple qui utilise des gestionnaires d’événements personnalisés, reportez-vous à la [Hello, iOS Getting Started Guide](~/ios/get-started/hello-ios/index.md).

### <a name="outline-view"></a>Mode plan

Le concepteur iOS peut également afficher la hiérarchie d’une interface de contrôles sous forme de plan. Le contour est disponible en sélectionnant le **structure du Document** onglet, comme indiqué ci-dessous :

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

[![La structure du Document](introduction-images/21-buttonoutlineview-vsmac.png "la structure du Document")](introduction-images/21-buttonoutlineview-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![La structure du Document](introduction-images/21-buttonoutlineview-vs.png "la structure du Document")](introduction-images/21-buttonoutlineview-vs-large.png#lightbox)

-----

Le contrôle sélectionné en mode plan reste synchronisé avec le contrôle sélectionné sur l’aire de conception.  Cette fonctionnalité est utile pour sélectionner un élément dans une hiérarchie d’interface profondément imbriquées.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

## <a name="revert-to-xcode"></a>Revenir à Xcode

Il est possible d’utiliser le concepteur iOS et Xcode Interface Builder de manière interchangeable. Pour ouvrir une table de montage séquentiel ou un fichier .xib dans Xcode Interface Builder, cliquez sur le fichier et sélectionnez **ouvrir avec > Xcode Interface Builder**, comme illustré par la capture d’écran ci-dessous :

[![Ouverture d’un storyboard dans Xcode Interface Builder](introduction-images/22-openinxcodeinterfacebuilder-vsmac.png "ouverture d’un storyboard dans Xcode Interface Builder")](introduction-images/22-openinxcodeinterfacebuilder-vsmac-large.png#lightbox)

Après avoir apporté des modifications dans Xcode Interface Builder, enregistrez le fichier et revenir à Visual Studio pour Mac. Les modifications sont synchronisées au projet Xamarin.iOS.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="revert-to-xcode"></a>Revenir à Xcode

Il est possible d’utiliser le concepteur iOS et Xcode Interface Builder indifféremment, mais Xcode Interface Builder est uniquement disponible sur Mac. Pour ouvrir un fichier de storyboard ou .xib dans Xcode Interface Builder sur un Mac, ouvrez la solution qui contient le projet Xamarin.iOS dans [Visual Studio pour Mac](/visualstudio/mac/), avec le bouton droit sur le fichier et sélectionnez **ouvrir avec > Xcode Interface Générateur**, comme illustré par la capture d’écran ci-dessous :

[![Ouverture d’un storyboard dans Xcode Interface Builder](introduction-images/22-openinxcodeinterfacebuilder-vsmac.png "ouverture d’un storyboard dans Xcode Interface Builder")](introduction-images/22-openinxcodeinterfacebuilder-vsmac-large.png#lightbox)

Après avoir apporté des modifications dans Xcode Interface Builder, enregistrez le fichier et revenir à Visual Studio pour Mac. Les modifications sont synchronisées au projet Xamarin.iOS.

-----

## <a name="xib-support"></a>prise en charge des fichiers .xib

Le concepteur iOS prend en charge la création, modification et la gestion des fichiers .xib. Ceux-ci sont des fichiers XML que représenter une unique et personnalisé les vues qui peuvent être ajoutés à la hiérarchie d’affichage d’une application. Un fichier .xib représente généralement l’interface pour un affichage unique ou un écran dans une application, tandis qu’une table de montage séquentiel représente de nombreux écrans et les transitions entre eux.

Il existe de nombreuses opinions sur les solutions – fichiers .xib, tables de montage séquentiel ou code – convient le mieux pour créer et maintenir une interface utilisateur. En réalité, il n’existe aucune solution parfaite, et il est toujours intéressant d’examiner le meilleur outil pour le travail en cours. Ceci dit, les fichiers .xib sont généralement plus puissants lorsqu’il est utilisé pour représenter une vue personnalisée nécessaire à plusieurs endroits dans une application, comme une cellule d’affichage personnalisé de table. 

Vous trouverez plus d’informations sur l’utilisation de fichiers .xib dans les recettes suivantes :

- [À l’aide du modèle d’affichage de fichiers .xib](https://github.com/xamarin/recipes/tree/master/Recipes/ios/general/templates/using_the_ios_view_xib_template)
- [Création d’un TableViewCell personnalisé à l’aide d’un .xib](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/tables/custom-tableviewcell)
- [Création d’un écran de lancement à l’aide d’un .xib](https://github.com/xamarin/recipes/tree/master/Recipes/ios/general/templates/launchscreen-xib)

Pour plus d’informations concernant l’utilisation des animations, reportez-vous à la [Introduction aux Storyboards](~/ios/user-interface/storyboards/index.md).

Cela et autres guides liés au concepteur iOS font référence à l’utilisation des animations en tant que l’approche standard pour la création d’interfaces utilisateur, étant donné que la plupart des modèles de projet de nouveau Xamarin.iOS fournissent une table de montage par défaut.

## <a name="summary"></a>Récapitulatif

Ce guide a présenté le concepteur, qui décrit ses fonctionnalités et de mise en relief les outils qu’il propose pour concevoir de superbes interfaces utilisateurs iOS.

## <a name="related-links"></a>Liens associés

- [Introduction aux storyboards](~/ios/user-interface/storyboards/index.md)
- [procédure pas à pas de contrôles concevable d’iOS](~/ios/user-interface/designer/ios-designable-controls-walkthrough.md)
- [Hello, iOS](~/ios/get-started/hello-ios/index.md)
- [Hello, iOS Multiscreen](~/ios/get-started/hello-ios-multiscreen/index.md)
- [Vue d’ensemble du concepteur Android](~/android/user-interface/android-designer/index.md)
- [Classes et méthodes partielles](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods)
- [Vous plonger dans le Concepteur de Xamarin pour iOS - 2014 à évoluer (vidéo)](https://www.youtube.com/watch?v=W4H9uLjoEjM)
- [À l’aide du concepteur iOS pour créer un écran de lancement (vidéo)](https://university.xamarin.com/lightninglectures/using-the-ios-designer-to-create-a-launch-screen)
