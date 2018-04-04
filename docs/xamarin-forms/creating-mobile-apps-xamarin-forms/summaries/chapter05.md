---
title: Résumé du chapitre 5. Vous traitez des tailles
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 486800E9-C09F-4B95-9AC2-C0F8FE563BCF
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 71d40136d7c3fc780815471cb822e94a4fa704c5
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-5-dealing-with-sizes"></a>Résumé du chapitre 5. Vous traitez des tailles

Plusieurs tailles dans Xamarin.Forms ont été rencontrées :

- La hauteur de la barre d’état iOS est 20
- Le `BoxView` a une largeur par défaut et une hauteur de 40
- La valeur par défaut `Padding` dans un `Frame` est 20.
- La valeur par défaut `Spacing` sur la `StackLayout` est 6
- Le `Device.GetNamedSize` méthode retourne la taille de police numérique

Ces tailles ne sont pas des pixels. Au lieu de cela, ils sont unités indépendantes du périphérique reconnues indépendamment à chaque plateforme.

## <a name="pixels-points-dps-dips-and-dius"></a>Pixels, points, std, adresses IP dynamiques et DIUs

Au début de l’historique de l’Apple Mac et Microsoft Windows, les programmeurs qui a travaillé en unités de pixels. Toutefois, l’apparition de résolution supérieure affiche requis une approche plus abstraite et virtualisée coordonnées d’écran. Dans l’univers Mac, les programmeurs qui a travaillé en unités de *points*, traditionnellement 1/72 pouce, les développeurs de Windows utilisée lors de la *unités indépendantes du périphérique* (DIUs) en fonction de 1/96e de pouce.

Toutefois, les périphériques mobiles sont généralement conservés plus proches de la face et ont une résolution supérieure à celle de bureau des écrans, ce qui implique qu’une plus grande densité de pixels peut être tolérée.

Les programmeurs de cibler des appareils iPhone et iPad d’Apple continuent à travailler dans des unités de *points*, mais il n’y a 160 de ces points par pouce. Selon l’appareil, il peut être 1, 2 ou 3 pixels au point.

Android est similaire. Les programmeurs qui fonctionnent en unités de *indépendant de la densité de pixels* (STD), et la relation entre STD et de pixels est basée sur 160 STD par pouce.

Le Windows Runtime a également créé les facteurs d’échelle qui impliquent un élément proche 160 unités indépendantes du périphérique par pouce.

En résumé, un programmeur Xamarin.Forms ciblant les téléphones portables et tablettes peut supposer que toutes les unités de mesure sont basées sur le critère suivant :

- 160 unités par pouce, équivalent à
- unités de 64 pour le centimètre

En lecture seule [ `Width` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Width/) et [ `Height` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Height/) propriétés définies par `VisualElement` ont par défaut les valeurs de « simuler » &ndash;1. Uniquement lorsqu’un élément est dimensionné et prises en charge dans la disposition de ces propriétés reflètent la taille réelle de l’élément en unités indépendantes du périphérique. Cette taille inclut un `Padding` défini sur l’élément, mais pas les `Margin`.

Un élément visuel déclenche le [ `SizeChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.SizeChanged/) événement lors de son `Width` ou `Height` a changé. Le [ **WhatSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/WhatSize) utilise cet événement pour afficher la taille de l’écran de programme.

## <a name="metrical-sizes"></a>Tailles SURMONTANT

Le [ **MetricalBoxView** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/MetricalBoxView) utilise [ `WidthRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/) et [ `HeightRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/) pour afficher un `BoxView` un pouce en hauteur et l’autre centimètre large.

## <a name="estimated-font-sizes"></a>Tailles de police estimé

Le [ **tailles de police** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FontSizes) exemple illustre l’utilisation de la règle de 160 unités-à-du-pouce pour spécifier les tailles de police dans les unités de points. La cohérence visual entre les plateformes à l’aide de cette technique est la meilleure `Device.GetNamedSize`.

## <a name="fitting-text-to-available-size"></a>Ajustement du texte à la taille disponible

Il est possible d’ajuster un bloc de texte dans un rectangle donné en calculant un `FontSize` de la `Label` à l’aide des critères suivants :

- Interligne est de 120 % de la taille de police (130 % sur les plateformes Windows).
- Largeur de caractère moyenne est de 50 % de la taille de police.

Le [ **EstimatedFontSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/EstimatedFontSize) exemple illustre cette technique. Ce programme a été écrit avant du [ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) propriété était disponible, de sorte qu’elle utilise un [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) avec un [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) paramètre pour simuler un marge.

[![Capture d’écran de triple de la taille de police estimé](images/ch05fg07-small.png "texte ajuster à la taille disponible")](images/ch05fg07-large.png#lightbox "texte ajuster à la taille disponible")

## <a name="a-fit-to-size-clock"></a>Une horloge de la taille de l’ajustement

Le [ **FitToSizeClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FitToSizeClock) exemple illustre l’utilisation [ `Device.StartTimer` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.StartTimer/p/System.TimeSpan/System.Func%7BSystem.Boolean%7D/) pour démarrer un minuteur qui notifie périodiquement l’application au moment de la mise à jour de l’horloge. La taille de police est définie à un sixième de la largeur de page pour rendre l’affichage aussi grand que possible.

## <a name="accessibility-issues"></a>Problèmes d’accessibilité

Le **EstimatedFontSize** programme et le **FitToSizeClock** programme les deux contenir une faille subtile : si l’utilisateur modifie les paramètres d’accessibilité du téléphone sur Android ou Windows 10 Mobile, le programme n’est plus pouvez estimer la taille du texte est rendu en fonction de la taille de police. Le [ **AccessibilityTest** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/AccessibilityTest) exemple illustre ce problème.

## <a name="empirically-fitting-text"></a>Ajustement de texte de manière empirique

Une autre façon d’ajuster le texte à un rectangle consiste à empirique calculer la taille du rendu de texte et l’ajuster vers le haut ou vers le bas. Le programme dans les appels de livre [ `GetSizeRequest` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.GetSizeRequest/p/System.Double/System.Double/) sur un élément visuel pour obtenir la taille souhaitée de l’élément. Méthode a été déconseillée et les programmes doivent appeler à la place [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/).

Pour un `Label`, le premier argument doit être la largeur du conteneur (pour autoriser l’habillage), tandis que le second argument doit être défini à `Double.PositiveInfinity` pour que la hauteur sans contrainte. Le [ **EmpiricalFontSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/EmpiricalFontSize) exemple illustre cette technique.



## <a name="related-links"></a>Liens associés

- [Recherche en texte intégral chapitre 5 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch05-Apr2016.pdf)
- [Exemples de chapitre 5](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05)
- [Exemples de chapitre 5 F #](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FS)
