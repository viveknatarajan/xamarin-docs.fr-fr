---
title: "Résumé du chapitre 23. Les déclencheurs et les comportements"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 19E84B5D-46B4-4B6D-A255-87BEFB011261
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: ddb76e00cfe1c19a9d31dc3e53b80a2be0697dbc
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/15/2018
---
# <a name="summary-of-chapter-23-triggers-and-behaviors"></a>Résumé du chapitre 23. Les déclencheurs et les comportements

Les déclencheurs et les comportements sont similaires, dans la mesure où ils sont tous deux destinées à être utilisée dans les fichiers XAML pour simplifier les interactions élément au-delà de l’utilisation des liaisons de données et pour étendre les fonctionnalités des éléments XAML. Les déclencheurs et les comportements sont presque toujours utilisés avec les objets d’interface utilisateur visual.

Pour prendre en charge les déclencheurs et les comportements, les deux `VisualElement` et `Style` prennent en charge deux propriétés de collection :

- [`VisualElement.Triggers`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Triggers/) et [ `Style.Triggers` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.Triggers/) de type `IList<TriggerBase>`
- [`VisualElement.Behaviors`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) et [ `Style.Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.Behaviors/) de type `IList<Behavior>`

## <a name="triggers"></a>Déclencheurs

Un déclencheur est une condition (une modification de propriété ou le déclenchement d’un événement) qui résulte dans une réponse (un autre modification de la propriété ou du code en cours d’exécution). Le `Triggers` propriété du `VisualElement` et `Style` est de type `IList<TriggersBase>`. [`TriggerBase`](https://developer.xamarin.com/api/type/Xamarin.Forms.TriggerBase/) est une classe abstraite à partir de laquelle dérivent les classes sealed quatre :

- [`Trigger`](https://developer.xamarin.com/api/type/Xamarin.Forms.Trigger/) pour les réponses en fonction des modifications de propriété
- [`EventTrigger`](https://developer.xamarin.com/api/type/Xamarin.Forms.EventTrigger/) pour les réponses en fonction de déclenchement d’événement
- [`DataTrigger`](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTrigger/) pour les réponses basées sur les liaisons de données
- [`MultiTrigger`](https://developer.xamarin.com/api/type/Xamarin.Forms.MultiTrigger/) pour les réponses en fonction de plusieurs déclencheurs

Le déclencheur est toujours défini sur l’élément dont la propriété est en cours de modification par le déclencheur.

### <a name="the-simplest-trigger"></a>Le déclencheur de la plus simple

Le [ `Trigger` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Trigger/) recherche un changement d’une valeur de propriété de classe et répond en définissant une autre propriété du même élément.

`Trigger` définit les trois propriétés :

- [`Property`](https://developer.xamarin.com/api/property/Xamarin.Forms.Trigger.Property/) de type `BindableProperty`
- [`Value`](https://developer.xamarin.com/api/property/Xamarin.Forms.Trigger.Value/) de type `Object`
- [`Setters`](https://developer.xamarin.com/api/property/Xamarin.Forms.Trigger.Setters/) de type `IList<SetterBase>`, la propriété de contenu de `Trigger`

En outre, `Trigger` nécessite que la propriété suivante héritée `TriggerBase` être définies :

- [`TargetType`](https://developer.xamarin.com/api/property/Xamarin.Forms.TriggerBase.TargetType/) pour indiquer le type de l’élément sur lequel le `Trigger` est attaché

Le `Property` et `Value` constituent la condition et le `Setters` collection constitue la réponse. Lorsque indiqué `Property` a la valeur indiquée par `Value`, puis le `Setter` des objets dans le `Setters` collection sont appliquées. Lorsque la `Property` a une valeur différente, les accesseurs Set sont supprimés. `Setter` définit les deux propriétés qui sont les mêmes que les deux premières propriétés de `Trigger`:

- [`Property`](https://developer.xamarin.com/api/property/Xamarin.Forms.Setter.Property/) de type `BindableProperty`
- [`Value`](https://developer.xamarin.com/api/property/Xamarin.Forms.Setter.Value/) de type `Object`

Le [ **EntryPop** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntryPop) exemple montre comment un `Trigger` appliqué à une `Entry` peut augmenter la taille de la `Entry` via la `Scale` propriété lorsque le `IsFocused`propriété de la `Entry` est `true`.

Bien qu’il n’est pas courant, la `Trigger` peut être défini dans le code, comme le [ **EntryPopCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntryPopCode) illustre.

Le [ **StyledTriggers** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/StyledTriggers) exemple montre comment la `Trigger` peut être définie dans un `Style` à appliquer à plusieurs `Entry` éléments.

### <a name="trigger-actions-and-animations"></a>Actions du déclencheur et les animations

Il est également possible d’exécuter un peu de code basé sur un déclencheur. Ce code peut être une animation qui cible une propriété. Une façon courante consiste à utiliser un [ `EventTrigger` ](https://developer.xamarin.com/api/type/Xamarin.Forms.EventTrigger/), qui définit deux propriétés :

- [`Event`](https://developer.xamarin.com/api/property/Xamarin.Forms.EventTrigger.Event/) de type `string`, le nom d’un événement
- [`Actions`](https://developer.xamarin.com/api/property/Xamarin.Forms.EventTrigger.Actions/) de type `IList<TriggerAction>`, une liste d’actions à exécuter en réponse.

Pour l’utiliser, vous devez écrire une classe qui dérive de [ `TriggerAction<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TriggerAction%3CT%3E/), généralement `TriggerAction<VisualElement>`. Vous pouvez définir les propriétés de cette classe. Il s’agit des propriétés CLR standard au lieu des propriétés pouvant être liées, car `TriggerAction` ne dérive pas de `BindableObject`. Vous devez substituer la [ `Invoke` ](https://developer.xamarin.com/api/member/Xamarin.Forms.TriggerAction%3CT%3E.Invoke/p/T/) méthode est appelée lorsque l’action est appelée. L’argument est l’élément cible.

Le [ `ScaleAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ScaleAction.cs) classe dans le [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) bibliothèque est un exemple. Il appelle le `ScaleTo` propriété à animer le `Scale` propriété d’un élément. Étant donné qu’un de ses propriétés est de type `Easing`, le [ `EasingConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/EasingConverter.cs) classe vous permet d’utiliser la norme `Easing` champs statiques dans XAML.

Le [ **EntrySwell** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntrySwell) exemple montre comment appeler le `ScaleAction` à partir de `EventTrigger` objets qui surveille le `Focused` et `Unfocused` événements.

Le [ **CustomEasingSwell** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/CustomEasingSwell) exemple montre comment définir une fonction d’accélération personnalisée pour `ScaleAction` dans un fichier code-behind.

Vous pouvez également appeler des actions en utilisant un `Trigger` (distinguished de `EventTrigger`). Cela nécessite que vous avez pris note que `TriggerBase` définit deux collections :

- [`EnterActions`](https://developer.xamarin.com/api/property/Xamarin.Forms.TriggerBase.EnterActions/) de type `IList<TriggerAction>`
- [`ExitActions`](https://developer.xamarin.com/api/property/Xamarin.Forms.TriggerBase.ExitActions/) de type `IList<TriggerAction>`

Le [ **EnterExitSwell** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EnterExitSwell) exemple illustre l’utilisation de ces collections.

### <a name="more-event-triggers"></a>Plusieurs déclencheurs d’événements

Le [ `ScaleUpAndDownAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ScaleUpAndDownAction.cs) classe dans le [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) bibliothèque appelle `ScaleTo` à deux reprises pour évoluer. Le [ **ButtonGrowth** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ButtonGrowth) utilise ce dans l’élément `EventTrigger` pour fournir une rétroaction visuelle lorsqu’un `Button` est enfoncé. Cette animation double est également possible à l’aide de deux actions dans la collection de type [`DelayedScaleAction`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DelayedScaleAction.cs)

Le [ `ShiverAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ShiverAction.cs) classe dans le **Xamarin.FormsBook.Toolkit** bibliothèque définit une action shiver personnalisable. Le [ **ShiverButtonDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ShiverButtonDemo) exemple montre comment il.

Le [ `NumericValidationAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NumericValidationAction.cs) classe dans le **Xamarin.FormsBook.Toolkit** bibliothèque est limitée à `Entry` éléments et les jeux de la `TextColor` propriété rouge se le `Text` propriété n’est pas un `double`. Le [ **TriggerEntryValidation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TriggerEntryValidation) exemple montre comment il.

### <a name="data-triggers"></a>Déclencheurs de données

Le [ `DataTrigger` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTrigger/) est similaire à la `Trigger` , sauf qu’au lieu de l’analyse d’une propriété pour la valeur est modifiée, il analyse une liaison de données. Cela permet à une propriété dans un élément pour affecter une propriété dans un autre élément.

`DataTrigger` définit les trois propriétés :

- [`Binding`](https://developer.xamarin.com/api/property/Xamarin.Forms.DataTrigger.Binding/) de type `BindingBase`
- [`Value`](https://developer.xamarin.com/api/property/Xamarin.Forms.DataTrigger.Value/) de type `Object`
- [`Setters`](https://developer.xamarin.com/api/property/Xamarin.Forms.DataTrigger.Setters/) de type `IList<SetterBase>`

Le [ **GenderColors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/GenderColors) exemple requiert le [ **SchoolOfFineArt** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt) bibliothèque et définit les couleurs des noms des étudiants en bleu ou rose selon le `Sex` propriété :

[![Capture d’écran de triple des couleurs de sexe](images/ch23fg04-small.png "sexe couleurs")](images/ch23fg04-large.png#lightbox "sexe couleurs")

Le [ **ButtonEnabler** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ButtonEnabler) exemples de jeux de la `IsEnabled` propriété d’un `Entry` à `False` si le `Length` propriété de la `Text` propriété de la `Entry`est égal à 0. Notez que la `Text` propriété est initialisée avec une chaîne vide ; par défaut est `null`et le `DataTrigger` ne fonctionnerait pas correctement.

### <a name="combining-conditions-in-the-multitrigger"></a>Combinaison de conditions dans le MultiTrigger

Le [ `MultiTrigger` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MultiTrigger/) est une collection de conditions. Lorsqu’ils sont tous les `true`, puis les accesseurs Set est appliquées. La classe définit deux propriétés :

- [`Conditions`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiTrigger.Conditions/) de type `IList<Condition>`
- [`Setters`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiTrigger.Setters/) de type `IList<Setter>`

[`Condition`](https://developer.xamarin.com/api/type/Xamarin.Forms.Condition/) est une classe abstraite et dispose de deux classes descendant :

- [`PropertyCondition`](https://developer.xamarin.com/api/type/Xamarin.Forms.Condition/), qui a [ `Property` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PropertyCondition.Property/) et [ `Value` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PropertyCondition.Value/) propriétés, telles que `Trigger`
- [`BindingCondition`](https://developer.xamarin.com/api/type/Xamarin.Forms.BindingCondition/), qui a [ `Binding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindingCondition.Binding/) et [ `Value` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindingCondition.Value/) propriétés, telles que `DataTrigger`

Dans le [ **conditionsd** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/AndConditions) exemple, un `BoxView` est coloré uniquement lorsqu’il y a quatre `Switch` éléments sont tous activés.

Le [ **OrConditions** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/OrConditions) exemple montre comment vous pouvez effectuer un `BoxView` une couleur lorsque *tout* de quatre `Switch` éléments sont sous tension. Cela nécessite une application de droit et en inversant toute la logique de Morgan De.

Combinaison de AND et ou logique n’est pas facile et nécessite généralement invisible `Switch` éléments des résultats intermédiaires. Le [ **XorConditions** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/XorConditions) exemple montre comment un `Button` peut être activée si le paramètre de deux `Entry` éléments ont un texte tapé, mais pas si elles ont toutes deux du texte tapé.

## <a name="behaviors"></a>comportements

Rien faire avec un déclencheur, vous pouvez également faire avec un comportement, mais comportements nécessitent toujours une classe qui dérive de [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) et remplace les deux méthodes suivantes :

- [`OnAttachedTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/T/)
- [`OnDetachingFrom`](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/T/)

L’argument est l’élément qui le comportement est attaché. En règle générale, les `OnAttachedTo` méthode attache des gestionnaires d’événements, et `OnDetachingFrom` détache les. Étant donné qu’une telle classe enregistre généralement un état, il généralement ne peut pas être partagé dans un `Style`.

[**BehaviorEntryValidation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/BehaviorEntryValidation) exemple est semblable à **TriggerEntryValidation** sauf qu’elle utilise un comportement &mdash; le [ `NumericValidationBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NumericValidationBehavior.cs) classe dans le [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) bibliothèque.

### <a name="behaviors-with-properties"></a>Comportements avec des propriétés

`Behavior<T>` dérive de `Behavior`, lequel dérive `BindableObject`, de sorte que les propriétés pouvant être liées peuvent être définies sur un comportement. Ces propriétés peuvent être actives dans les liaisons de données.

Cela est illustré dans le [ **EmailValidationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationDemo) programme qui utilisent le [ `ValidEmailBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ValidEmailBehavior.cs) classe dans le [  **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) bibliothèque. `ValidEmailBehavior` a une propriété en lecture seule et est considérée comme une source de liaisons de données.

Le [ **EmailValidationConv** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationConv) utilise ce même problème pour afficher un autre type d’indicateur pour signaler qu’une adresse de messagerie est valide.

Le [ **EmailValidationTrigger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationTrigger) exemple est une variante de l’exemple précédent. ButtonGlide utilise un `DataTrigger` en association avec ce comportement.

### <a name="toggles-and-check-boxes"></a>Active/désactive et cases à cocher

Il est possible d’encapsuler le comportement d’un bouton bascule dans une classe comme [ `ToggleBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ToggleBehavior.cs) dans les [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) bibliothèque, puis définissez toutes les les éléments visuels pour l’opération de basculement entièrement en XAML.

Le [ **ToggleLabel** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ToggleLabel) exemple utilise le `ToggleBehavior` avec un `DataTrigger` à utiliser un `Label` avec deux chaînes de texte pour le bouton bascule.

Le [ **FormattedTextToggle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/FormattedTextToggle) exemple étend ce concept en basculant entre deux `FormattedString` objets.

Le [ `ToggleBase` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ToggleBase.cs) classe dans le **Xamarin.FormsBook.Toolkit** dérive de la bibliothèque `ContentView`, définit un `IsToggled` propriété et incorpore un `ToggleBehavior` pour la bascule logique. Cela rend plus facile de définir le bouton bascule en XAML, comme illustré dans le [ **TranditionalCheckBox** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TraditionalCheckBox) exemple.

Le [ **SwitchCloneDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/SwitchCloneDemo) inclut un [ `SwitchClone` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter23/SwitchCloneDemo/SwitchCloneDemo/SwitchCloneDemo/SwitchClone.cs) classe qui dérive de `ToggleBase` et utilise un [ `TranslateAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/TranslateAction.cs)classe pour construire un bouton bascule qui ressemble à la Xamarin.Forms `Switch`.

A [ `RotateAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RotateAction.cs) dans les **Xamarin.FormsBook.Toolkit** fournit une animation utilisée pour effectuer un levier animé dans le [ **LeverToggle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/LeverToggle)exemple.

### <a name="responding-to-taps"></a>Réponse aux clics

L’inconvénient de `EventTrigger` est que vous ne pouvez pas attacher un `TapGestureRecognizer` pour répondre aux clics. Contourner ce problème est l’objectif de [ `TapBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/TapBehavior.cs) dans les [ **Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)

Le [ **BoxViewTapShiver** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/BoxViewTapShiver) exemple utilise `TapBehavior` à utiliser la plus antérieure `ShiverAction` pour tapées `BoxView` éléments.

Le [ **ShiverViews** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ShiverViews) montre comment réduire le balisage en encapsulant un `ShiverView` classe.

### <a name="radio-buttons"></a>Cases d’option

Le [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) bibliothèque possède également un [ `RadioBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RadioBehavior.cs) classe pour effectuer des cases d’option qui sont regroupés par un `string` nom du groupe.

Le [ **RadioLabels** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioLabels) programme utilise des chaînes de texte pour sa case. Le [ **RadioStyle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioStyle) exemple utilise un `Style` de la différence d’aspect entre les boutons checked et unchecked. Le [ **RadioImages** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioImages) exemple utilise des images convertie (boxed) pour ses boutons radio :

[![Capture d’écran de triple des Images de cases d’option](images/ch23fg17-small.png "Images des boutons Radio")](images/ch23fg17-large.png#lightbox "Images des boutons Radio")

Le [ **TraditionalRadios** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TraditionalRadios) exemple dessine traditionnel cases qui apparaît avec un point dans un cercle.

### <a name="fades-and-orientation"></a>Fondus et l’orientation

L’exemple final, [ **MultiColorSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/MultiColorSliders) vous permet de basculer entre les trois vues de sélection de couleur différent à l’aide de cases d’option. Les trois vues par atténuation en entrée et de sortie à l’aide un [ `FadeEnableAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/FadeEnableAction.cs) dans les [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) bibliothèque.

Le programme répond également aux modifications de l’orientation portrait ou paysage utilisant un [ `GridOrientationBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/GridOrientationBehavior.cs) dans les **Xamarin.FormsBook.Toolkit** bibliothèque.



## <a name="related-links"></a>Liens associés

- [Recherche en texte intégral chapitre 23 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch23-Apr2016.pdf)
- [Exemples de chapitre 23](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23)
- [Utilisation des déclencheurs](~/xamarin-forms/app-fundamentals/triggers.md)
- [Comportements de Xamarin.Forms](~/xamarin-forms/app-fundamentals/behaviors/creating.md)
