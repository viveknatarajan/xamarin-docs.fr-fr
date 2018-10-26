---
title: Retour haptique dans Xamarin.iOS
description: Ce document décrit comment fournir un retour haptique dans une application Xamarin.iOS. Il aborde UIImpactFeedbackGenerator, UINotificationFeedbackGenerator et UISelectionFeedbackGenerator.
ms.prod: xamarin
ms.assetid: 888106D1-58F4-453F-BACC-91D51FA39C80
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: b2c381c59ba1574e80babc2c7e68535a3deffe35
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50123126"
---
# <a name="providing-haptic-feedback-in-xamarinios"></a>Retour haptique dans Xamarin.iOS

<a name="Overview" />

## <a name="overview"></a>Vue d'ensemble

Sur l’iPhone 7 et iPhone 7 en outre, Apple a inclus les nouvelles réponses HAPTIQUES qui offrent des moyens supplémentaires de physiquement solliciter l’utilisateur. Retour haptique (souvent appelé simplement HAPTIQUES) utilise le sens de l’entrée tactile (par le biais de force, vibrations ou mouvement) dans la conception de l’Interface utilisateur. Utilisez ces nouvelles options de rétroaction tactile pour obtenir l’attention des utilisateurs et à renforcer leurs actions.

Les rubriques suivantes sont traitées en détail :

- [À propos du retour haptique](#About-Haptic-Feedback)
- [UIImpactFeedbackGenerator](#UIImpactFeedbackGenerator)
- [UINotificationFeedbackGenerator](#UINotificationFeedbackGenerator)
- [UISelectionFeedbackGenerator](#UISelectionFeedbackGenerator)

<a name="About-Haptic-Feedback" />

## <a name="about-haptic-feedback"></a>À propos du retour haptique

Plusieurs éléments d’interface utilisateur intégrés fournissent déjà retour haptique tels que les sélecteurs, les commutateurs et les curseurs. iOS 10 ajoute désormais la possibilité de déclencher par programmation HAPTIQUES à l’aide d’une sous-classe concrète de la `UIFeedbackGenerator` classe.

Le développeur peut utiliser une de ces `UIFeedbackGenerator` sous-classes de déclencheur par programme les retour haptique :

- `UIImpactFeedbackGenerator` -Utilisez ce générateur de commentaires pour compléter une action ou une tâche comme présentant un « thud » lorsqu’une vue diapositives en place ou si deux objets à l’écran sont en conflit.
- `UINotificationFeedbackGenerator` -Utilisez ce générateur de commentaires pour les notifications par exemple, un type de fin, défectueux ou toute autre action d’avertissement.
- `UISelectionFeedbackGenerator` -Utilisez ce générateur de commentaires pour une modification activement telles que la sélection d’un élément dans une liste de sélection.

<a name="UIImpactFeedbackGenerator" />

### <a name="uiimpactfeedbackgenerator"></a>UIImpactFeedbackGenerator

Utilisez ce générateur de commentaires pour compléter une action ou une tâche comme présentant un « thud » lorsqu’une vue diapositives en place ou si deux objets à l’écran sont en conflit.

Utilisez le code suivant à une évaluation d’impact de déclencheur :

```csharp
using UIKit;
...

// Initialize feedback
var impact = new UIImpactFeedbackGenerator (UIImpactFeedbackStyle.Heavy);
impact.Prepare ();

// Trigger feedback
impact.ImpactOccurred ();
```

Lorsque le développeur crée une nouvelle instance de la `UIImpactFeedbackGenerator` classe ils fournissent un `UIImpactFeedbackStyle` spécifiant la force des commentaires en tant que :

- `Heavy`
- `Medium`
- `Light`

Le `Prepare` méthode de la `UIImpactFeedbackGenerator` est appelée pour indiquer au système que retour haptique est sur le point de se produire afin qu’elle peut réduire la latence.

Le `ImpactOccurred` méthode déclenche ensuite retour haptique.

<a name="UINotificationFeedbackGenerator" />

### <a name="uinotificationfeedbackgenerator"></a>UINotificationFeedbackGenerator

Utilisez ce générateur de commentaires pour les notifications par exemple, un type de fin, défectueux ou toute autre action d’avertissement.

Utilisez le code suivant aux commentaires de notification de déclencheur :

```csharp
using UIKit;
...

// Initialize feedback
var notification = new UINotificationFeedbackGenerator ();
notification.Prepare ();

// Trigger feedback
notification.NotificationOccurred (UINotificationFeedbackType.Error);
```

Une nouvelle instance de la `UINotificationFeedbackGenerator` classe est créée et ses `Prepare` méthode est appelée pour indiquer au système que retour haptique est sur le point de se produire afin qu’elle peut réduire la latence.

Le `NotificationOccurred` est appelée pour déclencher un retour haptique une donnée `UINotificationFeedbackType` de :

- `Success`
- `Warning`
- `Error`

<a name="UISelectionFeedbackGenerator" />

### <a name="uiselectionfeedbackgenerator"></a>UISelectionFeedbackGenerator

Utilisez ce générateur de commentaires pour une modification activement telles que la sélection d’un élément dans une liste de sélection.

Utilisez le code suivant à des commentaires de sélection de déclencheur :

```csharp
using UIKit;
...

// Initialize feedback
var selection = new UISelectionFeedbackGenerator ();
selection.Prepare ();

// Trigger feedback
selection.SelectionChanged ();
```

Une nouvelle instance de la `UISelectionFeedbackGenerator` classe est créée et ses `Prepare` méthode est appelée pour indiquer au système que retour haptique est sur le point de se produire afin qu’elle peut réduire la latence.

Le `SelectionChanged` méthode déclenche ensuite retour haptique.

## <a name="summary"></a>Récapitulatif

Cet article a présenté les nouveaux types de retour haptique disponible dans iOS 10 et comment les implémenter dans Xamarin.iOS.

## <a name="related-links"></a>Liens associés

- [Exemples iOS 10](https://developer.xamarin.com/samples/ios/iOS10/)
