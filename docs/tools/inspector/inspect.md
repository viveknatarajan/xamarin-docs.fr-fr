---
title: "L’inspection des Applications en direct"
description: "Visualiser et déboguer votre application en temps réel"
ms.topic: article
ms.prod: xamarin
ms.assetid: 91B3206E-B2A5-4660-A6E5-B924B8FE69A7
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 03/29/2017
ms.openlocfilehash: bd6d47f98435cc68ecf4156423526c31dbac09da
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="inspecting-live-applications"></a>L’inspection des Applications en direct

Inspection de l’application en temps réel est disponible pour les clients d’entreprise.


1. [Installer Xamarin classeurs & Inspecteur.](~/tools/inspector/install.md)

1. Ouvrir une [prise en charge du projet d’application](~/tools/inspector/install.md#supported-platforms) dans Visual Studio pour Mac ou Visual Studio.
1. Exécutez votre application en mode débogage.
1. Cliquez sur le **inspecter** bouton dans la barre d’outils de l’IDE (dans Visual Studio, le **inspecter les applications en cours...**  élément de menu est également disponible dans le **outils** ou **déboguer** menu).



[ ![](inspect-images/mac-heres-the-button.png "Cliquez sur ce bouton dans la barre d’outils de l’IDE")](inspect-images/mac-heres-the-button.png)

Une nouvelle fenêtre de client Xamarin inspecteur s’ouvre, avec une nouvelle invite de commandes REPL.

[ ![](inspect-images/inspector-0.7.0-map-inspect-small.png "Une nouvelle fenêtre de client Xamarin inspecteur s’ouvre, avec une nouvelle invite de commandes REPL")](inspect-images/inspector-0.7.0-map-inspect.png)

Une fois cette fenêtre s’affiche, vous disposez d’une invite interactive c# que vous pouvez utiliser pour exécuter et évaluer des expressions et instructions c#. Ce qui complique uniques, c’est que le code est évalué dans le contexte du processus cible. Dans ce cas, nous montrons le code en cours d’exécution par rapport à l’application iOS affichée.

Les modifications que vous apportez à l’état de l’application sont produisent réellement sur le processus cible, afin de pouvoir utiliser c# pour modifier l’application dynamique, ou vous pouvez inspecter l’état de l’application active.

Par exemple, sur iOS, nous voulons localiser notre classe UIApplication délégué, qui est notre pilote principal (où nous stocker un grand nombre de l’état de l’application) :

    var del = (MyApp.AppDelegate) UIApplication.SharedApplication.Delegate
    del.Database.GetAllCustomers ()
    ...
    del.Database.AddCustomer (...)

(Notez que chaque présentation se produit dans un éditeur multiligne. `Shift + Enter` Crée une nouvelle ligne, et `Cmd + Enter` (`Ctrl + Enter` sur Windows) envoie le code pour l’évaluation. `Enter` envoie automatiquement lorsqu’il est sécurisé.)

Un moyen plus pratique d’obtenir les éléments visuels de votre application est à l’aide du bouton « Inspecter ». Une fois que vous appuyez sur cela, vous pouvez sélectionner un élément d’interface utilisateur en cliquant sur votre application. La variable `selectedView` sera affectée à pointer vers l’élément réel sur l’écran. Dans la capture d’écran ci-dessus, vous pouvez voir comment nous accessibles et modifiées puis `selectedView.BarTintColor` sur la `UISearchBar` nous avions sélectionné.

L’arborescence d’éléments visuels en direct est également très utile. Il représente l’instantané actuel de votre hiérarchie de la vue. Vous pouvez sélectionner des lignes pour définir `selectedView` dans la boucle REPL et pour afficher les valeurs de propriété de la vue. Sur Mac, vous pouvez interagir avec une visualisation éclatée 3D des vues en couche. Sous Windows, vous pouvez modifier les valeurs de propriété d’un affichage visuellement.

## <a name="known-limitations"></a>Limitations connues

 - Sélection de la vue est uniquement pris en charge sur votre écran d’affichage principal.
 - Modification de grille de propriété n’est pas disponible pour Mac et sur Windows est limitée à certains types de données. Utilisez la réplication de la modification plus puissant.
 - Tant que l’inspecteur addin/extension est installée et activée dans votre IDE, nous allons injecter du code dans votre application chaque fois qu’il démarre en mode débogage. Si vous remarquez un comportement étrange dans votre application, veuillez réessayer la désactivation ou la désinstallation du complément/extension de l’inspecteur, le redémarrage de l’IDE et revérification. Et [signaler des bogues](~/tools/inspector/install.md#reporting-bugs) faites-le nous savoir !
 - Si l’inspection d’un élément d’interface utilisateur rend tout de même modifier, veuillez [faites-nous savoir](~/tools/inspector/install.md#reporting-bugs), comme cela peut indiquer un bogue.

