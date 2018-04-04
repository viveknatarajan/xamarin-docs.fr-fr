---
title: Accessibilité sur macOS
description: Ce guide décrit les fonctionnalités de création d’une application Xamarin.Mac accessible.
ms.prod: xamarin
ms.assetid: D7F4892B-501A-4271-A7E0-BDD1586B63AD
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: ad04e0276c046f133a6f71abb38912343d2d86b6
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="accessibility-on-macos"></a>Accessibilité sur macOS

Cette page décrit comment utiliser l’API d’accessibilité macOS pour créer des applications en fonction de la [liste de vérification d’accessibilité](~/cross-platform/app-fundamentals/accessibility.md).
Reportez-vous à la [accessibilité Android](~/android/app-fundamentals/accessibility.md) et [iOS accessibilité](~/ios/app-fundamentals/accessibility.md) pages pour les autres API de la plateforme.

Pour comprendre comment l’API d’accessibilité fonctionnent dans macOS (anciennement appelé OS X), relisez la [modèle d’accessibilité du système d’exploitation X](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/OSXAXmodel.html).

## <a name="describing-ui-elements"></a>Décrivant les éléments d’interface utilisateur

AppKit utilise le `NSAccessibility` protocole pour exposer des API qui permettent de rendre l’interface utilisateur accessibles. Cela inclut un comportement par défaut qui tente de définir des valeurs explicites pour les propriétés d’accessibilité, telles que la définition d’un bouton `AccessibilityLabel`. L’étiquette est généralement un seul mot ou une phrase courte qui décrit le contrôle ou la vue.

### <a name="storyboard-files"></a>Fichiers de la table de montage séquentiel

Xamarin.Mac utilise le constructeur d’Interface Xcode pour modifier des fichiers de la table de montage séquentiel.
Informations sur l’accessibilité peuvent être modifiées dans le **inspecteur d’identité** lorsqu’un contrôle est sélectionné sur l’aire de conception (comme indiqué dans la capture d’écran ci-dessous) :

[![Ajout d’accessibilité dans le Générateur de Xcode Interface](accessibility-images/xcode.png "Ajout d’accessibilité dans le Générateur de Xcode Interface")](accessibility-images/xcode-large.png#lightbox)

### <a name="code"></a>Code

Xamarin.Mac n’expose pas actuellement en tant que `AccessibilityLabel` setter.  Ajoutez la méthode d’assistance suivante pour définir l’étiquette de l’accessibilité :

```csharp
public static class AccessibilityHelper
{
    [System.Runtime.InteropServices.DllImport (ObjCRuntime.Constants.ObjectiveCLibrary)]
    extern static void objc_msgSend (IntPtr handle, IntPtr selector, IntPtr label);

    static public void SetAccessibilityLabel (this NSView view, string value)
    {
        objc_msgSend (view.Handle, new ObjCRuntime.Selector ("setAccessibilityLabel:").Handle, new NSString (value).Handle);
    }
}
```

Cette méthode peut ensuite être utilisée dans le code comme indiqué :

```csharp
AccessibilityHelper.SetAccessibilityLabel (someButton, "New Accessible Description");
```

Le `AccessibilityHelp` propriété est utilisée pour obtenir une explication de ce que fait le contrôle ou une vue et ne doit être ajouté lors de l’étiquette ne peut pas fournir suffisamment d’informations. Le texte d’aide doit toujours rester aussi court que possible, pour l’exemple « supprime le document ».

Certains éléments d’interface utilisateur ne sont pas pertinentes pour l’accès accessible (par exemple, une étiquette en regard d’une entrée qui a son propre étiquette d’accessibilité et aide).
Dans ce cas, définissez `AccessibilityElement = false` afin que les contrôles ou les vues sont ignorées par les lecteurs d’écran ou d’autres outils d’accessibilité.

Apple offre [les règles d’accessibilité](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/EnhancingtheAccessibilityofStandardAppKitControls.html) qui décrit les meilleures pratiques pour les étiquettes et le texte d’accessibilité.

## <a name="custom-controls"></a>Contrôles personnalisés

Reportez-vous à Apple [des recommandations pour les contrôles personnalisés accessibles](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/ImplementingAccessibilityforCustomControls.html) pour plus d’informations sur les étapes supplémentaires requises.

## <a name="testing-accessibility"></a>Test de l’accessibilité

macOS fournit un **accessibilité inspecteur** que vous aide à tester les fonctionnalités d’accessibilité. L’inspecteur est inclus dans Xcode.

La première fois qu’elle est lancée, le **accessibilité inspecteur** aura besoin d’autorisation pour contrôler l’ordinateur par le biais d’accessibilité :

![Demande l’autorisation d’exécuter l’inspecteur accessibilité](accessibility-images/accessibility-inspector-1.png "demande l’autorisation d’exécuter l’inspecteur d’accessibilité")

Déverrouiller l’écran des paramètres (si nécessaire, dans le coin inférieur gauche) et les graduations le **accessibilité inspecteur**:

![Écran des paramètres pour activer l’inspecteur de l’accessibilité](accessibility-images/accessibility-inspector-2.png "écran des paramètres pour activer l’inspecteur de l’accessibilité")

Une fois activée, l’inspecteur apparaît comme une fenêtre flottante qui peut être déplacée sur l’écran. La capture d’écran ci-dessous montre l’inspecteur d’en cours d’exécution en regard d’un exemple d’application Mac. Comme le curseur est déplacé au-dessus de la fenêtre, l’inspecteur affiche toutes les propriétés accessibles de chaque contrôle :

[![Exemple de l’exécution de l’inspecteur de l’accessibilité](accessibility-images/accessibility-example.png "en cours d’exécution de l’exemple d’accessibilité inspecteur")](accessibility-images/accessibility-example-large.png#lightbox)

Pour plus d’informations, consultez la [test d’accessibilité pour le guide du système d’exploitation X](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/OSXAXTestingApps.html).



## <a name="related-links"></a>Liens associés

- [Accessibilité d’inter-plateformes](~/cross-platform/app-fundamentals/accessibility.md)
- [Accessibilité de Mac](https://www.apple.com/accessibility/mac/)
