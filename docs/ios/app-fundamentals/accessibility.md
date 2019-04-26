---
title: Accessibilité sur iOS
description: Ce document décrit l’accessibilité dans iOS, traitant des différentes propriétés et les fonctionnalités qui peuvent servir à rendre votre application utilisable par les utilisateurs autant que possible.
ms.prod: xamarin
ms.assetid: 88D59B36-05A3-4356-AE29-EC2B69CE7162
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/18/2016
ms.openlocfilehash: aa3e15797ae1dac621ea8a78345044be1387ebaa
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61179546"
---
# <a name="accessibility-on-ios"></a>Accessibilité sur iOS

Cette page décrit comment utiliser l’API d’accessibilité d’iOS pour créer des applications en fonction de la [liste de contrôle d’accessibilité](~/cross-platform/app-fundamentals/accessibility.md).
Reportez-vous à la [accessibilité Android](~/android/app-fundamentals/accessibility.md) et [l’accessibilité du système d’exploitation X](~/mac/app-fundamentals/accessibility.md) pages pour les autres API de la plateforme.

## <a name="describing-ui-elements"></a>Décrivant les éléments d’interface utilisateur

iOS fournit le `AccessibilityLabel` et `AccessibilityHint` propriétés aux développeurs d’ajouter un texte descriptif qui peut être utilisé par le VoiceOver écran lecteur pour rendre les contrôles plus accessible. Les contrôles peuvent également être identifiés avec un ou plusieurs traits qui fournissent un contexte supplémentaire dans les modes accessibles.

Certains contrôles devront ne peut-être pas être accessible (par exemple, une étiquette sur une entrée de texte ou une image qui est purement décorative) – la `IsAccessibilityElement` est fourni pour désactiver l’accessibilité dans ces cas.

**Concepteur d’interface utilisateur**

Le **panneau Propriétés** contient une section d’accessibilité qui permet à ces paramètres soient modifiés lorsqu’un contrôle est sélectionné dans le Concepteur d’interface utilisateur iOS :

![](accessibility-images/ios-designer-sml.png "Paramètres d’accessibilité")

**C#**

Ces propriétés peuvent également être définies directement dans le code :

```csharp
usernameInput.AccessibilityLabel = "Search";
usernameInput.Hint = "Press Enter after typing to search employee list";
someLabel.IsAccessibilityElement = false;
displayOnlyText.AccessibilityTraits = UIAccessibilityTrait.Header | UIAccessibilityTrait.Selected;
```

### <a name="what-is-accessibilityidentifier"></a>Qu’est AccessibilityIdentifier ?

Le `AccessibilityIdentifier` est utilisé pour définir une clé unique qui peut être utilisée pour faire référence aux éléments d’interface utilisateur via l’API UIAutomation.

La valeur de `AccessibilityIdentifier` n’est jamais prononcé ou affichées à l’utilisateur.

<a name="postnotification" />

## <a name="postnotification"></a>PostNotification

Le `UIAccessibility.PostNotification` méthode autorise les événements à déclencher à l’utilisateur en dehors de l’interaction directe (par exemple, lorsqu’ils interagissent avec un contrôle spécifique).

### <a name="announcement"></a>Annonce

Une annonce peut être envoyée à partir du code pour informer l’utilisateur un statut ait changé (comme une opération d’arrière-plan est terminée). Cela peut être accompagné d’une indication visuelle dans l’interface utilisateur :

```csharp
UIAccessibility.PostNotification (
  UIAccessibilityPostNotification.Announcement,
    new NSString(@"Item was saved"));
```

### <a name="layoutchanged"></a>LayoutChanged

Le `LayoutChanged` annonce est utilisée lors de la disposition d’écran :

```csharp
UIAccessibility.PostNotification (
  UIAccessibilityPostNotification.LayoutChanged,
    someControl);  // someControl gets focus
```


## <a name="accessibility-and-localization"></a>Accessibilité et la localisation

Propriétés d’accessibilité, comme l’étiquette et l’indicateur peuvent être localisées juste comme tout autre texte dans l’interface utilisateur.

**MainStoryboard.strings**

Si l’interface utilisateur est disposé selon une table de montage, vous pouvez fournir des traductions pour les propriétés d’accessibilité de la même façon que les autres propriétés. Dans l’exemple ci-dessous, un `UITextField` a un **ID de localisation** de `Pqa-aa-ury` et deux propriétés d’accessibilité définies en espagnol :

```csharp
/* Accessibility */
"Pqa-aa-ury.accessibilityLabel" = "Notas input";
"Pqa-aa-ury.accessibilityHint" = "escriba más información";
```

Ce fichier est placé dans le **es.lproj** répertoire de contenu espagnol.

**Localizable.strings**

Vous pouvez également les traductions peuvent être ajoutées à la **Localizable.strings** fichier dans le répertoire de contenu localisé (par ex. **es.lproj** pour l’espagnol) :

```csharp
/* Accessibility */
"Notes" = "Notas input";
"Provide more information" = "escriba más información";
```

Ces traductions peuvent être utilisées dans C# via la `LocalizedString` méthode :

```csharp
notesText.AccessibilityLabel = NSBundle.MainBundle.LocalizedString ("Notes", "");
notesText.AccessibilityHint = NSBundle.MainBundle.LocalizedString ("Provide more information", "");
```

Reportez-vous à la [iOS localisation guide](~/ios/app-fundamentals/localization/index.md) pour plus d’informations sur la localisation de contenu.

<a name="testing" />

## <a name="testing-accessibility"></a>Test d’accessibilité

VoiceOver est activée dans le **paramètres** application en accédant à **général > Accessibilité > VoiceOver**:

![](accessibility-images/settings-sml.png "Définition de la vitesse d’énonciation")

Le **accessibilité** écran fournit également des paramètres de zoom, la taille de texte, les options de couleur et le contraste, les paramètres de reconnaissance vocale et autres options de configuration.

Suivez ces [des instructions VoiceOver](https://developer.apple.com/library/ios/technotes/TestingAccessibilityOfiOSApps/TestAccessibilityonYourDevicewithVoiceOver/TestAccessibilityonYourDevicewithVoiceOver.html) tester l’accessibilité sur les appareils iOS.


## <a name="simulator-testing"></a>Test du simulateur

Lorsque vous testez dans le simulateur, le **accessibilité inspecteur** est disponible pour aider à vérifier les propriétés d’accessibilité et les événements sont correctement configurés. Activer l’inspecteur dans le **paramètres** application en accédant à **général > Accessibilité > Accessibilité inspecteur**:

![](accessibility-images/settings-inspector-sml.png "Activer l’accessibilité Inspector")

Une fois activée, la fenêtre Inspecteur pointe sur l’écran iOS en permanence.
Voici un exemple de la sortie lorsqu’une ligne de vue de table est sélectionnée, notez le **étiquette** contient une phrase qui donne le contenu de la ligne et il est « done » (ie. le cycle est visible) :

![](accessibility-images/tableview-a11y-sml.png "Utilisation de l’inspecteur d’accessibilité")

Tandis que l’inspecteur est visible, utiliser l’icône « X » dans le coin supérieur gauche pour temporairement afficher et masquer la superposition et activer/désactiver les paramètres d’accessibilité.



## <a name="related-links"></a>Liens associés

- [Accessibilité multiplateforme](~/cross-platform/app-fundamentals/accessibility.md)
- [iOS d’accessibilité (Apple)](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/iPhoneAccessibility/Accessibility_on_iPhone/Accessibility_on_iPhone.html)
- [iOS VoiceOver](http://www.apple.com/accessibility/ios/voiceover/)
