---
title: fichiers .xib dans Xamarin.Mac
description: Cet article aborde l’utilisation avec les fichiers .xib créés dans l’Interface Builder de Xcode pour créer et gérer des interfaces utilisateur pour une application Xamarin.Mac.
ms.prod: xamarin
ms.assetid: 6AF3D216-448D-4B2D-9026-74E4FFF5923A
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 45eeee745b133646aef0f775bc879fa6a5d867c7
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61062642"
---
# <a name="xib-files-in-xamarinmac"></a>fichiers .xib dans Xamarin.Mac

_Cet article aborde l’utilisation avec les fichiers .xib créés dans l’Interface Builder de Xcode pour créer et gérer des interfaces utilisateur pour une application Xamarin.Mac._

> [!NOTE]
> Il est préférable de créer une interface utilisateur pour une application Xamarin.Mac avec des storyboards. Cette documentation a été laissée en place pour des raisons historiques et pour travailler avec les anciens projets Xamarin.Mac. Pour plus d’informations, consultez notre [Introduction aux Storyboards](~/mac/platform/storyboards/index.md) documentation.

## <a name="overview"></a>Vue d'ensemble

Lorsque vous travaillez avec C# et .NET dans une application Xamarin.Mac, vous avez accès aux mêmes éléments d’interface utilisateur et des outils fournis par un développeur qui travaille *Objective-C* et *Xcode* est. Comme Xamarin.Mac s’intègre directement à Xcode, vous pouvez utiliser de Xcode _Interface Builder_ pour créer et gérer vos interfaces utilisateur (ou éventuellement pour les créer directement en code c#).

Un fichier .xib est utilisé par macOS pour définir graphiquement les éléments d’interface utilisateur de votre application (par exemple, les Menus, les Windows, les vues, les étiquettes, les champs de texte) qui sont créés et gérés dans l’Interface Builder de Xcode.

[![Un exemple de l’application en cours d’exécution](xib-images/intro01.png "un exemple de l’application en cours d’exécution")](xib-images/intro01-large.png#lightbox)

Dans cet article, nous traiterons les notions de base de l’utilisation des fichiers .xib dans une application Xamarin.Mac. Il est fortement recommandé que vous travaillez via le [Hello, Mac](~/mac/get-started/hello-mac.md) article tout d’abord, car elle traite des principaux concepts et techniques que nous utilisons dans cet article.

Vous pouvez souhaiter de jeter un coup de œil à la [C# exposition de classes / méthodes vers Objective-C](~/mac/internals/how-it-works.md) section de la [éléments internes de Xamarin.Mac](~/mac/internals/how-it-works.md) de document, il explique le `Register` et `Export` attributs utilisé pour structurer vos classes c# pour les objets Objective-C et de l’interface utilisateur éléments.


## <a name="introduction-to-xcode-and-interface-builder"></a>Introduction à Xcode et Interface Builder

Dans le cadre de Xcode, Apple a créé un outil appelé Interface Builder, ce qui vous permet de créer votre Interface utilisateur visuellement dans un concepteur. Xamarin.Mac s’intègre facilement avec Interface Builder, ce qui vous permet de créer votre interface utilisateur avec les mêmes outils que font les utilisateurs Objective-C.


### <a name="components-of-xcode"></a>Composants de Xcode

Lorsque vous ouvrez un fichier .xib dans Xcode à partir de Visual Studio pour Mac, il s’ouvre avec un **Project Navigator** sur la gauche, le **hiérarchie des interfaces** et **Éditeur d’Interface** au milieu et un **propriétés et utilitaires** sur la droite :

[![Les composants de l’UI Xcode](xib-images/xcode03.png "les composants de l’UI Xcode")](xib-images/xcode03-large.png#lightbox)

Jetons un œil à chacune de ces Xcode sections n’et la façon dont vous les utiliserez pour créer l’interface de votre application Xamarin.Mac.


#### <a name="project-navigation"></a>Navigation dans le projet

Lorsque vous ouvrez un fichier .xib pour modification dans Xcode, Visual Studio pour Mac crée un fichier de projet Xcode en arrière-plan pour communiquer les modifications entre lui-même et Xcode. Plus tard, lorsque vous revenez à Visual Studio pour Mac à partir de Xcode, toutes les modifications apportées à ce projet sont synchronisées avec votre projet Xamarin.Mac par Visual Studio pour Mac.

Le **Navigation dans le projet** section vous permet de naviguer entre tous les fichiers qui constituent ce _shim_ projet Xcode. En règle générale, vous serez uniquement intéressé par les fichiers .xib dans cette liste comme **MainMenu.xib** et **MainWindow.xib**.


#### <a name="interface-hierarchy"></a>Hiérarchie des interfaces

Le **hiérarchie des interfaces** section vous permet d’accéder facilement aux plusieurs propriétés clés de l’Interface utilisateur telles que son **des espaces réservés** et principal **fenêtre**. Vous pouvez également utiliser cette section pour accéder aux éléments individuels (vues) qui rendent votre interface utilisateur et ajuster la façon dont ils sont imbriqués en les faisant glisser dans la hiérarchie.


#### <a name="interface-editor"></a>Éditeur d’interface

Le **Éditeur d’Interface** section fournit la surface sur laquelle vous graphiquement disposition votre de l’Interface utilisateur. Vous allez faire glisser depuis le **bibliothèque** section de la **propriétés et utilitaires** section pour créer votre conception. Lorsque vous ajoutez des éléments d’interface utilisateur (vues) à l’aire de conception, ils seront ajoutés à la **hiérarchie des interfaces** section dans l’ordre où ils apparaissent dans le **Éditeur d’Interface**.


#### <a name="properties--utilities"></a>Propriétés et utilitaires

Le **propriétés et utilitaires** section est divisée en deux sections principales que nous travaillons avec, **propriétés** (également appelées inspecteurs) et le **bibliothèque**:

![L’inspecteur de propriété](xib-images/xcode04.png "l’inspecteur de propriété")

Initialement, cette section est pratiquement vide, cependant si vous sélectionnez un élément dans le **Éditeur d’Interface** ou **hiérarchie des interfaces**, le **propriétés** section sera remplie avec des informations sur l’élément donné et les propriétés que vous pouvez ajuster.

Dans la section **Propriétés**, il existe 8 *onglets d’inspecteurs* différents, comme indiqué dans l’illustration suivante :

[![Une vue d’ensemble de tous les inspecteurs](xib-images/xcode05.png "une vue d’ensemble de tous les inspecteurs")](xib-images/xcode05-large.png#lightbox)

De gauche à droite, ces onglets sont :

-   **File Inspector** (Inspecteur de fichier) : l’onglet Inspecteur de fichier affiche des informations sur le fichier, telles que l’emplacement et le nom du fichier Xib qui est en cours de modification.
-   **Aide rapide** : l’onglet Aide rapide fournit une aide contextuelle basée sur la sélection dans Xcode.
-   **Identity Inspector** (Inspecteur d’identité) : l’onglet Inspecteur d’identité fournit des informations sur la vue/le contrôle sélectionné.
-   **Attributes Inspector** – l’inspecteur d’attributs vous permet de personnaliser différents attributs de la vue/au contrôle sélectionné.
-   **Size Inspector** – l’inspecteur de taille permet de contrôler la taille et le comportement de la vue/au contrôle sélectionné de redimensionnement.
-   **Inspecteur de connexions** – l’inspecteur de connexions affiche les connexions outlet et l’action des contrôles sélectionnés. Nous allons examiner les Outlets et Actions dans un instant.
-   **Inspecteur de liaisons** – l’inspecteur de liaisons vous permet de configurer des contrôles afin que leurs valeurs sont automatiquement liées aux modèles de données.
-   **Afficher l’inspecteur d’effets** – l’inspecteur d’effets vue vous permet de spécifier les effets sur les contrôles, tels que les animations.

Dans le **bibliothèque** section, vous pouvez trouver des contrôles et d’objets à placer sous forme graphique dans le concepteur pour générer votre interface utilisateur :

![Un exemple de l’inspecteur de bibliothèque](xib-images/xcode06.png "un exemple de l’inspecteur de bibliothèque")

Maintenant que vous êtes familiarisé avec l’IDE Xcode et Interface Builder, examinons l’utiliser pour créer une interface utilisateur.


## <a name="creating-and-maintaining-windows-in-xcode"></a>Création et maintenance de windows dans Xcode

Il est préférable pour la création de l’Interface utilisateur d’une application Xamarin.Mac avec des Storyboards (Veuillez consulter notre [Introduction aux Storyboards](~/mac/platform/storyboards/index.md) documentation pour plus d’informations) et, par conséquent, tout nouveau projet démarré dans Xamarin.Mac va utiliser des tables de montage séquentiel par défaut.

Pour passer à l’aide d’un .xib interface utilisateur basée sur, procédez comme suit :

1. Ouvrez Visual Studio pour Mac et démarrez un nouveau projet Xamarin.Mac.
2. Dans le **panneau solutions**, avec le bouton droit sur le projet et sélectionnez **ajouter** > **nouveau fichier...**
3. Sélectionnez **Mac** > **Windows contrôleur**:

    ![Ajoutez un nouveau contrôleur de fenêtre](xib-images/setup00.png "ajoutez un nouveau contrôleur de fenêtre")
4. Entrez `MainWindow` pour le nom et cliquez sur le **New** bouton :

    ![Ajout d’une nouvelle fenêtre principale](xib-images/setup01.png "Ajout d’une nouvelle fenêtre principale")
5. Clic droit sur le projet, puis sélectionnez **ajouter** > **nouveau fichier...**
6. Sélectionnez **Mac** > **Menu principal**:

    ![Ajout d’un nouveau Menu principal](xib-images/setup02.png "Ajout d’un nouveau Menu principal")
7. Conservez le nom `MainMenu` et cliquez sur le **New** bouton.
8. Dans le **panneau solutions** sélectionner le **Main.storyboard** , avec le bouton droit et sélectionnez **supprimer**:

    ![En sélectionnant le storyboard principal](xib-images/setup03.png "en sélectionnant le storyboard principal")
9. Dans la boîte de dialogue Supprimer, cliquez sur le **supprimer** bouton :

    ![Confirmer la suppression](xib-images/setup04.png "confirmant la suppression")
10. Dans le **panneau solutions**, double-cliquez sur le **Info.plist** fichier à ouvrir pour modification.
11. Sélectionnez `MainMenu` à partir de la **Interface principale** liste déroulante :

    [![Définition du menu principal](xib-images/setup05.png "définissant le menu principal")](xib-images/setup05-large.png#lightbox)
12. Dans le **panneau solutions**, double-cliquez sur le **MainMenu.xib** fichier à ouvrir pour modification dans Xcode Interface Builder.
13. Dans le **inspecteur de bibliothèque**, type `object` dans le champ de recherche puis faites glisser une nouvelle **objet** sur l’aire de conception :

    [![Modification du menu principal](xib-images/setup06.png "modifier le menu principal")](xib-images/setup06-large.png#lightbox)
14. Dans le **inspecteur d’identité**, entrez `AppDelegate` pour le **classe**:

    [![En sélectionnant le délégué de l’application](xib-images/setup07.png "en sélectionnant le délégué de l’application")](xib-images/setup07-large.png#lightbox)
15. Sélectionnez **le propriétaire du fichier** à partir de la **hiérarchie des interfaces**, basculez vers le **inspecteur de connexion** et faites glisser une ligne à partir du délégué à la `AppDelegate` **Objet** simplement ajouté au projet :

    [![Connectez le délégué de l’application](xib-images/setup08.png "connexion le délégué de l’application")](xib-images/setup08-large.png#lightbox)
16. Enregistrer les modifications et revenir à Visual Studio pour Mac.

Avec toutes ces modifications en place, vous devez modifier le **AppDelegate.cs** de fichiers et qu’elle ressemble à ce qui suit :

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

Maintenant la fenêtre d’application principal est définie dans un **.xib** fichier automatiquement inclus dans le projet lors de l’ajout d’un contrôleur de fenêtre. Pour modifier votre conception de windows, dans le **panneau solutions**, double-cliquez sur le **MainWindow.xib** fichier :

![Sélection du fichier MainWindow.xib](xib-images/edit01.png "en sélectionnant le fichier MainWindow.xib")

La conception de la fenêtre s’ouvre dans Xcode Interface Builder :

[![Modification de la MainWindow.xib](xib-images/edit02.png "modification le MainWindow.xib")](xib-images/edit02-large.png#lightbox)


### <a name="standard-window-workflow"></a>Flux de travail de fenêtre standard

Pour n’importe quelle fenêtre que vous créez et que vous travaillez dans votre application Xamarin.Mac, le processus est essentiellement le même :

1. Pour les nouvelles fenêtres qui ne sont pas la valeur par défaut ajouté automatiquement à votre projet, ajoutez une nouvelle définition de la fenêtre au projet.
2. Double-cliquez sur le fichier .xib pour ouvrir la fenêtre de conception pour la modification dans Xcode Interface Builder.
3. Définissez les propriétés de fenêtre requis le **inspecteur d’attributs** et **inspecteur de taille**.
4. Faites glisser dans les contrôles nécessaires pour générer votre interface et de les configurer dans le **inspecteur d’attributs**.
5. Utilisez le **inspecteur de taille** pour gérer le redimensionnement des éléments d’interface.
6. Exposer les éléments d’interface utilisateur de la fenêtre à C# code via les outlets et actions.
7. Enregistrez vos modifications et revenir à Visual Studio pour Mac se synchroniser avec Xcode.


### <a name="designing-a-window-layout"></a>Conception d’une disposition de fenêtre

Le processus qui permet de disposer d’une Interface utilisateur dans l’Interface builder est fondamentalement le même pour tous les éléments que vous ajoutez :

1. Recherchez le contrôle souhaité dans le **inspecteur de bibliothèque** et faites-le glisser vers le **Éditeur d’Interface** et le positionner.
2. Définissez les propriétés de fenêtre requis le **inspecteur d’attributs**.
3. Utilisez le **inspecteur de taille** pour gérer le redimensionnement des éléments d’interface.
4. Si vous utilisez une classe personnalisée, définissez-le dans les **inspecteur d’identité**.
5. Exposer les éléments d’interface utilisateur à C# code via les outlets et actions.
6. Enregistrez vos modifications et revenir à Visual Studio pour Mac se synchroniser avec Xcode.

Par exemple :

1. Dans Xcode, faites glisser un **bouton de commande** à partir de la **section Bibliothèque** :

    [![Sélection d’un bouton à partir de la bibliothèque](xib-images/xcode07.png "sélection d’un bouton à partir de la bibliothèque")](xib-images/xcode07-large.png#lightbox)
2. Placez le bouton sur le **fenêtre** dans le **Éditeur d’Interface**:

    [![Ajout d’un bouton dans la fenêtre](xib-images/xcode08.png "Ajout d’un bouton à la fenêtre")](xib-images/xcode08-large.png#lightbox)
3. Cliquez sur la propriété **Titre** dans l’**inspecteur d’attributs** et remplacez le titre du bouton par `Click Me` :

    ![Définir les attributs de bouton](xib-images/xcode09.png "définissant les attributs de bouton")
4. Faites glisser une **étiquette** à partir de la **section Bibliothèque** :

    [![Sélection d’une étiquette dans la bibliothèque](xib-images/xcode10.png "sélection d’une étiquette dans la bibliothèque")](xib-images/xcode10-large.png#lightbox)
5. Placez l’étiquette sur la **fenêtre** à côté du bouton dans l’**Éditeur d’interface** :

    [![Ajout d’une étiquette à la fenêtre](xib-images/xcode11.png "Ajout d’une étiquette à la fenêtre")](xib-images/xcode11-large.png#lightbox)
6. Saisissez la poignée droite de l’étiquette et faites-la glisser jusqu’à ce qu’elle soit près du bord de la fenêtre :

    [![Redimensionnement de l’étiquette](xib-images/xcode12.png "redimensionnement de l’étiquette")](xib-images/xcode12-large.png#lightbox)
7. Avec l’étiquette toujours sélectionné dans le **Éditeur d’Interface**, basculez vers le **inspecteur de taille**:

    ![Sélection de l’inspecteur de taille](xib-images/xcode13.png "en sélectionnant l’inspecteur de taille")
8. Dans le **boîte du redimensionnement automatique** cliquez sur le **Dim crochet rouge** à droite et la **Dim flèche horizontale Red** dans le centre de :

    ![Modifiez les propriétés de dimensionnement automatique](xib-images/xcode14.png "modifiant les propriétés de dimensionnement automatique")
9. Cela garantit que l’étiquette s’étire pour augmenter ou diminuer quand la fenêtre est redimensionnée dans l’application en cours d’exécution. Le **Red crochets** et haut et à gauche de la **boîte du redimensionnement automatique** boîte indiquer à l’étiquette de rester aux donnée de X et Y emplacements.
10. Enregistrez vos modifications dans l’Interface utilisateur

Lorsque vous ont été redimensionnement et le déplacement des contrôles, vous avez devez remarqué qu’Interface Builder donne brèves indications utiles qui sont basées sur [X Human Interface Guidelines pour OS](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). Ces recommandations vous aideront à créer des applications de haute qualité qui auront un aspect familier pour les utilisateurs Mac.

Si vous regardez le **hiérarchie des interfaces** section, notez comment la disposition et la hiérarchie des éléments qui composent notre Interface utilisateur sont affichées :

![Sélection d’un élément dans la hiérarchie des interfaces](xib-images/xcode15.png "en sélectionnant un élément dans la hiérarchie des interfaces")

À partir de là, vous pouvez sélectionner les éléments à modifier ou déplacer pour réorganiser les éléments d’interface utilisateur si nécessaire. Par exemple, si un élément d’interface utilisateur a été couvert par un autre élément, vous pourrez le glisser vers le bas de la liste pour en faire l’élément supérieur sur la fenêtre.

Pour plus d’informations sur l’utilisation de Windows dans une application Xamarin.Mac, veuillez consulter notre [Windows](~/mac/user-interface/window.md) documentation.


## <a name="exposing-ui-elements-to-c-code"></a>Exposition des éléments d’interface utilisateur à C# code

Une fois que vous avez terminé la mise en page de l’apparence de votre interface utilisateur dans l’Interface Builder, vous devez exposer les éléments de l’interface utilisateur afin qu’ils soient accessibles à partir de C# code. Pour ce faire, vous allez utiliser actions et outlets.


### <a name="setting-a-custom-main-window-controller"></a>Définition d’un contrôleur de fenêtre principale personnalisée

Pour être en mesure de créer des Outlets et Actions pour exposer les éléments d’interface utilisateur au code c#, l’application Xamarin.Mac doit utiliser un contrôleur de fenêtre personnalisée.

Effectuez ce qui suit :

1. Ouvrez le plan conceptuel de l’application dans Interface Builder de Xcode.
2. Sélectionnez le `NSWindowController` dans l’aire de conception.
3. Basculez vers le **inspecteur d’identité** afficher et entrez `WindowController` en tant que le **nom de la classe**:

    [![Modification du nom de classe](xib-images/windowcontroller01.png "modification du nom de classe")](xib-images/windowcontroller01-large.png#lightbox)
4. Enregistrez vos modifications et revenir à Visual Studio pour Mac pour la synchronisation.
5. Un **WindowController.cs** fichier sera ajouté à votre projet dans le **panneau solutions** dans Visual Studio pour Mac :

    ![Le nouveau nom de classe dans Visual Studio pour Mac](xib-images/windowcontroller02.png "le nouveau nom de classe dans Visual Studio pour Mac")
6. Rouvrez le plan conceptuel dans Interface Builder de Xcode.
7. Le **WindowController.h** fichier sera disponible pour une utilisation :

    [![Le fichier .h correspondant dans Xcode](xib-images/windowcontroller03.png "le fichier .h correspondant dans Xcode")](xib-images/windowcontroller03-large.png#lightbox)


### <a name="outlets-and-actions"></a>Outlets et actions

Quelles sont donc les outlets et actions ? Dans la programmation de l’interface utilisateur .NET traditionnelle, un contrôle dans l’interface utilisateur est automatiquement exposé en tant que propriété quand il est ajouté. Le fonctionnement est différent dans Mac : le simple ajout d’un contrôle à une vue ne le rend pas accessible au code. Le développeur doit exposer explicitement l’élément d’interface utilisateur au code. Pour ce faire, Apple propose deux options :

-  **Outlets** : les outlets ressemblent à des propriétés. Si vous associer un contrôle à un Outlet, il est exposé à votre code via une propriété, vous pouvez effectuer des opérations comme attacher des gestionnaires d’événements, appeler des méthodes, etc.
-  **Actions** : les actions ressemblent au modèle de commande dans WPF. Par exemple, lorsqu’une Action est effectuée sur un contrôle, par exemple un clic de bouton, le contrôle appelle automatiquement une méthode dans votre code. Les actions sont efficaces et pratiques, car vous pouvez associer plusieurs contrôles à la même Action.

Dans Xcode, outlets et actions sont ajoutées directement dans le code via *faisant glisser le contrôle*. Plus précisément, cela signifie que pour créer une sortie ou une action, vous choisissez quel élément de contrôle que vous souhaitez ajouter une action ou un outlet, maintenez la **contrôle** bouton sur le clavier et faites glisser ce contrôle directement dans votre code.

Pour les développeurs Xamarin.Mac, cela signifie que vous faites glisser dans les fichiers de stub Objective-C qui correspondent à la C# fichier où vous souhaitez créer la sortie ou l’action. Visual Studio pour Mac a créé un fichier appelé **MainWindow.h** dans le cadre du projet Xcode shim qu’il a généré pour utiliser Interface Builder :

[![Un exemple d’un fichier .h dans Xcode](xib-images/xcode16.png "un exemple d’un fichier .h dans Xcode")](xib-images/xcode16-large.png#lightbox)

Ce fichier .h de stub reflète le **MainWindow.designer.cs** qui est automatiquement ajouté à un projet Xamarin.Mac quand un nouveau `NSWindow` est créé. Ce fichier permet de synchroniser les modifications apportées par Interface Builder et est où nous créons votre outlets et actions afin que les éléments d’interface utilisateur sont exposées à C# code.


#### <a name="adding-an-outlet"></a>Ajout d’un outlet

Avec une connaissance élémentaire de quelles sont les outlets et actions, examinons la création d’un outlet pour exposer un élément d’interface utilisateur à votre C# code.

Effectuez ce qui suit :

1. Dans Xcode, dans le coin supérieur tout à droite de l’écran, cliquez sur le bouton **Double Circle** (Cercle double) pour ouvrir l’**Assistant Editor** (Éditeur d’Assistant) :

    [![Sélection de l’éditeur de l’Assistant](xib-images/outlet01.png "en sélectionnant l’éditeur de l’Assistant")](xib-images/outlet01-large.png#lightbox)
2. Xcode passe en mode fractionné avec l’**Éditeur d’interface** d’un côté et un **Éditeur de code** de l’autre.
3. Notez que Xcode a automatiquement sélectionné le **MainWindowController.m** de fichiers dans le **éditeur de Code**, ce qui est incorrect. Si vous vous souvenez de notre discussion sur ce que sont outlets et actions ci-dessus, nous devons disposer le **MainWindow.h** sélectionné.
4. En haut de la **éditeur de Code** cliquez sur le **lien automatique** et sélectionnez le **MainWindow.h** fichier :

    [![En sélectionnant le fichier .h correct](xib-images/outlet02.png "en sélectionnant le fichier .h correct")](xib-images/outlet02-large.png#lightbox)
5. Le fichier approprié doit maintenant être sélectionné pour Xcode :

    [![Le fichier approprié sélectionné](xib-images/outlet03.png "sélectionné le fichier approprié")](xib-images/outlet03-large.png#lightbox)
6. **La dernière étape était très importante !** Si vous n’avez pas sélectionné le fichier approprié, vous ne pourrez pas créer les outlets et actions ou qu’ils seront exposées à la classe incorrecte dans C#!
7. Dans le **Éditeur d’Interface**, maintenez la **contrôle** enfoncée sur le clavier, puis cliquez et faites glisser l’étiquette que nous avons créées précédemment sur l’éditeur de code juste en dessous du `@interface MainWindow : NSWindow { }` code :

    [![Déplacer pour créer un nouveau magasin](xib-images/outlet04.png "déplacer pour créer un nouveau magasin")](xib-images/outlet04-large.png#lightbox)
8. Une boîte de dialogue s’affiche. Laissez le **connexion** valeur outlet et entrez `ClickedLabel` pour le **nom**:

    [![Définition des propriétés Outlet](xib-images/outlet05.png "définissant les propriétés de sortie")](xib-images/outlet05-large.png#lightbox)
9. Cliquez sur le **Connect** bouton permettant de créer la sortie :

    ![La sortie terminée](xib-images/outlet06.png "l’Outlet terminée")
10. Enregistrez les modifications dans le fichier.


#### <a name="adding-an-action"></a>Ajout d’une action

Ensuite, jetons un œil à la création d’une action pour exposer une interaction utilisateur avec l’élément d’interface utilisateur à votre C# code.

Effectuez ce qui suit :

1. Assurez-vous que nous sommes toujours dans le **éditeur de l’Assistant** et **MainWindow.h** fichier est visible dans le **éditeur de Code**.
2. Dans le **Éditeur d’Interface**, maintenez la **contrôle** enfoncée sur le clavier, puis cliquez et faites glisser le bouton que nous avons créées précédemment sur l’éditeur de code juste en dessous du `@property (assign) IBOutlet NSTextField *ClickedLabel;` code :

    [![Déplacement de créer une Action](xib-images/action01.png "déplacer pour créer une Action")](xib-images/action01-large.png#lightbox)
3. Modifier le **connexion** type à l’action :

    [![Sélectionnez un type d’Action](xib-images/action02.png "sélectionner un type d’Action")](xib-images/action02-large.png#lightbox)
4. Entrez `ClickedButton` comme **Nom** :

    [![Configuration de l’Action](xib-images/action03.png "configuration de l’Action")](xib-images/action03-large.png#lightbox)
5. Cliquez sur le **Connect** bouton permettant de créer l’action :

    ![L’Action terminée](xib-images/action04.png "l’Action terminée")
6. Enregistrez les modifications dans le fichier.

Avec votre Interface utilisateur reliée et exposée à C# de code, revenez à Visual Studio pour Mac et laissez-le synchroniser les modifications à partir de Xcode et Interface Builder.


### <a name="writing-the-code"></a>Écriture du code

Avec votre Interface utilisateur créée et ses éléments exposés au code via les outlets et actions, vous êtes prêt à écrire le code pour faire de votre programme. Par exemple, ouvrez le **MainWindow.cs** fichier pour le modifier en double-cliquant dessus dans le **panneau solutions**:

[![Le fichier MainWindow.cs](xib-images/code01.png "MainWindow.cs le fichier")](xib-images/code01-large.png#lightbox)

Et ajoutez le code suivant à la `MainWindow` classe fonctionne avec la prise de l’exemple que vous avez créé ci-dessus :

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

Notez que le `NSLabel` est accessible dans C# par le nom direct que vous avez affecté dans Xcode lorsque vous avez créé la prise dans Xcode, dans ce cas, elle est appelée `ClickedLabel`. Vous pouvez accéder à toute méthode ou propriété de l’objet exposé la même façon que vous le feriez pour n’importe quel normal C# classe.

> [!IMPORTANT]
> Vous devez utiliser `AwakeFromNib`, au lieu d’une autre méthode comme `Initialize`, car `AwakeFromNib` est appelée _après_ le système d’exploitation a chargé et instancié l’Interface utilisateur à partir du fichier .xib. Si vous avez essayé d’accéder au contrôle label avant le fichier .xib a été entièrement chargé et instancié, vous obtiendriez un `NullReferenceException` erreur car le contrôle d’étiquette ne serait pas encore créé.

Ensuite, ajoutez la classe partielle suivante à la `MainWindow` classe :

```csharp
partial void ClickedButton (Foundation.NSObject sender) {

    // Update counter and label
    ClickedLabel.StringValue = string.Format("The button has been clicked {0} time{1}.",++numberOfTimesClicked, (numberOfTimesClicked < 2) ? "" : "s");
}
```

Ce code attache à l’action que vous avez créé dans Xcode et Interface Builder sera appelé chaque fois que l’utilisateur clique sur le bouton.

Certains éléments d’interface utilisateur ont créées automatiquement dans actions, par exemple, les éléments dans la barre de menus par défaut comme la **ouvrir...**  élément de menu (`openDocument:`). Dans le **panneau solutions**, double-cliquez sur le **AppDelegate.cs** fichier à ouvrir pour le modifier et ajoutez le code suivant ci-dessous le `DidFinishLaunching` méthode :

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

La ligne clé ici est `[Export ("openDocument:")]`, elle indique `NSMenu` qui le **AppDelegate** possède une méthode `void OpenDialog (NSObject sender)` qui répond à la `openDocument:` action.

Pour plus d’informations sur l’utilisation des Menus, consultez notre [Menus](~/mac/user-interface/menu.md) documentation.


## <a name="synchronizing-changes-with-xcode"></a>Synchronisation des modifications avec Xcode

Lorsque vous revenez à Visual Studio pour Mac à partir de Xcode, toutes les modifications que vous avez apportées dans Xcode seront automatiquement être synchronisées avec votre projet Xamarin.Mac.

Si vous sélectionnez le **MainWindow.designer.cs** dans le **panneau solutions** vous serez en mesure de voir comment notre prise et l’action ont été reliés dans notre C# code :

[![Synchronisation des modifications avec Xcode](xib-images/sync01.png "synchronisation des modifications avec Xcode")](xib-images/sync01-large.png#lightbox)

Notez comment les deux définitions dans le **MainWindow.designer.cs** fichier :

```csharp
[Outlet]
AppKit.NSTextField ClickedLabel { get; set; }

[Action ("ClickedButton:")]
partial void ClickedButton (Foundation.NSObject sender);
```

Monter d’une ligne avec les définitions dans le **MainWindow.h** fichier dans Xcode :

```objc
@property (assign) IBOutlet NSTextField *ClickedLabel;
- (IBAction)ClickedButton:(id)sender;
```

Comme vous pouvez le voir, Visual Studio pour Mac écoute les modifications apportées au fichier .h, puis synchronise automatiquement ces modifications dans respectifs **. designer.cs** fichier les exposer à votre application. Vous pouvez également remarquer que **MainWindow.designer.cs** est une classe partielle, afin que Visual Studio pour Mac n’aient à modifier **MainWindow.cs** qui remplacerait toutes les modifications que nous avons apportées à la classe.

Normalement, vous n’avez jamais besoin ouvrir le **MainWindow.designer.cs** vous-même, il a été présenté ici à des fins pédagogiques uniquement.

> [!IMPORTANT]
> Dans la plupart des cas, Visual Studio pour Mac sera automatiquement les modifications effectuées dans Xcode et les synchroniser avec votre projet Xamarin.Mac. Dans le cas isolé où la synchronisation ne se produit pas automatiquement, revenez à Xcode, puis à nouveau à Visual Studio pour Mac. Cela déclenche normalement un cycle de synchronisation.


## <a name="adding-a-new-window-to-a-project"></a>Ajout d’une nouvelle fenêtre à un projet

À part la fenêtre de document principal, une application Xamarin.Mac devrez peut-être afficher d’autres types de fenêtres à l’utilisateur, telles que les préférences ou les panneaux de l’inspecteur. Lorsque vous ajoutez une nouvelle fenêtre à votre projet, vous devez toujours utiliser le **fenêtre Cocoa avec contrôleur** option, comme cela rend le processus de chargement de la fenêtre à partir de la .xib fichier plus facile.

Pour ajouter une nouvelle fenêtre, procédez comme suit :

1. Dans le **panneau solutions**, avec le bouton droit sur le projet et sélectionnez **ajouter** > **nouveau fichier...** .
2. Dans la boîte de dialogue Nouveau fichier, sélectionnez **Xamarin.Mac** > **fenêtre Cocoa avec contrôleur**:

    ![Ajout d’un nouveau contrôleur de fenêtre](xib-images/new01.png "Ajout d’un nouveau contrôleur de fenêtre")
3. Entrez `PreferencesWindow` comme **Nom**, puis cliquez sur le bouton **Nouveau**.
4. Double-cliquez sur le **PreferencesWindow.xib** fichier à ouvrir pour modification dans l’Interface Builder :

    [![Modification de la fenêtre dans Xcode](xib-images/new02.png "modification de la fenêtre dans Xcode")](xib-images/new02-large.png#lightbox)
5. Concevoir votre interface :

    [![Conception de la disposition de windows](xib-images/new03.png "pour concevoir la disposition de windows")](xib-images/new03-large.png#lightbox)
6. Enregistrez vos modifications et revenir à Visual Studio pour Mac se synchroniser avec Xcode.

Ajoutez le code suivant à **AppDelegate.cs** pour afficher votre nouvelle fenêtre :

```csharp
[Export("applicationPreferences:")]
void ShowPreferences (NSObject sender)
{
    var preferences = new PreferencesWindowController ();
    preferences.Window.MakeKeyAndOrderFront (this);
}
```

Le `var preferences = new PreferencesWindowController ();` ligne crée une nouvelle instance du contrôleur de fenêtre qui affiche la fenêtre à partir du fichier .xib et l’augmente. Le `preferences.Window.MakeKeyAndOrderFront (this);` ligne affiche la nouvelle fenêtre à l’utilisateur.

Si vous exécutez le code et sélectionnez le **préférences...**  à partir de la **Menu Application**, la fenêtre s’affichera :

![L’exemple d’application en cours d’exécution](xib-images/new04.png "l’exemple d’application en cours d’exécution")

Pour plus d’informations sur l’utilisation de Windows dans une application Xamarin.Mac, veuillez consulter notre [Windows](~/mac/user-interface/window.md) documentation.


## <a name="adding-a-new-view-to-a-project"></a>Ajout d’une nouvelle vue à un projet

Il est parfois quand il est plus facile de conception de votre fenêtre les décomposer en plusieurs, fichiers .xib plus faciles à gérer. Par exemple, comme le contenu de la fenêtre principale de commutation lors de la sélection d’un élément de barre d’outils dans un **fenêtre Préférences** ou d’échanger du contenu en réponse à une **liste Source** sélection.

Lorsque vous ajoutez une nouvelle vue à votre projet, vous devez toujours utiliser le **affichage Cocoa avec contrôleur** option, comme cela rend le processus de chargement de la vue à partir de la .xib fichier plus facile.

Pour ajouter une nouvelle vue, procédez comme suit :

1. Dans le **panneau solutions**, avec le bouton droit sur le projet et sélectionnez **ajouter** > **nouveau fichier...** .
2. Dans la boîte de dialogue Nouveau fichier, sélectionnez **Xamarin.Mac** > **affichage Cocoa avec contrôleur**:

    ![Ajout d’une nouvelle vue](xib-images/view01.png "Ajout d’une nouvelle vue")
3. Entrez `SubviewTable` comme **Nom**, puis cliquez sur le bouton **Nouveau**.
4. Double-cliquez sur le **SubviewTable.xib** fichier à ouvrir pour modification dans l’Interface Builder et concevoir l’Interface utilisateur :

    [![Conception de la nouvelle vue dans Xcode](xib-images/view02.png "conception de la nouvelle vue dans Xcode")](xib-images/view02-large.png#lightbox)
5. Associer les actions requises et les prises de courant.
6. Enregistrez vos modifications et revenir à Visual Studio pour Mac se synchroniser avec Xcode.

Modifiez ensuite le **SubviewTable.cs** et ajoutez le code suivant à la **AwakeFromNib** fichier pour renseigner la nouvelle vue lorsqu’il est chargé :

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

Ajouter un enum pour le projet pour effectuer le suivi de la vue qui est en cours d’affichage. Par exemple, **SubviewType.cs**:

```csharp
public enum SubviewType
{
    None,
    TableView,
    OutlineView,
    ImageView
}
```

Modifiez le fichier .xib de la fenêtre qui consomme de la vue et de l’afficher. Ajouter un **vue personnalisée** qui agit comme conteneur pour l’affichage une fois qu’il est chargé en mémoire par C# de code et l’exposer à un outlet appelé `ViewContainer`:

[![Création de l’Outlet requis](xib-images/view03.png "création de l’Outlet requis")](xib-images/view03-large.png#lightbox)

Enregistrez vos modifications et revenir à Visual Studio pour Mac se synchroniser avec Xcode.

Ensuite, modifiez le fichier .cs de la fenêtre qui affichera la nouvelle vue (par exemple, **MainWindow.cs**) et ajoutez le code suivant :

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

Lorsque nous avons besoin afficher une nouvelle vue chargé à partir d’un fichier .xib dans conteneur de la fenêtre (le **vue personnalisée** ajoutées ci-dessus), ce code gère la suppression de n’importe quelle vue existante et la basculer ensuite out pour une nouvelle. Il recherche pour voir comment cela que vous disposez déjà d’un affichage, si par conséquent, il est supprimé à partir de l’écran. Ensuite, il faut la vue qui a été passée dans (comme il est chargé à partir d’un contrôleur d’affichage) est redimensionné pour tenir dans la zone de contenu et l’ajoute à du contenu à afficher.

Pour afficher une nouvelle vue, utilisez le code suivant :

```csharp
DisplaySubview(new SubviewTableController(), SubviewType.TableView);
```

Cela crée une nouvelle instance de contrôleur d’affichage de la nouvelle vue à afficher, définit son type (comme spécifié par l’enum ajouté au projet) et utilise le `DisplaySubview` méthode ajoutée à la classe de la fenêtre pour afficher la vue. Exemple :

[![L’exemple d’application en cours d’exécution](xib-images/view04.png "l’exemple d’application en cours d’exécution")](xib-images/view04-large.png#lightbox)

Pour plus d’informations sur l’utilisation de Windows dans une application Xamarin.Mac, veuillez consulter notre [Windows](~/mac/user-interface/window.md) et [boîtes de dialogue](~/mac/user-interface/dialog.md) documentation.


## <a name="summary"></a>Récapitulatif

Cet article a examiné en détail à l’utilisation des fichiers .xib dans une application Xamarin.Mac. Nous avons vu les différents types et les utilisations des fichiers .xib pour créer l’Interface utilisateur de votre application, comment créer et gérer des fichiers .xib des fichiers dans Xcode Interface Builder et comment travailler avec des fichiers .xib dans C# code.


## <a name="related-links"></a>Liens associés

- [MacImages (exemple)](https://developer.xamarin.com/samples/mac/MacImages/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Windows](~/mac/user-interface/window.md)
- [Menus](~/mac/user-interface/menu.md)
- [Boîtes de dialogue](~/mac/user-interface/dialog.md)
- [Utilisation d’images](~/mac/app-fundamentals/image.md)
- [Human Interface Guidelines pour macOS](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
