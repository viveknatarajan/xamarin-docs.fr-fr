---
title: Barres d’outils dans Xamarin.Mac
description: Cet article décrit l’utilisation des barres d’outils dans une application Xamarin.Mac. Il aborde la création et la maintenance des barres d’outils dans Xcode et Interface Builder, les exposer au code et leur utilisation par programmation.
ms.prod: xamarin
ms.assetid: C8D228CE-C860-47E1-85FD-69864BF91F20
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 6cb17ae0f60390564a8aa6bdb64ea612aae51b55
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61294930"
---
# <a name="toolbars-in-xamarinmac"></a>Barres d’outils dans Xamarin.Mac

_Cet article décrit l’utilisation des barres d’outils dans une application Xamarin.Mac. Il aborde la création et la maintenance des barres d’outils dans Xcode et Interface Builder, les exposer au code et leur utilisation par programmation._

Les développeurs Xamarin.Mac fonctionne avec Visual Studio pour Mac ont accès aux mêmes contrôles d’interface utilisateur qui permettent aux développeurs de macOS fonctionne avec Xcode, y compris le contrôle de barre d’outils. Comme Xamarin.Mac s’intègre directement à Xcode, il est possible d’utiliser Interface Builder de Xcode pour créer et gérer des éléments de barre d’outils. Ces éléments de barre d’outils peuvent également être créés en c#.

Barres d’outils dans macOS sont ajoutés à la section supérieure d’une fenêtre et fournissent un accès facile aux commandes en rapport avec ses fonctionnalités. Barres d’outils peuvent être masqués, affichés ou personnalisées par les utilisateurs d’une application, et ils peuvent présenter des éléments de barre d’outils de différentes manières.

Cet article aborde les principes fondamentaux de l’utilisation des barres d’outils et des éléments de barre d’outils dans une application Xamarin.Mac. 

Avant de continuer, lisez la [Hello, Mac](~/mac/get-started/hello-mac.md) article — spécifiquement le [Introduction à Xcode et Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) et [Outlets et Actions](~/mac/get-started/hello-mac.md#outlets-and-actions) sections, tel qu’il Décrit les concepts clés et les techniques qui seront utilisés tout au long de ce guide.

Également jeter un coup de œil à la [C# exposition de classes / méthodes vers Objective-C](~/mac/internals/how-it-works.md) section de la [éléments internes de Xamarin.Mac](~/mac/internals/how-it-works.md) document. Il explique le `Register` et `Export` attributs utilisés pour se connecter de classes c# pour les classes Objective-C.

## <a name="introduction-to-toolbars"></a>Introduction aux barres d’outils

N’importe quelle fenêtre dans une application macOS peut inclure une barre d’outils :

![Un exemple de fenêtre avec une barre d’outils](toolbar-images/info01.png "un exemple de fenêtre avec une barre d’outils")

Barres d’outils fournissent un moyen simple pour les utilisateurs de votre application pour accéder rapidement aux important ou les fonctionnalités couramment utilisées. Par exemple, une application de modification de documents peut fournir des éléments de barre d’outils pour définir la couleur du texte, modification de la police ou l’impression du document en cours.

Barres d’outils peuvent afficher les éléments de trois façons :

1. **Icône et texte** 

     ![Une barre d’outils avec des icônes et du texte](toolbar-images/info02.png "une barre d’outils avec des icônes et du texte")

2. **Icône uniquement** 

     ![Une barre d’outils de l’icône seule](toolbar-images/info03.png "une barre d’outils de l’icône uniquement")

3. **Texte uniquement** 

     ![Une barre d’outils uniquement du texte](toolbar-images/info04.png "une barre d’outils uniquement du texte")

Basculer entre ces modes en double-cliquant sur la barre d’outils et en sélectionnant un mode d’affichage dans le menu contextuel :

![Le menu contextuel pour une barre d’outils](toolbar-images/info05.png "le menu contextuel pour une barre d’outils")

Utilisez le même menu pour afficher la barre d’outils à une taille inférieure :

![Une barre d’outils Affichage de petites icônes](toolbar-images/info06.png "une barre d’outils Affichage de petites icônes")

Il permet également le menu de personnalisation de la barre d’outils :

![La boîte de dialogue permet de personnaliser une barre d’outils](toolbar-images/info07.png "la boîte de dialogue permet de personnaliser une barre d’outils")

Lorsque vous configurez une barre d’outils dans l’Interface Builder de Xcode, un développeur peut fournir des éléments de barre d’outils supplémentaires qui ne font pas partie de sa configuration par défaut. Les utilisateurs de l’application peuvent ensuite personnaliser la barre d’outils, ajout et suppression de ces éléments prédéfinis en fonction des besoins. Bien entendu, la barre d’outils peut être réinitialisée à sa configuration par défaut.

La barre d’outils se connecte automatiquement à la **vue** menu, qui permet aux utilisateurs de masquer, afficher et personnaliser :

![Les éléments liés à la barre d’outils dans le menu Affichage](toolbar-images/info08.png "liées à la barre d’outils des éléments dans le menu Affichage")

Consultez le [fonctionnalités intégrées de Menu](~/mac/user-interface/menu.md) documentation pour plus d’informations.

En outre, si la barre d’outils est correctement configuré dans Interface Builder, l’application automatiquement persistera personnalisations de la barre d’outils plusieurs lancements de l’application.

Les sections suivantes de ce guide décrivent comment créer et gérer des barres d’outils avec Interface Builder de Xcode et comment les utiliser dans le code.

## <a name="setting-a-custom-main-window-controller"></a>Définition d’un contrôleur de fenêtre principale personnalisée

Pour exposer les éléments d’interface utilisateur au code c# via outlets et actions, l’application Xamarin.Mac doit utiliser un contrôleur de fenêtres personnalisées :

1. Ouvrez le plan conceptuel de l’application dans Interface Builder de Xcode.
2. Sélectionnez le contrôleur de fenêtre sur l’aire de conception.
3. Basculez vers le **inspecteur d’identité** et entrez « WindowController » en tant que le **nom de la classe**: 

    [![Définition d’un nom de classe personnalisée pour le contrôleur de fenêtre](toolbar-images/windowcontroller01.png "définissant un nom de classe personnalisée pour le contrôleur de fenêtre")](toolbar-images/windowcontroller01-large.png#lightbox) 

4. Enregistrez vos modifications et revenir à Visual Studio pour Mac pour la synchronisation.
5. Un **WindowController.cs** fichier sera ajouté à votre projet dans le **panneau solutions** dans Visual Studio pour Mac : 

    ![En sélectionnant WindowController.cs dans le panneau Solution](toolbar-images/windowcontroller02.png "en sélectionnant WindowController.cs dans le panneau Solution")

6. Rouvrez le plan conceptuel dans Interface Builder de Xcode.
7. Le **WindowController.h** fichier sera disponible pour une utilisation : 

    [![Le fichier WindowController.h](toolbar-images/windowcontroller03.png "WindowController.h le fichier")](toolbar-images/windowcontroller03-large.png#lightbox)

## <a name="creating-and-maintaining-toolbars-in-xcode"></a>Création et la maintenance des barres d’outils dans Xcode

Barres d’outils sont créés et gérés avec Interface Builder de Xcode. Pour ajouter une barre d’outils à une application, modifiez le storyboard principal de l’application (dans ce cas **Main.storyboard**) en double-cliquant dessus dans le **panneau solutions**:

![Ouverture de Main.storyboard dans le panneau Solution](toolbar-images/edit01.png "ouverture Main.storyboard dans le panneau Solution")

Dans le **inspecteur de bibliothèque**, entrez « outil » dans le **zone de recherche** pour le rendre plus facile de voir tous les éléments de barre d’outils disponibles :

![L’inspecteur de bibliothèque, filtrés pour afficher les éléments de barre d’outils](toolbar-images/edit02.png "l’inspecteur de bibliothèque, filtré pour afficher les éléments de barre d’outils")

Faire glisser une barre d’outils dans la fenêtre de la **Éditeur d’Interface**. Avec la barre d’outils sélectionné, configurez son comportement en définissant des propriétés le **inspecteur d’attributs**:

![L’inspecteur d’attributs pour une barre d’outils](toolbar-images/edit04.png "l’inspecteur d’attributs pour une barre d’outils")

Les propriétés suivantes sont disponibles :

1. **Affichage** -contrôle si la barre d’outils affiche des icônes, texte ou les deux
2. **Visible au lancement** -si activée, la barre d’outils est visible par défaut.
3. **Personnalisable** -si sélectionné, les utilisateurs peuvent modifier et personnaliser la barre d’outils.
4. **Séparateur** -si sélectionné, une fine ligne horizontale sépare la barre d’outils de contenu de la fenêtre.
5. **Taille** -définit la taille de la barre d’outils
6. **Enregistrement automatique** -si sélectionné, l’application persistera les modifications de configuration de barre d’outils d’un utilisateur entre les lancements de l’application.

Sélectionnez le **Autosave** option et conservez toutes les autres propriétés à leurs paramètres par défaut. 

Après l’ouverture de la barre d’outils dans le **hiérarchie des interfaces**, afficher la boîte de dialogue de personnalisation en sélectionnant un élément de barre d’outils :

![Personnalisation de la barre d’outils](toolbar-images/edit05.png "personnalisation de la barre d’outils")

Utilisez cette boîte de dialogue pour définir les propriétés pour les éléments qui font déjà partie de la barre d’outils, à la conception de la barre d’outils par défaut pour l’application, et fournir des éléments de barre d’outils supplémentaires pour un utilisateur de sélectionner lors de la personnalisation de la barre d’outils. Pour ajouter des éléments à la barre d’outils, faites-les glisser depuis le **inspecteur de bibliothèque**:

![L’inspecteur de bibliothèque](toolbar-images/edit06.png "l’inspecteur de bibliothèque")

Les éléments de barre d’outils suivants peuvent être ajoutés :

- **Élément de barre d’outils de l’image** -un élément de barre d’outils avec une image personnalisée en tant qu’icône.
- **Élément de barre d’outils flexible espace** -espace Flexible utilisé pour justifier des éléments de barre d’outils suivants. Par exemple, un ou plusieurs éléments de barre d’outils suivie d’un élément de barre d’outils flexible d’espace et un autre élément de barre d’outils Épinglez le dernier élément à droite de la barre d’outils.
- **Élément de barre d’outils de l’espace** -correction d’espace entre les éléments dans la barre d’outils
- **Séparateur de barre d’outils** -visible séparateur entre deux ou plusieurs éléments de barre d’outils pour le regroupement
- **Personnaliser la barre d’outils élément** -permet aux utilisateurs de personnaliser la barre d’outils
- **Élément de barre d’outils d’impression** -permet aux utilisateurs d’imprimer le document ouvert
- **Afficher l’élément de barre d’outils de couleurs** -affiche le sélecteur de couleurs système standard : 

     ![Le sélecteur de couleurs système](toolbar-images/edit07.png "le sélecteur de couleurs système")

- **Afficher l’élément de barre d’outils de police** -affiche la boîte de dialogue de police système standard : 

     ![Le sélecteur de police](toolbar-images/edit08.png "le sélecteur de police")

> [!IMPORTANT]
> Comme indiqué plus loin, de nombreux contrôles de l’interface utilisateur Cocoa standards tels que les champs de recherche, les contrôles segmentés et les curseurs horizontales peuvent également être ajoutés à une barre d’outils.

### <a name="adding-an-item-to-a-toolbar"></a>Ajout d’un élément à une barre d’outils

Pour ajouter un élément à une barre d’outils, sélectionnez la barre d’outils dans le **hiérarchie des interfaces** et cliquez sur un de ses éléments, à l’origine de la boîte de dialogue Personnalisation. Ensuite, faites glisser un nouvel élément à partir de la **inspecteur de bibliothèque** à la **autorisé des éléments de barre d’outils** zone :

![Les éléments de barre d’outils autorisé dans la boîte de dialogue de personnalisation de barre d’outils](toolbar-images/add01.png "les éléments de barre d’outils autorisé dans la boîte de dialogue de personnalisation de barre d’outils")

Pour vous assurer qu’un nouvel élément fait partie de la barre d’outils par défaut, faites-le glisser vers le **éléments de barre d’outils par défaut** zone : 

![Réorganisation d’un élément de barre d’outils en faisant glisser](toolbar-images/add02.png "réorganisation d’un élément de barre d’outils en faisant glisser")

Pour réorganiser les éléments de barre d’outils par défaut, faites-les glisser gauche ou droite.

Ensuite, utilisez le **inspecteur d’attributs** pour définir les propriétés par défaut pour l’élément :

![Personnalisation d’un élément de barre d’outils à l’aide de l’inspecteur d’attributs](toolbar-images/add03.png "personnalisation d’un élément de barre d’outils à l’aide de l’inspecteur d’attributs")

Les propriétés suivantes sont disponibles :

- **Nom de l’image** -Image à utiliser comme icône pour l’élément
- **Étiquette** -texte à afficher pour l’élément dans la barre d’outils
- **Étiquette de palette** -texte à afficher pour l’élément dans le **autorisé des éléments de barre d’outils** zone
- **Balise** -un identificateur unique facultatif qui permet d’identifier l’élément dans le code.
- **Identificateur** -définit le type d’élément de barre d’outils. Une valeur personnalisée peut être utilisée pour sélectionner un élément de barre d’outils dans le code.
- **Sélectionnable** -si elle est activée, l’élément agit comme un bouton marche/arrêt.

> [!IMPORTANT]
> Ajouter un élément à la **autorisé des éléments de barre d’outils** zone mais pas les outils par défaut pour fournir des options de personnalisation pour les utilisateurs. 

### <a name="adding-other-ui-controls-to-a-toolbar"></a>Ajout d’autres contrôles d’interface utilisateur à une barre d’outils

Plusieurs éléments de l’interface utilisateur Cocoa tels que les champs de recherche et contrôles segmentés peuvent également être ajoutés à une barre d’outils.

Pour tester cela, ouvrez la barre d’outils dans le **hiérarchie des interfaces** et sélectionnez un élément de barre d’outils pour ouvrir la boîte de dialogue Personnalisation. Faites glisser un **champ de recherche** à partir de la **inspecteur de bibliothèque** à la **autorisé des éléments de barre d’outils** zone :

![À l’aide de la boîte de dialogue de personnalisation de barre d’outils](toolbar-images/add05.png "à l’aide de la boîte de dialogue de personnalisation de barre d’outils")

À partir de là, utiliser Interface Builder pour configurer le champ de recherche et l’exposer au code via une action ou de la sortie.

## <a name="built-in-toolbar-item-support"></a>Prise en charge des éléments de barre d’outils intégrée

Plusieurs éléments d’interface utilisateur de Cocoa interagissent avec les éléments de barre d’outils standard par défaut. Par exemple, faites glisser un **affichage de texte** sur la fenêtre de l’application et le positionner pour remplir la zone de contenu :

[![Ajout d’une vue de texte à l’application](toolbar-images/edit09.png "Ajout d’une vue de texte à l’application")](toolbar-images/edit09-large.png#lightbox)

Enregistrer le document, revenez à Visual Studio pour Mac pour se synchroniser avec Xcode, exécutez l’application, entrez du texte, sélectionnez-le, puis cliquez sur le **couleurs** élément de barre d’outils. Notez que l’affichage de texte fonctionne automatiquement avec le sélecteur de couleurs :

![Fonctionnalités de barre d’outils intégrée avec un sélecteur de vue et la couleur du texte](toolbar-images/edit10.png "des fonctionnalités de barre d’outils intégrée avec un sélecteur de vue et la couleur du texte")

## <a name="using-images-with-toolbar-items"></a>L’utilisation d’images avec les éléments de barre d’outils

À l’aide un **élément de barre d’outils Image**, n’importe quelle image bitmap est ajouté à la **ressources** dossier (et une action de génération **groupe de ressources**) peuvent être affichés sur la barre d’outils sous forme d’icône :

1. Dans Visual Studio pour Mac, dans le **panneau solutions**, avec le bouton droit le **ressources** dossier et sélectionnez **ajouter** > **ajouter des fichiers** .
2. À partir de la **ajouter des fichiers** boîte de dialogue zone, accédez aux images de votre choisis, sélectionnez-les et cliquez sur le **Open** bouton : 

    [![Sélection des images à ajouter](toolbar-images/edit11.png "sélection des images à ajouter")](toolbar-images/edit11-large.png#lightbox)

3. Sélectionnez **copie**, vérifiez **utiliser la même action pour tous les fichiers sélectionnés**, puis cliquez sur **OK**:

    ![En sélectionnant l’action de copie pour les images ajoutées](toolbar-images/edit12.png "en sélectionnant l’action de copie pour les images ajoutées.")

4. Dans le **panneau solutions**, double-cliquez sur **MainWindow.xib** pour l’ouvrir dans Xcode.

5. Sélectionnez la barre d’outils dans le **hiérarchie des interfaces** et cliquez sur un de ses éléments pour ouvrir la boîte de dialogue Personnalisation.

6. Faites glisser un **élément de barre d’outils Image** à partir de la **inspecteur de bibliothèque** à la barre d’outils **autorisé des éléments de barre d’outils** zone : 

    ![Un élément de barre d’outils Image ajouté à la zone d’éléments de barre d’outils autorisé](toolbar-images/edit14.png "un élément de barre d’outils Image ajoutée à la zone d’autorisé des éléments de barre d’outils")

7. Dans le **inspecteur d’attributs**, sélectionnez l’image qui vient d’être ajouté dans Visual Studio pour Mac : 

    ![Définition d’une image personnalisée pour un élément de barre d’outils](toolbar-images/edit15.png "définition d’une image personnalisée pour un élément de barre d’outils")

8. Définir le **étiquette** dans « La Corbeille » et le **étiquette de la Palette** « Effacement Document » : 

    ![Définition de la barre d’outils d’élément étiquette et étiquette de Palette](toolbar-images/edit16.png "définition de la barre d’outils d’élément étiquette et étiquette de Palette")

9. Faites glisser un **séparateur de barre d’outils** à partir de la **inspecteur de bibliothèque** à la barre d’outils **autorisé des éléments de barre d’outils** zone : 

    [![Un séparateur de barre d’outils ajoutés à la zone d’éléments de barre d’outils autorisé](toolbar-images/edit17.png "un séparateur de barre d’outils ajouté à la zone d’éléments de barre d’outils autorisé")](toolbar-images/edit17-large.png#lightbox)

10. Faites glisser le séparateur et l’élément « Corbeille » à la **par défaut des éléments de barre d’outils** zone et le jeu d’éléments de l’ordre de la barre d’outils à partir de gauche à droite comme suit (couleurs, polices, séparateur, la Corbeille, espace Flexible, impression) : 

    ![Les éléments de barre d’outils par défaut](toolbar-images/edit18.png "les éléments de barre d’outils par défaut")

11. Enregistrer les modifications et revenir à Visual Studio pour Mac se synchroniser avec Xcode.

Exécutez l’application pour vérifier que la nouvelle barre d’outils s’affiche par défaut :

![Une barre d’outils avec les éléments par défaut personnalisé](toolbar-images/edit19.png "une barre d’outils avec les éléments par défaut personnalisé")

## <a name="exposing-toolbar-items-with-outlets-and-actions"></a>Exposition des éléments de barre d’outils avec outlets et actions

Pour accéder à une barre d’outils ou un élément de barre d’outils dans le code, il doit être attaché à une prise de courant ou une action :

1. Dans le **panneau solutions**, double-cliquez sur **Main.storyboard** pour l’ouvrir dans Xcode.
2. Vérifiez que la classe personnalisée « WindowController » a été affecté au contrôleur de fenêtre principale dans le **inspecteur d’identité**:

    [![À l’aide de l’inspecteur d’identité pour définir une classe personnalisée pour le contrôleur de fenêtre](toolbar-images/edit20a.png "à l’aide de l’inspecteur d’identité pour définir une classe personnalisée pour le contrôleur de fenêtre")](toolbar-images/edit20a-large.png#lightbox)

3. Ensuite, sélectionnez l’élément de barre d’outils dans le **hiérarchie des interfaces**: 

    ![En sélectionnant l’élément de barre d’outils dans la hiérarchie des interfaces](toolbar-images/edit20.png "en sélectionnant l’élément de barre d’outils dans la hiérarchie des interfaces")  

4. Ouvrir le **Assistant vue**, sélectionnez le **WindowController.h** fichier et faites-le glisser à partir de l’élément de barre d’outils à la **WindowController.h** fichier.
5. Définir le **connexion** type **Action**, entrez « trashDocument » pour le **nom**et cliquez sur le **Connect** bouton : 

    [![Configuration d’une action pour un élément de barre d’outils](toolbar-images/edit23.png "configuration d’une action pour un élément de barre d’outils")](toolbar-images/edit23-large.png#lightbox)

6. Exposer le **affichage de texte** comme un outlet appelé « documentEditor » dans le **ViewController.h** fichier : 

    [![Configuration d’un outlet pour l’affichage de texte](toolbar-images/edit24.png "configuration d’un outlet pour l’affichage de texte")](toolbar-images/edit24-large.png#lightbox)

7. Enregistrez vos modifications et revenir à Visual Studio pour Mac se synchroniser avec Xcode.

Dans Visual Studio pour Mac, vous devez modifier le **ViewController.cs** fichier, puis ajoutez le code suivant :

```csharp
public void EraseDocument() {
    documentEditor.Value = "";
}
```

Ensuite, modifiez le **WindowController.cs** fichier, puis ajoutez le code suivant au bas de la `WindowController` classe :

```csharp
[Export ("trashDocument:")]
void TrashDocument (NSObject sender) {

    var controller = ContentViewController as ViewController;
    controller.EraseDocument ();
}
```

Lors de l’exécution de l’application, le **la Corbeille** élément de barre d’outils seront active :

![Une barre d’outils avec un élément de la Corbeille active](toolbar-images/edit25.png "une barre d’outils avec un élément de la Corbeille active")

Notez que le **la Corbeille** barre d’outils élément peut maintenant être utilisé pour supprimer le texte.

## <a name="disabling-toolbar-items"></a>Désactiver les éléments de barre d’outils

Pour désactiver un élément dans une barre d’outils, créez un personnalisé `NSToolbarItem` classe et substituer les `Validate` (méthode). Puis, dans l’Interface Builder, affecter le type personnalisé à l’élément que vous souhaitez activer/désactiver.

Pour créer un personnalisé `NSToolbarItem` classe, avec le bouton droit sur le projet et sélectionnez **ajouter** > **nouveau fichier...** . Sélectionnez **général** > **classe vide**, entrez « ActivatableItem » pour le **nom**, puis cliquez sur le **New** bouton : 

![Ajout d’une classe vide dans Visual Studio pour Mac](toolbar-images/custom01.png "Ajout d’une classe vide dans Visual Studio pour Mac")

Ensuite, modifiez le **ActivatableItem.cs** fichier comme suit :

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

Double-cliquez sur **Main.storyboard** pour l’ouvrir dans Xcode. Sélectionnez le **la Corbeille** élément de barre d’outils créé ci-dessus et remplacez sa classe « ActivatableItem » dans le **inspecteur d’identité**:

![Définition d’une classe personnalisée pour un élément de barre d’outils](toolbar-images/custom02.png "définissant une classe personnalisée pour un élément de barre d’outils")

Créer un outlet appelé `trashItem` pour le **la Corbeille** élément de barre d’outils. Enregistrer les modifications et revenir à Visual Studio pour Mac se synchroniser avec Xcode. Pour finir, ouvrez **MainWindow.cs** et mettre à jour le `AwakeFromNib` méthode comme suit :

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Disable trash
    trashItem.Active = false;
}
```

Exécutez l’application et notez que le **la Corbeille** élément est désormais désactivé dans la barre d’outils :

![Une barre d’outils avec un élément de la Corbeille inactive](toolbar-images/custom03.png "une barre d’outils avec un élément de la Corbeille inactif")

## <a name="summary"></a>Récapitulatif

Cet article a examiné en détail à l’utilisation des barres d’outils et des éléments de barre d’outils dans une application Xamarin.Mac. Il décrit comment créer et gérer des barres d’outils dans Xcode Interface Builder, comment des contrôles d’interface utilisateur fonctionnent automatiquement avec les éléments de barre d’outils, l’utilisation des barres d’outils dans le code c# et comment activer et désactiver des éléments de barre d’outils.


## <a name="related-links"></a>Liens associés

- [MacToolbar (exemple)](https://developer.xamarin.com/samples/mac/MacToolbar/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Human Interface Guidelines pour des barres d’outils](https://developer.apple.com/macos/human-interface-guidelines/windows-and-views/toolbars/)
- [Introduction aux barres d’outils](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/Toolbars/Toolbars.html)
