---
title: Storyboards dans Xamarin.Mac – Guide de démarrage rapide
description: Ce document fournit une présentation du démarrage rapide à la création d’interfaces utilisateur avec des storyboards dans Xamarin.Mac macOS. Il décrit comment créer un segue et créer une fenêtre de préférences.
ms.prod: xamarin
ms.assetid: 20719B5D-8147-4E8A-A23C-8D575C7ACCEE
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 05/02/2017
ms.openlocfilehash: 7f7d23a01a3c3c6567d6bab45d0abbfb078fb512
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50112570"
---
# <a name="storyboards-in-xamarinmac-quick-start"></a>Storyboards dans Xamarin.Mac – Guide de démarrage rapide

Une introduction rapide à l’aide de Storyboards pour définir l’Interface utilisateur d’une application Xamarin.Mac, nous allons commencer un nouveau projet Xamarin.Mac. Sélectionnez **Mac** > **Application** > **Application Cocoa** et cliquez sur le bouton **Suivant** :

[![](quickstart-images/qs01.png "Ajoutez une nouvelle application Cocoa")](quickstart-images/qs01.png#lightbox)

Utilisez le **nom de l’application** de `MacStoryboard` et cliquez sur le **suivant** bouton :

[![](quickstart-images/qs02.png "Définition du nom de l’application")](quickstart-images/qs02.png#lightbox)

Utilisez la valeur par défaut **nom_projet** et **nom de la Solution** et cliquez sur le **créer** bouton :

[![](quickstart-images/qs03.png "Les noms de projet et solution")](quickstart-images/qs03.png#lightbox)

Dans le **l’Explorateur de solutions**, double-cliquez sur le `Main.storyboard` fichier à ouvrir pour modification dans Xcode Interface Builder :

[![](quickstart-images/qs04.png "Modification de la table de montage séquentiel dans Xcode")](quickstart-images/qs04.png#lightbox)

Comme vous pouvez le voir ci-dessus, la table de montage séquentiel par défaut définit barre de menus de l’application et sa fenêtre principale avec ce contrôleur d’affichage et de la vue. Pour notre exemple d’application, nous allons créer une interface utilisateur qui a une main _affichage du contenu_ un côté et un _inspecteur vue_ dans la seconde.

Pour ce faire, nous devrons supprimer d’abord la valeur par défaut du contrôleur d’affichage et la vue qui est fourni avec la table de montage séquentiel à sélectionner dans Interface Builder et en appuyant sur la **supprimer** clé :

[![](quickstart-images/qs05.png "Supprimer le contrôleur d’affichage par défaut")](quickstart-images/qs05.png#lightbox)

Ensuite, tapez `split` dans le **filtre** zone, sélectionnez le contrôleur d’affichage fractionnement Vertical et faites-le glisser sur le _aire de conception_:

[![](quickstart-images/qs06.png "Recherchez le contrôleur d’affichage fractionné")](quickstart-images/qs06.png#lightbox)

Notez que le contrôleur inclus automatiquement enfants de deux contrôleurs d’affichage (et leurs vues associées), câblé-jusqu'à les côtés gauche et droit de la vue de fractionnement. Pour lier le mode fractionné à la fenêtre de son parent, appuyez sur la **contrôle** de clé, cliquez sur le contrôleur de fenêtre (le cercle bleu dans le cadre du contrôleur de fenêtre) et faites glisser une ligne pour le contrôleur d’affichage fractionné. Sélectionnez **contenu de la fenêtre** à partir de la fenêtre contextuelle :

[![](quickstart-images/qs07.png "Définissant les fenêtres Affichage de contenu")](quickstart-images/qs07.png#lightbox)

Cela lie l’élément de deux interface à l’aide d’un Segue :

[![](quickstart-images/qs08.png "Le Segue entre la fenêtre et le contenu")](quickstart-images/qs08.png#lightbox)

Nous voulons placer un affichage de texte dans la partie gauche de la vue de fractionnement et de remplir automatiquement la zone disponible lorsque la fenêtre ou le mode fractionné est redimensionné. Faites glisser un affichage de texte en haut contrôleur d’affichage associé à la vue de fractionnement et cliquez sur le **Pin** auto de contrainte de mise en page ((deuxième icône à droite en bas de l’aire de conception).

[![](quickstart-images/qs09.png "Configurer les contraintes")](quickstart-images/qs09.png#lightbox)

À partir d’ici, nous allons cliquer sur ces quatre le **en i** icônes englobant en haut du menu de contraintes, cliquez sur le **ajouter 4 contraintes** bouton en bas pour ajouter les contraintes requises.

Si nous retournez dans Visual Studio pour Mac et que vous exécutez le projet, notez que l’affichage de texte est automatiquement redimensionné pour remplir le côté gauche de la vue de fractionnement en tant que la fenêtre ou le fractionnement sont redimensionnées :

[![](quickstart-images/qs10.png "Un exemple de l’application en cours d’exécution")](quickstart-images/qs10.png#lightbox)

Étant donné que nous allons utiliser la partie droite de la vue de fractionnement comme une zone d’inspecteur, nous voulons pouvoir avoir une taille plus petite et de lui permettre d’être réduite. Revenez à Xcode et modifier la vue pour la partie droite en le sélectionnant dans l’aire de conception et en cliquant sur le **inspecteur de taille**. À partir d’ici, entrez un **largeur** de `250`:

[![](quickstart-images/qs11.png "Définition de la largeur")](quickstart-images/qs11.png#lightbox)

Sélectionnez suivant l’élément de fractionnement qui représente le côté droit, définissez un degré plus élevé **contenant une priorité** et cliquez sur le **utilisateur peut réduire** case à cocher :

[![](quickstart-images/qs12.png "Modification de la priorité d’exploitation")](quickstart-images/qs12.png#lightbox)

Si nous retournons à Visual Studio pour Mac et exécutez le projet maintenant, notez que la partie droite conserve est plus petite taille et la fenêtre est redimensionnée :

[![](quickstart-images/qs13.png "Un exemple de l’application en cours d’exécution")](quickstart-images/qs13.png#lightbox)

<a name="Defining-a-Presentation-Segue" />

## <a name="defining-a-presentation-segue"></a>Définition d’une présentation de Segue

Nous allons à disposition de la partie droite de la vue du fractionnement d’agir comme un inspecteur pour les propriétés du texte sélectionné. Nous allons faire glisser des contrôles la vue de bas à disposition de l’interface utilisateur de l’inspecteur. Pour le dernier contrôle, nous souhaitons afficher un menu qui permet à l’utilisateur à sélectionner à partir de quatre styles de caractères prédéfinie.

Nous allons ajouter un bouton à un contrôleur d’affichage à l’aire de conception et de l’inspecteur. Nous allons la redimensionner le contrôleur d’affichage à la taille que nous voulons que notre menu pour être et lui ajouter quatre boutons. Nous allons ensuite **contrôle** clé, cliquez sur le bouton dans la vue de l’inspecteur et faites glisser vers le contrôleur d’affichage qui représentera notre menu :

[![](quickstart-images/qs14.png "Glissement pour créer un nouveau segue")](quickstart-images/qs14.png#lightbox)

Dans le menu contextuel, nous allons sélectionner **menu**: 

[![](quickstart-images/qs15.png "Sélection du type de segue")](quickstart-images/qs15.png#lightbox)

Enfin, nous allons sélectionner le Segue dans l’aire de conception et définissez le **Edge préféré** à **gauche**. Ensuite, nous allons faire glisser une ligne à partir de la **d’ancrage vue** au bouton que nous voulons le menu à attacher à :

[![](quickstart-images/qs16.png "Glissement pour créer un nouveau segue")](quickstart-images/qs16.png#lightbox)

Si nous revenons à Visual Studio pour Mac, exécutez l’application et cliquez sur le **aucun** bouton dans l’inspecteur, le menu s’affiche :

[![](quickstart-images/qs17.png "Un exemple du segue en cours d’exécution")](quickstart-images/qs17.png#lightbox)

<a name="Creating-App-Preferences" />

## <a name="creating-app-preferences"></a>Création des préférences de l’application

La plupart des applications Mac OS fournissent un _boîte de dialogue Préférences_ qui permet à l’utilisateur définir plusieurs options qui contrôlent les différents aspects de l’application, tels que des comptes d’utilisateurs ou d’apparence.

Pour définir une fenêtre de boîte de dialogue Préférences standard, tout d’abord de faire glisser un contrôleur d’affichage onglet sur l’aire de conception :

[![](quickstart-images/qs18.png "Modification de la table de montage séquentiel dans Xcode")](quickstart-images/qs18.png#lightbox)

Là encore, cela se fera automatiquement avec l’enfant de deux contrôleurs d’affichage attachés. Par exemple les choses, nous allons ajouter une étiquette à chaque vue centre qu’il contient :

[![](quickstart-images/qs19.png "Définition de contraintes")](quickstart-images/qs19.png#lightbox)

Ensuite, nous souhaitons afficher la fenêtre de préférences lorsque l’utilisateur sélectionne le **préférences...**  élément de menu. Dans la barre de menus, sélectionnez l’élément de menu Préférences **contrôle** clé cliquez et faites glisser une ligne vers notre contrôleur d’affichage onglet :

[![](quickstart-images/qs20.png "Glissement pour créer un segue")](quickstart-images/qs20.png#lightbox)

À partir de la fenêtre contextuelle, nous allons sélectionner **modale** pour afficher cette fenêtre de boîte de dialogue modale :

[![](quickstart-images/qs21.png "Sélection du type de segue")](quickstart-images/qs21.png#lightbox)

Si nous enregistrons nos modifications, retournez dans Visual Studio pour Mac, exécutez l’application et sélectionnez le **préférences...**  élément de menu, nos nouvelles préférences de boîte de dialogue s’affiche :

[![](quickstart-images/qs22.png "Un exemple du segue en cours d’exécution")](quickstart-images/qs22.png#lightbox)

Vous pouvez remarquer que cela ne ressemble pas à une application macOS standard fenêtre de boîte de dialogue Préférences. Pour résoudre ce problème, incluez deux fichiers image dans l’application Xamarin.Mac `Resources` dossier dans le **l’Explorateur de solutions** et revenir à l’Interface Builder de Xcode.

Sélectionnez l’onglet vue de contrôleur et du commutateur son **Style** à **barre d’outils**: 

[![](quickstart-images/qs23.png "Définition du style de barre d’onglet")](quickstart-images/qs23.png#lightbox)

Sélectionnez chaque onglet et lui donner un **étiquette** et sélectionnez une des images pour le représenter :

[![](quickstart-images/qs24.png "Configuration de chaque onglet dans Xcode")](quickstart-images/qs24.png#lightbox)

Si nous enregistrons nos modifications, retournez dans Visual Studio pour Mac, exécutez l’application et sélectionnez le **préférences...**  élément de menu, la boîte de dialogue s’affiche désormais comme une application macOS standard :

[![](quickstart-images/qs25.png "Un exemple de la fenêtre de préférences en cours d’exécution")](quickstart-images/qs25.png#lightbox)

Pour plus d’informations, consultez notre [utilisation des Images](~/mac/app-fundamentals/image.md), [Menus](~/mac/user-interface/menu.md), [Windows](~/mac/user-interface/window.md) et [boîtes de dialogue](~/mac/user-interface/dialog.md) documentation.

## <a name="related-links"></a>Liens associés

- [MacStoryboard (exemple)](https://developer.xamarin.com/samples/mac/MacStoryboard/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Utilisation de Windows](~/mac/user-interface/window.md)
- [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Introduction à Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
