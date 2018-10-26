---
title: Accessibilité sur macOS
description: Ce document décrit comment utiliser des fonctionnalités d’accessibilité de macOS dans une application Xamarin.Mac. Il aborde les éléments d’interface utilisateur décrivant dans les tables de montage séquentiel et code, des contrôles personnalisés et test d’accessibilité.
ms.prod: xamarin
ms.assetid: D7F4892B-501A-4271-A7E0-BDD1586B63AD
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: fdda52309ffdb0d32cc42a4dff052cd9050b1e4f
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50116613"
---
# <a name="accessibility-on-macos"></a>Accessibilité sur macOS

Cette page décrit comment utiliser l’API d’accessibilité de macOS pour créer des applications en fonction de la [liste de contrôle d’accessibilité](~/cross-platform/app-fundamentals/accessibility.md).
Reportez-vous à la [accessibilité Android](~/android/app-fundamentals/accessibility.md) et [iOS accessibilité](~/ios/app-fundamentals/accessibility.md) pages pour les autres API de la plateforme.

Pour comprendre comment l’API d’accessibilité fonctionnent dans macOS (anciennement appelé OS X), le premier examen le [modèle d’accessibilité du système d’exploitation X](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/OSXAXmodel.html).

## <a name="describing-ui-elements"></a>Décrivant les éléments d’interface utilisateur

AppKit utilise le `NSAccessibility` protocole pour exposer les API qui permettent de rendre l’interface utilisateur accessibles. Cela inclut un comportement par défaut qui tente de définir des valeurs significatives pour les propriétés d’accessibilité, telles que la définition d’un bouton `AccessibilityLabel`. L’étiquette est généralement un seul mot ou phrase courte décrivant le contrôle ou la vue.

### <a name="storyboard-files"></a>Fichiers de la table de montage séquentiel

Xamarin.Mac utilise l’Interface Builder de Xcode pour modifier des fichiers de la table de montage séquentiel.
Informations d’accessibilité peuvent être modifiées dans le **inspecteur d’identité** quand un contrôle est sélectionné sur l’aire de conception (comme indiqué dans la capture d’écran ci-dessous) :

[![Ajout d’accessibilité dans Interface Builder de Xcode](accessibility-images/xcode.png "Ajout d’accessibilité dans Interface Builder de Xcode")](accessibility-images/xcode-large.png#lightbox)

### <a name="code"></a>Code

Xamarin.Mac n’expose pas actuellement en tant que `AccessibilityLabel` setter.  Ajoutez la méthode d’assistance suivante pour définir l’étiquette d’accessibilité :

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

Le `AccessibilityHelp` propriété est utilisée pour obtenir une explication de ce que fait le contrôle ou une vue et doivent être ajoutés uniquement lorsque l’étiquette ne peut pas fournir suffisamment d’informations. Le texte d’aide doit toujours rester aussi court que possible, pour l’exemple « supprime le document ».

Certains éléments d’interface utilisateur ne sont pas pertinentes pour un accès accessible (par exemple, une étiquette en regard d’une entrée qui a son propre étiquette d’accessibilité et aide).
Dans ces cas, définissez `AccessibilityElement = false` afin que ces contrôles ou les vues sont ignorées par les lecteurs d’écran ou d’autres outils d’accessibilité.

Apple fournit [lignes directrices d’accessibilité](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/EnhancingtheAccessibilityofStandardAppKitControls.html) qui explique les meilleures pratiques pour les étiquettes et le texte d’accessibilité.

## <a name="custom-controls"></a>Contrôles personnalisés

Reportez-vous à Apple [les instructions pour les contrôles personnalisés accessibles](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/ImplementingAccessibilityforCustomControls.html) pour plus d’informations sur les étapes supplémentaires requises.

## <a name="testing-accessibility"></a>Test de l’accessibilité

macOS fournit un **accessibilité inspecteur** que vous aide à tester les fonctionnalités d’accessibilité. L’inspecteur est inclus dans Xcode.

La première fois qu’elle est lancée, le **accessibilité inspecteur** aura besoin d’autorisation pour contrôler l’ordinateur par le biais d’accessibilité :

![Inspecteur d’accessibilité demande l’autorisation d’exécuter](accessibility-images/accessibility-inspector-1.png "inspecteur d’accessibilité demandant l’autorisation de s’exécuter")

Déverrouiller l’écran des paramètres (si nécessaire, dans le coin inférieur gauche) et les graduations le **accessibilité inspecteur**:

![Écran des paramètres pour activer l’inspecteur d’accessibilité](accessibility-images/accessibility-inspector-2.png "écran des paramètres pour activer l’inspecteur d’accessibilité")

Une fois activé, l’inspecteur s’affiche dans une fenêtre flottante qui peut être déplacée de l’écran. La capture d’écran ci-dessous montre l’inspecteur en cours d’exécution en regard d’un exemple d’application Mac. Comme le curseur est déplacé au-dessus de la fenêtre, l’inspecteur affiche toutes les propriétés accessibles de chaque contrôle :

[![Exemple de l’exécution de l’inspecteur d’accessibilité](accessibility-images/accessibility-example.png "en cours d’exécution exemple d’accessibilité inspecteur")](accessibility-images/accessibility-example-large.png#lightbox)

Pour plus d’informations, consultez le [test d’accessibilité pour le guide du système d’exploitation X](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/OSXAXTestingApps.html).



## <a name="related-links"></a>Liens associés

- [Accessibilité d’inter-plateformes](~/cross-platform/app-fundamentals/accessibility.md)
- [Accessibilité du Mac](https://www.apple.com/accessibility/mac/)
