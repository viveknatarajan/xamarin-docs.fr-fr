---
title: Table des parties et fonctionnalité dans Xamarin.iOS
description: Ce document décrit les différents éléments d’un UITableView dans iOS. Il traite des en-têtes de section, cellules, pieds de page de section, index et de mode d’édition.
ms.prod: xamarin
ms.assetid: B4139C8B-28F2-4C0F-297F-BF5432C5A915
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: c4d788cce12a9aabdd1170cd1a52915f3b30285f
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50113948"
---
# <a name="table-parts-and-functionality-in-xamarinios"></a>Table des parties et fonctionnalité dans Xamarin.iOS

Un UITableView peut avoir un style « groupé » ou « plain » et se compose des éléments suivants :

-  [En-tête de section](#Section_Header)
-  [Cellules](#Cells) (ou les lignes, si vous préférez)
-  [Section de pied de page](#Section_Footer)
-  [Index](#Index)
-  [Mode d’édition](#Edit_Features) (inclut « balayer pour supprimer » et faites glisser les poignées pour modifier l’ordre des lignes) 

Ces captures d’écran montrent comment les lignes de la section, des en-têtes, des pieds de page, des contrôles d’édition et l’index sont affichés.

 [![](table-parts-and-functionality-images/image1a.png "Ces captures d’écran montrent comment les lignes de la section, des en-têtes, des pieds de page, des contrôles d’édition et l’index sont affichés")](table-parts-and-functionality-images/image1a.png#lightbox)

Ces parties sont décrits plus en détail ci-dessous :

<a name="Section_Header" />

## <a name="section-header"></a>En-tête de section

Les cellules peuvent éventuellement être regroupées en sections, étiquetés avec un en-tête personnalisé, et/ou étiquetés avec un pied de page. L’en-tête peut être défini avec une valeur de chaîne ou une vue personnalisée peut être fournie pour permettre une disposition différente ou un style.

<a name="Cells" />

## <a name="cells"></a>Cellules

Les cellules sont l’élément d’interface utilisateur principale pour une table. En cas d’implémentation correctement, les cellules sont réutilisés pour une efficacité de la mémoire. Il existe quatre styles de cellules intégrées, et vous pouvez créer vos propres cellules personnalisés – dans le code ou dans le concepteur lorsque vous utilisez des tables de montage séquentiel.

<a name="Section_Footer"/>

## <a name="section-footer"></a>Section de pied de page

Le pied de page section facultative peut être définie avec une valeur de chaîne ou une vue personnalisée peut être fournie pour permettre une disposition différente ou un style. Les en-têtes de section et les pieds de page peuvent être définis indépendamment.

<a name="Index" />

## <a name="index"></a>Index

L’index s’affiche comme frange de caractères vers le bas le bord droit de la table.
Toucher ou en faisant glisser sur l’index accélère le défilement à cette partie de la table. Un index est facultatif mais recommandé pour aider à naviguer de longues listes. Un index n’est pas généralement utilisé avec le style de regroupement.

<a name="Edit_Features" />

## <a name="editing-mode"></a>Mode d’édition

Quelques fonctionnalités d’édition différents sont disponibles :

- Balayez pour supprimer des cellules individuelles.
- Passer en mode édition pour afficher des boutons de suppression sur chaque ligne 
- Passer en mode édition pour faire apparaître les poignées de nouveau classement. 
- Insertion de nouvelles cellules (avec l’animation).

Le reste de ce document montre comment mettre en œuvre toutes ces fonctionnalités UITableView avec Xamarin.iOS.


## <a name="classes-overview"></a>Vue d’ensemble de classes

Classes principales utilisées pour afficher les vues de table sont affichés ici :

[![](table-parts-and-functionality-images/classdiagram.png "Classes principales utilisées pour afficher les vues de table sont affichés ici")](table-parts-and-functionality-images/classdiagram.png#lightbox)

L’objectif de chaque classe est décrit ci-dessous :

- **UITableView** – une vue qui contient une collection de cellules à l’intérieur d’un conteneur de défilement. La vue de table utilise généralement la totalité de l’écran dans une application iPhone, mais peut exister en tant que partie d’une vue agrandie sur l’iPad (ou s’affichent dans un menu). 
- **UITableViewCell** – une vue qui représente une cellule unique (ou une ligne) dans un tableau. Il existe quatre types de cellule intégrées et il est possible de créer personnalisée des cellules à la fois dans C# ou avec le concepteur iOS. 
- **UITableViewSource** : Xamarin.iOS exclusif classe abstraite qui fournit toutes les méthodes requises pour afficher une table, y compris le nombre de lignes, retournant une vue de la cellule pour chaque ligne, gestion de la sélection de ligne et de nombreuses autres fonctionnalités facultatives. Vous *doit* sous-classe afin d’obtenir un travail UITableView. 
- **NSIndexPath** – Contains ligne et la Section des propriétés qui identifient la position d’une cellule dans une table. 
- **UITableViewController** – un UIViewController prêts à l’emploi qui a une codée en dur UITableView en tant que vue et accessible via la propriété TableView. 
- **UIViewController** : si la table n’occupe pas tout l’écran que vous pouvez ajouter un UITableView à n’importe quel UIViewController avec son cadre définissez de manière appropriée. 

UITableViewSource remplace les deux classes suivantes, qui sont toujours disponibles dans Xamarin.iOS, mais ne sont pas requises :

- **UITableViewDataSource** – Objective-C un protocole qui est modélisée dans Xamarin.iOS comme une classe abstraite. Doit être sous-classée pour fournir une table avec une vue pour chaque cellule, ainsi que des informations sur les en-têtes, pieds de page et le nombre de lignes et de sections dans le tableau. 
- **UITableViewDelegate** – Objective-C un protocole qui est modélisée dans Xamarin.iOS en tant que classe. Gère la sélection, la modification des fonctionnalités et autres fonctionnalités de table facultatifs. 

Dans ce document, les exemples utilisent tous UITableViewSource et ignorent ces deux classes. Elles sont mentionnées ici, car les exemples Objective-C trouvés dans la documentation d’Apple les référencent, ce qui est utile comprendre ce qu’ils font (et que vous pouvez utiliser à la place UITableViewSource de Xamarin.iOS).

## <a name="related-links"></a>Liens associés

- [WorkingWithTables (exemple)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables)
