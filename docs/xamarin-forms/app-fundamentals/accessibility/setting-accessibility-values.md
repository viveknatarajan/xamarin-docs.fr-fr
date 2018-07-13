---
title: Définition des valeurs de l’accessibilité sur les éléments d’Interface utilisateur
description: Cet article explique comment utiliser la classe AutomationProperties, afin qu’un lecteur d’écran peut énoncer sur les éléments dans la page.
ms.prod: xamarin
ms.assetid: c0bb6893-fd26-47e7-88e5-3c333c9f786c
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: 576fe34b0536c83825aa39bdd7111143d9474225
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998908"
---
# <a name="setting-accessibility-values-on-user-interface-elements"></a>Définition des valeurs de l’accessibilité sur les éléments d’Interface utilisateur

_Xamarin.Forms autorise des valeurs d’accessibilité à définir sur les éléments d’interface utilisateur à l’aide des propriétés jointes à partir de la classe AutomationProperties, qui à son tour définir les valeurs d’accessibilité natif. Cet article explique comment utiliser la classe AutomationProperties, afin qu’un lecteur d’écran peut énoncer sur les éléments dans la page._

## <a name="overview"></a>Vue d'ensemble

Xamarin.Forms autorise des valeurs d’accessibilité à définir sur les éléments d’interface utilisateur via les propriétés jointes suivantes :

- `AutomationProperties.IsInAccessibleTree` : indique si l’élément est disponible pour une application accessible. Pour plus d’informations, consultez [AutomationProperties.IsInAccessibleTree](#isinaccessibletree).
- `AutomationProperties.Name` – une brève description de l’élément qui sert d’identificateur speakable pour l’élément. Pour plus d’informations, consultez [AutomationProperties.Name](#name).
- `AutomationProperties.HelpText` – une description plus longue de l’élément, qui peut être considéré comme texte d’info-bulle associé à l’élément. Pour plus d’informations, consultez [AutomationProperties.HelpText](#helptext).
- `AutomationProperties.LabeledBy` – permet à un autre élément à définir les informations d’accessibilité pour l’élément actuel. Pour plus d’informations, consultez [AutomationProperties.LabeledBy](#labeledby).

Ces attaché valeurs de propriétés définies natif d’accessibilité afin qu’un lecteur d’écran peut énoncer sur l’élément. Pour plus d’informations sur les propriétés jointes, consultez [propriétés attaché](~/xamarin-forms/xaml/attached-properties.md).

> [!IMPORTANT]
> À l’aide de la `AutomationProperties` propriétés jointes peuvent avoir un impact sur l’exécution des tests de l’interface utilisateur sur Android. Le [ `AutomationId` ](xref:Xamarin.Forms.Element.AutomationId), `AutomationProperties.Name` et `AutomationProperties.HelpText` propriétés seront définies à la fois natif `ContentDescription` propriété, avec le `AutomationProperties.Name` et `AutomationProperties.HelpText` les valeurs de propriété étant prioritaires sur les `AutomationId`valeur (si les deux `AutomationProperties.Name` et `AutomationProperties.HelpText` sont définis, les valeurs seront concaténées). Cela signifie que tous les tests recherchez `AutomationId` échouera si `AutomationProperties.Name` ou `AutomationProperties.HelpText` sont également définies sur l’élément. Dans ce scénario, les Tests d’interface utilisateur doit être modifiées pour rechercher la valeur de `AutomationProperties.Name` ou `AutomationProperties.HelpText`, ou une concaténation des deux.

Chaque plateforme a un lecteur d’écran différente pour commenter les valeurs d’accessibilité :

- iOS a VoiceOver. Pour plus d’informations, consultez [accessibilité de Test sur votre appareil avec VoiceOver](https://developer.apple.com/library/content/technotes/TestingAccessibilityOfiOSApps/TestAccessibilityonYourDevicewithVoiceOver/TestAccessibilityonYourDevicewithVoiceOver.html) sur developer.apple.com.
- Android a TalkBack. Pour plus d’informations, consultez [accessibilité du test de votre application](https://developer.android.com/training/accessibility/testing.html#talkback) sur developer.android.com.
- Windows a le Narrateur. Pour plus d’informations, consultez [vérifier les scénarios d’application principale à l’aide du Narrateur](/windows/uwp/accessibility/accessibility-testing#verify-main-app-scenarios-by-using-narrator/).

Toutefois, le comportement exact d’un lecteur d’écran dépend du logiciel et sur la configuration de l’utilisateur de celui-ci. Par exemple, la plupart des lecteurs d’écran lire le texte associé à un contrôle lorsqu’il reçoit le focus, permettant aux utilisateurs d’orienter eux-mêmes, lorsqu’ils circulent entre les contrôles sur la page. Certains lecteurs d’écran lire également l’interface utilisateur entière quand une page s’affiche, ce qui permet à l’utilisateur recevoir toutes du contenu d’information disponible de la page avant de tenter de le parcourir.

Les lecteurs d’écran également lire les valeurs d’une accessibilité différente. Dans l’exemple d’application :

- VoiceOver lira le `Placeholder` valeur de la `Entry`, suivie des instructions pour l’utilisation du contrôle.
- TalkBack lira le `Placeholder` valeur de la `Entry`, suivi par le `AutomationProperties.HelpText` valeur, suivie des instructions sur l’utilisation du contrôle.
- Le Narrateur lira le `AutomationProperties.LabeledBy` valeur de la `Entry`, suivie des instructions sur l’utilisation du contrôle.

En outre, le Narrateur accorde la priorité `AutomationProperties.Name`, `AutomationProperties.LabeledBy`, puis `AutomationProperties.HelpText`. Sur Android, TalkBack peut combiner le `AutomationProperties.Name` et `AutomationProperties.HelpText` valeurs. Par conséquent, il est recommandé que le test d’accessibilité approfondie est effectuée sur chaque plateforme pour garantir une expérience optimale.

<a name="isinaccessibletree" />

## <a name="automationpropertiesisinaccessibletree"></a>AutomationProperties.IsInAccessibleTree

Le `AutomationProperties.IsInAccessibleTree` propriété jointe est un `boolean` qui détermine si l’élément est accessible et par conséquent visible aux lecteurs d’écran. Il doit être définie sur `true` à utiliser d’autres l’accessibilité des propriétés jointes. Cela peut être accompli dans XAML comme suit :

```xaml
<Entry AutomationProperties.IsInAccessibleTree="true" />
```

Aussi, il peut être configurée en c# comme suit :

```csharp
var entry = new Entry();
AutomationProperties.SetIsInAccessibleTree(entry, true);
```

> [!NOTE]
> Notez que le [ `SetValue` ](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) méthode peut également être utilisée pour définir le `AutomationProperties.IsInAccessibleTree` propriété – jointe `entry.SetValue(AutomationProperties.IsInAccessibleTreeProperty, true);`

<a name="name" />

## <a name="automationpropertiesname"></a>AutomationProperties.Name

Le `AutomationProperties.Name` valeur de propriété jointe doit être une chaîne de texte courts et descriptifs qui utilise d’un lecteur d’écran pour annoncer un élément. Cette propriété doit être définie pour les éléments qui ont une signification qui est importante pour comprendre le contenu ou d’interagir avec l’interface utilisateur. Cela peut être accompli dans XAML comme suit :

```xaml
<ActivityIndicator AutomationProperties.IsInAccessibleTree="true"
                   AutomationProperties.Name="Progress indicator" />
```

Aussi, il peut être configurée en c# comme suit :

```csharp
var activityIndicator = new ActivityIndicator();
AutomationProperties.SetIsInAccessibleTree(activityIndicator, true);
AutomationProperties.SetName(activityIndicator, "Progress indicator");
```

> [!NOTE]
> Notez que le [ `SetValue` ](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) méthode peut également être utilisée pour définir le `AutomationProperties.Name` propriété – jointe `activityIndicator.SetValue(AutomationProperties.NameProperty, "Progress indicator");`

<a name="helptext" />

## <a name="automationpropertieshelptext"></a>AutomationProperties.HelpText

La `AutomationProperties.HelpText` jointe propriété doit être définie sur le texte qui décrit l’élément d’interface utilisateur, et peut être considérée comme texte d’info-bulle associée à l’élément. Cela peut être accompli dans XAML comme suit :

```xaml
<Button Text="Toggle ActivityIndicator"
        AutomationProperties.IsInAccessibleTree="true"
        AutomationProperties.HelpText="Tap to toggle the activity indicator" />
```

Aussi, il peut être configurée en c# comme suit :

```csharp
var button = new Button { Text = "Toggle ActivityIndicator" };
AutomationProperties.SetIsInAccessibleTree(button, true);
AutomationProperties.SetHelpText(button, "Tap to toggle the activity indicator");
```

> [!NOTE]
> Notez que le [ `SetValue` ](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) méthode peut également être utilisée pour définir le `AutomationProperties.HelpText` propriété – jointe `button.SetValue(AutomationProperties.HelpTextProperty, "Tap to toggle the activity indicator");`

Sur certaines plateformes, pour modifier les contrôles tels qu’un [ `Entry` ](xref:Xamarin.Forms.Entry), le `HelpText` propriété peut parfois être omise et remplacée par le texte d’espace réservé. Par exemple, « entrez ici votre nom » est un bon candidat pour la [ `Entry.Placeholder` ](xref:Xamarin.Forms.Entry.Placeholder) propriété qui place le texte dans le contrôle avant l’entrée d’utilisateur réel.

<a name="labeledby" />

## <a name="automationpropertieslabeledby"></a>AutomationProperties.LabeledBy

Le `AutomationProperties.LabeledBy` propriété jointe permet à un autre élément à définir les informations d’accessibilité pour l’élément actuel. Par exemple, un [ `Label` ](xref:Xamarin.Forms.Label) regard un [ `Entry` ](xref:Xamarin.Forms.Entry) peut être utilisé pour décrire ce que le `Entry` représente. Cela peut être accompli dans XAML comme suit :

```xaml
<Label x:Name="label" Text="Enter your name: " />
<Entry AutomationProperties.IsInAccessibleTree="true"
       AutomationProperties.LabeledBy="{x:Reference label}" />
```

Aussi, il peut être configurée en c# comme suit :

```csharp
var nameLabel = new Label { Text = "Enter your name: " };
var entry = new Entry();
AutomationProperties.SetIsInAccessibleTree(entry, true);
AutomationProperties.SetLabeledBy(entry, nameLabel);
```

> [!NOTE]
> Notez que le [ `SetValue` ](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) méthode peut également être utilisée pour définir le `AutomationProperties.IsInAccessibleTree` propriété – jointe `entry.SetValue(AutomationProperties.LabeledByProperty, nameLabel);`

## <a name="summary"></a>Récapitulatif

Cet article a expliqué comment définir des valeurs d’accessibilité sur utilisateur éléments d’interface dans une application Xamarin.Forms à l’aide des propriétés jointes à partir de la `AutomationProperties` classe. Ces attaché valeurs de propriétés définies natif d’accessibilité afin qu’un lecteur d’écran peut énoncer sur les éléments dans la page.


## <a name="related-links"></a>Liens associés

- [Propriétés jointes](~/xamarin-forms/xaml/attached-properties.md)
- [Accessibilité (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Accessibility/)
