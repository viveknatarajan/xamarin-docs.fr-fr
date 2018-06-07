---
title: fichiers .xib Xamarin.Mac
description: Cet article traite de travail avec les fichiers .xib créés dans le Générateur de Xcode Interface pour créer et gérer des interfaces utilisateur pour une application Xamarin.Mac.
ms.prod: xamarin
ms.assetid: 6AF3D216-448D-4B2D-9026-74E4FFF5923A
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 3ef536ddb19ed60975368bd022e57c34c6f473dc
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792272"
---
# <a name="xib-files-in-xamarinmac"></a>fichiers .xib Xamarin.Mac

_Cet article traite de travail avec les fichiers .xib créés dans le Générateur de Xcode Interface pour créer et gérer des interfaces utilisateur pour une application Xamarin.Mac._

> [!NOTE]
> La meilleure façon de créer une interface utilisateur pour une application Xamarin.Mac est avec les plans conceptuels. Cette documentation a été laissée en place pour des raisons historiques et de l’utilisation avec les anciens projets Xamarin.Mac. Pour plus d’informations, consultez notre [Introduction à des plans conceptuels](~/mac/platform/storyboards/index.md) documentation.

## <a name="overview"></a>Vue d'ensemble

Lorsque vous travaillez avec c# et .NET dans une application Xamarin.Mac, vous avez accès aux mêmes éléments d’interface utilisateur et des outils fournis par un développeur travaillant dans *Objective-C* et *Xcode* est. Xamarin.Mac s’intègre directement avec Xcode, vous pouvez utiliser de Xcode _Interface Générateur_ pour créer et gérer vos interfaces utilisateur (ou éventuellement de les créer directement dans le code C#).

Un fichier .xib est utilisé par macOS pour définir graphiquement les éléments d’interface utilisateur de votre application (par exemple, les Menus, Windows, les vues, étiquettes, des champs de texte) qui sont créés et gérés dans le Générateur de Xcode Interface.

[![Un exemple de l’application en cours d’exécution](xib-images/intro01.png "un exemple de l’application en cours d’exécution")](xib-images/intro01-large.png#lightbox)

Dans cet article, nous aborderons les principes fondamentaux de l’utilisation de fichiers .xib dans une application Xamarin.Mac. Il est fortement recommandé que vous parcourez les [Hello, Mac](~/mac/get-started/hello-mac.md) de l’article en premier lieu, elle couvre les principaux concepts et techniques que nous utiliserons dans cet article.

Vous souhaiterez peut-être examiner la [classes exposition c# / méthodes pour Objective-C](~/mac/internals/how-it-works.md) section de la [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) de document, elle explique la `Register` et `Export` attributs utilisé pour associer vos classes c# pour les objets Objective-C et de l’interface utilisateur éléments.


## <a name="introduction-to-xcode-and-interface-builder"></a>Introduction à Xcode et Interface Builder

Dans le cadre de Xcode, Apple a créé un outil appelé générateur d’Interface, ce qui vous permet de créer visuellement de votre Interface utilisateur dans un concepteur. Xamarin.Mac intègre fluently Générateur d’Interface, ce qui vous permet de créer votre interface utilisateur avec les mêmes outils que les utilisateurs de Objective-C.


### <a name="components-of-xcode"></a>Composants de Xcode

Lorsque vous ouvrez un fichier .xib dans Xcode à partir de Visual Studio pour Mac, il s’ouvre avec un **Project Navigator** sur la gauche, le **hiérarchie de l’Interface** et **éditeur de l’Interface** au milieu et un **propriétés & utilitaires** section à droite :

[![Les composants de l’UI Xcode](xib-images/xcode03.png "les composants de l’UI Xcode")](xib-images/xcode03-large.png#lightbox)

Nous allons jeter un coup de œil à la fonction de chacun de ces Xcode sections n’a et la façon dont vous les utiliserez pour créer l’interface de votre application Xamarin.Mac.


#### <a name="project-navigation"></a>Navigation dans un projet

Lorsque vous ouvrez un fichier .xib pour le modifier dans Xcode, Visual Studio pour Mac crée un fichier de projet Xcode en arrière-plan pour communiquer des modifications entre lui-même et Xcode. Plus tard, lorsque vous basculez vers Visual Studio pour Mac à partir de Xcode, toutes les modifications apportées à ce projet sont synchronisées avec votre projet Xamarin.Mac par Visual Studio pour Mac.

Le **Navigation dans un projet** section vous permet de naviguer parmi tous les fichiers qui constituent ce _shim_ projet Xcode. En règle générale, vous ne serez intéressé par les fichiers .xib dans cette liste comme **MainMenu.xib** et **MainWindow.xib**.


#### <a name="interface-hierarchy"></a>Hiérarchie des interfaces

Le **hiérarchie de l’Interface** section permet d’accéder facilement plusieurs propriétés de clé de l’Interface utilisateur telles que son **des espaces réservés** et principal **fenêtre**. Cette section vous permet également d’accéder aux éléments individuels (vues) qui rendent votre interface utilisateur et l’ajuster la façon qu’elles sont imbriquées en les faisant glisser autour de la hiérarchie.


#### <a name="interface-editor"></a>Éditeur de l’interface

Le **éditeur de l’Interface** présente la surface sur laquelle vous graphiquement disposition votre de l’Interface utilisateur. Vous allez faire glisser des éléments à partir de la **bibliothèque** section de la **propriétés et les utilitaires** section pour créer votre conception. Lorsque vous ajoutez des éléments d’interface utilisateur (vues) à l’aire de conception, ils seront ajoutés à la **hiérarchie de l’Interface** section dans l’ordre où ils apparaissent dans le **éditeur de l’Interface**.


#### <a name="properties--utilities"></a>Propriétés et utilitaires

Le **propriétés & utilitaires** section est divisée en deux sections principales que nous travaillerons avec, **propriétés** (également appelées inspecteurs) et le **bibliothèque**:

![L’inspecteur de propriété](xib-images/xcode04.png "l’inspecteur de propriété")

Initialement cette section est pratiquement vide, cependant si vous sélectionnez un élément dans le **éditeur de l’Interface** ou **hiérarchie de l’Interface**, le **propriétés** section sera remplie des informations sur l’élément donné et les propriétés que vous pouvez ajuster.

Dans la section **Propriétés**, il existe 8 *onglets d’inspecteurs* différents, comme indiqué dans l’illustration suivante :

[![Vue d’ensemble de tous les inspecteurs](xib-images/xcode05.png "une vue d’ensemble de tous les inspecteurs")](xib-images/xcode05-large.png#lightbox)

De gauche à droite, ces onglets sont :

-   **File Inspector** (Inspecteur de fichier) : l’onglet Inspecteur de fichier affiche des informations sur le fichier, telles que l’emplacement et le nom du fichier Xib qui est en cours de modification.
-   **Aide rapide** : l’onglet Aide rapide fournit une aide contextuelle basée sur la sélection dans Xcode.
-   **Identity Inspector** (Inspecteur d’identité) : l’onglet Inspecteur d’identité fournit des informations sur la vue/le contrôle sélectionné.
-   **Attributs inspecteur** – l’inspecteur d’attributs vous permet de personnaliser les différents attributs de l’affichage de contrôle/sélectionné.
-   **Taille de l’inspecteur de** – l’inspecteur de taille permet de contrôler la taille et le comportement de l’affichage de contrôle/sélectionné de redimensionnement.
-   **L’inspecteur de connexions** – l’inspecteur de connexions affiche les connexions de sortie et l’action des contrôles sélectionnés. Nous allons aborder les sorties et les Actions dans un instant.
-   **L’inspecteur de liaisons** – l’inspecteur de liaisons vous permet de configurer des contrôles afin que leurs valeurs sont automatiquement liés aux modèles de données.
-   **Afficher les effets inspecteur** – l’inspecteur d’effets vue permet de spécifier les effets sur les contrôles, tels que les animations.

Dans le **bibliothèque** section, vous pouvez rechercher des contrôles et les objets à placer sous forme graphique dans le concepteur pour générer votre interface utilisateur :

![Un exemple de l’inspecteur de bibliothèque](xib-images/xcode06.png "un exemple de l’inspecteur de bibliothèque")

Maintenant que vous êtes familiarisé avec l’IDE de Xcode et d’un constructeur d’Interface, examinons l’utiliser pour créer une interface utilisateur.


## <a name="creating-and-maintaining-windows-in-xcode"></a>Création et gestion de windows dans Xcode

La méthode recommandée pour la création de l’Interface utilisateur d’une application Xamarin.Mac est avec les plans conceptuels (consultez notre [Introduction à des plans conceptuels](~/mac/platform/storyboards/index.md) documentation pour plus d’informations) et, par conséquent, tout nouveau projet démarré dans Xamarin.Mac seront Utilisez des animations par défaut.

Pour passer à l’aide d’un .xib interface utilisateur basée sur, procédez comme suit :

1. Ouvrez Visual Studio pour Mac et démarrer un nouveau projet Xamarin.Mac.
2. Dans le **Solution remplissage**, avec le bouton droit sur le projet et sélectionnez **ajouter** > **nouveau fichier...**
3. Sélectionnez **Mac** > **contrôleur Windows**:

    ![Ajoutez un nouveau contrôleur de fenêtre](xib-images/setup00.png "Ajout d’un nouveau contrôleur de fenêtre")
4. Entrez `MainWindow` pour le nom et cliquez sur le **nouveau** bouton :

    ![Ajout d’une nouvelle fenêtre principal](xib-images/setup01.png "Ajout d’une nouvelle fenêtre principale")
5. Avec le bouton droit sur le projet à nouveau, puis sélectionnez **ajouter** > **nouveau fichier...**
6. Sélectionnez **Mac** > **Menu principal**:

    ![Ajout d’un nouveau Menu principal](xib-images/setup02.png "Ajout d’un nouveau Menu principal")
7. Conservez le nom `MainMenu` et cliquez sur le **nouveau** bouton.
8. Dans le **Solution remplissage** sélectionner le **Main.storyboard** fichier, avec le bouton droit et sélectionnez **supprimer**:

    ![En sélectionnant le storyboard principal](xib-images/setup03.png "en sélectionnant le storyboard principal")
9. Dans la boîte de dialogue Supprimer, cliquez sur le **supprimer** bouton :

    ![Confirmer la suppression](xib-images/setup04.png "confirmant la suppression")
10. Dans le **Solution remplissage**, double-cliquez sur le **Info.plist** fichier à ouvrir pour le modifier.
11. Sélectionnez `MainMenu` à partir de la **Interface principale** liste déroulante :

    [![Définition du menu principal](xib-images/setup05.png "définition du menu principal")](xib-images/setup05-large.png#lightbox)
12. Dans le **Solution remplissage**, double-cliquez sur le **MainMenu.xib** fichier à ouvrir pour le modifier dans le Générateur de Xcode Interface.
13. Dans le **bibliothèque inspecteur**, type `object` dans le champ de recherche puis faites glisser une nouvelle **objet** sur l’aire de conception :

    [![Modification du menu principal](xib-images/setup06.png "modification du menu principal")](xib-images/setup06-large.png#lightbox)
14. Dans le **inspecteur d’identité**, entrez `AppDelegate` pour le **classe**:

    [![En sélectionnant le délégué de l’application](xib-images/setup07.png "en sélectionnant le délégué de l’application")](xib-images/setup07-large.png#lightbox)
15. Sélectionnez **propriétaire du fichier** à partir de la **hiérarchie de l’Interface**, basculez vers le **inspecteur de connexion** et faites glisser une ligne à partir du délégué à la `AppDelegate` **Objet** simplement ajouté au projet :

    [![Connectez le délégué de l’application](xib-images/setup08.png "connexion le délégué de l’application")](xib-images/setup08-large.png#lightbox)
16. Enregistrer les modifications et revenez à Visual Studio pour Mac.

Toutes ces modifications en place, de modifier le **AppDelegate.cs** de fichier et le rendre l’aspect suivant :

```csharp
using AppKit;
using Foundation;

namespace MacXib
{
    [Register ("AppDelegate")]
    public class AppDelegate : NSApplicationDelegate
    {
        public MainWindowController mainWindowController { get; set; }

        public AppDelegate ()
        {
        }

        public override void DidFinishLaunching (NSNotification notification)
        {
            // Insert code here to initialize your application
            mainWindowController = new MainWindowController ();
            mainWindowController.Window.MakeKeyAndOrderFront (this);
        }

        public override void WillTerminate (NSNotification notification)
        {
            // Insert code here to tear down your application
        }
    }
}
```

Maintenant la fenêtre d’application principal est définie dans un **.xib** fichier inclus automatiquement dans le projet lors de l’ajout d’un contrôleur de fenêtre. Pour modifier votre conception de windows, dans le **Solution remplissage**, double-cliquez sur le **MainWindow.xib** fichier :

![En sélectionnant le fichier MainWindow.xib](xib-images/edit01.png "en sélectionnant le fichier MainWindow.xib")

La conception de la fenêtre s’ouvre dans le Générateur de Xcode Interface :

[![Modification de la MainWindow.xib](xib-images/edit02.png "modifier le MainWindow.xib")](xib-images/edit02-large.png#lightbox)


### <a name="standard-window-workflow"></a>Flux de travail de fenêtre standard

Pour n’importe quelle fenêtre que vous créez et travaillez dans votre application Xamarin.Mac, le processus est essentiellement le même :

1. Pour les nouvelles fenêtres qui ne sont pas ajouté automatiquement à votre projet par défaut, ajoutez une nouvelle définition de la fenêtre pour le projet.
2. Double-cliquez sur le fichier .xib pour ouvrir la fenêtre de conception pour la modification dans le Générateur de Xcode Interface.
3. Définir les propriétés de la fenêtre requise dans le **inspecteur de l’attribut** et **taille inspecteur**.
4. Faites glisser les contrôles requis pour générer votre interface et de les configurer dans le **inspecteur de l’attribut**.
5. Utilisez le **taille inspecteur** pour gérer le redimensionnement de vos éléments d’interface utilisateur.
6. Exposer les éléments d’interface utilisateur de la fenêtre de code c# via les sorties et les actions.
7. Enregistrez vos modifications et revenez à Visual Studio pour Mac pour la synchronisation avec Xcode.


### <a name="designing-a-window-layout"></a>Conception d’une disposition de fenêtre

Le processus qui permet de disposer d’une Interface utilisateur dans le Générateur de Interface est fondamentalement le même pour chaque élément que vous ajoutez :

1. Rechercher le contrôle souhaité dans la **bibliothèque inspecteur** et faites-la glisser dans le **éditeur de l’Interface** et placez-le.
2. Définir les propriétés de la fenêtre requise dans le **inspecteur de l’attribut**.
3. Utilisez le **taille inspecteur** pour gérer le redimensionnement de vos éléments d’interface utilisateur.
4. Si vous utilisez une classe personnalisée, définissez-le le **inspecteur d’identité**.
5. Exposer les éléments d’interface utilisateur au code c# via prises et actions.
6. Enregistrez vos modifications et revenez à Visual Studio pour Mac pour la synchronisation avec Xcode.

Par exemple :

1. Dans Xcode, faites glisser un **bouton de commande** à partir de la **section Bibliothèque** :

    [![Sélection d’un bouton de la bibliothèque](xib-images/xcode07.png "en sélectionnant un bouton de la bibliothèque")](xib-images/xcode07-large.png#lightbox)
2. Relâchez le bouton sur le **fenêtre** dans les **éditeur de l’Interface**:

    [![Ajout d’un bouton dans la fenêtre](xib-images/xcode08.png "Ajout d’un bouton dans la fenêtre")](xib-images/xcode08-large.png#lightbox)
3. Cliquez sur la propriété **Titre** dans l’**inspecteur d’attributs** et remplacez le titre du bouton par `Click Me` :

    ![Définir les attributs du bouton](xib-images/xcode09.png "définissant les attributs de bouton")
4. Faites glisser une **étiquette** à partir de la **section Bibliothèque** :

    [![Sélection d’une étiquette dans la bibliothèque](xib-images/xcode10.png "en sélectionnant une étiquette dans la bibliothèque")](xib-images/xcode10-large.png#lightbox)
5. Placez l’étiquette sur la **fenêtre** à côté du bouton dans l’**Éditeur d’interface** :

    [![Ajout d’une étiquette à la fenêtre](xib-images/xcode11.png "Ajout d’une étiquette à la fenêtre")](xib-images/xcode11-large.png#lightbox)
6. Saisissez la poignée droite de l’étiquette et faites-la glisser jusqu’à ce qu’elle soit près du bord de la fenêtre :

    [![Redimensionnement de l’étiquette](xib-images/xcode12.png "l’étiquette de redimensionnement")](xib-images/xcode12-large.png#lightbox)
7. Avec l’étiquette toujours sélectionné dans le **éditeur de l’Interface**, basculez vers le **taille inspecteur**:

    ![Sélection de l’inspecteur de taille](xib-images/xcode13.png "en sélectionnant l’inspecteur de taille")
8. Dans le **boîte de redimensionnement automatique** cliquez sur le **Dim rouge crochet** à droite et la **Dim flèche horizontale de rouge** dans le centre de :

    ![Modifier les propriétés de redimensionnement automatique](xib-images/xcode14.png "modifiant les propriétés de redimensionnement automatique")
9. Cela garantit que l’étiquette s’étire pour augmenter et réduire pendant le redimensionnement de la fenêtre de l’application en cours d’exécution. Le **rouge crochets** et le haut et à gauche de la **boîte de redimensionnement automatique** zone indiquent l’étiquette à être bloquée à donnée de X et Y emplacements.
10. Enregistrez vos modifications dans l’Interface utilisateur

Quand vous ont été redimensionnement et le déplacement des contrôles autour, vous avez devez remarqué que Interface générateur vous donne des indicateurs de composant logiciel enfichable utiles qui sont basées sur [indications de l’Interface du système d’exploitation X humaine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). Ces recommandations vous aideront à créer des applications de haute qualité qui auront un aspect familier pour les utilisateurs Mac.

Si vous regardez dans le **hiérarchie de l’Interface** section, notez la manière dont la disposition et la hiérarchie des éléments qui composent notre Interface utilisateur sont affichés :

![Sélection d’un élément dans la hiérarchie de l’Interface](xib-images/xcode15.png "sélectionner un élément dans la hiérarchie des interfaces")

À ce stade, vous pouvez sélectionner des éléments à modifier, ou faites glisser pour réorganiser les éléments d’interface utilisateur, si nécessaire. Par exemple, si un élément d’interface utilisateur a été réalisée par un autre élément, vous impossible le glisser vers le bas de la liste pour le rendre l’élément supérieur de la fenêtre.

Pour plus d’informations sur l’utilisation de Windows dans une application Xamarin.Mac, veuillez consulter notre [Windows](~/mac/user-interface/window.md) documentation.


## <a name="exposing-ui-elements-to-c-code"></a>Exposer les éléments d’interface utilisateur pour le code c#

Lorsque vous avez terminé la disposition de l’apparence de votre interface utilisateur dans le constructeur d’Interface, vous devez exposer les éléments de l’interface utilisateur pour qu’ils sont accessibles à partir de code c#. Pour ce faire, vous allez utiliser prises et actions.


### <a name="setting-a-custom-main-window-controller"></a>Définition d’un contrôleur personnalisé de fenêtre principale

Pour être en mesure de créer des sorties et les Actions pour exposer les éléments d’interface utilisateur au code c#, l’application Xamarin.Mac doit être un contrôleur de fenêtre personnalisée à l’aide de.

Effectuez ce qui suit :

1. Ouvrez le plan conceptuel de l’application dans le Générateur de Xcode Interface.
2. Sélectionnez le `NSWindowController` dans l’aire de conception.
3. Basculez vers le **inspecteur d’identité** afficher et entrez `WindowController` en tant que le **nom de la classe**:

    [![Modification du nom de la classe](xib-images/windowcontroller01.png "modification du nom de classe")](xib-images/windowcontroller01-large.png#lightbox)
4. Enregistrez vos modifications et revenir à Visual Studio pour Mac à synchroniser.
5. A **WindowController.cs** fichier sera ajouté à votre projet dans le **Solution remplissage** dans Visual Studio pour Mac :

    ![Le nouveau nom de classe dans Visual Studio pour Mac](xib-images/windowcontroller02.png "le nouveau nom de classe dans Visual Studio pour Mac")
6. Rouvrez le plan conceptuel dans le Générateur de Xcode Interface.
7. Le **WindowController.h** fichier sera disponible pour une utilisation :

    [![Le fichier .h correspondant dans Xcode](xib-images/windowcontroller03.png "le fichier .h correspondant dans Xcode")](xib-images/windowcontroller03-large.png#lightbox)


### <a name="outlets-and-actions"></a>Prises et actions

Quelles sont donc prises et actions ? Dans la programmation de l’interface utilisateur .NET traditionnelle, un contrôle dans l’interface utilisateur est automatiquement exposé en tant que propriété quand il est ajouté. Le fonctionnement est différent dans Mac : le simple ajout d’un contrôle à une vue ne le rend pas accessible au code. Le développeur doit exposer explicitement l’élément d’interface utilisateur au code. Dans l’ordre pour cela, Apple nous offre deux options :

-  **Outlets** : les outlets ressemblent à des propriétés. Si vous rattachez un contrôle à une prise de courant, il est exposé à votre code via une propriété, afin d’effectuer diverses tâches telles que l’attachement des gestionnaires d’événements, d’appeler des méthodes sur, etc.
-  **Actions** : les actions ressemblent au modèle de commande dans WPF. Par exemple, lorsqu’une Action est effectuée sur un contrôle, par exemple un clic de bouton, le contrôle appelle automatiquement une méthode dans votre code. Actions sont pratique et puissant, car vous pouvez associer plusieurs contrôles à la même Action.

Dans Xcode, les sorties et les actions sont ajoutées directement dans le code via *faisant glisser le contrôle*. Plus précisément, cela signifie que pour créer une sortie ou une action, vous choisissez que vous souhaitez ajouter une sortie ou une action, appuyez sur l’élément contrôle le **contrôle** bouton sur le clavier et faites glisser ce contrôle directement dans votre code.

Pour les développeurs Xamarin.Mac, cela signifie que vous faites glisser dans les fichiers stub Objective-C qui correspondent au fichier c# où vous souhaitez créer l’action ou sortie. Visual Studio pour Mac a créé un fichier appelé **MainWindow.h** dans le cadre du projet Xcode shim il généré pour utiliser le Générateur de l’Interface :

[![Un exemple de fichier .h dans Xcode](xib-images/xcode16.png "un exemple de fichier .h dans Xcode")](xib-images/xcode16-large.png#lightbox)

Ce fichier .h de stub reflète le **MainWindow.designer.cs** qui est automatiquement ajouté à un projet Xamarin.Mac lorsqu’un nouveau `NSWindow` est créé. Ce fichier permet de synchroniser les modifications apportées par le constructeur d’Interface et est où nous allons créer vos points de vente et les actions afin que les éléments d’interface utilisateur sont exposés au code c#.


#### <a name="adding-an-outlet"></a>Ajout d’une prise de courant

Avec quelques connaissances de base quelles sont les actions et les sorties, examinons la création d’une prise pour exposer un élément d’interface utilisateur à votre code c#.

Effectuez ce qui suit :

1. Dans Xcode, dans le coin supérieur tout à droite de l’écran, cliquez sur le bouton **Double Circle** (Cercle double) pour ouvrir l’**Assistant Editor** (Éditeur d’Assistant) :

    [![Sélection de l’éditeur de l’Assistant](xib-images/outlet01.png "en sélectionnant l’éditeur de l’Assistant")](xib-images/outlet01-large.png#lightbox)
2. Xcode passe en mode fractionné avec l’**Éditeur d’interface** d’un côté et un **Éditeur de code** de l’autre.
3. Notez que Xcode a sélectionnée automatiquement le **MainWindowController.m** de fichiers dans le **éditeur de Code**, qui est incorrect. Si vous vous souvenez de notre discussion sur ce que sont prises et les actions ci-dessus, nous devons disposer le **MainWindow.h** sélectionné.
4. En haut de la **éditeur de Code** cliquez sur le **lien automatique** et sélectionnez le **MainWindow.h** fichier :

    [![En sélectionnant le fichier .h correct](xib-images/outlet02.png "en sélectionnant le fichier .h correct")](xib-images/outlet02-large.png#lightbox)
5. Le fichier approprié doit maintenant être sélectionné pour Xcode :

    [![Le fichier approprié sélectionné](xib-images/outlet03.png "le fichier approprié est sélectionné")](xib-images/outlet03-large.png#lightbox)
6. **La dernière étape était très importante !** Si vous n’avez pas le fichier approprié est sélectionné, vous ne pourrez pas créer prises et actions ou qu’ils seront exposées à la classe incorrecte dans c# !
7. Dans le **éditeur de l’Interface**, maintenez enfoncée la touche la **contrôle** touche du clavier et cliquez et faites glisser l’étiquette créée précédemment sur l’éditeur de code sous le `@interface MainWindow : NSWindow { }` code :

    [![Déplacement pour créer un nouveau magasin](xib-images/outlet04.png "déplacement pour créer un nouveau magasin")](xib-images/outlet04-large.png#lightbox)
8. Une boîte de dialogue s’affiche. Laissez le **connexion** valeur prise et entrez `ClickedLabel` pour le **nom**:

    [![Définition des propriétés de sortie](xib-images/outlet05.png "définissant les propriétés de sortie")](xib-images/outlet05-large.png#lightbox)
9. Cliquez sur le **Connect** bouton permettant de créer la sortie :

    ![La sortie terminée](xib-images/outlet06.png "la sortie terminée")
10. Enregistrez les modifications dans le fichier.


#### <a name="adding-an-action"></a>Ajout d’une action

Ensuite, nous allons examiner à la création d’une action afin d’exposer une interaction utilisateur avec l’élément d’interface utilisateur à votre code c#.

Effectuez ce qui suit :

1. Assurez-vous que nous sommes toujours dans le **Assistant éditeur** et **MainWindow.h** fichier est visible dans le **éditeur de Code**.
2. Dans le **éditeur de l’Interface**, maintenez enfoncée la touche la **contrôle** touche du clavier et cliquez et faites glisser le bouton créée précédemment sur l’éditeur de code sous le `@property (assign) IBOutlet NSTextField *ClickedLabel;` code :

    [![Pour créer une Action de déplacement](xib-images/action01.png "déplacement pour créer une Action")](xib-images/action01-large.png#lightbox)
3. Modifier la **connexion** type d’action :

    [![Sélectionnez un type d’Action](xib-images/action02.png "sélectionner un type d’Action")](xib-images/action02-large.png#lightbox)
4. Entrez `ClickedButton` comme **Nom** :

    [![Configuration de l’Action](xib-images/action03.png "configuration de l’Action")](xib-images/action03-large.png#lightbox)
5. Cliquez sur le **Connect** bouton permettant de créer l’action :

    ![L’Action terminée](xib-images/action04.png "l’Action terminée")
6. Enregistrez les modifications dans le fichier.

Avec votre Interface utilisateur accès câblé et exposée au code c#, revenez à Visual Studio pour Mac et permettent de synchroniser les modifications de Xcode et Générateur de l’Interface.


### <a name="writing-the-code"></a>L’écriture du code

Avec votre Interface utilisateur créée et ses éléments d’interface utilisateur exposées au code via les sorties et les actions, vous êtes prêt à écrire le code pour faire de votre programme. Par exemple, ouvrez le **MainWindow.cs** fichier pour le modifier en double-cliquant dessus dans le **Solution remplissage**:

[![Le fichier MainWindow.cs](xib-images/code01.png "MainWindow.cs le fichier")](xib-images/code01-large.png#lightbox)

Ajoutez le code suivant à la `MainWindow` classe fonctionne avec la sortie d’exemple que vous avez créé précédemment :

```csharp
private int numberOfTimesClicked = 0;
...

public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Set the initial value for the label
    ClickedLabel.StringValue = "Button has not been clicked yet.";
}
```

Notez que la `NSLabel` est accessible dans le langage c# par le nom direct que vous affectés dans Xcode lors de la création de sa sortie dans Xcode, dans ce cas, elle est appelée `ClickedLabel`. Vous pouvez accéder à toute méthode ou propriété de l’objet exposé la même façon que vous le feriez pour n’importe quelle classe c# normal.

> [!IMPORTANT]
> Vous devez utiliser `AwakeFromNib`, au lieu d’une autre méthode telle que `Initialize`, car `AwakeFromNib` est appelé _après_ le système d’exploitation a chargé et instancié l’Interface utilisateur à partir du fichier .xib. Si vous avez tenté d’accéder au contrôle d’étiquette avant le fichier .xib a été entièrement chargé et instancié, vous obtenez un `NullReferenceException` erreur étant donné que le contrôle d’étiquette n’est pas encore créé.

Ensuite, ajoutez la classe partielle suivante à la `MainWindow` classe :

```csharp
partial void ClickedButton (Foundation.NSObject sender) {

    // Update counter and label
    ClickedLabel.StringValue = string.Format("The button has been clicked {0} time{1}.",++numberOfTimesClicked, (numberOfTimesClicked < 2) ? "" : "s");
}
```

Ce code attache à l’action que vous avez créé dans Xcode et Interface générateur sera être appelée lorsque l’utilisateur clique sur le bouton.

Certains éléments d’interface utilisateur ont créées automatiquement dans actions, par exemple, dans la barre de menus par défaut des éléments tels que le **ouvrir...**  élément de menu (`openDocument:`). Dans le **remplissage de la Solution**, double-cliquez sur le **AppDelegate.cs** fichier à ouvrir pour le modifier et ajoutez le code suivant ci-dessous le `DidFinishLaunching` méthode :

```csharp
[Export ("openDocument:")]
void OpenDialog (NSObject sender)
{
    var dlg = NSOpenPanel.OpenPanel;
    dlg.CanChooseFiles = false;
    dlg.CanChooseDirectories = true;

    if (dlg.RunModal () == 1) {
        var alert = new NSAlert () {
            AlertStyle = NSAlertStyle.Informational,
            InformativeText = "At this point we should do something with the folder that the user just selected in the Open File Dialog box...",
            MessageText = "Folder Selected"
        };
        alert.RunModal ();
    }
}
```

La ligne de clé est `[Export ("openDocument:")]`, elle indique `NSMenu` qui le **AppDelegate** a une méthode `void OpenDialog (NSObject sender)` qui répond à la `openDocument:` action.

Pour plus d’informations sur l’utilisation des Menus, consultez notre [Menus](~/mac/user-interface/menu.md) documentation.


## <a name="synchronizing-changes-with-xcode"></a>Synchronisation des modifications avec Xcode

Lorsque vous basculez vers Visual Studio pour Mac à partir de Xcode, toutes les modifications que vous avez apportées dans Xcode sont automatiquement synchronisées avec votre projet Xamarin.Mac.

Si vous sélectionnez le **MainWindow.designer.cs** dans les **Solution remplissage** vous serez en mesure de voir comment notre prise et l’action ont été associés dans notre code c# :

[![Synchronisation des modifications avec Xcode](xib-images/sync01.png "synchronisation des modifications avec Xcode")](xib-images/sync01-large.png#lightbox)

Notez comment les deux définitions dans le **MainWindow.designer.cs** fichier :

```csharp
[Outlet]
AppKit.NSTextField ClickedLabel { get; set; }

[Action ("ClickedButton:")]
partial void ClickedButton (Foundation.NSObject sender);
```

Aligner sur les définitions dans le **MainWindow.h** fichier dans Xcode :

```objc
@property (assign) IBOutlet NSTextField *ClickedLabel;
- (IBAction)ClickedButton:(id)sender;
```

Comme vous pouvez le voir, Visual Studio pour Mac écoute les modifications apportées au fichier .h, puis synchronise automatiquement ces modifications dans la **. designer.cs** fichier les exposer à votre application. Vous pouvez également remarquer que **MainWindow.designer.cs** est une classe partielle, afin que Visual Studio pour Mac ne doit pas nécessairement modifier **MainWindow.cs** qui remplace toutes les modifications que nous avons apporté à la classe.

Normalement, vous n’avez jamais besoin ouvrir la **MainWindow.designer.cs** vous-même, il a été présenté ici à des fins pédagogiques uniquement.

> [!IMPORTANT]
> Dans la plupart des cas, Visual Studio pour Mac voir les modifications effectuées dans Xcode automatiquement et les synchroniser à votre projet Xamarin.Mac. Dans le cas isolé où la synchronisation ne se produit pas automatiquement, revenez à Xcode, puis à nouveau à Visual Studio pour Mac. Cela déclenche normalement un cycle de synchronisation.


## <a name="adding-a-new-window-to-a-project"></a>Ajout d’une nouvelle fenêtre à un projet

À l’exception de la fenêtre de document principal, une application Xamarin.Mac devrez peut-être afficher d’autres types de windows à l’utilisateur, telles que les préférences ou les panneaux de l’inspecteur. Lors de l’ajout d’une nouvelle fenêtre à votre projet, vous devez toujours utiliser le **fenêtre/Cocoa avec contrôleur** option, car cela rend le processus de chargement de la fenêtre à partir de la .xib de fichiers plus facilement.

Pour ajouter une nouvelle fenêtre, procédez comme suit :

1. Dans le **Solution remplissage**, avec le bouton droit sur le projet et sélectionnez **ajouter** > **nouveau fichier...** .
2. Dans la boîte de dialogue Nouveau fichier, sélectionnez **Xamarin.Mac** > **fenêtre/Cocoa avec contrôleur**:

    ![Ajout d’un nouveau contrôleur de fenêtre](xib-images/new01.png "Ajout d’un nouveau contrôleur de fenêtre")
3. Entrez `PreferencesWindow` comme **Nom**, puis cliquez sur le bouton **Nouveau**.
4. Double-cliquez sur le **PreferencesWindow.xib** fichier à ouvrir pour le modifier dans le Générateur de Interface :

    [![Modification de la fenêtre dans Xcode](xib-images/new02.png "modification de la fenêtre dans Xcode")](xib-images/new02-large.png#lightbox)
5. Concevoir votre interface :

    [![Définition de la disposition de windows](xib-images/new03.png "pour concevoir la disposition de windows")](xib-images/new03-large.png#lightbox)
6. Enregistrez vos modifications et revenir à Visual Studio pour Mac pour la synchronisation avec Xcode.

Ajoutez le code suivant à **AppDelegate.cs** pour afficher votre nouvelle fenêtre :

```csharp
[Export("applicationPreferences:")]
void ShowPreferences (NSObject sender)
{
    var preferences = new PreferencesWindowController ();
    preferences.Window.MakeKeyAndOrderFront (this);
}
```

Le `var preferences = new PreferencesWindowController ();` ligne crée une nouvelle instance du contrôleur de fenêtre qui affiche la fenêtre à partir du fichier .xib et l’augmente. Le `preferences.Window.MakeKeyAndOrderFront (this);` ligne affiche la nouvelle fenêtre pour l’utilisateur.

Si vous exécutez le code et sélectionnez le **préférences...**  à partir de la **Menu Application**, la fenêtre s’affichera :

![L’exemple d’application en cours d’exécution](xib-images/new04.png "l’exemple d’application en cours d’exécution")

Pour plus d’informations sur l’utilisation de Windows dans une application Xamarin.Mac, veuillez consulter notre [Windows](~/mac/user-interface/window.md) documentation.


## <a name="adding-a-new-view-to-a-project"></a>Ajout d’une nouvelle vue à un projet

Il est parfois plus facile de conception de votre fenêtre se répartissent en plusieurs, fichiers .xib plus facile à gérer. Par exemple, comme le contenu de la fenêtre principale de commutation lors de la sélection d’un élément de barre d’outils dans une **fenêtre Préférences** ou d’échanger du contenu en réponse à une **liste Source** sélection.

Lors de l’ajout d’une nouvelle vue à votre projet, vous devez toujours utiliser le **affichage/Cocoa avec contrôleur** option, car cela rend le processus de chargement de la vue à partir de la .xib de fichiers plus facilement.

Pour ajouter une nouvelle vue, procédez comme suit :

1. Dans le **Solution remplissage**, avec le bouton droit sur le projet et sélectionnez **ajouter** > **nouveau fichier...** .
2. Dans la boîte de dialogue Nouveau fichier, sélectionnez **Xamarin.Mac** > **affichage/Cocoa avec contrôleur**:

    ![Ajout d’une nouvelle vue](xib-images/view01.png "Ajout d’un nouvel affichage")
3. Entrez `SubviewTable` comme **Nom**, puis cliquez sur le bouton **Nouveau**.
4. Double-cliquez sur le **SubviewTable.xib** fichier à ouvrir pour le modifier dans le Générateur de Interface et concevoir l’Interface utilisateur :

    [![Conception de la nouvelle vue dans Xcode](xib-images/view02.png "conception de la nouvelle vue dans Xcode")](xib-images/view02-large.png#lightbox)
5. Rattachez toutes les actions requises et les sorties.
6. Enregistrez vos modifications et revenir à Visual Studio pour Mac pour la synchronisation avec Xcode.

Ensuite modifier la **SubviewTable.cs** et ajoutez le code suivant à la **AwakeFromNib** fichier pour remplir la nouvelle vue quand il est chargé :

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Create the Product Table Data Source and populate it
    var DataSource = new ProductTableDataSource ();
    DataSource.Products.Add (new Product ("Xamarin.iOS", "Allows you to develop native iOS Applications in C#"));
    DataSource.Products.Add (new Product ("Xamarin.Android", "Allows you to develop native Android Applications in C#"));
    DataSource.Products.Add (new Product ("Xamarin.Mac", "Allows you to develop Mac native Applications in C#"));
    DataSource.Sort ("Title", true);

    // Populate the Product Table
    ProductTable.DataSource = DataSource;
    ProductTable.Delegate = new ProductTableDelegate (DataSource);

    // Auto select the first row
    ProductTable.SelectRow (0, false);
}
```

Ajouter un enum au projet pour effectuer le suivi de vue qui est en cours d’affichage. Par exemple, **SubviewType.cs**:

```csharp
public enum SubviewType
{
    None,
    TableView,
    OutlineView,
    ImageView
}
```

Modifiez le fichier .xib de la fenêtre qui consomme de la vue et de l’afficher. Ajouter un **affichage personnalisé** qui agit comme conteneur pour la vue une fois qu’il est chargé en mémoire par le code c# et l’exposer à une sortie appelé `ViewContainer`:

[![Création de la sortie requise](xib-images/view03.png "création de la sortie requise")](xib-images/view03-large.png#lightbox)

Enregistrez vos modifications et revenir à Visual Studio pour Mac pour la synchronisation avec Xcode.

Ensuite, modifiez le fichier .cs de la fenêtre qui affichera le nouvel affichage (par exemple, **MainWindow.cs**) et ajoutez le code suivant :

```csharp
private SubviewType ViewType = SubviewType.None;
private NSViewController SubviewController = null;
private NSView Subview = null;
...

private void DisplaySubview(NSViewController controller, SubviewType type) {

    // Is this view already displayed?
    if (ViewType == type) return;

    // Is there a view already being displayed?
    if (Subview != null) {
        // Yes, remove it from the view
        Subview.RemoveFromSuperview ();

        // Release memory
        Subview = null;
        SubviewController = null;
    }

    // Save values
    ViewType = type;
    SubviewController = controller;
    Subview = controller.View;

    // Define frame and display
    Subview.Frame = new CGRect (0, 0, ViewContainer.Frame.Width, ViewContainer.Frame.Height);
    ViewContainer.AddSubview (Subview);
}
```

Lorsque nous aurons besoin afficher une nouvelle vue chargé à partir d’un fichier .xib dans conteneur la fenêtre (les **affichage personnalisé** ajoutées ci-dessus), ce code gère la suppression d’une vue existante et permuter pour une nouvelle. Il vérifie qu’il que vous disposez déjà d’un affichage, si par conséquent, il la supprime de l’écran. Ensuite il accepte la vue qui a été passée dans (tel qu’il est chargé à partir d’un contrôleur de vue) est redimensionné pour s’ajuster à la zone de contenu et l’ajoute à du contenu à afficher.

Pour afficher une nouvelle vue, utilisez le code suivant :

```csharp
DisplaySubview(new SubviewTableController(), SubviewType.TableView);
```

Cela crée une instance du contrôleur d’affichage pour la nouvelle vue à afficher, définit son type (comme spécifié par l’énumération ajoutée au projet) et utilise le `DisplaySubview` méthode ajoutée à la classe de la fenêtre pour afficher la vue. Exemple :

[![L’exemple d’application en cours d’exécution](xib-images/view04.png "l’exemple d’application en cours d’exécution")](xib-images/view04-large.png#lightbox)

Pour plus d’informations sur l’utilisation de Windows dans une application Xamarin.Mac, veuillez consulter notre [Windows](~/mac/user-interface/window.md) et [boîtes de dialogue](~/mac/user-interface/dialog.md) documentation.


## <a name="summary"></a>Récapitulatif

Cet article a pris une présentation détaillée sur l’utilisation des fichiers .xib dans une application Xamarin.Mac. Nous avons vu les différents types et les utilisations des fichiers de .xib pour créer l’Interface utilisateur de votre application, comment créer et gérer des .xib des fichiers dans Xcode l’Interface générateur et comment travailler avec des fichiers .xib en code c#.


## <a name="related-links"></a>Liens associés

- [MacImages (exemple)](https://developer.xamarin.com/samples/mac/MacImages/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Fenêtres](~/mac/user-interface/window.md)
- [Menus](~/mac/user-interface/menu.md)
- [Boîtes de dialogue](~/mac/user-interface/dialog.md)
- [Utilisation des images](~/mac/app-fundamentals/image.md)
- [macOS indications de l’Interface humaine](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
