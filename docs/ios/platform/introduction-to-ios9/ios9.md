---
title: iOS 9 compatibilité
description: Même si vous ne souhaitez pas ajouter des fonctionnalités d’iOS 9 à votre application affichera tout de suite, vous devez recréer vos applications avec la dernière version de Xamarin.
ms.prod: xamarin
ms.assetid: 69A05B0E-8A0A-489F-8165-B10AC46FAF3C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 2f22fdeaad1b276bb94d2b1ee5af4a6c24d22cb7
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350779"
---
# <a name="ios-9-compatibility"></a>iOS 9 compatibilité

_Même si vous ne souhaitez pas ajouter des fonctionnalités d’iOS 9 à votre application affichera tout de suite, vous devez recréer vos applications avec la dernière version de Xamarin._

> [!IMPORTANT]
> Les informations sur cette page sont des clients avec des applications déjà dans l’App Store ciblant iOS 8 ou version antérieure, qui n’ont pas déjà envoyé mises à jour pour assurer la compatibilité iOS 9. Si vous utilisez déjà les dernières versions - Xcode 7 et Xamarin.iOS 9 - votre développement d’applications, consultez le [introduction à iOS 9](~/ios/platform/introduction-to-ios9/index.md).

Lors des première bêta iOS 9 est apparu, nous avons identifié les deux problèmes avec les versions antérieures de Xamarin reconnu comme l’impossibilité de démarrer sur iOS 9 des applications plus anciennes.

Ces deux problèmes (en tant que [détaillées sur nos forums](http://forums.xamarin.com/discussion/comment/131529/#Comment_131529)) ont été :

- Créer des applications pour iOS 8 ou version antérieure n’est pas en mesure de démarrer sur des appareils 32 bits (y compris les applications développées avec le [API unifiée](~/cross-platform/macios/unified/index.md)).
- P/Invoke échouent avec le chemin d’accès complet n’est pas spécifié.

Mise à jour de votre installation de Xamarin vers la dernière version de canal Stable et puis régénérer et redéployer vos applications, de résout ces deux problèmes.

_Même si vous ne prévoyez pas de mettre à jour votre application avec des fonctionnalités d’iOS 9 tout de suite, nous vous recommandons reconstruisez avec la dernière version de Xamarin et soumettez à nouveau à l’App Store_.



Cela garantit que votre application s’exécutera sur iOS 9 après la mise à niveau de vos clients.
Vous pouvez continuer à prendre en charge iOS 8 - reconstruction avec la dernière version n’affecte pas la version cible de l’application.

Si vous rencontrez d’autres problèmes lors du test de vos applications existantes sur iOS 9, lisez le [améliorant la compatibilité](#compat) section ci-dessous.


### <a name="updating-with-visual-studio"></a>La mise à jour avec Visual Studio

Il est recommandé que vous vérifiez explicitement que Visual Studio est mis à jour vers la dernière version Stable.

## <a name="what-about-components-nugets-and-other-libraries"></a>Qu’en est-il des composants, de packages NuGet et d’autres bibliothèques ?

Vous faire **pas** ont besoin d’attendre de nouvelles versions des composants ou packages NuGet que vous utilisez pour résoudre deux problèmes mentionnés ci-dessus.
Ces problèmes sont résolus simplement régénérez votre application avec la dernière version Stable de Xamarin.iOS.

De même, les fournisseurs de composants et les auteurs de Nuget sont **pas** doivent soumettre de nouvelles builds juste pour résoudre deux problèmes mentionnés ci-dessus. Toutefois, le cas échéant un composant ou un Nuget utilise `UICollectionView` ou charger des vues à partir de **Xib** des fichiers, une mise à jour *peut* être nécessaires pour résoudre les problèmes de compatibilité iOS 9 mentionnées ci-dessous.


<a name="compat" />

## <a name="improving-compatibility-in-your-code"></a>Amélioration de la compatibilité dans votre Code

Il existe quelques cas de code des modèles qui *utilisé* fonctionne dans les versions antérieures d’iOS avec rupture dans iOS 9. Voici quelques problèmes possibles (et leurs solutions) qui peuvent survenir lorsque vous testez sur iOS 9 :

### <a name="uicollectionviewcellcontentview-is-null-in-constructors"></a>UICollectionViewCell.ContentView a la valeur null dans les constructeurs

**Raison :** dans iOS 9 le `initWithFrame:` constructeur est désormais nécessaire, en raison de changements de comportement dans iOS 9, comme le [UICollectionView les États de documentation](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionView_class/#//apple_ref/occ/instm/UICollectionView/dequeueReusableCellWithReuseIdentifier:forIndexPath). Si vous avez inscrit une classe pour l’identificateur spécifié et une nouvelle cellule doit être créée, la cellule est maintenant initialisée en appelant son `initWithFrame:` (méthode).

**Correctif :** ajouter le `initWithFrame:` constructeur comme suit :

```csharp
[Export ("initWithFrame:")]
public YourCellClassName (CGRect frame) : base (frame)
{
    Initialize (); // refactor initialize code into a method
}
```

Liées exemples : [MotionGraph](https://github.com/xamarin/monotouch-samples/commit/3c1b7a4170c001e7290db9babb2b7a6dddeb8bcb), [TextKitDemo](https://github.com/xamarin/monotouch-samples/commit/23ea01b37326963b5ebf68bbcc1edd51c66a28d6)



### <a name="uiview-fails-to-init-with-coder-when-loading-a-view-from-a-xibnib"></a>UIView ne parvient pas à init avec le codeur lors du chargement d’une vue à partir d’un Xib/Nib

**Raison :** le `initWithCoder:` constructeur est celle appelée lors du chargement d’une vue à partir d’un fichier Xib de générateur d’Interface. Si ce constructeur n’est pas exporté code non managé ne peut pas appeler notre version managée de celui-ci. Précédemment (par exemple). dans iOS 8) le `IntPtr` constructeur a été appelé pour initialiser la vue.

**Correctif :** créer et exporter le `initWithCoder:` constructeur comme suit :

```csharp
[Export ("initWithCoder:")]
public YourClassName (NSCoder coder) : base (coder)
{
    Initialize (); // refactor initialize code into a method
}
```

Exemple : [Chat](https://github.com/xamarin/monotouch-samples/commit/7b81138d52e5f3f1aa3769fcb08f46122e9b6a88)


### <a name="dyld-message-no-cache-image-with-name"></a>Dyld Message : aucune image de cache par nom...

Vous pouvez rencontrer un incident avec les informations suivantes dans le journal :

```csharp
Dyld Error Message:
Dyld Message: no cache image with name (/System/Library/PrivateFrameworks/JavaScriptCore.framework/JavaScriptCore)
```

**Raison :** il s’agit d’un bogue dans natif l’éditeur de liens Apple, ce qui se produit quand ils réalisent une infrastructure privée public (JavaScriptCore a été rendu public dans iOS 7, avant qu’il s’agissait d’une infrastructure privée), et la cible de déploiement de l’application est une version iOS lors de la Framework a été privé. Dans ce cas, l’éditeur de liens d’Apple sera liée avec la version privée du framework au lieu de la version publique.

**Correctif :** cela point sera abordé pour iOS 9, mais il existe une solution de contournement simple que vous pouvez les appliquer vous-même en attendant : simplement cibler une version iOS plus loin dans votre projet (vous pouvez essayer iOS 7 dans ce cas). Autres infrastructures peuvent présenter des problèmes similaires, par exemple le framework WebKit a été rendu public dans iOS 8 (et donc cibler iOS 7 génère cette erreur ; vous devez cibler iOS 8 pour utiliser WebKit dans votre application).



## <a name="related-links"></a>Liens associés

- [informations de version de compatibilité iOS 9](https://releases.xamarin.com/ios-hotfix-for-ios-9-preview-xcode-6/)
- [Introduction à iOS 9](~/ios/platform/introduction-to-ios9/index.md)
- [La mise à jour de vos applications Xamarin.iOS à iOS9 (vidéo)](https://university.xamarin.com/lightninglectures/Updating-your-XamariniOS-apps-to-iOS9)
