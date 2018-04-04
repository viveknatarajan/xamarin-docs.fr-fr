---
title: Envoi de commentaires tactile
description: Cet article couvre les nouveaux types de commentaires tactiles disponibles dans iOS 10 et comment les implémenter dans Xamarin.iOS.
ms.prod: xamarin
ms.assetid: 888106D1-58F4-453F-BACC-91D51FA39C80
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: f2d1bd73ea764cd5bf56775abd7c7357b039bc79
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="providing-haptic-feedback"></a>Envoi de commentaires tactile

_Cet article couvre les nouveaux types de commentaires tactiles disponibles dans iOS 10 et comment les implémenter dans Xamarin.iOS._

<a name="Overview" />

## <a name="overview"></a>Vue d'ensemble

Sur l’iPhone 7 et iPhone 7 Plus, Apple a inclus les nouvelles réponses tactiles qui fournissent des méthodes supplémentaires pour prendre part aux physiquement de l’utilisateur. Commentaires tactiles (souvent appelés HAPTIQUES) utilise toucher (via force, vibrations ou mouvement) dans la conception de l’Interface utilisateur. Utilisez ces nouvelles options de rétroaction tactile pour obtenir l’attention des utilisateurs et de renforcer leurs actions.

Les rubriques suivantes sont traitées en détail :

- [À propos des commentaires tactile](#About-Haptic-Feedback)
- [UIImpactFeedbackGenerator](#UIImpactFeedbackGenerator)
- [UINotificationFeedbackGenerator](#UINotificationFeedbackGenerator)
- [UISelectionFeedbackGenerator](#UISelectionFeedbackGenerator)

<a name="About-Haptic-Feedback" />

## <a name="about-haptic-feedback"></a>À propos des commentaires tactile

Plusieurs éléments d’interface utilisateur intégrées déjà commentaires tactiles tels que les sélecteurs, les commutateurs et les curseurs. iOS 10 ajoute à présent la possibilité de déclencher par programme HAPTIQUES à l’aide d’une sous-classe concrète de la `UIFeedbackGenerator` classe.

Le développeur peut utiliser une de ces `UIFeedbackGenerator` aux sous-classes de déclencheur par programme les commentaires tactile :

- `UIImpactFeedbackGenerator` -Utilisez ce générateur de commentaires pour compléter une action ou une tâche, telle que la présentation d’un « thud » lors d’une vue de diapositives en place ou si deux objets à l’écran sont en conflit.
- `UINotificationFeedbackGenerator` -Utilisez ce générateur de commentaires pour les notifications tels qu’un type de fin, défectueux ou toute autre action d’avertissement.
- `UISelectionFeedbackGenerator` -Utilisez ce générateur de commentaires pour une modification activement telles que la sélection d’un élément dans une liste de sélection.

<a name="UIImpactFeedbackGenerator" />

### <a name="uiimpactfeedbackgenerator"></a>UIImpactFeedbackGenerator

Utilisez ce générateur de commentaires pour compléter une action ou une tâche, telle que la présentation d’un « thud » lors d’une vue de diapositives en place ou si deux objets à l’écran sont en conflit.

Utilisez le code suivant à l’évaluation d’impact de déclencheur :

```csharp
using UIKit;
...

// Initialize feedback
var impact = new UIImpactFeedbackGenerator (UIImpactFeedbackStyle.Heavy);
impact.Prepare ();

// Trigger feedback
impact.ImpactOccurred ();
```

Lorsque le développeur crée une nouvelle instance de la `UIImpactFeedbackGenerator` classe qu’ils fournissent une `UIImpactFeedbackStyle` spécifiant la force des commentaires en tant que :

- `Heavy`
- `Medium`
- `Light`

Le `Prepare` méthode de la `UIImpactFeedbackGenerator` est appelée pour indiquer au système que les commentaires tactile sont sur le point de se produire afin qu’elle peut réduire la latence.

Le `ImpactOccurred` méthode déclenche ensuite des commentaires tactile.

<a name="UINotificationFeedbackGenerator" />

### <a name="uinotificationfeedbackgenerator"></a>UINotificationFeedbackGenerator

Utilisez ce générateur de commentaires pour les notifications tels qu’un type de fin, défectueux ou toute autre action d’avertissement.

Utilisez le code suivant pour les commentaires de notification de déclencheur :

```csharp
using UIKit;
...

// Initialize feedback
var notification = new UINotificationFeedbackGenerator ();
notification.Prepare ();

// Trigger feedback
notification.NotificationOccurred (UINotificationFeedbackType.Error);
```

Une nouvelle instance de la `UINotificationFeedbackGenerator` classe est créée et ses `Prepare` méthode est appelée pour indiquer au système que les commentaires tactile sont sur le point de se produire afin qu’elle peut réduire la latence.

Le `NotificationOccurred` est appelée pour déclencher un retour tactile une donnée `UINotificationFeedbackType` de :

- `Success`
- `Warning`
- `Error`

<a name="UISelectionFeedbackGenerator" />

### <a name="uiselectionfeedbackgenerator"></a>UISelectionFeedbackGenerator

Utilisez ce générateur de commentaires pour une modification activement telles que la sélection d’un élément dans une liste de sélection.

Utilisez le code suivant pour les commentaires de sélection de déclencheur :

```csharp
using UIKit;
...

// Initialize feedback
var selection = new UISelectionFeedbackGenerator ();
selection.Prepare ();

// Trigger feedback
selection.SelectionChanged ();
```

Une nouvelle instance de la `UISelectionFeedbackGenerator` classe est créée et ses `Prepare` méthode est appelée pour indiquer au système que les commentaires tactile sont sur le point de se produire afin qu’elle peut réduire la latence.

Le `SelectionChanged` méthode déclenche ensuite des commentaires tactile.

## <a name="summary"></a>Récapitulatif

Cet article a couvert les nouveaux types de commentaires tactiles disponibles dans iOS 10 et comment les implémenter dans Xamarin.iOS.

## <a name="related-links"></a>Liens associés

- [Exemples d’iOS 10](https://developer.xamarin.com/samples/ios/iOS10/)
