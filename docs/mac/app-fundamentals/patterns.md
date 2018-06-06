---
title: Communes et idiomes dans Xamarin.Mac
description: Ce document décrit les modèles de conception courants utilisés lors de la création d’applications de Xamarin.Mac. Elle décrit le modèle model-view-controller, les modèles de données source et le délégué et les protocoles.
ms.prod: xamarin
ms.assetid: BF0A3517-17D8-453D-87F7-C8A34BEA8FF5
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/17/2016
ms.openlocfilehash: f6bba5575edf2dcbddbd354b590e03f9fed06291
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34791535"
---
# <a name="common-patterns-and-idioms-in-xamarinmac"></a>Communes et idiomes dans Xamarin.Mac

Dans les API Apple exposés via c#, certains langages et les modèles de démarrage indéfiniment. Si vous avez aucune expérience en programmation avec Xamarin.iOS, il peuvent sembler familières. Documentation fait souvent référence à ces et idiomes à plusieurs reprises, afin d’avoir une connaissance approfondie des vous permettra de sens de la documentation que vous trouverez.

## <a name="mvc---model-view-controller"></a>MVC - Model View Controller

Model-View-Controller ou MVC pour faire court, est très courant recherché via/Cocoa. Une présentation détaillée est abordée dans ce document, mais en bref, il est une façon de structurer votre application en composants :

- **Modèle** objets représentent les données sous-jacentes à afficher et manipuler (comme des adresses dans le carnet d’adresses)
- **Vue** objets gérer le dessin d’un objet donné à l’écran et la gestion des interactions de l’utilisateur (un champ de texte montrant l’adresse sur l’écran)
- **Contrôleur** objets gérer l’interaction entre le modèle et la vue. Ils push des modifications apportées au modèle « jusqu'à » pour mettre à jour la vue et transmettre des « « modifications de la vue lorsque les utilisateurs apportent des modifications dans l’interface utilisateur.

Si vous êtes familiarisé avec MVVM (Model View ViewModel) à partir d’autres bibliothèques telles que WPF, le contrôleur se comporte comme le ViewModel mais il est souvent plus étroitement lié à des éléments spécifiques de l’interface utilisateur.

Vous trouverez ici des plus de détails :

- [MVC d’apprentissage sur le site Web d’Apple](https://developer.apple.com/library/ios/documentation/general/conceptual/devpedia-cocoacore/MVC.html)

- [Model View Controller dans Objective-C](https://developer.apple.com/library/ios/documentation/general/conceptual/CocoaEncyclopedia/Model-View-Controller/Model-View-Controller.html)
- [Utilisation des fenêtres](~/mac/user-interface/window.md)

## <a name="data-source--delegate--subclassing"></a>Source de données, la déléguer, sous-classement

Un autre modèle très courant dans/Cocoa porte fournissant des données à des éléments d’interface utilisateur et aux interactions de l’utilisateur. À l’aide de `NSTableView` , par exemple, vous devez fournir une certaine manière les données pour chaque ligne, certains définis représentant cette ligne, certains éléments d’interface utilisateur définir des comportements à réagir aux interactions de l’utilisateur et éventuellement une certaine quantité de personnalisation. Les modèles de données source et le délégué vous permettent de gérer la plupart des cas sans avoir recours au sous-classement `NSTableView` vous-même.

- Le `DataSource` est affectée à une instance d’une sous-classe personnalisée de `NSTableViewDataSource` qui est appelée pour remplir la table avec des données (via `GetRowCount` et `GetObjectValue`).

- Le `Delegate` est affectée à une instance d’une sous-classe personnalisée de `NSTableViewDelegate` qui fournit l’affichage d’un objet de modèle donné (via `GetViewForItem`) et gère les interactions de l’interface utilisateur (via `DidClickTableColumn`, `MouseDownInHeaderOfTableColumn`, etc.).

Dans certains cas, que vous souhaitez personnaliser un contrôle d’une manière au-delà les raccordements donné dans la source de données ou le délégué, et vous pouvez créer une sous-classe la vue directement. Veillez toutefois, dans de nombreux cas, la substitution de valeur par défaut comportement puis vous obligent à gérer tout ce comportement (Personnaliser le comportement de sélection pouvant vous obliger à implémenter tous les comportements de sélection).

Dans Xamarin.iOS, certaines API, telles que `UITableView` ont été étendues avec une propriété qui implémente le délégué et la source de données (`UITableViewSource`). Pour contourner la limitation commune qui a une classe c# peut avoir uniquement une classe de base et notre surfaces de protocoles cela via les classes de base.

Pour plus d’informations sur l’utilisation des vues de table dans une application Xamarin.Mac, veuillez consulter notre [Table vue](~/mac/user-interface/table-view.md) documentation.

## <a name="protocols"></a>Protocoles

Protocoles dans Objective-C peuvent être comparés aux interfaces en c# et dans de nombreux cas sont utilisés dans des situations similaires. Par exemple le `NSTableView` exemple ci-dessus, le délégué et la source de données sont réellement des protocoles. Xamarin.Mac expose comme des classes de base avec des méthodes virtuelles que vous pouvez substituer. La principale différence entre les interfaces c# et les protocoles Objective-C est que certaines méthodes dans un protocole peuvent être facultatives à implémenter. Vous devrez consulter la documentation et/ou de la définition d’une API pour déterminer ce qui est facultatif.

Plus d’informations, consultez notre [délégués, les protocoles et les événements](~/ios/app-fundamentals/delegates-protocols-and-events.md) documentation.



## <a name="related-links"></a>Liens associés

- [Vues de table](~/mac/user-interface/table-view.md)
- [Utilisation des fenêtres](~/mac/user-interface/window.md)
- [Les délégués, les protocoles et les événements](~/ios/app-fundamentals/delegates-protocols-and-events.md)
- [Model-View-Controller](https://developer.apple.com/library/ios/documentation/general/conceptual/CocoaEncyclopedia/Model-View-Controller/Model-View-Controller.html)
