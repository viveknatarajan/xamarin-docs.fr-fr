---
title: Contrôle de menu (Force Touch)
ms.prod: xamarin
ms.assetid: 5A7F83FB-9BC4-4812-92C5-CEC8DAE8211E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 4895f140acbc8a622cfb91c2fe9db6c5e9d9d1d7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="menu-control-force-touch"></a>Contrôle de menu (Force Touch)

Regardez que kit fournit un mouvement de Force tactile qui déclenche un menu lorsqu’elle est implémentée sur un écran de l’application espion.

![](menu-images/menu.png "Apple Watch affichant un menu")
<!-- watch image courtesy of http://infinitapps.com/bezel/ -->

## <a name="responding-to-force-touch"></a>Répondre à Force tactile

Si un `Menu` a été implémentée pour un contrôleur d’interface, lorsqu’un utilisateur effectue une Force, sélectionnez le menu s’affiche. Si aucun menu n’a été implémenté, l’écran est brièvement animé une aucune autre action ne se produit.

Force des touches finales ne sont pas associés à un élément particulier sur l’écran ; menu qu’une seule peut être associé à un contrôleur d’interface, et elle apparaîtra, quelle que soit l’où la Force tactile, appuyez sur se produit sur l’écran.

Entre un et quatre menu options peuvent être présentées.


## <a name="adding-a-menu"></a>Ajout d’un Menu

A `Menu` doit être ajouté à un `InterfaceController` sur le plan conceptuel au moment du design. Lorsque vous faites glisser un contrôle de menu sur un contrôleur d’interface il n’existe aucune indication visuelle sur l’aperçu de la table de montage séquentiel, mais le **Menu** s’affiche dans le **structure du Document** remplissage :

![](menu-images/menu-action.png "Modification d’un menu au moment du design")

Jusqu'à quatre menus éléments peuvent être ajoutés au contrôle de menu. Ils peuvent être configurés dans le **propriétés** remplissage. Les attributs suivants peuvent être définies :

- Titre, et
- Image personnalisée, ou
- Une image de système : accepter, ajouter, de bloc, refuser, Info, peut-être plus, désactiver, suspendre, lire, répéter, Resume, partage, lecture aléatoire, haut-parleur, Corbeille.

Créer un `Action` en sélectionnant le **événements** section de la **propriétés** remplissage et en tapant le nom de la méthode d’action. Une méthode partielle sera créée dans le code, qui peut être implémenté dans la classe de contrôleur d’interface, comme suit :

```csharp
partial void MenuItemTapped ()
{
    Console.WriteLine ("A menu item was tapped.");
}
```

### <a name="custom-images"></a>Images personnalisées

Comme pour les images d’onglet dans iOS, images d’élément de menu requièrent un modèle opaque avec un canal alpha qui permet à l’arrière-plan de transparaître à travers.

Vous devez ajouter les images utilisées pour le menu pour le projet d’application espion (pas l’espion extension projet d’application) pour de meilleures performances.


## <a name="changing-the-menu-items"></a>Modifier les éléments de Menu

<!--
### Design Time Items

Menu items added the the storyboard can be shown and hidden programmatically.
-->

### <a name="adding-at-runtime"></a>Ajouter à l’exécution

Vous ne pouvez pas provoquer une `Menu` à ajouter à un contrôleur d’interface lors de l’exécution, même si la collection de `MenuItem`s *peut* être modifié par programmation.
Utilisez la `AddMenuItem` méthode comme indiqué :

```csharp
AddMenuItem (WKMenuItemIcon.Accept, "Yes", new ObjCRuntime.Selector ("tapped"));
```

L’API de Kit espion Xamarin.iOS actuellement requiert un `selector` pour la `AdMenuItem` méthode, qui doit être déclaré comme suit :

```csharp
[Export("tapped")]
void MenuItemTapped ()
{
    Console.WriteLine ("The dynamically added 'Yes' menu item was tapped.");
}
```

### <a name="removing-at-runtime"></a>Suppression lors de l’exécution

Le `ClearAllMenuItems` méthode peut être appelée pour supprimer tous les *ajoutés par programmation* des éléments de menu.

Les éléments de menu configurés dans le plan conceptuel ne peut pas être effacés.



## <a name="related-links"></a>Liens associés

- [WatchKitCatalog (exemple)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Document du Menu d’Apple](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/Menus.html)
