---
title: "principes de base de concepteur d’iOS"
description: "Ce guide présente le Concepteur de Xamarin pour iOS. Il montre comment utiliser le concepteur iOS à visuellement disposer les contrôles, comment accéder à ces contrôles dans le code et comment modifier les propriétés."
ms.topic: article
ms.prod: xamarin
ms.assetid: E7045E41-0DEF-416B-BCDB-52502350F61C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/31/2018
ms.openlocfilehash: 3046d779239076098a8b2fb74fc87e2f211074e9
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="ios-designer-basics"></a>principes de base de concepteur d’iOS

_Ce guide présente le Concepteur de Xamarin pour iOS. Il montre comment utiliser le concepteur iOS à visuellement disposer les contrôles, comment accéder à ces contrôles dans le code et comment modifier les propriétés._

Le Concepteur de Xamarin pour iOS est un concepteur d’interface visuelle semblable au constructeur d’Interface de Xcode et le concepteur Android. Certaines de ses nombreuses fonctionnalités incluent l’intégration transparente avec Visual Studio pour Mac et Visual Studio 2015 et 2017, glisser-déplacer, une interface pour le paramétrage des gestionnaires d’événements et la possibilité d’effectuer le rendu des contrôles personnalisés.

## <a name="requirements"></a>Configuration requise

IOS concepteur est disponible dans Visual Studio pour Mac et dans Visual Studio 2015 et 2017 sur Windows. Dans Visual Studio 2015 ou 2017, le concepteur iOS requiert une connexion à un hôte de build Mac correctement configuré, si Xcode n’a pas besoin d’exécuter.

Ce guide suppose que vous connaissez le contenu traitées dans le [Getting Started Guide](~/ios/get-started/index.md).

<a name="how-it-works" />

## <a name="how-the-ios-designer-works"></a>Fonctionnement du concepteur iOS

Cette section décrit comment le concepteur iOS facilite la création d’une interface utilisateur et de sa connexion au code.

Le concepteur iOS permet aux développeurs de concevoir visuellement d’interface utilisateur d’une application. Comme indiqué dans le [Introduction à des plans conceptuels](~/ios/user-interface/storyboards/index.md) guide, une table de montage séquentiel décrit les écrans (affichage des contrôleurs) qui composent une application, les éléments d’interface (vues) placés sur les contrôleurs de la vue et de flux de navigation globale de l’application . 

Un contrôleur d’affichage comporte deux parties : une représentation visuelle dans le concepteur iOS et une classe c# associée :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Un contrôleur de vue dans le concepteur iOS](introduction-images/1-storyboardwithviewcontroller-vsmac.png "un contrôleur de vue dans le concepteur iOS")](introduction-images/1-storyboardwithviewcontroller-vsmac-large.png)

[![Le code d’un contrôleur de vue](introduction-images/2-viewcontrollercode-vsmac.png "le code d’un contrôleur de vue")](introduction-images/2-viewcontrollercode-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Un contrôleur de vue dans le concepteur iOS](introduction-images/1-storyboardwithviewcontroller-vs.png "un contrôleur de vue dans le concepteur iOS")](introduction-images/1-storyboardwithviewcontroller-vs-large.png)

[![Le code d’un contrôleur de vue](introduction-images/2-viewcontrollercode-vs.png "le code d’un contrôleur de vue")](introduction-images/2-viewcontrollercode-vs-large.png)

-----

Dans son état par défaut, un contrôleur de la vue ne fournit aucune fonctionnalité ; Il doit être rempli avec des contrôles. Ces contrôles sont placés dans la vue du contrôleur de la vue, la zone rectangulaire qui contient l’ensemble du contenu de l’écran. La plupart des contrôleurs de la vue contient des contrôles communs tels que des boutons, des étiquettes et des champs de texte, comme illustré dans la capture d’écran suivante, qui montre un contrôleur de vue contenant un bouton : 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Un contrôleur de vue contenant un bouton](introduction-images/3-viewcontrollerwithbutton-vsmac.png "un contrôleur de vue contenant un bouton")](introduction-images/3-viewcontrollerwithbutton-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Un contrôleur de vue contenant un bouton](introduction-images/3-viewcontrollerwithbutton-vs.png "un contrôleur de vue contenant un bouton")](introduction-images/3-viewcontrollerwithbutton-vs-large.png)

-----

Certains contrôles, tels que les étiquettes contenant du texte statique, peuvent être ajoutés au contrôleur de vue et seul. Toutefois, plus souvent non, les contrôles doivent être personnalisés par programme. Par exemple, le bouton ajouté ci-dessus doit faire quelque chose lorsque tapées, un gestionnaire d’événements doit être ajouté dans le code.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Pour accéder et manipuler le bouton dans le code, il doit avoir un identificateur unique. Fournir un identificateur unique en sélectionnant le bouton, ouvrant la **propriétés remplissage**et en définissant son **nom** champ à une valeur telle que « SubmitButton » :

[![Paramètre de nom d’un bouton dans la zone de propriétés](introduction-images/4-settingbuttonname-vsmac.png "définissant le nom d’un bouton dans la zone de propriétés")](introduction-images/4-settingbuttonname-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Pour accéder et manipuler le bouton dans le code, il doit avoir un identificateur unique. Fournir un identificateur unique en sélectionnant le bouton, ouvrant la **fenêtre Propriétés**et en définissant son **nom** champ à une valeur telle que « SubmitButton » :

[![Définition du nom d’un bouton dans la fenêtre Propriétés](introduction-images/4-settingbuttonname-vs.png "définissant le nom d’un bouton dans la fenêtre Propriétés")](introduction-images/4-settingbuttonname-vs-large.png)

-----

Maintenant que le bouton a un nom, il est accessible dans le code. Mais comment cela fonctionne-t-il ?

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Dans le **Solution remplissage**, la navigation vers **ViewController.cs** et en cliquant sur l’indicateur de divulgation d’informations indique que le contrôleur de vue `ViewController` étendues de définition de classe deux fichiers, chacun d’eux contient un [classe partielle](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods) définition :

[![Les deux fichiers qui composent la classe ViewController : ViewController.cs et ViewController.designer.cs](introduction-images/5-twoviewcontrollerfiles-vsmac.png "les deux fichiers qui composent la classe ViewController : ViewController.cs et ViewController.designer.cs")](introduction-images/5-twoviewcontrollerfiles-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Dans le **l’Explorateur de solutions**, la navigation vers **ViewController.cs** et en cliquant sur l’indicateur de divulgation d’informations indique que le contrôleur de vue `ViewController` définition de classe s’étend sur deux fichiers, chacun des qui contient un [classe partielle](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods) définition :

[![Les deux fichiers qui composent la classe ViewController : ViewController.cs et ViewController.designer.cs](introduction-images/5-twoviewcontrollerfiles-vs.png "les deux fichiers qui composent la classe ViewController : ViewController.cs et ViewController.designer.cs")](introduction-images/5-twoviewcontrollerfiles-vs-large.png)

-----

- **ViewController.cs** doit être rempli avec du code personnalisé lié à la `ViewController` classe. Dans ce fichier, la `ViewController` classe peut répondre à diverses iOS afficher les méthodes de cycle de vie du contrôleur, personnaliser l’interface utilisateur et répondre à l’utilisateur d’entrée tels qu’appuie sur le bouton.

- **ViewController.designer.cs** est un fichier généré, créé par le concepteur pour mapper l’interface visuellement construite à du code iOS. Étant donné que les modifications apportées à ce fichier seront remplacées, il ne doit pas être modifié. Déclarations de propriétés dans ce fichier de rendent possible pour le code dans le `ViewController` classe à l’accès par **nom**, contrôle le jeu dans le concepteur iOS. Ouverture **ViewController.designer.cs** révèle le code suivant :

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

Le `SubmitButton` déclaration de propriété connecte à l’ensemble du `ViewController` classe - pas uniquement le **ViewController.designer.cs** fichier – au bouton défini dans le plan conceptuel. Étant donné que **ViewController.cs** définit la partie de la `ViewController` (classe), il a accès à `SubmitButton`.

La capture d’écran suivante illustre que IntelliSense reconnaît maintenant la `SubmitButton` référencer dans **ViewController.cs**:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![IntelliSense reconnaître la référence SubmitButton](introduction-images/6-submitbuttonintellisense-vsmac.png "IntelliSense reconnaître la référence SubmitButton")](introduction-images/6-submitbuttonintellisense-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![IntelliSense reconnaître la référence SubmitButton](introduction-images/6-submitbuttonintellisense-vs.png "IntelliSense reconnaître la référence SubmitButton")](introduction-images/6-submitbuttonintellisense-vs-large.png)

-----

Cette section a montré comment créer un bouton dans le concepteur iOS et accéder à ce bouton dans le code.

Le reste de ce document fournit une autre vue d’ensemble du concepteur iOS.

## <a name="ios-designer-basics"></a>principes de base de concepteur d’iOS

Cette section présente les différentes parties du concepteur iOS et fournit une visite guidée de ses fonctionnalités.

### <a name="launching-the-ios-designer"></a>Lancer le concepteur iOS

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Les projets de Xamarin.iOS créées avec Visual Studio pour Mac incluent un plan conceptuel. Pour afficher le contenu d’un storyboard, double-cliquez sur le fichier .storyboard le **Solution remplissage**:

[![Un plan conceptuel ouvrir dans le concepteur iOS](introduction-images/7-storyboardopen-vsmac.png "un storyboard ouvrir dans le concepteur iOS")](introduction-images/7-storyboardopen-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

La plupart des projets de Xamarin.iOS créés avec Visual Studio 2015 ou 2017 incluent un plan conceptuel. Pour afficher le contenu d’un storyboard, double-cliquez sur le fichier .storyboard le **l’Explorateur de solutions**:

[![Un plan conceptuel ouvrir dans le concepteur iOS](introduction-images/7-storyboardopen-vs.png "un storyboard ouvrir dans le concepteur iOS")](introduction-images/7-storyboardopen-vs-large.png)

-----

<a name="iOS_Designer_features"/>

### <a name="ios-designer-features"></a>fonctionnalités du Concepteur d’iOS

Le concepteur iOS a six sections principales :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Sections du concepteur iOS](introduction-images/8-sixpartsofiosdesigner-vsmac.png "Sections du concepteur iOS")](introduction-images/8-sixpartsofiosdesigner-vsmac-large.png)

1. **Aire de conception** : espace de travail principal du concepteur iOS. Indiqué dans la zone de document, il permet la construction visuelle des interfaces utilisateur.
2. **Barre d’outils de contraintes** – permet de basculer entre le mode édition et mode de modification de contrainte, deux façons différentes pour positionner des éléments dans une interface utilisateur de frame.
3. **Boîte à outils** : répertorie les contrôleurs, objets, contrôles, des vues de données, les reconnaissances de mouvement, windows et des barres qui peut être déplacé vers l’aire de conception et ajouté à l’interface utilisateur.
4. **Remplissage des propriétés** – affiche les propriétés du contrôle sélectionné, y compris l’identité, les styles visuels, l’accessibilité, disposition et comportement.
5. **Structure du document** – affiche l’arborescence des contrôles qui composent la disposition de l’interface en cours de modification. En cliquant sur un élément dans l’arborescence pour le sélectionner dans le concepteur iOS et affiche ses propriétés dans le **propriétés remplissage**. Cela est pratique pour la sélection d’un contrôle spécifique dans une interface utilisateur de profondément imbriquées.
6. **Barre d’outils en bas** – contient des options permettant de modifier la façon dont le concepteur iOS affiche le fichier .storyboard ou .xib, y compris les appareils, l’orientation et zoom.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Sections du concepteur iOS](introduction-images/8-sixpartsofiosdesigner-vs.png "Sections du concepteur iOS")](introduction-images/8-sixpartsofiosdesigner-vs-large.png)

1. **Aire de conception** : espace de travail principal du concepteur iOS. Indiqué dans la zone de document, il permet la construction visuelle des interfaces utilisateur.
2. **Barre d’outils de contraintes** – permet de basculer entre le mode édition et mode de modification de contrainte, deux façons différentes pour positionner des éléments dans une interface utilisateur de frame.
3. **Boîte à outils** : répertorie les contrôleurs, objets, contrôles, des vues de données, les reconnaissances de mouvement, windows et des barres qui peut être déplacé vers l’aire de conception et ajouté à l’interface utilisateur.
4. **Fenêtre Propriétés** – affiche les propriétés du contrôle sélectionné, y compris l’identité, les styles visuels, l’accessibilité, disposition et comportement.
5. **Structure du document** – affiche l’arborescence des contrôles qui composent la disposition de l’interface en cours de modification. En cliquant sur un élément dans l’arborescence pour le sélectionner dans le concepteur iOS et affiche ses propriétés dans le **fenêtre Propriétés**. Cela est pratique pour la sélection d’un contrôle spécifique dans une interface utilisateur de profondément imbriquées.
6. **Barre d’outils en bas** – contient des options permettant de modifier la façon dont le concepteur iOS affiche le fichier .storyboard ou .xib, y compris les appareils, l’orientation et zoom.

-----

### <a name="design-workflow"></a>Concevoir des flux de travail

#### <a name="adding-a-control-to-the-interface"></a>Ajout d’un contrôle à l’interface

Pour ajouter un contrôle à une interface, faites-la glisser à partir de la **boîte à outils** et déposez-la sur l’aire de conception. Lors de l’ajout ou le positionnement d’un contrôle, instructions verticales et horizontales mettre en surbrillance les positions couramment utilisé la mise en page telles que le centre vertical, Centre horizontal et les marges :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)
 
![Sur l’aire de conception, des recommandations mettre en surbrillance les positions couramment utilisé la mise en page](introduction-images/9-layoutguides-vsmac.png "sur l’aire de conception, des recommandations mettre en surbrillance les positions couramment utilisé la mise en page")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Sur l’aire de conception, des recommandations mettre en surbrillance les positions couramment utilisé la mise en page](introduction-images/9-layoutguides-vs.png "sur l’aire de conception, des recommandations mettre en surbrillance les positions couramment utilisé la mise en page")

-----

La ligne en pointillés bleue dans l’exemple ci-dessus fournit des indications visual alignement horizontal center pour aider à l’emplacement du bouton.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

#### <a name="context-menu-commands"></a>Commandes de menu contextuel

Un menu contextuel est disponible sur l’aire de conception et dans le **structure du Document**. Ce menu fournit des commandes pour le contrôle sélectionné et son parent, ce qui est utile lorsque vous travaillez avec des vues dans une hiérarchie imbriquée :

[![Le menu contextuel sur l’aire de conception](introduction-images/10-contextmenudesignsurface-vsmac.png "le menu contextuel sur l’aire de conception")](introduction-images/10-contextmenudesignsurface-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-----

### <a name="constraints-toolbar"></a>Barre d’outils de contraintes

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)
 
[![La barre d’outils de contraintes](introduction-images/11-constraintstoolbar-vsmac.png "la barre d’outils de contraintes")](introduction-images/11-constraintstoolbar-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![La barre d’outils de contraintes](introduction-images/11-constraintstoolbar-vs.png "la barre d’outils de contraintes")](introduction-images/11-constraintstoolbar-vs-large.png)

-----

A été mis à jour de la barre d’outils de contraintes et comporte désormais deux contrôles : le mode d’édition de frame / contrainte modification activer/désactiver le mode et les contraintes de mise à jour / mise à jour de bouton de frames.

#### <a name="frame-editing-mode--constraint-editing-mode-toggle"></a>Mode d’édition de frame / activer/désactiver le mode de modification de contrainte

Dans les versions précédentes du concepteur iOS, en cliquant sur une vue déjà sélectionné sur l’aire de conception activé entre le frame de modification de mode et le mode de modification de contrainte. À présent, un contrôle de bouton bascule dans la barre d’outils de contraintes bascule entre ces modes d’édition.

- Mode de modification de cadre :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

![Frame de modification de bouton mode](introduction-images/12a-frameeditingmode-vsmac.png "Frame bouton du mode d’édition")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Frame de modification de bouton mode](introduction-images/12a-frameeditingmode-vs.png "Frame bouton du mode d’édition")

-----

- Mode de modification de contrainte :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

![Bouton de mode de modification de contrainte](introduction-images/12b-constrainteditingmode-vsmac.png "bouton mode édition de contrainte")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Bouton de mode de modification de contrainte](introduction-images/12b-constrainteditingmode-vs.png "bouton mode édition de contrainte")

-----

#### <a name="update-constraints--update-frames-button"></a>Contraintes de mise à jour / de mise à jour de bouton cadres

Les contraintes de mise à jour / mise à jour des frames réside bouton à droite de l’image en mode d’édition / activer/désactiver le mode de modification de contrainte.

- Dans le cadre de mode d’édition, cliquez sur ce bouton ajuste les frames de tous les éléments sélectionnés pour correspondre à leurs contraintes.
- En mode de modification de contrainte, cliquez sur ce bouton ajuste les contraintes de tous les éléments sélectionnés pour correspondre à leurs images.

### <a name="bottom-toolbar"></a>Barre d’outils inférieure

La barre d’outils inférieure fournit un moyen de sélectionner le périphérique, l’orientation et zoom utilisé pour afficher un fichier de plan conceptuel ou .xib dans le concepteur iOS :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![La barre d’outils inférieure, permet de sélectionner un périphérique et l’orientation de l’aire de conception](introduction-images/13-bottomtoolbar-vsmac.png "la barre d’outils inférieure, permet de sélectionner un périphérique et l’orientation de l’aire de conception")](introduction-images/13-bottomtoolbar-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![La barre d’outils inférieure, permet de sélectionner un périphérique et l’orientation de l’aire de conception](introduction-images/13-bottomtoolbar-vs.png "la barre d’outils inférieure, permet de sélectionner un périphérique et l’orientation de l’aire de conception")](introduction-images/13-bottomtoolbar-vs-large.png)

-----

#### <a name="device-and-orientation"></a>Appareil et l’orientation

Développée, la barre d’outils du bas affiche tous les périphériques, les orientations et/ou les adaptations applicables pour le document actif. En cliquant sur les modifications apportées à l’affichage sur l’aire de conception. 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![La barre d’outils inférieure, développée pour afficher les périphériques et les orientations](introduction-images/14-bottomtoolbarexpanded-vsmac.png "la barre d’outils inférieure, développée pour afficher les périphériques et les orientations")](introduction-images/14-bottomtoolbarexpanded-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![La barre d’outils inférieure, développée pour afficher les périphériques et les orientations](introduction-images/14-bottomtoolbarexpanded-vs.png "la barre d’outils inférieure, développée pour afficher les périphériques et les orientations")](introduction-images/14-bottomtoolbarexpanded-vs-large.png)

-----

Notez que la sélection d’un appareil et l’orientation modifie uniquement comment le concepteur iOS affiche un aperçu de la conception. Quelle que soit la sélection actuelle, qui vient d’être ajouté les contraintes sont appliquées sur tous les périphériques et les orientations, sauf si le **modifier les caractéristiques** bouton a été utilisé pour spécifier dans le cas contraire.

Lorsque [classes de taille](~/ios/user-interface/storyboards/unified-storyboards.md#size-classes) sont [activé](~/ios/user-interface/storyboards/unified-storyboards.md#enabling-size-classes), le **modifier les caractéristiques** bouton s’affiche dans la barre d’outils du bas développé.  En cliquant sur le **modifier les caractéristiques** bouton affiche des options pour la création d’une variation de l’interface basée sur la classe de taille représentée par le périphérique sélectionné et l’orientation. Prenons les exemples suivants :

- Si **iPhone SE** / **Portrait**, est sélectionné, popover offre des options pour créer une variation de l’interface pour la largeur compacte, la classe de taille hauteur normale. 
- Si **iPad Pro 9.7"** / **paysage** / **plein écran** est sélectionnée, popover offre des options pour créer une variante de l’interface pour la largeur régulière, de la classe de taille de hauteur normale.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![La barre d’outils inférieure utilisé pour faire varier d’une interface par la classe de taille](introduction-images/15-edittraitsbutton-vsmac.png "la barre d’outils inférieure utilisé pour faire varier d’une interface par la classe de taille")](introduction-images/15-edittraitsbutton-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![La barre d’outils inférieure utilisé pour faire varier d’une interface par la classe de taille](introduction-images/15-edittraitsbutton-vs.png "la barre d’outils inférieure utilisé pour faire varier d’une interface par la classe de taille")](introduction-images/15-edittraitsbutton-vs-large.png)

-----

#### <a name="zoom-controls"></a>Contrôles de zoom

L’aire de conception prend en charge le zoom via plusieurs contrôles :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)
 
![Les contrôles de zoom dans la barre d’outils inférieure](introduction-images/16-zoomcontrols-vsmac.png "les contrôles de zoom dans la barre d’outils en bas")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Les contrôles de zoom dans la barre d’outils inférieure](introduction-images/16-zoomcontrols-vs.png "les contrôles de zoom dans la barre d’outils en bas")

-----

Les contrôles sont les suivantes :

1. Zoom pour ajuster
2. Zoom arrière
3. Zoom avant
4. Taille réelle (la taille en pixels de 1:1)

Ces contrôles ajustent le zoom sur l’aire de conception. Ils n’affectent pas l’interface utilisateur de l’application lors de l’exécution.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

### <a name="properties-pad"></a>Remplissage des propriétés

Utilisez le **propriétés remplissage** pour modifier l’identité, les styles visuels, accessibilité et le comportement d’un contrôle. La capture d’écran suivante illustre le **propriétés remplissage** options pour un bouton :

[![La zone de propriétés pour un bouton](introduction-images/17-buttonpropertiespad-vsmac.png "le remplissage de propriétés pour un bouton")](introduction-images/17-buttonpropertiespad-vsmac-large.png)
#### <a name="properties-pad-sections"></a>Sections de remplissage de propriétés

Le **propriétés remplissage** contient trois sections :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

### <a name="properties-window"></a>Propriétés (fenêtre)

Utilisez le **fenêtre Propriétés** pour modifier l’identité, les styles visuels, accessibilité et le comportement d’un contrôle. La capture d’écran suivante illustre le **fenêtre Propriétés** options pour un bouton :

[![La fenêtre Propriétés pour un bouton](introduction-images/17-buttonpropertieswindow-vs.png "la fenêtre de propriétés pour un bouton")](introduction-images/17-buttonpropertieswindow-vs-large.png)

#### <a name="properties-window-sections"></a>Sections de fenêtre de propriétés

Le **fenêtre Propriétés** contient trois sections :

-----

1.  **Widget** – propriétés principales du contrôle, comme le nom, les classes, les propriétés de style, etc. Propriétés de gestion du contenu du contrôle sont généralement placées ici.
2.  **Disposition** – propriétés effectuer le suivi de la position et la taille du contrôle, y compris les contraintes et les images, sont répertoriées ici.
3.  **Événements** : événements et gestionnaires d’événements spécifiés ici. Utile pour la gestion des événements tels que les fonctions tactiles, tap, faites glisser, etc. Les événements peuvent également être gérés directement dans le code.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

#### <a name="editing-properties-in-the-properties-pad"></a>Modification des propriétés dans la zone de propriétés

En plus de la modification visuelle sur l’aire de conception, le concepteur iOS prend en charge la modification des propriétés dans le **propriétés remplissage**. La modification de propriétés disponibles en fonction du contrôle sélectionné, comme illustré dans les captures d’écran ci-dessous :

[![Bouton Propriétés](introduction-images/18a-buttonpropertiespad-vsmac.png "bouton Propriétés")](introduction-images/18a-buttonpropertiespad-vsmac-large.png)

[![Afficher les propriétés du contrôleur](introduction-images/18b-viewcontrollerpropertiespad-vsmac.png "afficher les propriétés du contrôleur")](introduction-images/18b-viewcontrollerpropertiespad-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

#### <a name="editing-properties-in-the-properties-window"></a>Modification des propriétés dans la fenêtre Propriétés

En plus de la modification visuelle sur l’aire de conception, le concepteur iOS prend en charge la modification des propriétés dans le **fenêtre Propriétés**. La modification de propriétés disponibles en fonction du contrôle sélectionné, comme illustré dans les captures d’écran ci-dessous :

[![Bouton Propriétés](introduction-images/18a-buttonpropertieswindow-vs.png "bouton Propriétés")](introduction-images/18a-buttonpropertieswindow-vs-large.png)

[![Afficher les propriétés du contrôleur](introduction-images/18b-viewcontrollerpropertieswindow-vs.png "afficher les propriétés du contrôleur")](introduction-images/18b-viewcontrollerpropertieswindow-vs-large.png)

-----

> [!IMPORTANT]
> La section d’identité du remplissage des propriétés maintenant présente un **Module** champ. Il est nécessaire pour remplir cette section uniquement lors de l’interaction avec les classes Swift. Il permet d’entrer un nom de module pour les classes Swift, qui sont namespaced.

#### <a name="default-values"></a>Valeurs par défaut

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Plusieurs propriétés dans le **propriétés remplissage** afficher aucune valeur ou une valeur par défaut. Toutefois, le code de l’application peut toujours modifier ces valeurs. Le **propriétés remplissage** n’affiche pas les valeurs définies dans le code.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Nombre de propriétés dans le **fenêtre Propriétés** afficher aucune valeur ou une valeur par défaut. Toutefois, le code de l’application peut toujours modifier ces valeurs. Le **fenêtre Propriétés** n’affiche pas les valeurs définies dans le code.

-----

#### <a name="event-handlers"></a>Gestionnaires d’événements

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Pour spécifier les gestionnaires d’événements personnalisés pour différents événements, utilisez le **événements** onglet de la **propriétés remplissage**. Par exemple, dans la capture d’écran ci-dessous, un `HandleClick` méthode gère du bouton **tactile dans** événement :

[![La liste des propriétés, avec un gestionnaire d’événements pour un bouton](introduction-images/19-buttonpropertiespadevents-vsmac.png "le remplissage de propriétés, avec un gestionnaire d’événements pour un bouton")](introduction-images/19-buttonpropertiespadevents-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Pour spécifier les gestionnaires d’événements personnalisés pour différents événements, utilisez le **événements** onglet de la **fenêtre Propriétés**. Par exemple, dans la capture d’écran ci-dessous, un `HandleClick` méthode gère du bouton **tactile dans** événement :

[![La fenêtre Propriétés, avec un gestionnaire d’événements pour un bouton](introduction-images/19-buttonpropertieswindowevents-vs.png "la fenêtre Propriétés, avec un gestionnaire d’événements pour un bouton")](introduction-images/19-buttonpropertieswindowevents-vs-large.png)

-----

Une fois qu’un gestionnaire d’événements a été spécifié, une méthode portant le même nom doit être ajoutée à la classe de contrôleur vue correspondante. Sinon, un `unrecognized selector` exception se produit lorsque l’utilisateur clique sur le bouton :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Une exception non reconnu sélecteur](introduction-images/20-unrecognizedselector-vsmac.png "une exception de sélecteur non reconnu")](introduction-images/20-unrecognizedselector-vsmac-large.png)

Notez que, après un gestionnaire d’événements a été spécifié dans le **propriétés remplissage**, iOS concepteur sera immédiatement ouvrir le fichier de code correspondant et d’offrir à insérer la déclaration de méthode. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Une exception non reconnu sélecteur](introduction-images/20-unrecognizedselector-vs.png "une exception de sélecteur non reconnu")](introduction-images/20-unrecognizedselector-vs-large.png)

-----

Pour obtenir un exemple qui utilise des gestionnaires d’événements personnalisés, reportez-vous à la [Hello, iOS Getting Started Guide](~/ios/get-started/hello-ios/index.md).

### <a name="outline-view"></a>Mode plan

Le concepteur iOS peut également afficher la hiérarchie d’une interface de contrôles sous forme de plan. Le plan est disponible en sélectionnant le **structure du Document** onglet, comme indiqué ci-dessous :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![La structure du Document](introduction-images/21-buttonoutlineview-vsmac.png "la structure du Document")](introduction-images/21-buttonoutlineview-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![La structure du Document](introduction-images/21-buttonoutlineview-vs.png "la structure du Document")](introduction-images/21-buttonoutlineview-vs-large.png)

-----

Le contrôle sélectionné en mode plan reste synchronisé avec le contrôle sélectionné sur l’aire de conception.  Cette fonctionnalité est utile pour la sélection d’un élément d’une hiérarchie de l’interface profondément imbriquée.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

## <a name="revert-to-xcode"></a>Revenir à Xcode

Il est possible d’utiliser le concepteur et le générateur Xcode Interface iOS indifféremment. Pour ouvrir un plan conceptuel ou un fichier .xib dans Xcode Interface Builder, cliquez sur le fichier et sélectionnez **ouvrir avec > Xcode Interface Builder**, comme illustré par la capture d’écran ci-dessous :

[![Ouverture d’un storyboard dans Xcode Interface Builder](introduction-images/22-openinxcodeinterfacebuilder-vsmac.png "ouverture d’un storyboard dans Xcode Interface Builder")](introduction-images/22-openinxcodeinterfacebuilder-vsmac-large.png)

Après avoir apporté des modifications dans Xcode Interface Builder, enregistrez le fichier et revenir à Visual Studio pour Mac. Les modifications sont alors synchronisées avec le projet Xamarin.iOS.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-----

## <a name="xib-support"></a>prise en charge .xib

Le concepteur iOS prend en charge la création, la modification et la gestion des fichiers de .xib. Ceux-ci sont des fichiers XML qui affiche de représenter une unique et personnalisées qui peut être ajoutés pour afficher la hiérarchie d’une application. Un fichier .xib représente en général l’interface pour une vue unique ou d’un écran dans une application, tandis qu’un storyboard représente plusieurs écrans et les transitions entre eux.

Il existe de nombreux avis sur les solutions – .xib fichiers, des animations ou code – convient le mieux pour créer et maintenir une interface utilisateur. En réalité, il n’existe aucune solution parfaite, et il est toujours intéressant d’utiliser l’outil idéal pour le travail en cours. Ceci dit, les fichiers .xib sont généralement plus puissants lorsqu’il est utilisé pour représenter un affichage personnalisé nécessaire à plusieurs endroits dans une application, par exemple une cellule de la vue de table personnalisée. 

Vous trouverez plus d’informations sur l’utilisation de fichiers de .xib dans les recettes suivantes :

- [À l’aide du modèle d’affichage .xib](https://developer.xamarin.com/recipes/ios/general/templates/using_the_ios_view_xib_template/)
- [Création d’un TableViewCell personnalisé à l’aide d’un .xib](https://developer.xamarin.com/recipes/ios/content_controls/tables/custom-tableviewcell/)
- [Création d’un écran de lancement à l’aide d’un .xib](https://developer.xamarin.com/recipes/ios/general/templates/launchscreen-xib/)

Pour plus d’informations sur l’utilisation des plans conceptuels, reportez-vous à la [Introduction à des plans conceptuels](~/ios/user-interface/storyboards/index.md).

Cela et les autres guides de concepteur iOS font référence à l’utilisation des plans conceptuels que l’approche standard pour la création d’interfaces utilisateur, étant donné que la plupart des modèles de projet de nouveau Xamarin.iOS fournissent un storyboard par défaut.

## <a name="summary"></a>Récapitulatif

Ce guide a présenté dans le concepteur, qui décrit ses fonctionnalités et la mise en relief les outils qu’il propose pour la conception de belles interfaces utilisateurs iOS.

## <a name="related-links"></a>Liens associés

- [Introduction aux storyboards](~/ios/user-interface/storyboards/index.md)
- [procédure pas à pas de contrôles possible d’iOS](~/ios/user-interface/designer/ios-designable-controls-walkthrough.md)
- [Hello, iOS](~/ios/get-started/hello-ios/index.md)
- [Hello, iOS Multiscreen](~/ios/get-started/hello-ios-multiscreen/index.md)
- [Vue d’ensemble du concepteur Android](~/android/user-interface/android-designer/index.md)
- [Classes et méthodes partielles](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods)
- [Plonger dans le Concepteur de Xamarin pour iOS - 2014 d’évoluer (vidéo)](https://www.youtube.com/watch?v=W4H9uLjoEjM)
- [À l’aide du Concepteur d’iOS pour créer un écran de lancement (vidéo)](https://university.xamarin.com/lightninglectures/using-the-ios-designer-to-create-a-launch-screen)
