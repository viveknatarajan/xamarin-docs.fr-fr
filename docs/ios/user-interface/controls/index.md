---
title: "Contrôles"
description: "Xamarin.iOS expose tous les objets d’interface utilisateur native fournies par Apple. Ils sont facilement ajoutés à des applications de Xamarin.iOS à l’aide de l’iOS, le concepteur Interface Builder de Xcode ou par programme. Quelle que soit la méthode choisie, Xamarin.iOS expose toutes les propriétés de l’objet interface utilisateur et les méthodes en c#."
ms.topic: article
ms.prod: xamarin
ms.assetid: C00EA232-ADCC-42AD-BF86-B526414A21C6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: d661cf873baad43a51b40fb59fecd5bc298bcac4
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="controls"></a>Contrôles

_Xamarin.iOS expose tous les objets d’interface utilisateur native fournies par Apple. Ils sont facilement ajoutés à des applications de Xamarin.iOS à l’aide de l’iOS, le concepteur Interface Builder de Xcode ou par programme. Quelle que soit la méthode choisie, Xamarin.iOS expose toutes les propriétés de l’objet interface utilisateur et les méthodes en c#._

Ce document présente certains des contrôles d’interface utilisateur iOS courants et comment les utiliser.

## <a name="alertsalertsmd"></a>[Alertes](alerts.md)

À compter d’iOS 8, UIAlertController a terminé UIActionSheet remplacé et UIAlertView qui sont désormais déconseillées.

## <a name="buttonsbuttonsmd"></a>[Buttons](buttons.md)

La classe UIButton est utilisée pour représenter différents différents styles de bouton dans les écrans d’iOS. Cette section présente les différentes options pour l’utilisation des boutons dans iOS.

## <a name="collection-viewsuicollectionviewmd"></a>[Vues de collection](uicollectionview.md)

Vues de collection, disponibles dans le `UICollectionView` de classe, sont un nouveau concept dans iOS 6 qui introduisent présentant plusieurs éléments à l’écran à l’aide de dispositions. Les modèles pour fournir des données à un `UICollectionView` pour créer des éléments et d’interagir avec ces suivent des éléments du même délégation et source de données modèles couramment utilisés dans le développement d’iOS.

## <a name="imagesimagemd"></a>[Images](image.md)

Ajout d’images à votre application passe en deux étapes : tout d’abord, ajoutez les images à votre projet ; Ensuite, ajoutez des contrôles et du code pour les afficher sur un écran. Reportez-vous à la [utilisation des Images](~/ios/app-fundamentals/images-icons/index.md) article pour plus de couverture d’image de la gestion dans Xamarin.iOS.

## <a name="manual-camera-controlsintro-to-manual-camera-controlsmd"></a>[Contrôles de la caméra manuelle](intro-to-manual-camera-controls.md)

Les contrôles de caméra manuel, fournis par le `AVFoundation Framework` dans iOS 8, autoriser une application mobile afin de prendre le contrôle total sur la caméra d’un appareil iOS. Ce niveau de granularité fin de contrôle peut être utilisé pour créer des applications professionnelles et de fournir des compositions d’artiste en modifiant les paramètres de la caméra tout en tirant toujours une image ou une vidéo.

## <a name="mapsios-mapsindexmd"></a>[Maps](ios-maps/index.md)

Les cartes sont une fonctionnalité courante dans tous les systèmes d’exploitation mobiles modernes. iOS offre la prise en charge du mappage en mode natif par le biais du Kit de la carte. Kit de la carte, les applications peuvent facilement ajouter des mappages riches et interactives. Ces mappages peuvent être personnalisés dans une variété de différentes manières, comme l’ajout d’annotations pour marquer des emplacements sur une carte, recouvrir des graphiques des formes arbitraires. Kit de carte possède même prise en charge intégrée pour afficher l’emplacement actuel d’un périphérique.

## <a name="labelslabelsmd"></a>[Étiquettes](labels.md)

Le `UILabel` contrôle est utilisé pour l’affichage unique et plusieurs lignes, texte en lecture seule.

## <a name="pickers-and-date-pickerspickermd"></a>[Sélecteurs et les sélecteurs de Date](picker.md)

Le contrôle de sélecteur affiche 'roue-like' contrôle qui contient une liste déroulante de valeurs avec la valeur sélectionnée est en cours de mise en surbrillance. Les utilisateurs tournez la molette pour sélectionner l’option de que leur choix.

Un utilisateur spécifique de cas pour les sélecteurs pour définir la date et / ou l’heure. Prévoir cette Apple a créé une sous-classe personnalisée de la classe UIPickerView appelée UIDatePicker.

## <a name="progress-and-activity-indicatorsprogress-activity-indicatormd"></a>[Progression et les indicateurs d’activité](progress-activity-indicator.md)

iOS offre deux méthodes principales pour indiquer la progression de votre application : indicateurs d’activité (y compris un spécifique _réseau_ voyant d’activité) et les barres de progression.

## <a name="search-barssearchbarmd"></a>[Barres de recherche](searchbar.md)

Le UISearchBar est utilisé pour une recherche dans une liste de valeurs. 

## <a name="sliders-steppers-and-segmented-controlsslider-switch-segmented-controlsmd"></a>[Curseurs, les exécutions pas à pas et les contrôles segmentés](slider-switch-segmented-controls.md)

Le contrôle slider permet la sélection simple d’une valeur numérique dans une plage. e/s utilise le `UISwitch` comme entrée d’une valeur booléenne qui peut être représenté par une case sur d’autres plateformes. Un contrôle segmenté est organisée permet d’autoriser les utilisateurs à interagir avec un petit nombre d’options.

## <a name="stack-viewuistackviewmd"></a>[Vue de la pile](uistackview.md)

Le contrôle de la vue de la pile (`UIStackView`) tire parti de la puissance de disposition automatique et les Classes de taille pour gérer une pile de sous-vues, horizontalement ou verticalement, qui répond dynamiquement à la taille d’écran et l’orientation de l’appareil iOS.

## <a name="tables-and-cellstablesindexmd"></a>[Tables et des cellules](tables/index.md)

sa section introduit les classes utilisées pour créer et afficher les tables, puis fournit des exemples de leur utilisation dans Xamarin.iOS. Elle couvre l’utilisation de l’apparence par défaut pour les tables, personnalisation de la disposition, la mise en œuvre de la modification et à l’aide de Xamarin iOS concepteur pour concevoir une table visuellement. Parfois, l’affichage est à l’évidence une liste de lignes (par exemple, l’application de la musique) et d’autres moments, qu'il est difficile d’identifier le contrôle de table (par exemple la modification dans l’application Contacts ou une conversation dans l’application Messages).

## <a name="text-inputtext-inputmd"></a>[Entrée de texte](text-input.md)

Accepter une entrée de texte utilisateur est effectuée avec le `UITextField` pour les entrées de ligne simple et UITextView pour le texte modifiable contenant plusieurs lignes. Vous pouvez faire glisser un de ces contrôles sur un écran et double-cliquez sur pour définir le texte initial.

## <a name="tab-bars-and-tab-bar-controllerscreating-tabbed-applicationsmd"></a>[Onglet Barres et les contrôleurs de la barre d’onglet](creating-tabbed-applications.md)

les applications iOS à l’aide d’une interface utilisateur de navigation de l’onglet sont générées à l’aide de la classe UITabBarController. Dans cet article, nous examinerons comment configurer une application à onglets qui contient plusieurs contrôleurs et vues. Nous examinerons ensuite comment charger un UITabBarController quand il n’est pas le contrôleur de la racine, par exemple après un écran de connexion.

## <a name="web-viewsuiwebviewmd"></a>[Affichages Web](uiwebview.md)

Dans cet article, nous allons examiner chacun des trois vues Web fournies par Apple : `UIWebView`, `WKWebview`, et `SFSafariViewController`, les similitudes et différences et comment ils peuvent être utilisés.

## <a name="related-links"></a>Liens associés

- [Contrôles (exemple)](https://developer.xamarin.com/samples/Controls/)
