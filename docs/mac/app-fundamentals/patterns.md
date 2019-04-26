---
title: Modèles et idiomes dans Xamarin.Mac courants
description: Ce document décrit les modèles de conception courants utilisés lors de la création d’applications Xamarin.Mac. Il aborde le modèle model-view-controller, les modèles de données source et le délégué et les protocoles.
ms.prod: xamarin
ms.assetid: BF0A3517-17D8-453D-87F7-C8A34BEA8FF5
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 06/17/2016
ms.openlocfilehash: b4266582ce0cec522e207cd06987f2d0eeff8b2d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61090859"
---
# <a name="common-patterns-and-idioms-in-xamarinmac"></a>Modèles et idiomes dans Xamarin.Mac courants

Tout au long de l’API Apple exposées via c#, certains modèles et idiomes apparu indéfiniment. Si vous êtes habitué avec la programmation avec Xamarin.iOS, il peuvent vous paraître familiers. Documentation fait souvent référence à ces modèles et idiomes d’à plusieurs reprises, afin d’avoir une connaissance approfondie des vous permettra de sens de la documentation que vous trouverez.

## <a name="mvc---model-view-controller"></a>MVC - Model View Controller

Model View Controller ou MVC pour faire court, est un modèle très courant de trouver tout au long de Cocoa. Une présentation détaillée est abordée dans ce document, mais en bref, il est une façon de structurer votre application en composants :

- **Modèle** objets représentent les données sous-jacentes à affichées et manipulées (comme des adresses dans un carnet d’adresses)
- **Vue** objets gérer le dessin d’un objet donné à l’écran et en gérant l’interaction utilisateur (un champ de texte montrant l’adresse sur l’écran)
- **Contrôleur** objets gèrent l’interaction entre le modèle et la vue. Ils push des modifications apportées au modèle « jusqu'à » pour mettre à jour la vue et propager » « modifications de la vue lorsque des utilisateurs dans l’interface utilisateur.

Si vous êtes familiarisé avec MVVM (Model View ViewModel) à partir d’autres bibliothèques telles que WPF, le contrôleur se comporte comme le ViewModel, mais est souvent plus étroitement lié aux éléments d’interface utilisateur spécifiques.

Vous trouverez ici des plus de détails :

- [Apprentissage de MVC sur un site Web d’Apple](https://developer.apple.com/library/ios/documentation/general/conceptual/devpedia-cocoacore/MVC.html)

- [Model View Controller dans Objective-C](https://developer.apple.com/library/ios/documentation/general/conceptual/CocoaEncyclopedia/Model-View-Controller/Model-View-Controller.html)
- [Utilisation de Windows](~/mac/user-interface/window.md)

## <a name="data-source--delegate--subclassing"></a>Source de données / délégué / sous-classement

Un autre modèle très courant dans Cocoa porte sur fournir des données aux éléments d’interface utilisateur et de réagir aux interactions de l’utilisateur. À l’aide de `NSTableView` par exemple, vous devez fournir une certaine manière les données pour chaque ligne, définir certaines des éléments d’interface qui représentent cette ligne, certaines ensemble de comportements pour réagir aux interactions de l’utilisateur et éventuellement une certaine quantité de personnalisation. Les modèles de données source et le délégué vous permettent de gérer la plupart des cas sans avoir recours au sous-classement `NSTableView` vous-même.

- Le `DataSource` une instance d’une sous-classe personnalisée est affectée à la propriété `NSTableViewDataSource` qui est appelée pour remplir la table avec des données (via `GetRowCount` et `GetObjectValue`).

- Le `Delegate` une instance d’une sous-classe personnalisée est affectée à la propriété `NSTableViewDelegate` qui fournit la vue pour un objet de modèle donné (par le biais de `GetViewForItem`) et gère les interactions de l’interface utilisateur (par le biais de `DidClickTableColumn`, `MouseDownInHeaderOfTableColumn`, etc.).

Dans certains cas, vous souhaitez personnaliser un contrôle de manière au-delà les raccordements donné dans la délégué ou source de données et vous pouvez créer une sous-classe la vue directement. Soyez toutefois prudent, dans de nombreux cas, remplacement par défaut comportement puis vous obligera à gérer tout ce comportement (personnalisation du comportement de sélection peut vous obliger à implémenter tous les comportements de sélection).

Dans Xamarin.iOS, certaines API, telles que `UITableView` ont été étendues avec une propriété qui implémente le délégué et la source de données (`UITableViewSource`). Cette informatique à contourner la limitation courants qui un seul C# classe peut uniquement avoir une classe de base, et notre en exposant des protocoles s’effectue par le biais de classes de base.

Pour plus d’informations sur l’utilisation des vues de table dans une application Xamarin.Mac, veuillez consulter notre [Table vue](~/mac/user-interface/table-view.md) documentation.

## <a name="protocols"></a>Protocoles

Protocoles en Objective-C peuvent être comparés aux interfaces dans C#et dans de nombreux cas sont utilisés dans des situations similaires. Par exemple le `NSTableView` exemple ci-dessus, le délégué et la source de données sont en fait des protocoles. Xamarin.Mac expose ceux-ci comme classes de base avec des méthodes virtuelles, que vous pouvez remplacer. La principale différence entre C# interfaces et protocoles de Objective-C est que certaines méthodes dans un protocole peuvent être facultatives à implémenter. Vous devez consulter la documentation ou de la définition d’une API pour déterminer ce qui est facultatif.

Plus d’informations, consultez notre [délégués, des protocoles et des événements](~/ios/app-fundamentals/delegates-protocols-and-events.md) documentation.



## <a name="related-links"></a>Liens associés

- [Vues de table](~/mac/user-interface/table-view.md)
- [Utilisation de windows](~/mac/user-interface/window.md)
- [Les délégués, des protocoles et des événements](~/ios/app-fundamentals/delegates-protocols-and-events.md)
- [Model-View-Controller](https://developer.apple.com/library/ios/documentation/general/conceptual/CocoaEncyclopedia/Model-View-Controller/Model-View-Controller.html)
