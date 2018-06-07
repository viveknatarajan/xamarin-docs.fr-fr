---
title: Windows dans Xamarin.Mac
description: Cet article décrit l’utilisation des fenêtres et des panneaux dans une application Xamarin.Mac. Il décrit la création des fenêtres et des panneaux dans Xcode et Générateur de l’Interface, les charger à partir des plans conceptuels et les fichiers .xib et leur utilisation par programmation.
ms.prod: xamarin
ms.assetid: 4F6C67E9-BBFF-44F7-B29E-AB47D7F44287
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 39efcf3554469219cc29d70ee059fe645c41280d
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34794070"
---
# <a name="windows-in-xamarinmac"></a>Windows dans Xamarin.Mac

_Cet article décrit l’utilisation des fenêtres et des panneaux dans une application Xamarin.Mac. Il décrit la création des fenêtres et des panneaux dans Xcode et Générateur de l’Interface, les charger à partir des plans conceptuels et les fichiers .xib et leur utilisation par programmation._

Lorsque vous travaillez avec c# et .NET dans une application Xamarin.Mac, vous avez accès à la même Windows et panneaux de qui un développeur travaillant dans *Objective-C* et *Xcode* est. Xamarin.Mac s’intègre directement avec Xcode, vous pouvez utiliser de Xcode _Interface Générateur_ pour créer et maintenir les panneaux et Windows (ou éventuellement de les créer directement dans le code C#).

En fonction de son objectif, une application Xamarin.Mac peut présenter une ou plusieurs fenêtres sur l’écran pour gérer et coordonner les informations qu’il s’affiche et fonctionne avec. Les principales fonctions d’une fenêtre sont :

1. Pour fournir une zone dans laquelle les contrôles et les vues peuvent être placés et gérées.
2. Pour accepter et répondre aux événements en réponse à une interaction utilisateur avec le clavier et la souris.

Windows peut être utilisée dans un état non modal (par exemple, un éditeur de texte qui peut ouvrir plusieurs documents en une seule fois) ou modale (par exemple, une boîte de dialogue d’exportation et doit être fermée avant que l’application peut continuer).

Les panneaux sont un type spécial de fenêtre (une sous-classe de la base de `NSWindow` classe), qui n’ont généralement une fonction auxiliaire dans une application, tels que les fenêtres d’utilitaire telles que des inspecteurs de format de texte et de système de sélecteur de couleurs.

[![](window-images/intro01.png "Modification d’une fenêtre dans Xcode")](window-images/intro01.png#lightbox)

Dans cet article, nous aborderons les principes fondamentaux de l’utilisation des fenêtres et des panneaux dans une application Xamarin.Mac. Il est fortement recommandé que vous parcourez le [Hello, Mac](~/mac/get-started/hello-mac.md) article tout d’abord, en particulier le [Introduction à Xcode et Interface Générateur](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) et [prises et Actions](~/mac/get-started/hello-mac.md#Outlets_and_Actions) sections, telle qu’elle couvre les principaux concepts et techniques que nous utiliserons dans cet article.

Vous souhaiterez peut-être examiner la [classes exposition c# / méthodes pour Objective-C](~/mac/internals/how-it-works.md) section de la [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) de document, elle explique la `Register` et `Export` commandes permet de câble des vos classes c# pour les objets Objective-C et les éléments d’interface utilisateur.

<a name="Introduction_to_Windows" />

## <a name="introduction-to-windows"></a>Introduction à Windows

Comme indiqué ci-dessus, une fenêtre fournit une zone dans laquelle les contrôles et les vues peuvent être placés et gérées et répond aux événements en fonction de l’interaction de l’utilisateur (soit via le clavier ou la souris).

En fonction d’Apple, il existe cinq types de Windows dans une application macOS :

- **Fenêtre de document** -une fenêtre de document contient des données d’utilisateur basés sur le fichier comme une feuille de calcul ou un document texte.
- **Fenêtre de l’application** -une fenêtre de l’application est la fenêtre principale d’une application qui n’est pas en fonction de document (par exemple, l’application calendrier sur un Mac).
- **Panneau de configuration** : un panneau flotte au-dessus des autres fenêtres et fournit des outils ou les contrôles que les utilisateurs peuvent travailler avec si des documents sont ouvrir. Dans certains cas, un panneau de configuration peut être translucide (par exemple quand utilisation des graphiques volumineux).
- **Boîte de dialogue** -une boîte de dialogue s’affiche en réponse à une action de l’utilisateur et généralement fournit aux utilisateurs des moyens d’effectuer l’action. Une boîte de dialogue requiert une réponse de l’utilisateur avant d’être fermée. (Consultez [utilisation des boîtes de dialogue](~/mac/user-interface/dialog.md))
- **Alertes** -une alerte est un type spécial de la boîte de dialogue qui s’affiche lorsqu’un problème grave se produit (par exemple, une erreur) ou en tant qu’avertissement (par exemple, la préparation de la suppression d’un fichier). Une alerte étant une boîte de dialogue, elle requiert également une réponse de l’utilisateur avant d’être fermée. (Consultez [utilisation des alertes](~/mac/user-interface/alert.md))

Pour plus d’informations, consultez la [sur Windows](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowAppearanceBehavior.html#//apple_ref/doc/uid/20000957-CH33-SW1) section d’Apple [indications de l’Interface du système d’exploitation X humaine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Main_Key_and_Inactive_Windows" />

### <a name="main-key-and-inactive-windows"></a>Principal, la clé et les fenêtres inactives

Windows dans une application Xamarin.Mac pouvez ressemblent et se comportent différemment en fonction de la façon dont l’utilisateur est actuellement interagir avec elles. Le Document ou la fenêtre de l’application qui est actuellement le focus de l’attention des utilisateurs plus importante est appelé le _fenêtre principale_. Dans la plupart des cas cette fenêtre sera également le _fenêtre clé_ (la fenêtre qui accepte l’entrée d’utilisateur actuellement). Mais ce n’est pas toujours le cas, par exemple, un sélecteur de couleurs peut être ouvert et la fenêtre de la clé qui interagit avec l’utilisateur pour modifier l’état d’un élément dans la fenêtre de Document (qui serait toujours la fenêtre principale).

Principale et clé de Windows (s’ils sont distincts) sont toujours actives, _les fenêtres inactives_ sont des fenêtres qui ne sont pas au premier plan. Par exemple, une application de l’éditeur de texte peut avoir plus d’un document ouvert à la fois, seule la fenêtre principale est donc active, tous les autres seraient inactifs. 

Pour plus d’informations, consultez la [sur Windows](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowAppearanceBehavior.html#//apple_ref/doc/uid/20000957-CH33-SW1) section d’Apple [indications de l’Interface du système d’exploitation X humaine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Naming_Windows" />

### <a name="naming-windows"></a>Windows d’affectation de noms

Une fenêtre peut afficher une barre de titre, et lorsque le titre est affiché, il est généralement le nom de l’application, le nom du document en cours de traitement ou la fonction de la fenêtre (par exemple, l’inspecteur). Certaines applications ne pas afficher une barre de titre, car ils ne sont reconnues par la vue et ne fonctionnent pas avec les documents.

Apple suggérer les consignes suivantes :

- Utilisez le nom de votre application pour le titre d’une fenêtre principale, non-document. 
- Nom d’une nouvelle fenêtre de document `untitled`. Pour le premier nouveau document, ne pas ajouter un numéro d’au titre (tel que `untitled 1`). Si l’utilisateur crée un autre document avant d’enregistrer et le titre de la première, appelez cette fenêtre `untitled 2`, `untitled 3`, etc.

Pour plus d’informations, consultez la [d’affectation de noms de Windows](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowNaming.html#//apple_ref/doc/uid/20000957-CH35-SW1) section d’Apple [indications de l’Interface du système d’exploitation X humaine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Full-Screen_Windows" />

### <a name="full-screen-windows"></a>Plein écran Windows

Dans macOS, fenêtre d’une application peut accéder masquage de plein écran y compris la barre de menus d’Application (qui peut être affichée en déplaçant le curseur vers le haut de l’écran) pour fournir de vous laisser distraire par interaction libre est contenue.

Apple suggère les consignes suivantes :

- Déterminer s’il convient d’une fenêtre passer du mode plein écran. Les applications qui fournissent des interactions brèves (par exemple, une calculatrice) ne doivent pas fournir un mode plein écran.
- Afficher la barre d’outils si la tâche de plein écran est nécessaire. En règle générale, la barre d’outils est masquée en mode plein écran.
- La fenêtre en plein écran doit avoir tous les utilisateurs de fonctionnalités à effectuer la tâche.
- Si possible, évitez les interactions de recherche alors que l’utilisateur est dans une fenêtre plein écran.
- Tirer parti de l’espace d’écran accrue sans passer le focus hors de la tâche principale.

Pour plus d’informations, consultez la [plein écran Windows](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/FullScreen.html#//apple_ref/doc/uid/20000957-CH61-SW1) section d’Apple [indications de l’Interface du système d’exploitation X humaine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Panels" />

### <a name="panels"></a>Panneaux

Un panneau de configuration est une fenêtre auxiliaire qui contient des contrôles et les options qui affectent le document actif ou la sélection (par exemple, le système de sélecteur de couleurs) :

[![](window-images/panel01.png "Un panneau de couleur")](window-images/panel01.png#lightbox)

Panneaux peut être _spécifique à l’application_ ou _système_. Les panneaux spécifiques de l’application Dissocier du haut des fenêtres de document de l’application et disparaissent lorsque l’application est en arrière-plan. Panneaux de l’échelle du système (telles que la **polices** Panneau de configuration), float au-dessus de toutes les fenêtres ouvertes, quel que soit l’application. 

Apple suggère les consignes suivantes :

- En règle générale, utilisez un panneau de configuration standard, les panneaux transparents ne doivent être utilisées avec modération et pour des tâches gourmandes en ressources graphiques.
- Envisagez d’utiliser un panneau de configuration pour accorder aux utilisateurs un accès facile aux informations affecte directement le leur tâche ou de contrôles importants.
- Masquer et afficher les panneaux en fonction des besoins.
- Panneaux doit toujours inclure la barre de titre.
- Panneaux ne doit pas inclure un bouton réduire active.

#### <a name="inspectors"></a>inspecteurs

Les applications les plus récents macOS présentent les options qui affectent le document actif ou la sélection en tant qu’et les contrôles auxiliaires _inspecteurs_ qui font partie de la fenêtre principale (comme le **Pages** application illustrée ci-dessous), au lieu d’utiliser le panneau de configuration Windows :

[![](window-images/panel02.png "Un inspecteur de l’exemple")](window-images/panel02.png#lightbox)

Pour plus d’informations, consultez la [panneaux](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowPanels.html#//apple_ref/doc/uid/20000957-CH42-SW1) section d’Apple [indications de l’Interface du système d’exploitation X humaine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/) et notre [MacInspector](https://developer.xamarin.com/samples/mac/MacInspector/) exemple d’application pour une implémentation complète d’un **Interface inspecteur** dans une application Xamarin.Mac.

<a name="Creating_and_Maintaining_Windows_in_Xcode" />

## <a name="creating-and-maintaining-windows-in-xcode"></a>Création et gestion de Windows dans Xcode

Lorsque vous créez une nouvelle application/Cocoa de Xamarin.Mac, vous obtenez une fenêtre vide, standard par défaut. Ce paramètre, windows est défini dans un `.storyboard` fichier inclus automatiquement dans le projet. Pour modifier votre conception de windows, dans le **l’Explorateur de solutions**, double-cliquez sur le `Main.storyboard` fichier :

[![](window-images/edit01.png "En sélectionnant le storyboard principal")](window-images/edit01.png#lightbox)

La conception de la fenêtre s’ouvre dans le Générateur de Xcode Interface :

[![](window-images/edit02.png "Modification de l’interface utilisateur dans Xcode")](window-images/edit02.png#lightbox)

Dans le **inspecteur de l’attribut**, il existe plusieurs propriétés que vous pouvez utiliser pour définir et contrôler votre fenêtre :

- **Titre** -il s’agit du texte qui s’affichera dans la barre de titre de la fenêtre.
- **Cette fonctionnalité** -il s’agit de la _clé_ qui sera utilisé pour l’ID de la fenêtre lorsque sa position et les paramètres sont enregistrés automatiquement.
- **Barre de titre** -la fenêtre s’affiche une barre de titre.
- **Unifiée de titre et la barre d’outils** - si la fenêtre comprend une barre d’outils, il doit être la partie de la barre de titre.
- **Complète la vue de contenu de taille** -permet de la zone de contenu de la fenêtre d’être sous la barre de titre.
- **Clichés instantanés** -la fenêtre dispose d’une ombre.
- **Une texture** -texture windows peuvent utiliser des effets (comme le dynamisme de notre place) et peuvent être déplacées en faisant glisser n’importe où dans leurs corps.
- **Fermez** -la fenêtre possède un bouton Fermer.
- **Réduire** -la fenêtre possède un bouton réduire.
- **Redimensionner** -la fenêtre possède un contrôle de redimensionnement.
- **Bouton de barre d’outils** -la fenêtre a un bouton de barre d’outils Afficher/masquer.
- **Pouvant être restaurée** -est la position de la fenêtre et les paramètres automatiquement enregistré et restauré.
- **Visible à lancer** -la fenêtre apparaît automatiquement quand le `.xib` fichier est chargé.
- **Masquer sur désactivé** -la fenêtre est masquée lorsque l’application passe à l’arrière-plan.
- **Lors de la fermeture de la version** -est la fenêtre supprimée de la mémoire lorsqu’il est fermé.
- **Toujours les info-bulles de l’affichage** -sont les info-bulles affichées en permanence.
- **Recalcule la boucle d’affichage** -est l’ordre d’affichage recalculée avant que la fenêtre est dessinée.
- **Espaces**, **Exposé** et **cycles** -définir le comportement de la fenêtre dans ces environnements macOS. 
- **Plein écran** -détermine si cette fenêtre pouvez entrer le mode plein écran. 
- **Animation** -déterminent le type d’animation disponible pour la fenêtre.
- **Apparence** -contrôle l’apparence de la fenêtre. Pour l’instant, il n'existe qu’un seul aspect, cyan.

Consultez d’Apple [Introduction à Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1) et [NSWindow](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSWindow_Class/index.html#//apple_ref/occ/cl/NSWindow) documentation pour plus de détails.

<a name="Setting_the_Default_Size_and_Location" />

### <a name="setting-the-default-size-and-location"></a>Définition de la taille par défaut et l’emplacement

Pour définir la position initiale de la fenêtre et contrôler sa taille, basculez vers le **taille inspecteur**:

[![](window-images/edit07.png "La taille par défaut et l’emplacement")](window-images/edit07.png#lightbox)

À partir d’ici vous pouvez définir la taille initiale de la fenêtre, lui donner une taille minimale et maximale, définir l’emplacement d’origine sur l’écran et contrôler les bordures autour de la fenêtre.

<a name="Setting-a-Custom-Main-Window-Controller" />

### <a name="setting-a-custom-main-window-controller"></a>Définition d’un contrôleur personnalisé de fenêtre principale

Pour être en mesure de créer des sorties et les Actions pour exposer les éléments d’interface utilisateur au code c#, l’application Xamarin.Mac doit être un contrôleur de fenêtre personnalisée à l’aide de.

Effectuez ce qui suit :

1. Ouvrez le plan conceptuel de l’application dans le Générateur de Xcode Interface.
2. Sélectionnez le `NSWindowController` dans l’aire de conception.
3. Basculez vers le **inspecteur d’identité** afficher et entrez `WindowController` en tant que le **nom de la classe**: 

    [![](window-images/windowcontroller01.png "Définition du nom de classe")](window-images/windowcontroller01.png#lightbox)
4. Enregistrez vos modifications et revenir à Visual Studio pour Mac à synchroniser.
5. A `WindowController.cs` fichier sera ajouté à votre projet dans le **l’Explorateur de solutions** dans Visual Studio pour Mac : 

    [![](window-images/windowcontroller02.png "En sélectionnant le contrôleur de windows")](window-images/windowcontroller02.png#lightbox)
6. Rouvrez le plan conceptuel dans le Générateur de Xcode Interface.
7. Le `WindowController.h` fichier sera disponible pour une utilisation : 

    [![](window-images/windowcontroller03.png "Modification du fichier WindowController.h")](window-images/windowcontroller03.png#lightbox)

<a name="Adding_UI_Elements" />

### <a name="adding-ui-elements"></a>Ajout d’éléments d’interface utilisateur

Pour définir le contenu d’une fenêtre, faites glisser les contrôles à partir de la **bibliothèque inspecteur** sur la **éditeur de l’Interface**. Veuillez consulter notre [Introduction à Xcode et Interface Générateur](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) documentation pour plus d’informations sur l’utilisation du Générateur d’Interface pour créer et activer des contrôles.

Par exemple, nous allons faire glisser une barre d’outils à partir de la **bibliothèque inspecteur** sur la fenêtre de la **éditeur de l’Interface**:

[![](window-images/edit03.png "Sélection d’une barre d’outils à partir de la bibliothèque")](window-images/edit03.png#lightbox)

Ensuite, faites glisser un **mode texte** et le redimensionner pour remplir la zone sous la barre d’outils :

[![](window-images/edit04.png "Ajout d’une vue de texte")](window-images/edit04.png#lightbox)

Étant donné que nous voulons le **affichage de texte** pour réduire et développer en tant que les modifications de la taille de la fenêtre, passons à la **éditeur de contrainte** et ajoutez les contraintes suivantes :

[![](window-images/edit05.png "Modification des contraintes")](window-images/edit05.png#lightbox)

En cliquant sur le pour **rouge I-FAISCEAUX** en haut de l’éditeur et en cliquant sur **ajouter des contraintes de 4**, nous indiquons à l’affichage de texte, respectez les coordonnées X, Y donnée et la croissance ou la réduction horizontalement et verticalement en tant que la fenêtre est redimensionnée.

Enfin, nous allons exposer le **affichage de texte** à l’aide de code une **prise** (en veillant à sélectionner le `ViewController.h` fichier) :

[![](window-images/edit06.png "Configuration d’une prise de courant")](window-images/edit06.png#lightbox)

Enregistrez vos modifications et revenez à Visual Studio pour Mac pour la synchronisation avec Xcode.

Pour plus d’informations sur l’utilisation de **prises** et **Actions**, veuillez consulter notre [prise et Action](~/mac/get-started/hello-mac.md#Outlets_and_Actions) documentation.

<a name="Standard_Window_Workflow" />

### <a name="standard-window-workflow"></a>Fenêtre standard de flux de travail

Pour n’importe quelle fenêtre que vous créez et travaillez dans votre application Xamarin.Mac, le processus est fondamentalement identique à ce que nous avons simplement ci-dessus :

1. Pour les nouvelles fenêtres qui ne sont pas ajouté automatiquement à votre projet par défaut, ajoutez une nouvelle définition de la fenêtre pour le projet. Ce sujet est abordé en détail ci-dessous.
2. Double-cliquez sur le `Main.storyboard` fichier pour ouvrir la fenêtre de conception pour la modification dans le Générateur de Xcode Interface.
3. Faites glisser une nouvelle fenêtre à la conception de l’Interface utilisateur et raccorder l’à l’aide de la fenêtre principale de la fenêtre _Segues_ (pour plus d’informations, consultez la [Segues](~/mac/platform/storyboards/indepth.md#Segues) section de notre [utilisation des animations](~/mac/platform/storyboards/indepth.md) documentation).
3. Définir les propriétés de la fenêtre requise dans le **inspecteur de l’attribut** et **taille inspecteur**.
4. Faites glisser les contrôles requis pour générer votre interface et de les configurer dans le **inspecteur de l’attribut**.
5. Utilisez le **taille inspecteur** pour gérer le redimensionnement de vos éléments d’interface utilisateur.
6. Exposer les éléments d’interface utilisateur de la fenêtre de code c# via **prises** et **Actions**.
7. Enregistrez vos modifications et revenez à Visual Studio pour Mac pour la synchronisation avec Xcode.

Maintenant que nous avons créée une fenêtre de base, nous allons examiner les processus classiques un Xamarin.Mac fait de l’application lorsque vous travaillez avec windows. 

<a name="Displaying_the_Default_Window" />

## <a name="displaying-the-default-window"></a>Affichage de la fenêtre par défaut

Par défaut, une nouvelle application Xamarin.Mac s’affiche automatiquement la fenêtre définie dans le `MainWindow.xib` fichier lorsqu’il est démarré :

[![](window-images/display01.png "Une fenêtre de l’exemple en cours d’exécution")](window-images/display01.png#lightbox)

Étant donné que nous avons modifié la conception de cette fenêtre ci-dessus, il inclut désormais une barre d’outils par défaut et **mode texte** contrôle. Section de ce qui suit dans le `Info.plist` fichier est chargé d’afficher cette fenêtre :

[![](window-images/display00.png "Modification du fichier Info.plist.")](window-images/display00.png#lightbox)

Le **Interface principale** liste déroulante permet de sélectionner la table de montage séquentiel qui sera utilisé en tant que l’interface utilisateur principal de l’application (dans ce cas `Main.storyboard`).

Un contrôleur de la vue est automatiquement ajouté au projet pour contrôler que Windows principale qui s’affiche (ainsi que sa vue principal). Il est défini dans le `ViewController.cs` de fichiers et attaché à la **propriétaire du fichier** dans le Générateur de Interface sous le **inspecteur d’identité**:

[![](window-images/display02.png "Définir le propriétaire du fichier")](window-images/display02.png#lightbox)

Pour notre fenêtre, nous souhaiterions qu’il ait un titre de `untitled` lorsqu’il commence par essai, commençons par remplacement le `ViewWillAppear` méthode dans le `ViewController.cs` ressemble à ce qui suit :

```csharp
public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Set Window Title
    this.View.Window.Title = "untitled";
}
``` 

> [!NOTE]
> Nous définissons la valeur de la fenêtre `Title` propriété dans le `ViewWillAppear` méthode au lieu du `ViewDidLoad` (méthode), car, lorsque la vue peut être chargée en mémoire, il n’est pas encore entièrement instancié. Si nous avons tenté d’accéder à la `Title` propriété dans le `ViewDidLoad` méthode, vous obtenez un `null` exception depuis la fenêtre n’a pas été construite et filaire à distance à la propriété encore.

<a name="Programmatically_Closing_a_Window" />

## <a name="programmatically-closing-a-window"></a>Fermeture d’une fenêtre par programme

Il peut arriver que vous souhaitez fermer par programmation une fenêtre dans une application Xamarin.Mac, autre que d’avoir l’utilisateur, cliquez sur la fenêtre **fermer** bouton ou à l’aide d’un élément de menu. macOS fournit deux façons différentes pour fermer une `NSWindow` par programmation : `PerformClose` et `Close`.

<a name="PerformClose" />

### <a name="performclose"></a>PerformClose

Appel de la `PerformClose` méthode d’un `NSWindow` simule l’utilisateur clique sur la fenêtre **fermer** bouton par momentanément la mise en surbrillance le bouton, puis en fermant la fenêtre.

Si l’application implémente la `NSWindow`de `WillClose` événement sera déclenché avant la fermeture de la fenêtre. Si l’événement retourne `false`, puis la fenêtre ne sera pas fermée. Si la fenêtre n’a pas un **fermer** bouton ou ne peut pas être fermé pour une raison quelconque, le système d’exploitation émet le son d’alerte.

Exemple :

```csharp
MyWindow.PerformClose(this);
```

Tente de fermer le `MyWindow` `NSWindow` instance. Si elle a réussi, la fenêtre sera fermée, sinon le son d’alerte est émis et le ne restera ouvert.

<a name="Close" />

### <a name="close"></a>Fermer

Appel de la `Close` méthode d’un `NSWindow` ne simule pas l’utilisateur clique sur la fenêtre **fermer** bouton par momentanément la mise en surbrillance le bouton, il ferme simplement la fenêtre.

Une fenêtre n’a pas à être visible à fermer et un `NSWindowWillCloseNotification` notification est publiée sur le centre de Notification par défaut pour la fenêtre en cours de fermeture.

Le `Close` méthode diffère sur deux points importants à partir de la `PerformClose` méthode :

1. Il ne tente pas de déclencher la `WillClose` événement.
2. Il ne simule pas l’utilisateur en cliquant sur le **fermer** bouton en momentanément mettant en surbrillance le bouton.

Exemple :

```csharp
MyWindow.Close();
```

Pour fermer la `MyWindow` `NSWindow` instance.

<a name="Modified-Windows-Content" />

## <a name="modified-windows-content"></a>Contenu modifié Windows

Dans macOS, Apple a fourni un moyen d’informer l’utilisateur que le contenu d’une fenêtre (`NSWindow`) a été modifié par l’utilisateur et doit être enregistré. Si la fenêtre contient le contenu modifié, un petit point noir s’affichera dans son **fermer** widget :

[![](window-images/close01.png "Une fenêtre avec le marqueur modifié")](window-images/close01.png#lightbox)

Si l’utilisateur tente de fermer la fenêtre ou quitter l’application Mac lorsqu’il y a non enregistrées modifications apportées au contenu de la fenêtre, vous devez présenter un [boîte de dialogue](~/mac/user-interface/dialog.md) ou [feuille modale](~/mac/user-interface/dialog.md) et autoriser l’utilisateur à enregistrer les modifications premier :

[![](window-images/close02.png "Un enregistrement feuille est affichée lorsque la fenêtre est fermée.")](window-images/close02.png#lightbox)

### <a name="marking-a-window-as-modified"></a>Marquage d’une fenêtre modifiée

Pour marquer une fenêtre comme ayant modifié le contenu, utilisez le code suivant :

```csharp
// Mark Window content as modified
Window.DocumentEdited = true;
```

Et une fois que la modification a été enregistrée, désactivez l’indicateur modifié à l’aide de :

```csharp
// Mark Window content as not modified
Window.DocumentEdited = false;
```

### <a name="saving-changes-before-closing-a-window"></a>Enregistrer les modifications avant de fermer une fenêtre

Pour surveiller l’utilisateur de fermeture d’une fenêtre et en leur permettant d’enregistrer le contenu modifié au préalable, vous devez créer une sous-classe de `NSWindowDelegate` et remplacez son `WindowShouldClose` (méthode). Exemple :

```csharp
using System;
using AppKit;
using System.IO;
using Foundation;

namespace SourceWriter
{
    public class EditorWidowDelegate : NSWindowDelegate
    {
        #region Computed Properties
        public NSWindow Window { get; set;}
        #endregion

        #region constructors
        public EditorWidowDelegate (NSWindow window)
        {
            // Initialize
            this.Window = window;

        }
        #endregion

        #region Override Methods
        public override bool WindowShouldClose (Foundation.NSObject sender)
        {
            // is the window dirty?
            if (Window.DocumentEdited) {
                var alert = new NSAlert () {
                    AlertStyle = NSAlertStyle.Critical,
                    InformativeText = "Save changes to document before closing window?",
                    MessageText = "Save Document",
                };
                alert.AddButton ("Save");
                alert.AddButton ("Lose Changes");
                alert.AddButton ("Cancel");
                var result = alert.RunSheetModal (Window);

                // Take action based on resu;t
                switch (result) {
                case 1000:
                    // Grab controller
                    var viewController = Window.ContentViewController as ViewController;

                    // Already saved?
                    if (Window.RepresentedUrl != null) {
                        var path = Window.RepresentedUrl.Path;

                        // Save changes to file
                        File.WriteAllText (path, viewController.Text);
                        return true;
                    } else {
                        var dlg = new NSSavePanel ();
                        dlg.Title = "Save Document";
                        dlg.BeginSheet (Window, (rslt) => {
                            // File selected?
                            if (rslt == 1) {
                                var path = dlg.Url.Path;
                                File.WriteAllText (path, viewController.Text);
                                Window.DocumentEdited = false;
                                viewController.View.Window.SetTitleWithRepresentedFilename (Path.GetFileName(path));
                                viewController.View.Window.RepresentedUrl = dlg.Url;
                                Window.Close();
                            }
                        });
                        return true;
                    }
                    return false;
                case 1001:
                    // Lose Changes
                    return true;
                case 1002:
                    // Cancel
                    return false;
                }
            }

            return true;
        }
        #endregion
    }
}
```

Pour attacher une instance de ce délégué à la fenêtre, utilisez le code suivant :

```csharp
// Set delegate
Window.Delegate = new EditorWidowDelegate(Window);
```

### <a name="saving-changes-before-closing-the-app"></a>Enregistrer les modifications avant la fermeture de l’application

Enfin, votre application Xamarin.Mac doit vérifier si une de ses fenêtres contiennent le contenu modifié et autoriser l’utilisateur à enregistrer les modifications avant de quitter. Pour ce faire, modifiez votre `AppDelegate.cs` de fichiers, substituez le `ApplicationShouldTerminate` (méthode) et le rendre l’aspect suivant :

```csharp
public override NSApplicationTerminateReply ApplicationShouldTerminate (NSApplication sender)
{
    // See if any window needs to be saved first
    foreach (NSWindow window in NSApplication.SharedApplication.Windows) {
        if (window.Delegate != null && !window.Delegate.WindowShouldClose (this)) {
            // Did the window terminate the close?
            return NSApplicationTerminateReply.Cancel;
        }
    }

    // Allow normal termination
    return NSApplicationTerminateReply.Now;
}
```

<a name="Working_with_Multiple_Windows" />

## <a name="working-with-multiple-windows"></a>Utilisation de plusieurs fenêtres

La plupart des applications pour Mac en fonction de document peuvent modifier plusieurs documents en même temps. Par exemple, un éditeur de texte peut avoir plusieurs fichiers texte ouvert pour modification en même temps. Par défaut, notre nouvelle application Xamarin.Mac a un **fichier** menu avec un **nouveau** élément automatiquement câblé-jusqu'à la `newDocument:` **Action**.

Nous allons activer ce nouvel élément et autoriser l’utilisateur à ouvrir plusieurs copies de notre fenêtre principale pour modifier plusieurs documents en même temps.

Permet de modifier notre `AppDelegate.cs` et ajoutez les éléments suivants calculée propriété :

```csharp
public int UntitledWindowCount { get; set;} =1;
```

Nous utiliserons pour suivre le nombre de fichiers non enregistrés afin de nous pouvons envoyer des commentaires à l’utilisateur (par les instructions d’Apple comme indiqué ci-dessus).

Ensuite, ajoutez la méthode suivante :

```csharp
[Export ("newDocument:")]
void NewDocument (NSObject sender) {
    // Get new window
    var storyboard = NSStoryboard.FromName ("Main", null);
    var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

    // Display
    controller.ShowWindow(this);

    // Set the title
    controller.Window.Title = (++UntitledWindowCount == 1) ? "untitled" : string.Format ("untitled {0}", UntitledWindowCount);
}
```

Ce code crée une nouvelle version de notre contrôleur fenêtre charge de la nouvelle fenêtre, rend le principal et la fenêtre de la clé et lui affecte un titre. Maintenant, si nous exécuter notre application, puis sélectionnez **nouveau** à partir de la **fichier** menu sera ouvert et affiche une nouvelle fenêtre d’éditeur :

[![](window-images/display04.png "Une nouvelle fenêtre sans nom a été ajoutée.")](window-images/display04.png#lightbox)

Si nous allons ouvrir le **Windows** menu, vous pouvez voir l’application est automatiquement suivi et de gestion de nos fenêtres ouvertes :

[![](window-images/display05.png "Le menu Windows")](window-images/display05.png#lightbox)

Pour plus d’informations sur l’utilisation des Menus dans une application Xamarin.Mac, veuillez consulter notre [utilisation des Menus](~/mac/user-interface/menu.md) documentation.

<a name="Getting_the_Currently_Active_Window" />

### <a name="getting-the-currently-active-window"></a>Mise en route de la fenêtre Active

Dans une application Xamarin.Mac qui peut ouvrir plusieurs fenêtres (documents), voici les heures lorsque vous devez obtenir la fenêtre en cours, au premier plan (la fenêtre de clé). Le code suivant retourne la fenêtre de clé :

```csharp
var window = NSApplication.SharedApplication.KeyWindow;
```

Il peut être appelée dans une classe ou une méthode qui doit accéder à la fenêtre en cours, la clé. Si aucune fenêtre n’est ouverte, elle retournera `null`.

<a name="Accessing-All-App-Windows" />

### <a name="accessing-all-app-windows"></a>L’accès à toutes les fenêtres d’application

Il peut arriver dans lequel vous avez besoin pour accéder à toutes les fenêtres que votre application Xamarin.Mac a d’ouvertes. Par exemple, pour voir si un fichier que l’utilisateur souhaite ouvrir est déjà ouvert dans une fenêtre de sortie.

Le `NSApplication.SharedApplication` conserve un `Windows` propriété qui contient un tableau de toutes les fenêtres ouvertes dans votre application. Vous pouvez itérer sur ce tableau pour accéder à toutes les fenêtres de l’application. Exemple :

```csharp
// Is the file already open?
for(int n=0; n<NSApplication.SharedApplication.Windows.Length; ++n) {
    var content = NSApplication.SharedApplication.Windows[n].ContentViewController as ViewController;
    if (content != null && path == content.FilePath) {
        // Bring window to front
        NSApplication.SharedApplication.Windows[n].MakeKeyAndOrderFront(this);
        return true;
    }
}
```

Dans l’exemple de code nous allons effectuer un cast de chaque fenêtre personnalisé retourné `ViewController` classe dans notre application et le test de la valeur d’une personnalisée `Path` une propriété et le chemin d’accès d’un fichier de l’utilisateur souhaite ouvrir. Si le fichier est déjà ouvert, il déroule cette fenêtre au premier plan.

<a name="Adjusting_the_Window_Size_in_Code" />

## <a name="adjusting-the-window-size-in-code"></a>Ajustement de la taille de la fenêtre de Code

Il est parfois lorsque l’application a besoin pour redimensionner une fenêtre de code. Pour redimensionner et repositionner une fenêtre, vous le régler de `Frame` propriété. Lorsque vous adaptez la taille d’une fenêtre, vous devez généralement également ajuster son origine, pour garder la fenêtre au même emplacement en raison du système de coordonnées de macOS.

Contrairement à iOS où l’angle supérieur gauche représente (0,0) et macOS utilise un système de coordonnées mathématique où le coin inférieur gauche de l’écran représente (0,0). Les coordonnées iOS augmentent lorsque vous vous déplacez vers le bas vers la droite. Dans macOS, les coordonnées augmentent vers le haut à droite. 

L’exemple de code suivant redimensionne une fenêtre :

```csharp
nfloat y = 0;

// Calculate new origin
y = Frame.Y - (768 - Frame.Height);

// Resize and position window
CGRect frame = new CGRect (Frame.X, y, 1024, 768);
SetFrame (frame, true);
```

> [!IMPORTANT]
> Lorsque vous ajustez une taille de windows et l’emplacement dans le code, vous devez vous assurer que vous respectez les tailles minimales et maximales que vous avez définie dans le constructeur d’Interface. Cela n’est pas automatiquement honoré et vous serez en mesure d’établir la fenêtre supérieure ou inférieure à ces limites.

<a name="Monitoring-Window-Size-Changes" />

## <a name="monitoring-window-size-changes"></a>Surveillance des modifications de taille de fenêtre

Il peut arriver dans lequel vous avez besoin pour surveiller les modifications de la taille d’une fenêtre à l’intérieur de votre application Xamarin.Mac. Par exemple, pour redessiner le contenu en fonction de la nouvelle taille.

Pour surveiller les modifications de taille, vous devez d’abord vérifier que vous avez attribué une classe personnalisée pour le contrôleur de fenêtre dans le Générateur de Xcode Interface. Par exemple, `MasterWindowController` dans l’exemple suivant :

[![](window-images/resize01.png "L’inspecteur de l’identité")](window-images/resize01.png#lightbox)

Modifiez ensuite la classe de contrôleur de fenêtre personnalisés et de moniteur le `DidResize` événement dans la fenêtre du contrôleur pour être averti des modifications de taille dynamique. Exemple :

```csharp
public override void WindowDidLoad ()
{
    base.WindowDidLoad ();

    Window.DidResize += (sender, e) => {
        // Do something as the window is being live resized
    };
}
```

Si vous le souhaitez, vous pouvez utiliser la `DidEndLiveResize` événement ne reçoit une notification une fois que l’utilisateur a fini de modifier la taille de la fenêtre. Par exemple :

```csharp
public override void WindowDidLoad ()
{
    base.WindowDidLoad ();

        Window.DidEndLiveResize += (sender, e) => {
        // Do something after the user's finished resizing
        // the window
    };
}
```

<a name="Setting_a_Window’s_Title_and_Represented_File" />

## <a name="setting-a-windows-title-and-represented-file"></a>Définition d’une fenêtre de titre et représenté fichier

Lorsque vous travaillez avec windows qui représentent des documents, `NSWindow` a un `DocumentEdited` propriété que si la valeur `true` affiche un petit point dans le bouton Fermer pour indiquer à l’utilisateur que le fichier a été modifié et doit être enregistré avant du fermer.

Permet de modifier notre `ViewController.cs` et apportez les modifications suivantes :

```csharp
public bool DocumentEdited {
    get { return View.Window.DocumentEdited; }
    set { View.Window.DocumentEdited = value; }
}
...

public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Set Window Title
    this.View.Window.Title = "untitled";

    View.Window.WillClose += (sender, e) => {
        // is the window dirty?
        if (DocumentEdited) {
            var alert = new NSAlert () {
                AlertStyle = NSAlertStyle.Critical,
                InformativeText = "We need to give the user the ability to save the document here...",
                MessageText = "Save Document",
            };
            alert.RunModal ();
        }
    };
}

public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Show when the document is edited
    DocumentEditor.TextDidChange += (sender, e) => {
        // Mark the document as dirty
        DocumentEdited = true;
    };

    // Overriding this delegate is required to monitor the TextDidChange event
    DocumentEditor.ShouldChangeTextInRanges += (NSTextView view, NSValue[] values, string[] replacements) => {
        return true;
    };

}
```

Nous sommes analyse également les `WillClose` événement dans la fenêtre et la vérification de l’état de la `DocumentEdited` propriété. S’il s’agit `true` nous devons donner à l’utilisateur la possibilité d’enregistrer les modifications dans le fichier. Si nous exécuter notre application et que vous entrez du texte, le point s’affichera :

[![](window-images/file01.png "Une fenêtre modifiée")](window-images/file01.png#lightbox)

Si vous essayez de fermer la fenêtre, nous obtenons une alerte :

[![](window-images/file02.png "Affichage d’un enregistrement boîte de dialogue")](window-images/file02.png#lightbox)

Si nous allons charger un document à partir d’un fichier, nous pouvons définir le titre de la fenêtre pour le fichier nom à l’aide de la `window.SetTitleWithRepresentedFilename (Path.GetFileName(path));` (méthode) (étant donné que `path` est une chaîne qui représente le fichier ouvert). En outre, nous pouvons définir l’URL du fichier à l’aide du `window.RepresentedUrl = url;` (méthode).

Si l’URL pointe vers un type de fichier connu par le système d’exploitation, son icône sera affichée dans la barre de titre. Si l’utilisateur clique droit sur l’icône, le chemin d’accès au fichier s’affichera.

Permet de modifier le `AppDelegate.cs` et ajoutez la méthode suivante :

```csharp
[Export ("openDocument:")]
void OpenDialog (NSObject sender)
{
    var dlg = NSOpenPanel.OpenPanel;
    dlg.CanChooseFiles = true;
    dlg.CanChooseDirectories = false;

    if (dlg.RunModal () == 1) {
        // Nab the first file
        var url = dlg.Urls [0];

        if (url != null) {
            var path = url.Path;

            // Get new window
            var storyboard = NSStoryboard.FromName ("Main", null);
            var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

            // Display
            controller.ShowWindow(this);

            // Load the text into the window
            var viewController = controller.Window.ContentViewController as ViewController;
            viewController.Text = File.ReadAllText(path);
                    viewController.View.Window.SetTitleWithRepresentedFilename (Path.GetFileName(path));
            viewController.View.Window.RepresentedUrl = url;

        }
    }
}
```

Maintenant si nous exécutons notre application, sélectionnez **ouvrir...**  à partir de la **fichier** menu, sélectionnez un fichier texte à partir de la **ouvrir** boîte de dialogue zone et ouvrez-le :

[![](window-images/file03.png "Une boîte de dialogue Ouvrir")](window-images/file03.png#lightbox)

Le fichier s’affiche et le titre sera défini avec l’icône du fichier :

[![](window-images/file04.png "Le contenu d’un fichier chargé")](window-images/file04.png#lightbox)

<a name="Adding_a_New_Window_to_a_Project" />

## <a name="adding-a-new-window-to-a-project"></a>Ajout d’une nouvelle fenêtre à un projet

À l’exception de la fenêtre de document principal, une application Xamarin.Mac devrez peut-être afficher d’autres types de windows à l’utilisateur, telles que les préférences ou les panneaux de l’inspecteur.

Pour ajouter une nouvelle fenêtre, procédez comme suit :

1. Dans le **l’Explorateur de solutions**, double-cliquez sur le `Main.storyboard` fichier à ouvrir pour le modifier dans le Générateur de Xcode Interface.
2. Faites glisser une nouvelle **fenêtre contrôleur** à partir de la **bibliothèque** et déposez-la sur la **aire de conception**:

    [![](window-images/new01.png "Sélection d’un nouveau contrôleur de fenêtre dans la bibliothèque")](window-images/new01.png#lightbox)
3. Dans le **inspecteur d’identité**, entrez `PreferencesWindow` pour le **ID de la table de montage séquentiel**: 

    [![](window-images/new02.png "Définition de l’ID de la table de montage séquentiel")](window-images/new02.png#lightbox)
5. Concevoir votre interface : 

    [![](window-images/new03.png "Conception de l’interface utilisateur")](window-images/new03.png#lightbox)
6. Ouvrez le Menu de l’application (`MacWindows`), sélectionnez **préférences...** , Cliquez sur le contrôle et faites glisser vers la nouvelle fenêtre : 

    [![](window-images/new05.png "Création d’un segue")](window-images/new05.png#lightbox)
7. Sélectionnez **afficher** dans le menu contextuel.
6. Enregistrez vos modifications et revenir à Visual Studio pour Mac pour la synchronisation avec Xcode.

Si nous exécuter le code et sélectionnez le **préférences...**  à partir de la **Menu Application**, la fenêtre s’affichera :

[![](window-images/new04.png "Un menu de préférences de l’exemple")](window-images/new04.png#lightbox)

<a name="Working_with_Panels" />

## <a name="working-with-panels"></a>Utilisation des panneaux

Comme indiqué au début de cet article, un panneau flotte au-dessus des autres fenêtres et fournit des outils ou les contrôles que les utilisateurs peuvent travailler avec si des documents sont ouverts. 

Comme tout autre type de fenêtre que vous créez et travaillez dans votre application Xamarin.Mac, le processus est essentiellement le même :

1. Ajouter une nouvelle définition de fenêtre pour le projet.
2. Double-cliquez sur le `.xib` fichier pour ouvrir la fenêtre de conception pour la modification dans le Générateur de Xcode Interface.
2. Définir les propriétés de la fenêtre requise dans le **inspecteur de l’attribut** et **taille inspecteur**.
4. Faites glisser les contrôles requis pour générer votre interface et de les configurer dans le **inspecteur de l’attribut**.
5. Utilisez le **taille inspecteur** pour gérer le redimensionnement de vos éléments d’interface utilisateur.
6. Exposer les éléments d’interface utilisateur de la fenêtre de code c# via **prises** et **Actions**.
7. Enregistrez vos modifications et revenez à Visual Studio pour Mac pour la synchronisation avec Xcode.

Dans le **inspecteur de l’attribut**, vous disposez des options suivantes spécifiques aux panneaux :

[![](window-images/panel03.png "L’inspecteur de l’attribut")](window-images/panel03.png#lightbox)

- **Style** -vous permettent d’ajuster le style du Panneau de : panneau normal (semblable à une fenêtre standard), l’utilitaire du Panneau de configuration (a une plus petite barre de titre), panneau de configuration HUD (est translucide et la barre de titre fait partie de l’arrière-plan).
- **Activation non** -détermine dans le panneau de configuration devient la fenêtre de clé.
- **Document Modal** -si Document modale, le panneau de configuration uniquement flotte au-dessus de windows de l’application, sans quoi il flotte au-dessus de tous les.


Pour ajouter un nouveau panneau de configuration, procédez comme suit :

1. Dans le **l’Explorateur de solutions**, avec le bouton droit sur le projet et sélectionnez **ajouter** > **nouveau fichier...** .
2. Dans la boîte de dialogue Nouveau fichier, sélectionnez **Xamarin.Mac** > **fenêtre/Cocoa avec contrôleur**:

    [![](window-images/panels00.png "Ajoutez un nouveau contrôleur de fenêtre")](window-images/panels00.png#lightbox)
3. Entrez `DocumentPanel` comme **Nom**, puis cliquez sur le bouton **Nouveau**.
4. Double-cliquez sur le `DocumentPanel.xib` fichier à ouvrir pour le modifier dans le Générateur de Interface : 

    [![](window-images/new02.png "Modification de la pannel")](window-images/new02.png#lightbox)
5. Supprimer la fenêtre existante et faites glisser un panneau de configuration à partir de la **bibliothèque inspecteur** dans le le **éditeur de l’Interface**: 

    [![](window-images/panels01.png "Suppression de la fenêtre existante")](window-images/panels01.png#lightbox)
6. Le panneau de raccordement jusqu'à la **propriétaire du fichier*-**fenêtre*- **prise**: 

    [![](window-images/panels02.png "En faisant glisser permet de rattacher le panneau de configuration")](window-images/panels02.png#lightbox)
7. Basculez vers le **inspecteur d’identité** et les classe du panneau par la valeur `DocumentPanel`: 

    [![](window-images/panels03.png "Définition de la classe du panneau")](window-images/panels03.png#lightbox)
6. Enregistrez vos modifications et revenir à Visual Studio pour Mac pour la synchronisation avec Xcode.
7. Modifier la `DocumentPanel.cs` de fichiers et de modifier la définition de classe pour les éléments suivants : 

    `public partial class DocumentPanel : NSPanel`
8. Enregistrez les modifications dans le fichier.

Modifier la `AppDelegate.cs` et effectuez le `DidFinishLaunching` détaillée de la méthode comme suit :

```csharp
public override void DidFinishLaunching (NSNotification notification)
{
        // Display panel
    var panel = new DocumentPanelController ();
    panel.Window.MakeKeyAndOrderFront (this);
}

```

Si nous exécutons notre application, le panneau s’affiche :

[![](window-images/panels04.png "Le panneau de configuration dans une application en cours d’exécution")](window-images/panels04.png#lightbox)

> [!IMPORTANT]
> Panneau de configuration Windows ont été déconseillées par Apple et doit être remplacés par **inspecteur Interfaces**. Pour obtenir un exemple complet de création d’un **inspecteur** dans une application Xamarin.Mac, veuillez consulter notre [MacInspector](https://developer.xamarin.com/samples/mac/MacInspector/) exemple d’application.

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a pris une présentation détaillée sur l’utilisation des fenêtres et des panneaux dans une application Xamarin.Mac. Nous avons vu les différents types et utilise des fenêtres et volets, comment créer et gérer des fenêtres et des panneaux dans le Générateur de Xcode Interface et l’utilisation des fenêtres et des panneaux dans le code c#.

## <a name="related-links"></a>Liens associés

- [MacWindows (exemple)](https://developer.xamarin.com/samples/mac/MacWindows/)
- [MacInspector (exemple)](https://developer.xamarin.com/samples/mac/MacInspector/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Utilisation des Menus](~/mac/user-interface/menu.md)
- [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Introduction à Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
