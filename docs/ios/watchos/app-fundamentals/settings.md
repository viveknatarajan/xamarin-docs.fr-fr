---
title: Utilisation des paramètres
ms.prod: xamarin
ms.assetid: 4B2EB192-F0A2-4010-B141-0431520594C0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 6de70eae1eb1c498336a62b4d7be5e2805de11f9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-settings"></a>Utilisation des paramètres

Apple Watch applications peuvent utiliser la même fonctionnalité de paramètres comme des applications iOS : l’interface utilisateur des paramètres s’affiche dans le **Apple Watch** application iPhone, mais les valeurs sont accessibles dans votre application iPhone et également l’extension de surveillance.

![](settings-images/intro.png "Apple Watch applications peuvent utiliser la même fonctionnalité de paramètres comme des applications iOS")

Les paramètres seront stockées dans un emplacement de fichier partagé qui est accessible à l’application iOS et l’extension d’application espion, défini par une **application groupe**. Vous devez [configurer un groupe de l’application](~/ios/watchos/app-fundamentals/app-groups.md) avant d’ajouter les paramètres en suivant les instructions ci-dessous.

## <a name="add-settings-in-a-watch-solution"></a>Ajouter des paramètres dans une Solution de surveillance

Dans le **application iPhone** dans votre solution (*pas* l’application watch ou extension) :

1. Avec le bouton droit **Ajouter > nouveau fichier...**  et choisissez **Settings.bundle** (vous ne pouvez pas modifier le nom dans la **nouveau fichier** boîte de dialogue) :

   [![](settings-images/settings-add-sml.png "Ajouter un nouveau groupe de paramètres")](settings-images/settings-add.png#lightbox)

2. Remplacez le nom par **paramètres-Watch.bundle** (sélectionnez et tapez **commande + R** renommer) :

   ![](settings-images/settings-rename.png "Renommer le groupe")

3. Ajoutez une nouvelle clé `ApplicationGroupContainerIdentifier` à la **Root.plist** avec la valeur définie pour le groupe d’application que vous avez configuré, (par exemple). `group.com.xamarin.WatchSettings` dans l’exemple) :

   [ ![](settings-images/settings-appgroup-sml.png "Ajouter une clé ApplicationGroupContainerIdentifier le Root.plist")](settings-images/settings-appgroup.png#lightbox)

4. Modifier la **Settings-Watch.bundle/Root.plist** pour contenir les options que vous souhaitez utiliser - le fichier modèle contient un groupe.
  TextField, bouton bascule et curseur par défaut (que vous pouvez supprimer et remplacer avec vos propres paramètres) :

  [![](settings-images/rootplist-sml.png "Modifier le Settings-Watch.bundle/Root.plist")](settings-images/rootplist.png#lightbox)


## <a name="use-settings-in-the-watch-app"></a>Utiliser les paramètres dans l’application de surveillance

Pour accéder aux valeurs sélectionnées par l’utilisateur, créez un `NSUserDefaults` à l’aide du groupe d’application et en spécifiant l’instance `NSUserDefaultsType.SuiteName`:

```csharp
NSUserDefaults shared = new NSUserDefaults(
    "group.com.xamarin.WatchSettings",
    NSUserDefaultsType.SuiteName);

var isEnabled = shared.BoolForKey ("enabled_preference");
var userName = shared.StringForKey ("name_preference");
```

## <a name="apple-watch-app"></a>Application d’Apple Watch

[![](settings-images/settings-app-sml.png "La nouvelle application Apple Watch sur l’iPhone")](settings-images/settings-app.png#lightbox)

Les utilisateurs interagissent avec les paramètres via la nouvelle **Apple Watch** application sur leur iPhone. Cette application permet à l’utilisateur permettant d’afficher/masquer les applications sur la surveillance, ainsi que modifier les paramètres exposées à l’aide du **Watch.bundle de paramètres**.

![](settings-images/applewatch-1.png "Exemple de paramètres d’application") ![ ] (settings-images/applewatch-2.png "exemple de paramètres d’application")



## <a name="related-links"></a>Liens associés

- [Document de paramètres d’Apple](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/Settings.html#//apple_ref/doc/uid/TP40014969-CH22-SW1)
