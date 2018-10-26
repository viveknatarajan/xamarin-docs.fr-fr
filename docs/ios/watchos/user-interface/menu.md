---
title: watchOS (Force Touch) contrôle de Menu dans Xamarin
description: Ce document décrit comment utiliser le mouvement tactile de force watchOS dans Xamarin. Elle explique comment répondre à une pression tactile force, l’ajout d’un menu et en modifiant les éléments de menu.
ms.prod: xamarin
ms.assetid: 5A7F83FB-9BC4-4812-92C5-CEC8DAE8211E
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 7696c820ab6fdf19bdef46db31061fb5914e6cf4
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50109755"
---
# <a name="watchos-menu-control-force-touch-in-xamarin"></a>watchOS (Force Touch) contrôle de Menu dans Xamarin

Regardez que kit fournit un mouvement Force Touch qui déclenche un menu en cas d’implémentation sur un écran de l’application watch.

![](menu-images/menu.png "Apple Watch affichant un menu")
<!-- watch image courtesy of http://infinitapps.com/bezel/ -->

## <a name="responding-to-force-touch"></a>Répondre aux entrées tactiles de Force

Si un `Menu` a été implémentée pour un contrôleur d’interface, lorsqu’un utilisateur effectue un Touch Force le menu s’affiche. Si aucun menu n’a été implémenté, l’écran est animée brièvement une aucune autre action ne se produit.

Force finales ne sont pas associés à un élément particulier sur l’écran ; menu qu’un seul peut être attaché à un contrôleur d’interface et il s’affiche, quel que soit l’où la presse Force Touch se produit sur l’écran.

Entre un et quatre menu options peuvent être présentées.


## <a name="adding-a-menu"></a>Ajout d’un Menu

Un `Menu` doit être ajouté à un `InterfaceController` sur le plan conceptuel au moment du design. Lorsque vous faites glisser un contrôle de menu sur un contrôleur d’interface il n’existe aucune indication visuelle sur l’aperçu de la table de montage séquentiel, mais le **Menu** s’affiche dans le **structure du Document** panneau :

![](menu-images/menu-action.png "Modification d’un menu au moment du design")

Jusqu'à quatre menu éléments peuvent être ajoutés au contrôle de menu. Ils peuvent être configurés dans le **propriétés** remplissage. Les attributs suivants peuvent être définies :

- Titre, et
- Image personnalisée, ou
- Une image de système : accepter, ajouter, de bloc, refuser, Info, peut-être, de plus, couper, Pause, lire, répétez, Resume, partage, lecture aléatoire, l’orateur, la Corbeille.

Créer un `Action` en sélectionnant le **événements** section de la **propriétés** panneau, puis en tapant le nom de la méthode d’action. Une méthode partielle sera créée dans le code, qui peut être implémenté dans la classe de contrôleur d’interface, comme suit :

```csharp
partial void MenuItemTapped ()
{
    Console.WriteLine ("A menu item was tapped.");
}
```

### <a name="custom-images"></a>Images personnalisées

Comme pour les images d’onglet dans iOS, images d’élément de menu nécessitent un modèle opaque avec un canal alpha qui permet à l’arrière-plan de transparaître à travers.

Vous devez ajouter les images utilisées pour le menu pour le projet d’application watch (pas le suivi application projet d’extension) pour de meilleures performances.


## <a name="changing-the-menu-items"></a>Modifier les éléments de Menu

<!--
### Design Time Items

Menu items added the storyboard can be shown and hidden programmatically.
-->

### <a name="adding-at-runtime"></a>Ajout lors de l’exécution

Vous ne peut pas entraîner un `Menu` à ajouter à un contrôleur d’interface lors de l’exécution, bien que la collection de `MenuItem`s *pouvez* être modifiée par programmation.
Utilisez le `AddMenuItem` méthode comme indiqué :

```csharp
AddMenuItem (WKMenuItemIcon.Accept, "Yes", new ObjCRuntime.Selector ("tapped"));
```

L’API de Kit Xamarin.iOS espion nécessite actuellement un `selector` pour le `AdMenuItem` (méthode), qui doit être déclarée comme suit :

```csharp
[Export("tapped")]
void MenuItemTapped ()
{
    Console.WriteLine ("The dynamically added 'Yes' menu item was tapped.");
}
```

### <a name="removing-at-runtime"></a>Suppression lors de l’exécution

Le `ClearAllMenuItems` méthode peut être appelée pour supprimer tous les *ajoutés par programme* des éléments de menu.

Impossible d’effacer les éléments de menu configurés dans le storyboard.



## <a name="related-links"></a>Liens associés

- [WatchKitCatalog (exemple)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Document du Menu d’Apple](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/Menus.html)
