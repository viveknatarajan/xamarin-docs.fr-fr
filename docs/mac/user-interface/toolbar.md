---
title: Barres d'outils
description: Cet article décrit l’utilisation des barres d’outils dans une application Xamarin.Mac. Elle traite des barres d’outils de création et la maintenance dans Xcode et Générateur de l’Interface, les exposer au code et leur utilisation par programmation.
ms.prod: xamarin
ms.assetid: C8D228CE-C860-47E1-85FD-69864BF91F20
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 729c5c69d80c52047585d1026d7c675f3267f34e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="toolbars"></a>Barres d'outils

_Cet article décrit l’utilisation des barres d’outils dans une application Xamarin.Mac. Elle traite des barres d’outils de création et la maintenance dans Xcode et Générateur de l’Interface, les exposer au code et leur utilisation par programmation._

Les développeurs de Xamarin.Mac utilisation de Visual Studio pour Mac ont accès aux mêmes contrôles d’interface utilisateur disponibles pour les développeurs macOS Xcode, y compris le contrôle de barre d’outils. Xamarin.Mac s’intègre directement avec Xcode, il est possible d’utiliser le Générateur de Xcode Interface pour créer et gérer des éléments de barre d’outils. Ces éléments de barre d’outils peuvent également être créés en c#.

Barres d’outils dans macOS sont ajoutés à la partie supérieure d’une fenêtre et fournissent un accès aisé aux commandes liées à ses fonctionnalités. Barres d’outils peuvent être masqués, affichés ou personnalisés par les utilisateurs d’une application, et ils peuvent présenter des éléments de barre d’outils de différentes manières.

Cet article décrit les principes fondamentaux de l’utilisation des barres d’outils et des éléments de barre d’outils dans une application Xamarin.Mac. 

Avant de continuer, lisez la [Hello, Mac](~/mac/get-started/hello-mac.md) article : spécifiquement le [Introduction à Xcode et Interface Générateur](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) et [prises et les Actions](~/mac/get-started/hello-mac.md#Outlets_and_Actions) sections, tel qu’il Décrit les concepts clés et les techniques qui seront utilisés dans ce guide.

Également examiner la [classes exposition c# / méthodes pour Objective-C](~/mac/internals/how-it-works.md) section de la [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) document. Il explique le `Register` et `Export` attributs utilisés pour se connecter de classes c# pour les classes Objective-C.

## <a name="introduction-to-toolbars"></a>Introduction aux barres d’outils

N’importe quelle fenêtre dans une application macOS peut inclure une barre d’outils :

![Une fenêtre d’exemple avec une barre d’outils](toolbar-images/info01.png "une fenêtre d’exemple avec une barre d’outils")

Barres d’outils fournissent un moyen simple pour les utilisateurs de votre application à accéder rapidement aux important ou les fonctionnalités couramment utilisées. Par exemple, une application de modification de documents peut fournir des éléments de barre d’outils pour définir la couleur du texte, la modification de la police ou l’impression du document en cours.

Barres d’outils peuvent afficher les éléments de trois façons :

1. **Icône et texte** 

     ![Une barre d’outils avec des icônes et du texte](toolbar-images/info02.png "une barre d’outils avec des icônes et du texte")

2. **Icône uniquement** 

     ![Une barre d’outils de l’icône seule](toolbar-images/info03.png "une barre d’outils de l’icône uniquement")

3. **Texte uniquement** 

     ![Une barre d’outils uniquement du texte](toolbar-images/info04.png "une barre d’outils uniquement du texte")

Basculer entre ces modes en double-cliquant sur la barre d’outils et en sélectionnant un mode d’affichage dans le menu contextuel :

![Le menu contextuel pour une barre d’outils](toolbar-images/info05.png "le menu contextuel pour une barre d’outils")

Utilisez le menu pour afficher la barre d’outils à une taille inférieure :

![Une barre d’outils avec petites icônes](toolbar-images/info06.png "une barre d’outils avec petites icônes")

Le menu permet également de personnalisation de la barre d’outils :

![La boîte de dialogue permet de personnaliser une barre d’outils](toolbar-images/info07.png "la boîte de dialogue permet de personnaliser une barre d’outils")

Lorsque vous configurez une barre d’outils dans le Générateur de Xcode Interface, un développeur peut fournir des éléments de barre d’outils supplémentaires qui ne font pas partie de sa configuration par défaut. Les utilisateurs de l’application peuvent ensuite personnaliser la barre d’outils, ajout et suppression de ces éléments prédéfinis selon les besoins. Bien entendu, la barre d’outils peut être réinitialisée à sa configuration par défaut.

La barre d’outils se connecte automatiquement à la **vue** menu, ce qui permet aux utilisateurs de masquer, afficher et personnaliser :

![Les éléments liés à la barre d’outils dans le menu Affichage](toolbar-images/info08.png "liés à la barre d’outils des éléments dans le menu Affichage")

Consultez le [les fonctionnalités de Menu intégré](~/mac/user-interface/menu.md) documentation pour plus de détails.

En outre, si la barre d’outils est configuré correctement dans l’Interface générateur, l’application automatiquement persistera personnalisations de la barre d’outils lance plusieurs de l’application.

Les sections suivantes de ce guide décrivent comment créer et gérer des barres d’outils avec le Générateur de Xcode Interface et comment les utiliser dans le code.

## <a name="setting-a-custom-main-window-controller"></a>Définition d’un contrôleur personnalisé de fenêtre principale

Pour exposer les éléments d’interface utilisateur au code c# via prises et actions, l’application Xamarin.Mac doit utiliser un contrôleur de fenêtres personnalisées :

1. Ouvrez le plan conceptuel de l’application dans le Générateur de Xcode Interface.
2. Sélectionnez le contrôleur de la fenêtre sur l’aire de conception.
3. Basculez vers le **inspecteur d’identité** et entrez « WindowController » en tant que le **nom de la classe**: 

    [![Définition d’un nom de classe personnalisée pour le contrôleur de fenêtre](toolbar-images/windowcontroller01.png "définition d’un nom de classe personnalisée pour le contrôleur de fenêtre")](toolbar-images/windowcontroller01-large.png#lightbox) 

4. Enregistrez vos modifications et revenir à Visual Studio pour Mac à synchroniser.
5. A **WindowController.cs** fichier sera ajouté à votre projet dans le **Solution remplissage** dans Visual Studio pour Mac : 

    ![En sélectionnant WindowController.cs dans la zone Solution](toolbar-images/windowcontroller02.png "en sélectionnant WindowController.cs dans la zone de la Solution")

6. Rouvrez le plan conceptuel dans le Générateur de Xcode Interface.
7. Le **WindowController.h** fichier sera disponible pour une utilisation : 

    [![Le fichier WindowController.h](toolbar-images/windowcontroller03.png "WindowController.h le fichier")](toolbar-images/windowcontroller03-large.png#lightbox)

## <a name="creating-and-maintaining-toolbars-in-xcode"></a>Créer et maintenir les barres d’outils dans Xcode

Barres d’outils sont créés et gérés avec le Générateur de Xcode Interface. Pour ajouter une barre d’outils à une application, modifiez le storyboard principal de l’application (dans ce cas **Main.storyboard**) en double-cliquant dessus dans le **Solution remplissage**:

![Ouverture de Main.storyboard dans la zone Solution](toolbar-images/edit01.png "ouverture Main.storyboard dans la zone de la Solution")

Dans le **bibliothèque inspecteur**, entrez « outil » dans le **zone de recherche** pour le rendre plus facile de voir tous les éléments de barre d’outils disponibles :

![L’inspecteur de bibliothèque, filtrés pour afficher les éléments de barre d’outils](toolbar-images/edit02.png "l’inspecteur de bibliothèque, filtré pour afficher les éléments de barre d’outils")

Faire glisser une barre d’outils dans la fenêtre de la **éditeur de l’Interface**. La barre d’outils sélectionné, configurez son comportement en définissant des propriétés le **attributs inspecteur**:

![L’inspecteur d’attributs pour une barre d’outils](toolbar-images/edit04.png "l’inspecteur d’attributs pour une barre d’outils")

Les propriétés suivantes sont disponibles :

1. **Affichage** -contrôle si la barre d’outils affiche des icônes, le texte ou les deux
2. **Visible au lancement** -si sélectionné, la barre d’outils est visible par défaut.
3. **Personnalisable** -si sélectionné, les utilisateurs peuvent modifier et personnaliser la barre d’outils.
4. **Séparateur** -si sélectionné, une fine ligne horizontale sépare la barre d’outils à partir du contenu de la fenêtre.
5. **Taille** -définit la taille de la barre d’outils
6. **Cette fonctionnalité** -si sélectionné, l’application persistera modifications de configuration de barre d’outils d’un utilisateur lance d’application.

Sélectionnez le **enregistrement automatique** option et conservez toutes les autres propriétés de leurs paramètres par défaut. 

Après l’ouverture de la barre d’outils dans le **hiérarchie de l’Interface**, afficher la boîte de dialogue de personnalisation en sélectionnant un élément de barre d’outils :

![Personnalisation de la barre d’outils](toolbar-images/edit05.png "personnalisation de la barre d’outils")

Utilisez cette boîte de dialogue pour définir les propriétés pour les éléments qui font déjà partie de la barre d’outils, à la conception de la barre d’outils par défaut pour l’application, et pour fournir des éléments de barre d’outils supplémentaires pour un utilisateur de sélectionner lors de la personnalisation de la barre d’outils. Pour ajouter des éléments à la barre d’outils, faites-les glisser à partir de la **bibliothèque inspecteur**:

![L’inspecteur de bibliothèque](toolbar-images/edit06.png "l’inspecteur de bibliothèque")

Les éléments de barre d’outils suivants peuvent être ajoutées :

- **Élément de barre d’outils de l’image** -un élément de barre d’outils avec une image personnalisée sous forme d’icône.
- **Élément de barre d’outils flexible espace** -Flexible espace utilisé pour justifier des éléments de barre d’outils suivants. Par exemple, un ou plusieurs éléments de barre d’outils suivi d’un élément de barre d’outils espace flexible et un autre élément de barre d’outils Épinglez le dernier élément à droite de la barre d’outils.
- **Élément de barre d’outils de l’espace** -fixe quantité d’espace entre les éléments de la barre d’outils
- **Séparateur de barre d’outils** -visible séparateur entre deux ou plusieurs éléments de barre d’outils pour le regroupement
- **Personnaliser la barre d’outils** -permet aux utilisateurs de personnaliser la barre d’outils
- **Élément de barre d’outils d’impression** -permet aux utilisateurs d’imprimer le document ouvert
- **Afficher l’élément de barre d’outils de couleurs** -affiche le sélecteur de couleur système standard : 

     ![Le sélecteur de couleurs système](toolbar-images/edit07.png "le sélecteur de couleurs système")

- **Afficher l’élément de barre d’outils de police** -affiche la boîte de dialogue de police système standard : 

     ![Le sélecteur de polices](toolbar-images/edit08.png "le sélecteur de polices")

> [!IMPORTANT]
> Comme indiqué plus loin, de nombreux contrôles de l’interface utilisateur/Cocoa standards tels que les champs de recherche, les contrôles segmentés et les curseurs horizontales peuvent également être ajoutés à une barre d’outils.

### <a name="adding-an-item-to-a-toolbar"></a>Ajout d’un élément à une barre d’outils

Pour ajouter un élément à une barre d’outils, sélectionnez la barre d’outils dans le **hiérarchie de l’Interface** et cliquez sur un de ses éléments, à l’origine de la boîte de dialogue de personnalisation apparaisse. Ensuite, faites glisser un élément à partir de la **bibliothèque inspecteur** à la **autorisé des éléments de barre d’outils** zone :

![Les éléments de barre d’outils autorisé dans la boîte de dialogue de personnalisation de barre d’outils](toolbar-images/add01.png "les éléments de barre d’outils autorisé dans la boîte de dialogue de personnalisation de barre d’outils")

Pour vous assurer qu’un nouvel élément fait partie de la barre d’outils par défaut, faites-le glisser vers le **des éléments de barre d’outils par défaut** zone : 

![Réorganisation d’un élément de barre d’outils en faisant glisser](toolbar-images/add02.png "réorganisation d’un élément de barre d’outils en faisant glisser")

Pour réorganiser les éléments de barre d’outils par défaut, faites-les glisser gauche ou droit.

Ensuite, utilisez le **attributs inspecteur** pour définir les propriétés par défaut pour l’élément :

![Personnalisation d’un élément de barre d’outils à l’aide de l’inspecteur d’attributs](toolbar-images/add03.png "personnalisation d’un élément de barre d’outils à l’aide de l’inspecteur d’attributs")

Les propriétés suivantes sont disponibles :

- **Nom de l’image** -Image à utiliser en tant qu’icône pour l’élément
- **Étiquette** -texte à afficher pour l’élément dans la barre d’outils
- **Étiquette de palette** -texte à afficher pour l’élément dans le **autorisé des éléments de barre d’outils** zone
- **Balise** -un identificateur unique facultatif qui permet d’identifier l’élément dans le code.
- **Identificateur** -définit le type d’élément de la barre d’outils. Une valeur personnalisée peut être utilisée pour sélectionner un élément de barre d’outils dans le code.
- **Sélectionnable** -si elle est activée, l’élément agit comme un bouton marche/arrêt.

> [!IMPORTANT]
> Ajouter un élément à la **autorisé des éléments de barre d’outils** zone mais pas les outils par défaut pour fournir des options de personnalisation pour les utilisateurs. 

### <a name="adding-other-ui-controls-to-a-toolbar"></a>Ajout d’autres contrôles d’interface utilisateur à une barre d’outils

Plusieurs éléments de l’interface utilisateur/Cocoa tels que les champs de recherche et segmentés contrôles peuvent également être ajoutés à une barre d’outils.

Pour tester cela, ouvrez la barre d’outils dans le **hiérarchie de l’Interface** et sélectionnez un élément de barre d’outils pour ouvrir la boîte de dialogue de personnalisation. Faites glisser un **champ de recherche** à partir de la **inspecteur de bibliothèque** à la **autorisé des éléments de barre d’outils** zone :

![À l’aide de la boîte de dialogue de personnalisation de barre d’outils](toolbar-images/add05.png "à l’aide de la boîte de dialogue de personnalisation de barre d’outils")

À ce stade, utilisez le Générateur de Interface pour configurer le champ de recherche et l’exposer au code via une action ou une sortie.

## <a name="built-in-toolbar-item-support"></a>Prise en charge intégrée de barre d’outils

Plusieurs éléments de l’interface utilisateur/Cocoa interagissent avec les éléments de la barre d’outils standard par défaut. Par exemple, faites glisser un **mode texte** sur la fenêtre de l’application et placez-le pour remplir la zone de contenu :

[![Ajout d’une vue de texte à l’application](toolbar-images/edit09.png "Ajout d’une vue de texte à l’application")](toolbar-images/edit09-large.png#lightbox)

Enregistrez le document, revenez à Visual Studio pour Mac à synchroniser avec Xcode, exécutez l’application, entrez du texte, sélectionnez-le, puis cliquez sur le **couleurs** élément de barre d’outils. Notez que l’affichage de texte fonctionne automatiquement avec le sélecteur de couleurs :

![Les fonctionnalités de barre d’outils avec un sélecteur de vue et la couleur du texte](toolbar-images/edit10.png "les fonctionnalités de barre d’outils avec un sélecteur de vue et la couleur du texte")

## <a name="using-images-with-toolbar-items"></a>L’utilisation d’images avec des éléments de barre d’outils

À l’aide un **élément de barre d’outils d’Image**, une image bitmap est ajouté à la **ressources** dossier (et une action de génération **groupe de ressources**) peuvent être affichés sur la barre d’outils sous forme d’icône :

1. Dans Visual Studio pour Mac, dans le **Solution remplissage**, avec le bouton droit le **ressources** et sélectionnez **ajouter** > **ajouter des fichiers** .
2. À partir de la **ajouter des fichiers** boîte de dialogue zone, accédez aux images de votre choisis, sélectionnez-les et cliquez sur le **ouvrir** bouton : 

    [![Sélection d’images à ajouter](toolbar-images/edit11.png "sélection d’images à ajouter")](toolbar-images/edit11-large.png#lightbox)

3. Sélectionnez **copie**, vérifiez **utilisent la même action pour tous les fichiers sélectionnés**, puis cliquez sur **OK**:

    ![En sélectionnant l’action de copie pour les images ajoutées](toolbar-images/edit12.png "en sélectionnant l’action de copie pour les images ajoutées.")

4. Dans le **Solution remplissage**, double-cliquez sur **MainWindow.xib** pour l’ouvrir dans Xcode.

5. Sélectionnez la barre d’outils dans le **hiérarchie de l’Interface** et cliquez sur un de ses éléments pour ouvrir la boîte de dialogue de personnalisation.

6. Faites glisser une **élément de barre d’outils d’Image** à partir de la **bibliothèque inspecteur** la barre d’outils **autorisé des éléments de barre d’outils** zone : 

    ![Un élément de barre d’outils Image ajouté à la zone d’autorisé des éléments de barre d’outils](toolbar-images/edit14.png "un élément de barre d’outils Image ajoutée à la zone d’autorisé des éléments de barre d’outils")

7. Dans le **attributs inspecteur**, sélectionnez l’image qui vient d’être ajouté dans Visual Studio pour Mac : 

    ![Définition d’une image personnalisée pour un élément de barre d’outils](toolbar-images/edit15.png "définition d’une image personnalisée pour un élément de barre d’outils")

8. Définir le **étiquette** à « Corbeille » et le **Palette étiquette** « Effacement Document » : 

    ![Définition de la barre d’outils élément Palette étiquettes et](toolbar-images/edit16.png "définition de la barre d’outils élément étiquette et étiquette de Palette")

9. Faites glisser un **séparateur de barre d’outils** à partir de la **bibliothèque inspecteur** la barre d’outils **autorisé des éléments de barre d’outils** zone : 

    [![Un séparateur de barre d’outils est ajouté à la zone d’autorisé des éléments de barre d’outils](toolbar-images/edit17.png "un séparateur de barre d’outils élément ajouté à la zone d’autorisé des éléments de barre d’outils")](toolbar-images/edit17-large.png#lightbox)

10. Faites glisser le séparateur et l’élément « Corbeille » à la **par défaut des éléments de barre d’outils** zone et le jeu d’éléments de l’ordre de la barre d’outils de gauche à droite comme suit (couleurs, polices, séparateur, Corbeille, espace Flexible, impression) : 

    ![Les éléments de barre d’outils par défaut](toolbar-images/edit18.png "les éléments de barre d’outils par défaut")

11. Enregistrer les modifications et revenir à Visual Studio pour Mac pour la synchronisation avec Xcode.

Exécutez l’application pour vérifier que la nouvelle barre d’outils s’affiche par défaut :

![Une barre d’outils avec des éléments par défaut personnalisé](toolbar-images/edit19.png "une barre d’outils avec des éléments par défaut personnalisé")

## <a name="exposing-toolbar-items-with-outlets-and-actions"></a>Exposer des éléments de barre d’outils avec les sorties et des actions

Pour accéder à une barre d’outils ou un élément de barre d’outils dans le code, il doit être attaché à une prise de courant ou une action :

1. Dans le **Solution remplissage**, double-cliquez sur **Main.storyboard** pour l’ouvrir dans Xcode.
2. Vérifiez que la classe personnalisée « WindowController » a été attribué au contrôleur de fenêtre principale dans le **identité inspecteur**:

    [![Pour définir une classe personnalisée pour le contrôleur de fenêtre à l’aide de l’inspecteur de l’identité](toolbar-images/edit20a.png "à l’aide de l’inspecteur de l’identité pour définir une classe personnalisée pour le contrôleur de fenêtre")](toolbar-images/edit20a-large.png#lightbox)

3. Ensuite, sélectionnez l’élément de barre d’outils dans le **hiérarchie de l’Interface**: 

    ![Sélection de l’élément de barre d’outils dans la hiérarchie de l’Interface](toolbar-images/edit20.png "en sélectionnant l’élément de barre d’outils dans la hiérarchie des interfaces")  

4. Ouvrir le **l’Assistant vue**, sélectionnez le **WindowController.h** de fichiers et faites glisser le contrôle de l’élément de barre d’outils pour le **WindowController.h** fichier.
5. Définir le **connexion** type **Action**, entrez « trashDocument » pour le **nom**et cliquez sur le **Connect** bouton : 

    [![Configuration d’une action pour un élément de barre d’outils](toolbar-images/edit23.png "configuration d’une action pour un élément de barre d’outils")](toolbar-images/edit23-large.png#lightbox)

6. Exposer le **mode texte** comme une sortie appelée « documentEditor » le **ViewController.h** fichier : 

    [![Configuration d’une prise de courant pour l’affichage de texte](toolbar-images/edit24.png "configuration d’une prise de courant pour l’affichage de texte")](toolbar-images/edit24-large.png#lightbox)

7. Enregistrez vos modifications et revenir à Visual Studio pour Mac pour la synchronisation avec Xcode.

Dans Visual Studio pour Mac, vous devez modifier le **ViewController.cs** et ajoutez le code suivant :

```csharp
public void EraseDocument() {
    documentEditor.Value = "";
}
```

Modifiez ensuite la **WindowController.cs** et ajoutez le code suivant en bas de la `WindowController` classe :

```csharp
[Export ("trashDocument:")]
void TrashDocument (NSObject sender) {

    var controller = ContentViewController as ViewController;
    controller.EraseDocument ();
}
```

Lors de l’exécution de l’application, le **Corbeille** élément de barre d’outils est activé :

![Une barre d’outils avec un élément de la Corbeille active](toolbar-images/edit25.png "une barre d’outils avec un élément de la Corbeille active")

Notez que la **Corbeille** élément de barre d’outils permet désormais de supprimer le texte.

## <a name="disabling-toolbar-items"></a>Désactiver les éléments de barre d’outils

Pour désactiver un élément dans une barre d’outils, créez une personnalisée `NSToolbarItem` classe et substituer la `Validate` (méthode). Ensuite, dans le générateur Interface, assignez le type personnalisé à l’élément que vous souhaitez activer ou désactiver.

Pour créer un personnalisé `NSToolbarItem` classe, avec le bouton droit sur le projet et sélectionnez **ajouter** > **nouveau fichier...** . Sélectionnez **général** > **classe vide**, entrez « ActivatableItem » pour le **nom**, puis cliquez sur le **nouveau** bouton : 

![Ajout d’une classe vide dans Visual Studio pour Mac](toolbar-images/custom01.png "Ajout d’une classe vide dans Visual Studio pour Mac")

Modifiez ensuite la **ActivatableItem.cs** fichier comme suit :

```csharp
using System;

using Foundation;
using AppKit;

namespace MacToolbar
{
    [Register("ActivatableItem")]
    public class ActivatableItem : NSToolbarItem
    {
        public bool Active { get; set;} = true;

        public ActivatableItem ()
        {
        }

        public ActivatableItem (IntPtr handle) : base (handle)
        {
        }

        public ActivatableItem (NSObjectFlag  t) : base (t)
        {
        }

        public ActivatableItem (string title) : base (title)
        {
        }

        public override void Validate ()
        {
            base.Validate ();
            Enabled = Active;
        }
    }
}
```

Double-cliquez sur **Main.storyboard** pour l’ouvrir dans Xcode. Sélectionnez le **Corbeille** élément de barre d’outils créé ci-dessus et remplacez sa classe « ActivatableItem » dans le **identité inspecteur**:

![Définition d’une classe personnalisée pour un élément de barre d’outils](toolbar-images/custom02.png "en définissant une classe personnalisée pour un élément de barre d’outils")

Créer une sortie appelée `trashItem` pour le **Corbeille** élément de barre d’outils. Enregistrer les modifications et revenir à Visual Studio pour Mac pour la synchronisation avec Xcode. Enfin, ouvrez **MainWindow.cs** et mettre à jour le `AwakeFromNib` méthode comme suit :

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Disable trash
    trashItem.Active = false;
}
```

Exécutez l’application et notez que le **Corbeille** est maintenant désactivée dans la barre d’outils :

![Une barre d’outils avec un élément de la Corbeille inactive](toolbar-images/custom03.png "une barre d’outils avec un élément de la Corbeille inactif")

## <a name="summary"></a>Récapitulatif

Cet article a pris une présentation détaillée sur l’utilisation des barres d’outils et des éléments de barre d’outils dans une application Xamarin.Mac. Il décrit comment créer et gérer des barres d’outils dans le Générateur de Xcode Interface, certains contrôles d’interface utilisateur automatiquement fonctionnement avec des éléments de barre d’outils, l’utilisation des barres d’outils dans le code c# et comment activer et désactiver les éléments de barre d’outils.


## <a name="related-links"></a>Liens associés

- [MacToolbar (exemple)](https://developer.xamarin.com/samples/mac/MacToolbar/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Interface utilisateur des recommandations pour les barres d’outils](https://developer.apple.com/macos/human-interface-guidelines/windows-and-views/toolbars/)
- [Introduction aux barres d’outils](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/Toolbars/Toolbars.html)
