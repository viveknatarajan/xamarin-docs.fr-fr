---
title: Présentation des modèles de contrôle de Xamarin.Forms
description: Xamarin.Forms les modèles de contrôle permettent de facilement et re pages d’application lors de l’exécution. Cet article fournit une introduction aux modèles de contrôle.
ms.prod: xamarin
ms.assetid: 8B8E2360-6531-44A3-A7C8-9A8808DE9B86
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: a8e5c84bfa2525a28e9af5343be0ee156564bdd6
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35242516"
---
# <a name="introduction-to-xamarinforms-control-templates"></a>Présentation des modèles de contrôle de Xamarin.Forms

_Xamarin.Forms les modèles de contrôle permettent de facilement et re pages d’application lors de l’exécution. Cet article fournit une introduction aux modèles de contrôle._

Les contrôles ont des propriétés différentes, telles que `BackgroundColor` et `TextColor`, qui peut définir les aspects de l’apparence du contrôle. Ces propriétés peuvent être définies à l’aide de [styles](~/xamarin-forms/user-interface/styles/index.md), qui peut être modifié pendant l’exécution pour implémenter les thèmes de base. Toutefois, les styles ne conservez pas une séparation nette entre l’apparence d’une page et son contenu, et les modifications qui peuvent être rendues en définissant des propriétés sont limitées.

Les modèles de contrôle fournissent une séparation nette entre l’apparence d’une page et son contenu, par conséquent, l’activation de la création de pages qui peuvent être facilement à thème. Par exemple, une application peut contenir des modèles de contrôle au niveau de l’application qui fournissent un thème sombre et un thème clair. Chaque [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) dans l’application peut être à thème en appliquant l’un des modèles de contrôle sans modifier le contenu affiché par chaque page. En outre, les thèmes proposés par les modèles de contrôle ne sont pas limités à la modification des propriétés des contrôles. Ils peuvent également modifier les contrôles utilisés pour implémenter le thème.

## <a name="creating-a-controltemplate"></a>Création d’un ControlTemplate

A [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) Spécifie l’apparence d’une page ou une vue et contient une présentation de la racine et la mise en page, les contrôles qui implémentent le modèle. En règle générale, un `ControlTemplate` utilisera un [ `ContentPresenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/) pour marquer où apparaît le contenu devant être affiché par la page ou la vue. La page ou la vue qui consomme la `ControlTemplate` définirez ensuite le contenu devant être affiché par le `ContentPresenter`. Le diagramme suivant illustre un `ControlTemplate` pour une page qui contient un nombre de contrôles, y compris un `ContentPresenter` marquée par un rectangle bleu :

![](introduction-images/control-template.png "Modèle de contrôle d’une Page")

A [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) peut être appliqué aux types suivants en définissant leurs `ControlTemplate` propriétés :

- [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)
- [`ContentView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/)
- [`TemplatedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedPage/)
- [`TemplatedView`](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedView/)

Lorsqu’un [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) est créé et affecté à ces types, tout aspect existant est remplacé par l’apparence définie dans le `ControlTemplate`. En outre, ainsi que de définir l’apparence à l’aide de la `ControlTemplate` propriété, les modèles peuvent également être appliqués à l’aide de styles pour davantage de contrôle développer la possibilité de thème.

> [!NOTE]
>  *Quelles sont les `TemplatedPage` et `TemplatedView` types ?* `TemplatedPage` est la classe de base pour `ContentPage`et est le type de page de base fourni par Xamarin.Forms. Contrairement aux `ContentPage`, `TemplatedPage` n’a pas un `Content` propriété. Par conséquent, le contenu ne peut pas être ajouté directement à un `TemplatedPage` instance. Au lieu de cela, le contenu est ajouté à la définition du modèle de contrôle pour le `TemplatedPage` instance. De même, `TemplatedView` est la classe de base pour `ContentView`. Contrairement aux `ContentView`, `TemplatedView` n’a pas un `Content` propriété. Par conséquent, le contenu ne peut pas être ajouté directement à un `TemplatedView` instance. Au lieu de cela, le contenu est ajouté à la définition du modèle de contrôle pour le `TemplatedView` instance.

Modèles de contrôle peuvent être créées en XAML et c# :

- Les modèles de contrôle créés en XAML sont définies dans un [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) qui est assignée à la [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) collecte d’une page, ou plus généralement à la [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.Resources/) collecte de l’application.
- Modèles de contrôle créés en c# sont généralement définies dans la classe de la page ou dans une classe qui peut être accessible globalement.

Choix de l’emplacement définir un [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) impacts où il peut être utilisé de l’instance :

- [`ControlTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) instances définies au niveau de la page peuvent être appliquées uniquement à la page.
- [`ControlTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) les instances définies au niveau de l’application peuvent être appliqués aux pages tout au long de l’application.

Modèles de contrôle de niveau inférieur dans la hiérarchie sont prioritaires sur ceux définis plus haut. Par exemple, un [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) nommé `DarkTheme` qui est définie au niveau de la page sera prioritaire sur un modèle portant le même nom défini au niveau de l’application. Par conséquent, un modèle de contrôle qui définit un thème à appliquer à chaque page dans une application doit être défini au niveau de l’application.


## <a name="related-links"></a>Liens associés

- [Styles](~/xamarin-forms/user-interface/styles/index.md)
- [ControlTemplate](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/)
- [ContentPresenter](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/)
