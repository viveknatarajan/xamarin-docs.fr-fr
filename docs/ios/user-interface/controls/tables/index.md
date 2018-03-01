---
title: Utilisation des Tables et des cellules
description: "Affichage des données à l’aide de UITableView avec Xamarin.iOS"
ms.topic: article
ms.prod: xamarin
ms.assetid: 04DF47DD-4E17-75D7-AC7C-8CF4A574CD21
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/06/2016
ms.openlocfilehash: f6abcaa3a771954785df83e80c7e46dd200e6986
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="working-with-tables-and-cells"></a>Utilisation des Tables et des cellules


Cette section présente les classes utilisées pour créer et afficher les tables, puis fournit des exemples de leur utilisation dans Xamarin.iOS. Elle couvre l’utilisation de l’apparence par défaut pour les tables, personnalisation de la disposition, la mise en œuvre de la modification et à l’aide de Xamarin iOS concepteur pour concevoir une table visuellement. Parfois, l’affichage est à l’évidence une liste de lignes (par exemple, l’application de la musique) et d’autres moments, qu'il est difficile d’identifier le contrôle de table (par exemple la modification dans l’application Contacts ou une conversation dans l’application Messages).

Pour ceux qui travaillent sur des applications multiplateformes avec Xamarin.Android, le contrôle UITableView est similaire à la classe de ListView dans Android (et la classe UITableViewSource est similaire aux classes d’adaptateur d’Android).

Ces articles prennent une vision détaillée de travailler avec les tables, y compris :

-   **Table des parties** – présentation et expliquant les éléments visuels de la `UITableView` contrôle. 
-   **Affichage des données dans les tables** – montrant comment créer et remplir une table, utilisez différents styles de tableau et de cellule et éviter les problèmes de mémoire par le recyclage des objets de la cellule. 
-   **L’utilisation avancée** : création de cellules personnalisés et à l’aide des fonctionnalités de modification de la classe UITableView. 
-   **Création d’une table visuellement** – à l’aide du Concepteur de Xamarin pour iOS pour créer une interface pilotée par table avec une table de montage séquentiel. 


## <a name="contents"></a>Sommaire

 [Table des parties &amp; fonctionnalités](~/ios/user-interface/controls/tables/table-parts-and-functionality.md)

 [Remplissage d’une Table avec des données](~/ios/user-interface/controls/tables/populating-a-table-with-data.md)

 [Personnaliser l’apparence de la Table](~/ios/user-interface/controls/tables/customizing-table-appearance.md)

 [Modification](~/ios/user-interface/controls/tables/editing.md)
 
 [Actions de ligne](~/ios/user-interface/controls/tables/row-action.md)

 [Création de Tables dans une table de montage séquentiel](~/ios/user-interface/controls/tables/creating-tables-in-a-storyboard.md)
 
 [Hauteur de ligne de redimensionnement automatique](~/ios/user-interface/controls/tables/autosizing-row-height.md)


## <a name="related-links"></a>Liens associés

- [WorkingWithTables (exemple)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables/)
- [Tables dans les plans conceptuels (exemple)](https://developer.xamarin.com/samples/monotouch/StoryboardTable/)
- [Introduction à des plans conceptuels](~/ios/user-interface/storyboards/index.md)
- [Table de montage séquentiel recette TableView](https://developer.xamarin.com/recipes/ios/general/storyboard/storyboard_a_tableview)
- [Introduction aux MonoTouch.Dialog](~/ios/user-interface/monotouch.dialog/index.md)
- [Exemple TableEditing sur Github](https://github.com/xamarin/monotouch-samples/tree/master/TableEditing)
- [Exemple TableParts sur Github](https://github.com/xamarin/monotouch-samples/tree/master/TableParts)
- [Exemple TableAndCellStyles sur Github](https://github.com/xamarin/mobile-samples/tree/master/TablesLists)
- [Référence de classe de UITableView](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableView_Class/)
- [Référence de classe de UITableViewCell](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewCell_Class/)
- [UITableViewDelegate](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewDelegate_Protocol/)
- [UITableViewDataSource](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewDataSource_Protocol/)
