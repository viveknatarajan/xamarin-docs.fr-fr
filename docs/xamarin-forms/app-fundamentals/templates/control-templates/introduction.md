---
title: Présentation des modèles de contrôle Xamarin.Forms
description: Modèles de contrôle Xamarin.Forms permettent de facilement des thèmes et re-theme pages d’application lors de l’exécution. Cet article fournit une introduction aux modèles de contrôle.
ms.prod: xamarin
ms.assetid: 8B8E2360-6531-44A3-A7C8-9A8808DE9B86
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 6b7a6c6d9c9c541e1d5e821fc2dac202e98bec62
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994423"
---
# <a name="introduction-to-xamarinforms-control-templates"></a>Présentation des modèles de contrôle Xamarin.Forms

_Modèles de contrôle Xamarin.Forms permettent de facilement des thèmes et re-theme pages d’application lors de l’exécution. Cet article fournit une introduction aux modèles de contrôle._

Les contrôles ont des propriétés différentes, telles que `BackgroundColor` et `TextColor`, qui peut définir les aspects de l’apparence du contrôle. Ces propriétés peuvent être définies à l’aide de [styles](~/xamarin-forms/user-interface/styles/index.md), qui peut être modifié pendant l’exécution pour implémenter des thèmes de base. Toutefois, les styles ne maintiennent une séparation nette entre l’apparence d’une page et son contenu, et les modifications qui peuvent être apportées en définissant ces propriétés sont limitées.

Les modèles de contrôle fournissent une séparation nette entre l’apparence d’une page et son contenu, ce qui permet la création de pages qui peuvent facilement être à thème. Par exemple, une application peut contenir des modèles de contrôle au niveau de l’application qui fournissent un thème sombre et un thème clair. Chaque [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) dans l’application peut être à thème en appliquant l’un des modèles de contrôle sans modifier le contenu affiché par chaque page. En outre, les thèmes proposés par les modèles de contrôle ne sont pas limités à la modification des propriétés des contrôles. Ils peuvent également modifier les contrôles permettant d’implémenter le thème.

## <a name="creating-a-controltemplate"></a>Création d’un ControlTemplate

Un [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) Spécifie l’apparence d’une page ou une vue et contient une disposition de la racine et la mise en page, les contrôles qui implémentent le modèle. En règle générale, un `ControlTemplate` utilisera un [ `ContentPresenter` ](xref:Xamarin.Forms.ContentPresenter) pour marquer où apparaît le contenu devant être affiché par la page ou la vue. La page ou la vue qui consomme le `ControlTemplate` définirez ensuite le contenu devant être affiché par le `ContentPresenter`. Le diagramme suivant illustre un `ControlTemplate` pour une page qui contient un nombre de contrôles, y compris un `ContentPresenter` marqué par un rectangle bleu :

![](introduction-images/control-template.png "Modèle de contrôle d’une Page")

Un [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) peut être appliqué aux types suivants en définissant leurs `ControlTemplate` propriétés :

- [`ContentPage`](xref:Xamarin.Forms.ContentPage)
- [`ContentView`](xref:Xamarin.Forms.ContentView)
- [`TemplatedPage`](xref:Xamarin.Forms.TemplatedPage)
- [`TemplatedView`](xref:Xamarin.Forms.TemplatedView)

Quand un [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) est créé et affecté à ces types, n’importe quel aspect existant est remplacé par l’apparence définie dans le `ControlTemplate`. En outre, ainsi que de définir l’apparence à l’aide de la `ControlTemplate` propriété de contrôle modèles peuvent également être appliqués à l’aide de styles pour en savoir plus, développez la possibilité de thème.

> [!NOTE]
>  *Quelles sont les `TemplatedPage` et `TemplatedView` types ?* `TemplatedPage` est la classe de base pour `ContentPage`et est le type de page de base fourni par Xamarin.Forms. Contrairement aux `ContentPage`, `TemplatedPage` n’a pas un `Content` propriété. Par conséquent, contenu ne peut pas être ajouté directement à un `TemplatedPage` instance. Au lieu de cela, le contenu est ajouté en définissant le modèle de contrôle pour le `TemplatedPage` instance. De même, `TemplatedView` est la classe de base pour `ContentView`. Contrairement aux `ContentView`, `TemplatedView` n’a pas un `Content` propriété. Par conséquent, contenu ne peut pas être ajouté directement à un `TemplatedView` instance. Au lieu de cela, le contenu est ajouté en définissant le modèle de contrôle pour le `TemplatedView` instance.

Modèles de contrôle peuvent être créés dans XAML et c# :

- Les modèles de contrôle créés dans XAML sont définies dans un [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) qui est affectée à la [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) collecte d’une page, ou plus généralement à la [ `Resources` ](xref:Xamarin.Forms.Application.Resources) collecte de l’application.
- Les modèles de contrôle créés en c# sont généralement définies dans la classe de la page, ou dans une classe qui est accessible dans le monde entier.

Choix de l’emplacement définir un [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) instance impacts où il peut être utilisé :

- [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) instances définies au niveau de la page peuvent uniquement être appliquées à la page.
- [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) les instances définies au niveau de l’application peuvent être appliqués aux pages tout au long de l’application.

Modèles de contrôle plus bas dans la hiérarchie d’affichage sont prioritaires sur ceux définis plus haut. Par exemple, un [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) nommé `DarkTheme` qui est définie au niveau de la page sera prioritaire sur un modèle portant le même nom défini au niveau de l’application. Par conséquent, un modèle de contrôle qui définit un thème à appliquer à chaque page dans une application doit être défini au niveau de l’application.


## <a name="related-links"></a>Liens associés

- [Styles](~/xamarin-forms/user-interface/styles/index.md)
- [ControlTemplate](xref:Xamarin.Forms.ControlTemplate)
- [ContentPresenter](xref:Xamarin.Forms.ContentPresenter)
