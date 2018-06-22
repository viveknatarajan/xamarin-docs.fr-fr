---
title: iOS 9 compatibilité
description: Même si vous ne prévoyez pas d’ajouter directement des fonctionnalités d’iOS 9 à votre application, vous devez reconstruire vos applications avec la dernière version de Xamarin.
ms.prod: xamarin
ms.assetid: 69A05B0E-8A0A-489F-8165-B10AC46FAF3C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: b7cbec3f064e6000f991fb0f9ce256415f6ce5dd
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30777549"
---
# <a name="ios-9-compatibility"></a>iOS 9 compatibilité

_Même si vous ne prévoyez pas d’ajouter directement des fonctionnalités d’iOS 9 à votre application, vous devez reconstruire vos applications avec la dernière version de Xamarin._

> [!IMPORTANT]
> Les informations de cette page sont pour les clients avec les applications déjà dans le magasin d’applications ciblant iOS 8 ou version antérieure, qui n’ont pas déjà envoyé mises à jour pour la compatibilité iOS 9. Si vous utilisez déjà les dernières versions - Xcode 7 et Xamarin.iOS 9 - pour le développement de vos applications, visitez le [introduction à iOS 9](~/ios/platform/introduction-to-ios9/index.md).

Lors des première bêta iOS 9 est apparue, nous avons identifié deux problèmes avec les versions antérieures de Xamarin qui se manifeste en tant qu’applications anciennes, l’impossibilité de démarrer sur iOS 9.

Ces deux problèmes (en tant que [détaillées sur les forums](http://forums.xamarin.com/discussion/comment/131529/#Comment_131529)) ont été :

- Génération d’applications pour iOS 8 ou une version antérieure n’est ne pas en mesure de démarrer sur les appareils de 32 bits (y compris les applications générées avec le [API unifiée](~/cross-platform/macios/unified/index.md)).
- P/Invoke échouent avec le chemin d’accès complet n’est pas spécifié.

Mise à jour de votre installation de Xamarin vers la dernière version du canal de Stable et puis régénérer et redéployer vos applications, résout ces deux problèmes.

_Même si vous ne sont pas l’intention de mettre à jour immédiatement votre application avec les fonctionnalités d’iOS 9, nous vous recommandons de vous reconstruisez avec la dernière version de Xamarin et soumettre à nouveau à l’App Store_.



Cela garantit que votre application s’exécutera sur iOS 9 après la mise à niveau de vos clients.
Vous pouvez continuer à prendre en charge iOS 8 - reconstruction avec la version la plus récente n’affecte pas la version cible de l’application.

Si vous rencontrez d’autres problèmes lors du test de vos applications existantes sur iOS 9, lisez le [améliorer la compatibilité](#compat) section ci-dessous.


### <a name="updating-with-visual-studio"></a>Mise à jour avec Visual Studio

Il est recommandé que vous vérifiez explicitement que Visual Studio est mis à jour vers la dernière version Stable.

## <a name="what-about-components-nugets-and-other-libraries"></a>Qu’en est-il des composants, Nugets et d’autres bibliothèques ?

Vous faire **pas** doivent attendre les nouvelles versions des composants ou Nugets que vous utilisez pour adresser les deux problèmes mentionnés ci-dessus.
Ces problèmes sont résolus simplement à régénérer votre application avec la dernière version Stable de Xamarin.iOS.

De même, les fournisseurs de composants et les auteurs de Nuget sont **pas** doivent soumettre de nouvelles builds pour résoudre les deux problèmes mentionnés ci-dessus. Toutefois, le cas échéant de Nuget utilise `UICollectionView` ou vues à partir de la charge **Xib** une mise à jour, les fichiers *peut* être nécessaire pour résoudre les problèmes de compatibilité iOS 9 mentionnées ci-dessous.


<a name="compat" />

## <a name="improving-compatibility-in-your-code"></a>Amélioration de la compatibilité de votre Code

Il existe quelques cas de code des modèles qui *utilisé* fonctionne dans les versions antérieures d’iOS avec rupture dans iOS 9. Voici quelques problèmes possibles (et leurs solutions) qui peuvent survenir lorsque vous testez sur iOS 9 :

### <a name="uicollectionviewcellcontentview-is-null-in-constructors"></a>UICollectionViewCell.ContentView a la valeur null dans les constructeurs

**Raison :** dans iOS 9 le `initWithFrame:` constructeur est désormais obligatoire, en raison de changements de comportement dans iOS 9 en tant que le [UICollectionView les États de documentation](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionView_class/#//apple_ref/occ/instm/UICollectionView/dequeueReusableCellWithReuseIdentifier:forIndexPath). Si vous avez inscrit une classe pour l’identificateur spécifié et une nouvelle cellule doit être créée, la cellule est maintenant initialisée en appelant son `initWithFrame:` (méthode).

**Correctif :** ajouter la `initWithFrame:` constructeur comme suit :

```csharp
[Export ("initWithFrame:")]
public YourCellClassName (CGRect frame) : base (frame)
{
    Initialize (); // refactor initialize code into a method
}
```

Liées exemples : [MotionGraph](https://github.com/xamarin/monotouch-samples/commit/3c1b7a4170c001e7290db9babb2b7a6dddeb8bcb), [TextKitDemo](https://github.com/xamarin/monotouch-samples/commit/23ea01b37326963b5ebf68bbcc1edd51c66a28d6)



### <a name="uiview-fails-to-init-with-coder-when-loading-a-view-from-a-xibnib"></a>UIView ne parvient pas initialiser avec codeur lors du chargement d’une vue à partir d’un Xib/Nib

**Raison :** le `initWithCoder:` constructeur est appelé lors du chargement d’une vue à partir d’un fichier de l’Interface générateur Xib celui. Si ce constructeur n’est pas exporté le code non managé ne peut pas appeler notre version gérée de celui-ci. Précédemment (par exemple). dans iOS 8) le `IntPtr` constructeur a été appelé pour initialiser la vue.

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

**Raison :** il s’agit d’un bogue dans natif l’éditeur de liens Apple, ce qui se produit lorsqu’ils effectuent une infrastructure privée public (JavaScriptCore a été rendu public dans iOS 7, avant qu’il s’agissait d’une infrastructure privée), et la cible de déploiement de l’application est une version iOS lors de la Framework a été privé. Dans ce cas, l’éditeur de liens d’Apple allez lier avec la version privée du framework au lieu de la version publique.

**Correctif :** cela sera résolu pour iOS 9, mais il existe une solution de contournement facilement appliquer vous-même en attendant : simplement cibler une version iOS ultérieure dans votre projet (vous pouvez essayer d’iOS 7 dans ce cas). Autres infrastructures peuvent présenter des problèmes similaires, par exemple le framework WebKit a été rendu public dans iOS 8 (et pour cibler iOS 7 génère cette erreur ; vous devez cibler iOS 8 Utilisation WebKit dans votre application).



## <a name="related-links"></a>Liens associés

- [informations de version de compatibilité iOS 9](https://releases.xamarin.com/ios-hotfix-for-ios-9-preview-xcode-6/)
- [Introduction à iOS 9](~/ios/platform/introduction-to-ios9/index.md)
- [Mise à jour de vos applications Xamarin.iOS à iOS9 (vidéo)](https://university.xamarin.com/lightninglectures/Updating-your-XamariniOS-apps-to-iOS9)
