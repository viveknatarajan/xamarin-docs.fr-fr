---
title: Utilisation des Tables et cellules dans Xamarin.iOS
description: Ce document contient des liens vers des guides qui expliquent comment afficher des données avec le contrôle UITableView dans une application Xamarin.iOS.
ms.prod: xamarin
ms.assetid: 04DF47DD-4E17-75D7-AC7C-8CF4A574CD21
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/06/2016
ms.openlocfilehash: ea5b6ba532d577bd503529065eef803acf3a7aa9
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241696"
---
# <a name="working-with-tables-and-cells-in-xamarinios"></a>Utilisation des Tables et cellules dans Xamarin.iOS

Cette section présente les classes utilisées pour créer et afficher les tables, puis fournit des exemples montrant comment les utiliser dans Xamarin.iOS. Il couvre à l’aide de l’apparence par défaut pour les tables, personnalisation de la disposition, la mise en œuvre de la modification et l’utilisation du Concepteur de Xamarin iOS à concevoir une table visuellement. Parfois, l’affichage est évidemment une liste de lignes (par exemple, l’application Music) et d’autres moments, qu'il est difficile de reconnaître le contrôle de table (par exemple la modification dans l’application Contacts ou une conversation dans l’application Messages).

Pour ceux qui travaillent sur des applications multiplateformes avec Xamarin.Android, le contrôle UITableView est similaire à la classe ListView dans Android (et la classe UITableViewSource est similaire aux classes d’adaptateur d’Android).

Ces articles prendra une vision détaillée de l’utilisation de tables, y compris :

-   **Table des parties** – présentation et expliquant les éléments visuels de la `UITableView` contrôle. 
-   **Affichage des données dans les tables** – montrant comment créer et remplir une table, utilisez différents styles de table et des cellules et éviter les problèmes de mémoire en recyclant les objets cell. 
-   **Utilisation avancée** : création de cellules personnalisés et à l’aide de la classe UITableView les fonctionnalités d’édition. 
-   **Création d’une table visuellement** – à l’aide du Concepteur de Xamarin pour iOS pour créer une interface pilotée par table avec un Storyboard. 

## <a name="contents"></a>Sommaire

 [Table des parties &amp; fonctionnalités](~/ios/user-interface/controls/tables/table-parts-and-functionality.md)

 [Remplissage d’un tableau avec des données](~/ios/user-interface/controls/tables/populating-a-table-with-data.md)

 [Personnalisation de l’apparence d’un tableau](~/ios/user-interface/controls/tables/customizing-table-appearance.md)

 [Modification](~/ios/user-interface/controls/tables/editing.md)
 
 [Actions de ligne](~/ios/user-interface/controls/tables/row-action.md)

 [Création de Tables dans un Storyboard](~/ios/user-interface/controls/tables/creating-tables-in-a-storyboard.md)
 
 [Redimensionnement automatique de la hauteur de ligne](~/ios/user-interface/controls/tables/autosizing-row-height.md)

## <a name="related-links"></a>Liens associés

- [WorkingWithTables (exemple)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables/)
- [Tables dans les tables de montage séquentiel (exemple)](https://developer.xamarin.com/samples/monotouch/StoryboardTable/)
- [Introduction aux storyboards](~/ios/user-interface/storyboards/index.md)
- [Table de montage séquentiel une recette TableView](https://github.com/xamarin/recipes/tree/master/Recipes/ios/general/storyboard/storyboard_a_tableview)
- [Introduction à MonoTouch.Dialog](~/ios/user-interface/monotouch.dialog/index.md)
- [Exemple de TableEditing sur Github](https://github.com/xamarin/monotouch-samples/tree/master/TableEditing)
- [Exemple de TableParts sur Github](https://github.com/xamarin/monotouch-samples/tree/master/TableParts)
- [Exemple de TableAndCellStyles sur Github](https://github.com/xamarin/mobile-samples/tree/master/TablesLists)
- [Référence de classe de UITableView](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableView_Class/)
- [Référence de classe de UITableViewCell](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewCell_Class/)
- [UITableViewDelegate](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewDelegate_Protocol/)
- [UITableViewDataSource](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewDataSource_Protocol/)
