---
title: Définition des valeurs d’accessibilité sur les éléments d’Interface utilisateur
description: Xamarin.Forms autorise les valeurs d’accessibilité à définir sur les éléments d’interface utilisateur à l’aide des propriétés jointes à partir de la classe AutomationProperties, qui à son tour valeurs accessibilité natif. Cet article explique comment utiliser la classe AutomationProperties, afin qu’un lecteur d’écran peut présenter sur les éléments sur la page.
ms.prod: xamarin
ms.assetid: c0bb6893-fd26-47e7-88e5-3c333c9f786c
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: cf9071684061b584e1cb75cfd50b33212f42bf79
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="setting-accessibility-values-on-user-interface-elements"></a>Définition des valeurs d’accessibilité sur les éléments d’Interface utilisateur

_Xamarin.Forms autorise les valeurs d’accessibilité à définir sur les éléments d’interface utilisateur à l’aide des propriétés jointes à partir de la classe AutomationProperties, qui à son tour valeurs accessibilité natif. Cet article explique comment utiliser la classe AutomationProperties, afin qu’un lecteur d’écran peut présenter sur les éléments sur la page._

## <a name="overview"></a>Vue d'ensemble

Xamarin.Forms autorise les valeurs d’accessibilité à définir sur les éléments d’interface utilisateur via les propriétés jointes suivantes :

- `AutomationProperties.IsInAccessibleTree` – Indique si l’élément est disponible pour une application accessible. Pour plus d’informations, consultez [AutomationProperties.IsInAccessibleTree](#isinaccessibletree).
- `AutomationProperties.Name` – une brève description de l’élément qui sert d’identificateur de l’élément speakable. Pour plus d’informations, consultez [AutomationProperties.Name](#name).
- `AutomationProperties.HelpText` – description longue de l’élément, qui peut être considéré comme texte d’info-bulle associé à l’élément. Pour plus d’informations, consultez [AutomationProperties.HelpText](#helptext).
- `AutomationProperties.LabeledBy` : permet à un autre élément définir les informations d’accessibilité de l’élément actuel. Pour plus d’informations, consultez [AutomationProperties.LabeledBy](#labeledby).

Ces attaché propriétés accessibilité natif valeurs afin qu’un lecteur d’écran peut présenter sur l’élément. Pour plus d’informations sur les propriétés jointes, consultez [propriétés rattaché](~/xamarin-forms/xaml/attached-properties.md).

> [!IMPORTANT]
> À l’aide de la `AutomationProperties` propriétés jointes peuvent avoir un impact sur l’exécution du Test de l’interface utilisateur sur Android. Le [ `AutomationId` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.AutomationId/), `AutomationProperties.Name` et `AutomationProperties.HelpText` propriétés seront définies à la fois natif `ContentDescription` propriété, avec le `AutomationProperties.Name` et `AutomationProperties.HelpText` les valeurs de propriété prévaut sur la `AutomationId`valeur (si les deux `AutomationProperties.Name` et `AutomationProperties.HelpText` sont définis, les valeurs seront concaténées). Cela signifie que tous les tests de recherche de `AutomationId` échouera si `AutomationProperties.Name` ou `AutomationProperties.HelpText` sont également définies sur l’élément. Dans ce scénario, les Tests d’interface utilisateur doivent être modifiés pour rechercher la valeur de `AutomationProperties.Name` ou `AutomationProperties.HelpText`, ou une concaténation des deux.

Chaque plateforme possède un lecteur d’écran différente pour commenter les valeurs de l’accessibilité :

- iOS a VoiceOver. Pour plus d’informations, consultez [accessibilité de Test sur votre appareil avec VoiceOver](https://developer.apple.com/library/content/technotes/TestingAccessibilityOfiOSApps/TestAccessibilityonYourDevicewithVoiceOver/TestAccessibilityonYourDevicewithVoiceOver.html) sur developer.apple.com.
- Android a TalkBack. Pour plus d’informations, consultez [accessibilité du test de votre application](https://developer.android.com/training/accessibility/testing.html#talkback) sur developer.android.com.
- Windows a le Narrateur. Pour plus d’informations, consultez [vérifier les scénarios d’application principal à l’aide de Narrateur](/windows/uwp/accessibility/accessibility-testing#verify-main-app-scenarios-by-using-narrator/).

Toutefois, le comportement exact d’un lecteur d’écran dépend du logiciel et sur la configuration de l’utilisateur de celui-ci. Par exemple, la plupart des lecteurs d’écran lisent le texte associé à un contrôle lorsqu’il reçoit le focus, permettant aux utilisateurs d’orienter eux-mêmes lors de leur déplacement entre les contrôles sur la page. Certains lecteurs d’écran de lecture également l’interface utilisateur de toute application quand une page s’affiche, ce qui permet à l’utilisateur recevoir le contenu d’information disponibles de la page avant de tenter d’y naviguer.

Les lecteurs d’écran également lire les valeurs d’une accessibilité différente. Dans l’exemple d’application :

- VoiceOver lira le `Placeholder` valeur de la `Entry`, suivie des instructions sur l’utilisation du contrôle.
- TalkBack lira le `Placeholder` valeur de la `Entry`, suivi par le `AutomationProperties.HelpText` valeur, suivie des instructions sur l’utilisation du contrôle.
- Le Narrateur lit le `AutomationProperties.LabeledBy` valeur de la `Entry`, suivie des instructions sur l’utilisation du contrôle.

En outre, de priorité Narrateur `AutomationProperties.Name`, `AutomationProperties.LabeledBy`, puis `AutomationProperties.HelpText`. Sur Android, TalkBack peut combiner le `AutomationProperties.Name` et `AutomationProperties.HelpText` valeurs. Par conséquent, il est recommandé que le test d’accessibilité approfondie est effectué sur chaque plateforme pour garantir une expérience optimale.

<a name="isinaccessibletree" />

## <a name="automationpropertiesisinaccessibletree"></a>AutomationProperties.IsInAccessibleTree

Le `AutomationProperties.IsInAccessibleTree` propriété jointe est un `boolean` qui détermine si l’élément est accessible et donc visibles aux lecteurs d’écran. Il doit avoir la valeur `true` à utiliser d’autres l’accessibilité des propriétés jointes. Cela peut être accompli en XAML comme suit :

```xaml
<Entry AutomationProperties.IsInAccessibleTree="true" />
```

Sinon, elle peut être définie en c# comme suit :

```csharp
var entry = new Entry();
AutomationProperties.SetIsInAccessibleTree(entry, true);
```

> [!NOTE]
> Notez que la [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) méthode peut également être utilisée pour définir le `AutomationProperties.IsInAccessibleTree` propriété – jointe `entry.SetValue(AutomationProperties.IsInAccessibleTreeProperty, true);`

<a name="name" />

## <a name="automationpropertiesname"></a>AutomationProperties.Name

Le `AutomationProperties.Name` valeur de la propriété jointe doit être une chaîne de texte courts et descriptifs par un lecteur d’écran à l’aide d’un élément. Cette propriété doit être définie pour les éléments qui ont une signification est importante de comprendre le contenu ou l’interaction avec l’interface utilisateur. Cela peut être accompli en XAML comme suit :

```xaml
<ActivityIndicator AutomationProperties.IsInAccessibleTree="true"
                   AutomationProperties.Name="Progress indicator" />
```

Sinon, elle peut être définie en c# comme suit :

```csharp
var activityIndicator = new ActivityIndicator();
AutomationProperties.SetIsInAccessibleTree(activityIndicator, true);
AutomationProperties.SetName(activityIndicator, "Progress indicator");
```

> [!NOTE]
> Notez que la [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) méthode peut également être utilisée pour définir le `AutomationProperties.Name` propriété – jointe `activityIndicator.SetValue(AutomationProperties.NameProperty, "Progress indicator");`

<a name="helptext" />

## <a name="automationpropertieshelptext"></a>AutomationProperties.HelpText

La `AutomationProperties.HelpText` jointe propriété doit être définie pour le texte qui décrit l’élément d’interface utilisateur, et peut être considérée comme texte d’info-bulle associée avec l’élément. Cela peut être accompli en XAML comme suit :

```xaml
<Button Text="Toggle ActivityIndicator"
        AutomationProperties.IsInAccessibleTree="true"
        AutomationProperties.HelpText="Tap to toggle the activity indicator" />
```

Sinon, elle peut être définie en c# comme suit :

```csharp
var button = new Button { Text = "Toggle ActivityIndicator" };
AutomationProperties.SetIsInAccessibleTree(button, true);
AutomationProperties.SetHelpText(button, "Tap to toggle the activity indicator");
```

> [!NOTE]
> Notez que la [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) méthode peut également être utilisée pour définir le `AutomationProperties.HelpText` propriété – jointe `button.SetValue(AutomationProperties.HelpTextProperty, "Tap to toggle the activity indicator");`

Sur certaines plates-formes, pour modifier les contrôles tels qu’un [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/), le `HelpText` propriété peut parfois être omise et remplacée par le texte d’espace réservé. Par exemple, « entrez ici votre nom » est un bon candidat pour la [ `Entry.Placeholder` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.Placeholder/) propriété qui place le texte dans le contrôle avant l’entrée d’utilisateur réel.

<a name="labeledby" />

## <a name="automationpropertieslabeledby"></a>AutomationProperties.LabeledBy

Le `AutomationProperties.LabeledBy` propriété attachée permet à un autre élément définir les informations d’accessibilité de l’élément actuel. Par exemple, un [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) ensuite à une [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) peut être utilisé pour décrire la procédure la `Entry` représente. Cela peut être accompli en XAML comme suit :

```xaml
<Label x:Name="label" Text="Enter your name: " />
<Entry AutomationProperties.IsInAccessibleTree="true"
       AutomationProperties.LabeledBy="{x:Reference label}" />
```

Sinon, elle peut être définie en c# comme suit :

```csharp
var nameLabel = new Label { Text = "Enter your name: " };
var entry = new Entry();
AutomationProperties.SetIsInAccessibleTree(entry, true);
AutomationProperties.SetLabeledBy(entry, nameLabel);
```

> [!NOTE]
> Notez que la [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) méthode peut également être utilisée pour définir le `AutomationProperties.IsInAccessibleTree` propriété – jointe `entry.SetValue(AutomationProperties.LabeledByProperty, nameLabel);`

## <a name="summary"></a>Récapitulatif

Cet article a expliqué comment définir des valeurs d’accessibilité sur l’utilisateur les éléments d’interface dans une application de Xamarin.Forms à l’aide des propriétés jointes à partir de la `AutomationProperties` classe. Ces attaché propriétés accessibilité natif valeurs afin qu’un lecteur d’écran peut présenter sur les éléments sur la page.


## <a name="related-links"></a>Liens associés

- [Propriétés jointes](~/xamarin-forms/xaml/attached-properties.md)
- [Accessibilité (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Accessibility/)
