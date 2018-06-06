---
title: Accessibilité sur iOS
description: Ce document décrit l’accessibilité dans iOS, traitant des différentes propriétés et les fonctionnalités qui peuvent servir à rendre votre application utilisable par les utilisateurs autant que possible.
ms.prod: xamarin
ms.assetid: 88D59B36-05A3-4356-AE29-EC2B69CE7162
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/18/2016
ms.openlocfilehash: fa85459870211ff26c3bfdd3cc25f722a635952c
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783569"
---
# <a name="accessibility-on-ios"></a>Accessibilité sur iOS

Cette page décrit comment utiliser l’API d’accessibilité d’iOS pour créer des applications en fonction de la [liste de vérification d’accessibilité](~/cross-platform/app-fundamentals/accessibility.md).
Reportez-vous à la [accessibilité Android](~/android/app-fundamentals/accessibility.md) et [accessibilité du système d’exploitation X](~/mac/app-fundamentals/accessibility.md) pages pour les autres API de la plateforme.

## <a name="describing-ui-elements"></a>Décrivant les éléments d’interface utilisateur

e/s fournit le `AccessibilityLabel` et `AccessibilityHint` propriétés aux développeurs d’ajouter un texte descriptif qui peut être utilisé par le VoiceOver écran lecteur pour rendre les contrôles plus accessible. Les contrôles peuvent également être identifiés avec un ou plusieurs des caractéristiques qui fournissent un contexte supplémentaire dans les modes accessibles.

Certains contrôles devront ne peut-être pas être accessible (par exemple, une étiquette sur une entrée de texte ou une image est purement décorative) – la `IsAccessibilityElement` est fournie pour désactiver l’accessibilité dans ces cas.

**Concepteur de l’interface utilisateur**

Le **propriétés remplissage** contient une section d’accessibilité qui permet d’être modifié lorsqu’un contrôle est sélectionné dans le Concepteur de l’interface utilisateur d’iOS ces paramètres :

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

Le `AccessibilityIdentifier` est utilisée pour définir une clé unique qui peut être utilisée pour faire référence aux éléments d’interface utilisateur via l’API UIAutomation.

La valeur de `AccessibilityIdentifier` n’est jamais la prononciation ou affichées à l’utilisateur.

<a name="postnotification" />

## <a name="postnotification"></a>PostNotification

Le `UIAccessibility.PostNotification` méthode autorise les événements à déclencher à l’utilisateur en dehors de l’interaction directe (par exemple, lorsqu’ils interagissent avec un contrôle spécifique).

### <a name="announcement"></a>Annonce

Une annonce peut être envoyée à partir du code pour informer l’utilisateur qui a un état a changé (comme une opération en arrière-plan). Cela peut être accompagné d’une indication visuelle de l’interface utilisateur :

```csharp
UIAccessibility.PostNotification (
  UIAccessibilityPostNotification.Announcement,
    new NSString(@"Item was saved"));
```

### <a name="layoutchanged"></a>LayoutChanged

Le `LayoutChanged` annonce est utilisé lors de la disposition d’écran :

```csharp
UIAccessibility.PostNotification (
  UIAccessibilityPostNotification.LayoutChanged,
    someControl);  // someControl gets focus
```


## <a name="accessibility-and-localization"></a>Accessibilité et la localisation

Les propriétés d’accessibilité que l’étiquette et l’indicateur peuvent être localisées simplement comme tout autre texte dans l’interface utilisateur.

**MainStoryboard.strings**

Si l’interface utilisateur est disposé dans une table de montage séquentiel, vous pouvez fournir des traductions pour les propriétés d’accessibilité dans la même façon que d’autres propriétés. Dans l’exemple ci-dessous, un `UITextField` a un **ID de localisation** de `Pqa-aa-ury` et deux propriétés d’accessibilité définies en espagnol :

```csharp
/* Accessibility */
"Pqa-aa-ury.accessibilityLabel" = "Notas input";
"Pqa-aa-ury.accessibilityHint" = "escriba más información";
```

Ce fichier est placé dans le **es.lproj** répertoire de contenu espagnol.

**Localizable.Strings**

Vous pouvez également les traductions peuvent être ajoutées à la **Localizable.strings** fichier dans le répertoire de contenu localisé (par exemple). **es.lproj** pour l’espagnol) :

```csharp
/* Accessibility */
"Notes" = "Notas input";
"Provide more information" = "escriba más información";
```

Ces traductions peuvent être utilisées dans c#, via le `LocalizedString` méthode :

```csharp
notesText.AccessibilityLabel = NSBundle.MainBundle.LocalizedString ("Notes", "");
notesText.AccessibilityHint = NSBundle.MainBundle.LocalizedString ("Provide more information", "");
```

Reportez-vous à la [iOS localisation guide](~/ios/app-fundamentals/localization/index.md) pour plus d’informations sur la localisation de contenu.

<a name="testing" />

## <a name="testing-accessibility"></a>Test de l’accessibilité

VoiceOver est activée dans le **paramètres** application en accédant à **général > Accessibilité > VoiceOver**:

![](accessibility-images/settings-sml.png "Définition de la vitesse d’énonciation")

Le **accessibilité** écran fournit également des paramètres de zoom, la taille du texte, les options de couleur et de contraste, paramètres de reconnaissance vocale et autres options de configuration.

Suivez ces [les instructions VoiceOver](https://developer.apple.com/library/ios/technotes/TestingAccessibilityOfiOSApps/TestAccessibilityonYourDevicewithVoiceOver/TestAccessibilityonYourDevicewithVoiceOver.html) tester l’accessibilité sur les appareils iOS.


## <a name="simulator-testing"></a>Test du simulateur

Lorsque vous testez dans le simulateur, le **accessibilité inspecteur** est disponible pour vérifier les propriétés d’accessibilité et les événements sont correctement configurés. Activer l’inspecteur dans le **paramètres** application en accédant à **général > Accessibilité > Inspecteur d’accessibilité**:

![](accessibility-images/settings-inspector-sml.png "Activer l’inspecteur de l’accessibilité")

Une fois activée, la fenêtre d’inspecteur est placé sur l’écran d’e/s en permanence.
Voici un exemple de sortie lorsqu’une ligne de la vue table est sélectionnée, notez le **étiquette** contient une phrase qui donne le contenu de la ligne et il est « done » (ie. le cycle est visible) :

![](accessibility-images/tableview-a11y-sml.png "À l’aide de l’inspecteur d’accessibilité")

Alors que l’inspecteur est visible, utilisez l’icône « X » dans le coin supérieur gauche de temporairement afficher et masquer la superposition et d’activer ou désactiver les paramètres d’accessibilité.



## <a name="related-links"></a>Liens associés

- [Accessibilité d’inter-plateformes](~/cross-platform/app-fundamentals/accessibility.md)
- [iOS (Apple) d’accessibilité](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/iPhoneAccessibility/Accessibility_on_iPhone/Accessibility_on_iPhone.html)
- [iOS VoiceOver](http://www.apple.com/accessibility/ios/voiceover/)
