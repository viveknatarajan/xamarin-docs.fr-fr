---
title: "Fonctionnalités et les parties de la table"
ms.topic: article
ms.prod: xamarin
ms.assetid: B4139C8B-28F2-4C0F-297F-BF5432C5A915
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 1a47cad858e4b0b362da18ebe58142ade2574be2
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="table-parts-and-functionality"></a>Fonctionnalités et les parties de la table

Un UITableView peut avoir un style « groupé » ou « simple » et se compose des éléments suivants :

-  [En-tête de section](#Section_Header)
-  [Cellules](#Cells) (ou des lignes, si vous préférez)
-  [Section de pied de page](#Section_Footer)
-  [Index](#Index)
-  [Mode d’édition](#Edit_Features) (inclut 'faites défiler pour supprimer' et faites glisser les poignées pour modifier l’ordre des lignes) 


Ces captures d’écran montrent comment les lignes de la section, des en-têtes, des pieds de page, les contrôles d’édition et l’index sont affichés.

 [ ![](table-parts-and-functionality-images/image1a.png "Ces captures d’écran montrent comment les lignes de la section, des en-têtes, des pieds de page, les contrôles d’édition et l’index sont affichés")](table-parts-and-functionality-images/image1a.png)

Ces parties sont décrites plus en détail ci-dessous :

 <a name="Section_Header" />


## <a name="section-header"></a>En-tête de section

Les cellules peuvent éventuellement être regroupés dans des sections, étiquetés avec un en-tête personnalisé et/ou étiquetés avec un pied de page. L’en-tête peut être défini avec une valeur de chaîne ou un affichage personnalisé peut être fourni pour permettre une mise en page différente ou un style.

 <a name="Cells" />


## <a name="cells"></a>Cellules

Les cellules sont l’élément d’interface utilisateur principale pour une table. Lorsqu’elle est implémentée correctement, les cellules sont réutilisées pour l’efficacité de la mémoire. Il existe quatre styles de cellule prédéfinis, et vous pouvez créer vos propres cellules personnalisées – dans le code ou dans le concepteur lorsque vous utilisez des animations.


## <a name="section-footer"></a>Section de pied de page

Le pied de page de section facultative peut être définie avec une valeur de chaîne, ou un affichage personnalisé peut être fourni pour permettre une mise en page différente ou un style. Les pieds de page et les en-têtes de section peuvent être définies indépendamment.

 <a name="Index" />


## <a name="index"></a>Index

L’index apparaît sous la forme d’une bande de caractères vers le bas le bord droit de la table.
Toucher ou en faisant glisser sur l’index accélère le défilement à la partie de la table. Un index est facultatif mais recommandé pour aider à parcourir de longues listes. Un index n’est généralement pas utilisé avec le style de regroupement.

 <a name="Edit_Features" />


## <a name="editing-mode"></a>Mode d’édition

Quelques fonctionnalités d’édition différents sont disponibles :

-  Faites défiler pour supprimer des cellules individuelles.
-  Passer en mode d’édition pour afficher des boutons de suppression sur chaque ligne 
-  Passer en mode édition pour faire apparaître les poignées de nouveau tris. 
-  Insertion de nouvelles cellules (avec animation).


Le reste de ce document montre comment implémenter toutes ces fonctionnalités UITableView avec Xamarin.iOS.

 <a name="Classes_Overview" />


## <a name="classes-overview"></a>Vue d’ensemble des classes

Classes principales utilisées pour afficher les vues de table sont illustrés ici :

 [ ![](table-parts-and-functionality-images/classdiagram.png "Classes principales utilisées pour afficher les vues de table sont affichés ici")](table-parts-and-functionality-images/classdiagram.png)

L’objectif de chaque classe est décrite ci-dessous :

-   **UITableView** – une vue qui contient une collection de cellules à l’intérieur d’un conteneur de défilement. La vue de la table utilise en général, la totalité de l’écran dans une application iPhone, mais peut exister en tant que partie d’une vue agrandie sur l’iPad (ou s’affichent dans un popover). 
-   **UITableViewCell** – un affichage qui représente une cellule unique (ou une ligne) dans une table. Il existe quatre types de cellule intégré et il est possible de créer des cellules personnalisées à la fois en c# ou avec le Concepteur d’iOS. 
-   **UITableViewSource** – exclusive Xamarin.iOS classe abstraite qui fournit toutes les méthodes requises pour afficher une table, y compris le nombre de lignes, retourner une vue de la cellule pour chaque ligne, gestion de la sélection de ligne et de nombreuses autres fonctionnalités facultatives. Vous *doit* sous-classe cette option pour obtenir un UITableView. 
-   **NSIndexPath** – Contains ligne et la Section des propriétés qui identifient la position d’une cellule dans une table. 
-   **UITableViewController** – un UIViewController prêt à l’emploi qui a un codé en dur UITableView comme sa vue et est accessible via la propriété TableView. 
-   **UIViewController** : si la table n’occupe pas de l’écran entier, vous pouvez ajouter un UITableView à n’importe quel UIViewController avec son cadre définies de manière appropriée. 


UITableViewSource remplace les deux classes suivantes, qui sont toujours disponibles dans Xamarin.iOS mais ne sont pas requises :

-   **UITableViewDataSource** – Objective-C un protocole qui est modelée dans Xamarin.iOS comme une classe abstraite. Doit être sous-classé pour fournir une table avec une vue pour chaque cellule, ainsi que des informations sur les en-têtes, les pieds de page et le nombre de lignes et des sections dans le tableau. 
-   **UITableViewDelegate** – Objective-C un protocole qui est modelée dans Xamarin.iOS en tant que classe. Gère la sélection, la modification des fonctionnalités et autres fonctionnalités de table facultatifs. 


Dans ce document, les exemples utilisent UITableViewSource et ignorer ces deux classes. Ils sont mentionnés ici, car les exemples Objective-C trouvées dans la documentation d’Apple les référencent, il est donc utile de comprendre ce qu’elles font (et que vous pouvez utiliser à la place UITableViewSource de Xamarin.iOS).


## <a name="related-links"></a>Liens associés

- [WorkingWithTables (exemple)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables)
