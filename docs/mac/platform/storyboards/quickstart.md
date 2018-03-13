---
title: "Démarrage rapide de plans conceptuels"
description: "Mise en route de génération démarrée macOS des interfaces utilisateur avec des plans conceptuels."
ms.topic: article
ms.prod: xamarin
ms.assetid: 20719B5D-8147-4E8A-A23C-8D575C7ACCEE
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/02/2017
ms.openlocfilehash: fe3a93557509aba4b33b1470879cd2504ed0f2a2
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/12/2018
---
# <a name="starting-a-new-storyboard-based-project"></a>À partir d’une table de montage séquentiel en fonction de projet

Comme une présentation rapide à l’aide de tables de montage séquentiel pour définir l’Interface utilisateur d’une application Xamarin.Mac, nous pouvons commencer un nouveau projet Xamarin.Mac. Sélectionnez **Mac** > **Application** > **Application Cocoa** et cliquez sur le bouton **Suivant** :

[![](quickstart-images/qs01.png "Ajout d’une nouvelle application/Cocoa")](quickstart-images/qs01.png#lightbox)

Utilisez le **nom de l’application** de `MacStoryboard` et cliquez sur le **suivant** bouton :

[![](quickstart-images/qs02.png "Définition du nom de l’application")](quickstart-images/qs02.png#lightbox)

Utilisez la valeur par défaut **nom du projet** et **nom de la Solution** et cliquez sur le **créer** bouton :

[![](quickstart-images/qs03.png "Les noms de projet et solution")](quickstart-images/qs03.png#lightbox)

Dans le **l’Explorateur de solutions**, double-cliquez sur le `Main.storyboard` fichier à ouvrir pour le modifier dans le Générateur de Xcode Interface :

[![](quickstart-images/qs04.png "Modification du plan conceptuel dans Xcode")](quickstart-images/qs04.png#lightbox)

Comme vous pouvez le voir ci-dessus, la table de montage séquentiel de la valeur par défaut définit la barre de menus de l’application et sa fenêtre principale avec lui contrôleur d’affichage et de la vue. Pour notre exemple d’application, nous allons créer une interface utilisateur qui a un principal _affichage du contenu_ d’un côté et un _inspecteur vue_ dans la seconde.

Pour ce faire, nous devons d’abord supprimer la valeur par défaut du contrôleur de la vue et la vue qui est fourni avec le plan conceptuel à sélectionner dans le Générateur de l’Interface et en appuyant sur la **supprimer** clé :

[![](quickstart-images/qs05.png "Suppression du contrôleur de la vue par défaut")](quickstart-images/qs05.png#lightbox)

Ensuite, tapez `split` dans les **filtre** zone, sélectionnez le contrôleur de vue de fractionnement Vertical et faites-le glisser sur le _aire de conception_:

[![](quickstart-images/qs06.png "Recherchez le contrôleur d’affichage fractionné")](quickstart-images/qs06.png#lightbox)

Notez que le contrôleur inclus automatiquement enfant deux contrôleurs de la vue (et leurs vues associées), câblé-jusqu'à les côtés gauche et droit du mode fractionné. Pour lier le mode fractionné à sa fenêtre parente, appuyez sur la **contrôle** de clé, cliquez sur le contrôleur de fenêtre (le cercle bleu dans le cadre de la fenêtre du contrôleur) et faites glisser une ligne pour le contrôleur d’affichage fractionné. Sélectionnez **contenu de la fenêtre** à partir de la fenêtre contextuelle :

[![](quickstart-images/qs07.png "Définir les fenêtres Affichage de contenu")](quickstart-images/qs07.png#lightbox)

Cela lie l’élément de deux interface à l’aide d’un Segue :

[![](quickstart-images/qs08.png "Le Segue entre la fenêtre et le contenu")](quickstart-images/qs08.png#lightbox)

Vous souhaitez placer une vue de texte dans la partie gauche de la vue de fractionnement et de remplir automatiquement la zone disponible quand la fenêtre ou le mode fractionné est redimensionné. Faites glisser une vue de texte en haut contrôleur d’affichage associé à la vue de fractionnement et cliquez sur le **code confidentiel** automatique de contrainte de mise en page ((deuxième icône à droite en bas de l’aire de conception).

[![](quickstart-images/qs09.png "Configuration des contraintes")](quickstart-images/qs09.png#lightbox)

À partir d’ici, nous devra cliquer sur les quatre le **i** icônes autour du cadre englobant en haut de contraintes Popover puis cliquez sur le **ajouter des contraintes de 4** situé en bas à ajouter les contraintes nécessaires.

Si nous revenir à Visual Studio pour Mac et que vous exécutez le projet, notez que l’affichage de texte est automatiquement redimensionné pour remplir le côté gauche de l’affichage fractionné en tant que la fenêtre ou le fractionnement sont redimensionnées :

[![](quickstart-images/qs10.png "Un exemple de l’application en cours d’exécution")](quickstart-images/qs10.png#lightbox)

Étant donné que nous allons utiliser la partie droite de la vue de fractionnement comme une zone d’inspecteur, nous souhaitons à avoir une taille plus petite et de lui permettre d’être réduit. Revenir à Xcode et modifier la vue de la partie droite en la sélectionnant dans l’aire de conception et en cliquant sur le **taille inspecteur**. À partir d’ici, entrez un **largeur** de `250`:

[![](quickstart-images/qs11.png "Définition de la largeur")](quickstart-images/qs11.png#lightbox)

Sélectionnez suivant l’élément de fractionnement qui représente le côté droit, définissez un degré plus élevé **contenant une priorité** et cliquez sur le **utilisateur peut réduire le** case à cocher :

[![](quickstart-images/qs12.png "Modification de la priorité d’exploitation")](quickstart-images/qs12.png#lightbox)

Si nous renvoyons à Visual Studio pour Mac et exécutez le projet maintenant, notez que la partie droite conserve est plus petite taille et la fenêtre est redimensionnée :

[![](quickstart-images/qs13.png "Un exemple de l’application en cours d’exécution")](quickstart-images/qs13.png#lightbox)

<a name="Defining-a-Presentation-Segue" />

## <a name="defining-a-presentation-segue"></a>Définition d’une présentation Segue

Nous allons à disposition de la partie droite de la vue du fractionnement d’agir comme un inspecteur pour les propriétés du texte sélectionné. Nous allons faire glisser certains contrôles l’affichage inférieur à la disposition de l’inspecteur de l’interface utilisateur. Pour le dernier contrôle que vous souhaitez afficher un popover qui permet à l’utilisateur à sélectionner à partir de quatre styles de caractères prédéfinis.

Nous allons ajouter un bouton à un contrôleur de la vue à l’aire de conception et de l’inspecteur. Nous allons la redimensionner le contrôleur de vue à la taille que nous souhaitons que notre Popover et ajouter quatre boutons. Nous allons ensuite **contrôle** clé, cliquez sur le bouton dans la vue de l’inspecteur et faites glisser vers le contrôleur de vue qui représentera notre popover :

[![](quickstart-images/qs14.png "Glisser-déplacer pour créer un nouveau segue")](quickstart-images/qs14.png#lightbox)

Dans le menu contextuel, sélectionnez **Popover**: 

[![](quickstart-images/qs15.png "Sélection du type segue")](quickstart-images/qs15.png#lightbox)

Enfin, nous allons sélectionner le Segue dans l’aire de conception et définissez la **préféré de bord** à **gauche**. Ensuite, nous allons faire glisser une ligne à partir de la **d’ancrage vue** au bouton que nous souhaitons popover à attacher à :

[![](quickstart-images/qs16.png "Glisser-déplacer pour créer un nouveau segue")](quickstart-images/qs16.png#lightbox)

Si vous revenez à Visual Studio pour Mac, exécutez l’application et cliquez sur le **aucun** bouton dans l’inspecteur de popover s’affichera :

[![](quickstart-images/qs17.png "Un exemple de la segue en cours d’exécution")](quickstart-images/qs17.png#lightbox)

<a name="Creating-App-Preferences" />

## <a name="creating-app-preferences"></a>Création des préférences de l’application

La plupart des applications de macOS offrent un _boîte de dialogue Préférences_ qui permet à l’utilisateur de définir plusieurs options qui contrôlent les différents aspects de l’application, telles que les comptes d’utilisateur ou d’apparence.

Pour définir une fenêtre de boîte de dialogue standard de préférence, faites glisser un onglet Vue contrôleur sur l’aire de conception :

[![](quickstart-images/qs18.png "Modification du plan conceptuel dans Xcode")](quickstart-images/qs18.png#lightbox)

Là encore, cela se fera automatiquement avec deux enfant attachés des contrôleurs de la vue. Par exemple l’exploration, nous allons ajouter une étiquette à chaque vue de centre à l’intérieur :

[![](quickstart-images/qs19.png "Définition de contraintes")](quickstart-images/qs19.png#lightbox)

Ensuite, nous souhaitons afficher la fenêtre de préférences lorsque l’utilisateur sélectionne le **préférences...**  élément de menu. Dans la barre de menus, sélectionnez l’élément de menu Préférences **contrôle** clé cliquez et faites glisser une ligne à notre onglet View Controller :

[![](quickstart-images/qs20.png "Glisser-déplacer pour créer un segue")](quickstart-images/qs20.png#lightbox)

Dans le menu contextuel, sélectionnez **modale** pour afficher cette fenêtre comme une boîte de dialogue modale :

[![](quickstart-images/qs21.png "Sélection du type segue")](quickstart-images/qs21.png#lightbox)

Si nous enregistrer nos modifications, revenez à Visual Studio pour Mac, exécutez l’application et sélectionnez le **préférences...**  élément de menu, vos préférences nouvelle boîte de dialogue s’affiche :

[![](quickstart-images/qs22.png "Un exemple de la segue en cours d’exécution")](quickstart-images/qs22.png#lightbox)

Vous pouvez noter que cela ne ressemble pas à une application standard macOS fenêtre de boîte de dialogue Préférences. Pour résoudre ce problème, incluez deux fichiers image dans l’application de Xamarin.Mac `Resources` dossier dans le **l’Explorateur de solutions** et revenir au constructeur d’Interface de Xcode.

Sélectionnez l’onglet vue de contrôleur et du commutateur son **Style** à **barre d’outils**: 

[![](quickstart-images/qs23.png "Définir le style de barre d’onglet")](quickstart-images/qs23.png#lightbox)

Sélectionnez chaque onglet et lui donner un **étiquette** et sélectionnez une des images pour le représenter :

[![](quickstart-images/qs24.png "Configuration de chaque onglet dans Xcode")](quickstart-images/qs24.png#lightbox)

Si nous enregistrer nos modifications, revenez à Visual Studio pour Mac, exécutez l’application et sélectionnez le **préférences...**  élément de menu, la boîte de dialogue s’affiche désormais comme une application macOS standard :

[![](quickstart-images/qs25.png "Un exemple de la fenêtre de préférences en cours d’exécution")](quickstart-images/qs25.png#lightbox)

Pour plus d’informations, consultez notre [utilisation des Images](~/mac/app-fundamentals/image.md), [Menus](~/mac/user-interface/menu.md), [Windows](~/mac/user-interface/window.md) et [boîtes de dialogue](~/mac/user-interface/dialog.md) documentation.

## <a name="related-links"></a>Liens associés

- [MacStoryboard (exemple)](https://developer.xamarin.com/samples/mac/MacStoryboard/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Utilisation des fenêtres](~/mac/user-interface/window.md)
- [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Introduction à Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
