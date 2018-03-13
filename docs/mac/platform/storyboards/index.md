---
title: "Introduction à des plans conceptuels"
description: "Cet article fournit une introduction à l’utilisation des animations dans une application Xamarin.Mac. Il aborde la création et la gestion de l’interface utilisateur de l’application à l’aide de storyboards et d’Interface Builder de Xcode."
ms.topic: article
ms.prod: xamarin
ms.assetid: F37BA503-0B25-489F-80A8-58C493291A55
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: ccee60b5d953987e858ef592d005cec9803b8b96
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="introduction-to-storyboards"></a>Introduction à des plans conceptuels

_Cet article fournit une introduction à l’utilisation des animations dans une application Xamarin.Mac. Elle couvre la création et la gestion de l’interface utilisateur de l’application à l’aide de plans conceptuels et Interface Builder de Xcode._

Storyboards permettent de développer une Interface utilisateur pour votre application Xamarin.Mac qui comprend non seulement les contrôles et les définitions de la fenêtre, mais contient également les liens entre les différentes fenêtres (via est parfait pour) et des États d’affichage.

[![](images/intro01.png "Un exemple d’interface utilisateur dans Xcode")](images/intro01.png#lightbox)

Cet article fournit une introduction à l’utilisation des animations pour définir l’Interface utilisateur de l’application d’un Xamarin.Mac.

<a name="What-are-Storyboards" />

## <a name="what-are-storyboards"></a>Quelles sont les animations ?

À l’aide de plans conceptuels, tous de l’interface utilisateur d’une application Xamarin.Mac peuvent être définis dans un emplacement unique avec la navigation entre ses éléments individuels et des interfaces utilisateur. Storyboards pour Xamarin.Mac, fonctionnent de manière très similaire à des plans conceptuels pour Xamarin.iOS. Toutefois, ils contiennent un ensemble différent de _Segue des Types_ en raison des idiomes interface différente.

<a name="Working-with-Scenes" />

### <a name="working-with-scenes"></a>Utilisation des séquences

Comme indiqué ci-dessus, un plan conceptuel définit l’ensemble de l’interface utilisateur pour une application donnée est divisée en une vue d’ensemble fonctionnelle de son _affichage des contrôleurs de_. Dans le Générateur de Xcode Interface, chacune de ces contrôleurs réside dans son propre _scène_.

[![](images/intro02.png "Un contrôleur de vue d’exemple")](images/intro02.png#lightbox)

Chaque séquence représente une vue et la vue contrôleur paire donné avec un ensemble de lignes (appelée Segues) qui se connectent à chaque séquence dans l’interface utilisateur, montrant ainsi leurs relations. Certains Segues définir comment une vue contrôleur contient un ou plusieurs vues enfants ou les contrôleurs de la vue. Autres Segues, définir des transitions entre le contrôleur d’affichage (par exemple, l’affichage d’une boîte de dialogue ou popover). 

[![](images/intro03.png "Un segue exemple")](images/intro03.png#lightbox)

Le plus important de noter est que chaque Segue représente le flux de certains types de données entre l’élément donné de l’interface utilisateur de l’application.

<a name="Working-with-View-Controllers" />

### <a name="working-with-view-controllers"></a>Utilisation de contrôleurs de la vue

Affichage des contrôleurs définissent les relations entre une vue donnée d’informations dans une application Mac et le modèle de données qui fournit ces informations. Chaque séquence de niveau supérieur dans la table de montage séquentiel représente un contrôleur de vue dans le code de l’application Xamarin.Mac.

[![](images/intro04.png "Un exemple des bons de contrôleur d’affichage")](images/intro04.png#lightbox)

De cette manière, chaque contrôleur d’affichage est un couplage autonomes, réutilisables de représentation visuelle de l’information (vue) et la logique de présentation et de contrôler ces informations.

Dans une séquence donnée, vous pouvez effectuer toutes les opérations qui aurait normalement été traitées par personne `.xib` fichiers : 

 - Place subviews et contrôle (par exemple, des boutons et des zones de texte).
 - Définir les positions de l’élément et les contraintes de disposition automatique.
 - Câble des Actions et prises pour exposer les éléments d’interface utilisateur au code.

<a name="Working-with-Segues" />

### <a name="working-with-segues"></a>Utilisation d’est parfait pour

Comme indiqué ci-dessus, Segues fournissent les relations entre tous les scènes qui définissent l’interface utilisateur de votre application. Si vous êtes familiarisé avec l’utilisation dans les plans conceptuels dans iOS, vous savez qu’est parfait pour iOS définissent généralement les transitions entre les vues de plein écran. Cela diffère macOS, lorsque Segues définissent généralement « imbrication » (où une scène est l’enfant d’un parent de scène).

Dans macOS, la plupart des applications ont tendance à leur point de vue dans la même fenêtre à l’aide des éléments d’interface utilisateur tels que les affichages fractionnés et onglets de groupe. Contrairement à iOS, où vues doivent être transférée sur et hors écran, en raison de la limite physique affiche l’espace.

Étant donné les tendances de macOS vers la relation contenant-contenu, il existe des situations où _présentation est parfait pour_ sont utilisés, tels que des fenêtres modales, tableaux et Popover.

Lorsque vous utilisez la présentation est parfait pour, vous pouvez remplacer le `PrepareForSegue` méthode du contrôleur de la vue parent pour la présentation d’initialisation et de variables et fournir des données au contrôleur de vue en cours de présentation.

<a name="Design-and-Run-Times" />

### <a name="design-and-run-times"></a>Conception et le temps d’exécution

Au moment du Design (lors de la disposition de l’interface utilisateur dans le Générateur de Xcode Interface), chaque élément d’interface utilisateur de l’application est divisée dans ses éléments constitutifs :

- **Scènes** , sont composés de :
    - **Afficher le contrôleur** -qui définissent les relations entre les vues et les données qui les prennent en charge.
    - **Vues et sous-vues** -éléments réels qui composent l’interface utilisateur.
    - **Relation contenant-contenu Segues** -qui définissent les relations parent-enfant entre les séquences.
- **Présentation Segues** -qui définissent les modes de présentation individuels. 

En définissant chaque élément de cette façon, il permet le chargement différé de chaque élément uniquement si elle est nécessaire pendant l’exécution. Dans macOS, l’ensemble du processus a été conçu pour permettre au développeur de créer des Interfaces utilisateur complexes et flexible qui nécessitent un minimum de code pour qu’ils fonctionnent, de stockage tout en étant aussi efficaces avec les ressources système que possible.

<a name="Storyboard-Quick-Start" />

## <a name="storyboard-quick-start"></a>Démarrage rapide de table de montage séquentiel

Dans le [démarrage rapide de plan conceptuel](~/mac/platform/storyboards/quickstart.md) guide, nous allons créer une simple application Xamarin.Mac qui présente les concepts clés de l’utilisation de tables de montage séquentiel pour créer une Interface utilisateur. L’exemple d’application se compose d’une vue de Spilt contenant un _zone de contenu_ et un _inspecteur zone_ et présente une fenêtre de boîte de dialogue Préférences simple. Nous utiliserons Segues pour regrouper tous les éléments d’Interface utilisateur.

<a name="Working-with-Storyboards" />

## <a name="working-with-storyboards"></a>Utilisation des animations

Cette section aborde les informations détaillées de [utilisation des animations](~/mac/platform/storyboards/indepth.md) dans une application Xamarin.Mac. Nous prendre approfondi en arrière-plan et comment ils sont composés de contrôleurs de la vue et la vue. Ensuite, nous allons examiner comment les scènes sont liées par Segues. Enfin, nous étudierons une utilisation avec des types Segue personnalisés. 

<a name="Complex-Storyboard-Example" />

## <a name="complex-storyboard-example"></a>Exemple de plan conceptuel complexe

Pour obtenir un exemple d’un exemple complex de l’utilisation de plans conceptuels dans une application Xamarin.Mac, consultez la [SourceWriter, exemple d’application](https://developer.xamarin.com/samples/mac/SourceWriter/). SourceWriter est un éditeur de code source simple qui assure la prise en charge de la complétion de code et de la coloration syntaxique de base.

Le code SourceWriter a été entièrement commenté et, le cas échéant, des liens ont être fournis entre les principales technologies ou méthodes et des informations pertinentes dans la documentation des guides Xamarin.Mac.

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a pris un coup de œil rapide à l’utilisation des animations dans une application Xamarin.Mac. Nous avons vu comment créer une application à l’aide de plans conceptuels et comment définir une interface utilisateur. Nous avons également vu comment faire pour naviguer entre les différentes fenêtres et états d’affichage à l’aide est parfait pour.


## <a name="related-links"></a>Liens associés

- [Hello, Mac (exemple)](https://developer.xamarin.com/samples/mac/Hello_Mac/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Utilisation des fenêtres](~/mac/user-interface/window.md)
- [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Introduction à Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
