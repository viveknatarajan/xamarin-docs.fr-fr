---
title: Liaison de données Xamarin.Forms
description: Liaison de données est la technique de liaison des propriétés de deux objets afin que les modifications apportées à une propriété sont automatiquement reflétées dans l’autre propriété. Liaison de données fait partie intégrante de l’architecture d’application Model-View-ViewModel (MVVM).
ms.prod: xamarin
ms.assetid: 938E85C8-521D-43B9-92CB-D591A06D98A6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: a5ea5dcb5b108da52634f131fd36a91ba82f7da4
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35240351"
---
# <a name="xamarinforms-data-binding"></a>Liaison de données Xamarin.Forms

_Liaison de données est la technique de liaison des propriétés de deux objets afin que les modifications apportées à une propriété sont automatiquement reflétées dans l’autre propriété. Liaison de données fait partie intégrante de l’architecture d’application Model-View-ViewModel (MVVM)._

## <a name="the-data-linking-problem"></a>Le problème de liaison de données

Une application Xamarin.Forms se compose d’une ou plusieurs pages, chacune d’elles contient généralement plusieurs objets d’interface utilisateur appelés *vues*. Une des tâches du programme principales est conserver ces vues synchronisées et pour suivre les valeurs ou les sélections qu’ils représentent différents. Fréquence à laquelle les affichages représentent les valeurs d’une source de données sous-jacente, et l’utilisateur manipule ces vues pour modifier ces données. Lorsque la vue est modifiée, les données sous-jacentes doivent refléter ce changement, et de même, lorsque les données sous-jacentes changent, cette modification doit être mentionnée dans la vue.

Pour gérer cette tâche avec succès, le programme doit être averti des modifications apportées à ces vues ou les données sous-jacentes. La solution courante consiste à définir les événements qui indiquent lorsqu’une modification se produit. Un gestionnaire d’événements peut être installé qui est notifié de ces modifications. Il répond en transférant les données d’un objet vers un autre. Toutefois, lorsqu’il existe plusieurs vues, il doit y avoir plusieurs gestionnaires d’événements, et beaucoup de code obtient impliqué.

## <a name="the-data-binding-solution"></a>La Solution de liaison de données

Liaison de données automatise ce travail et restitue les gestionnaires d’événements inutiles. (Les événements sont toujours nécessaires, toutefois, étant donné que l’infrastructure de liaison de données utilise les). Liaisons de données peuvent être implémentées dans le code ou en XAML, mais elles sont beaucoup plus courantes dans XAML où elles contribuent à réduire la taille du fichier code-behind. En remplaçant les procédures de code dans les gestionnaires d’événements par du code déclaratif ou de balise, l’application est simplifiée et a été clarifiée.

Les deux objets impliqués dans une liaison de données est presque toujours un élément qui dérive de `View` et constitue une partie de l’interface visuelle d’une page. L’autre objet est le suivant :

- Un autre `View` dérivés, généralement sur la même page.
- Un objet dans un fichier de code.

Dans les programmes de démonstration, tels que ceux dans le [ **DataBindingDemos** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) exemple, les liaisons de données entre deux `View` dérivés sont souvent affichés pour des raisons de simplicité et de clarté. Toutefois, les mêmes principes peuvent être appliqués à des liaisons de données entre un `View` et d’autres objets. Quand une application est générée à l’aide de l’architecture Model-View-ViewModel (MVVM), la classe avec les données sous-jacentes est souvent appelée un ViewModel.

Liaisons de données sont abordées dans la série d’articles suivante :

## <a name="basic-bindingsbasic-bindingsmd"></a>[Liaisons de base](basic-bindings.md)

Découvrez la différence entre la cible et source de liaison de données et les liaisons de données simple dans le code et XAML.

## <a name="binding-modebinding-modemd"></a>[Mode de liaison](binding-mode.md)

Découvrez comment le mode de liaison peut contrôler le flux de données entre les deux objets.

## <a name="string-formattingstring-formattingmd"></a>[Formatage de chaîne](string-formatting.md)

Utilisez une liaison de données pour mettre en forme et afficher les objets sous forme de chaînes.

## <a name="binding-pathbinding-pathmd"></a>[Chemin de liaison](binding-path.md)

Plonger dans le `Path` propriété de la liaison de données pour accéder aux membres de collection et de sous-propriétés.

## <a name="binding-value-convertersconvertersmd"></a>[Convertisseurs de valeur de liaison](converters.md)

Utilisez des convertisseurs de valeurs de liaison pour modifier les valeurs au sein de la liaison de données.

## <a name="the-command-interfacecommandingmd"></a>[Interface de commande](commanding.md)

Implémentez la `Command` propriété avec des liaisons de données.



## <a name="related-links"></a>Liens associés

- [Démonstrations de liaison de données (exemple)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Chapitre de liaison de données à partir du carnet de Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
- [Extensions de balisage XAML](~/xamarin-forms/xaml/markup-extensions/index.md)
