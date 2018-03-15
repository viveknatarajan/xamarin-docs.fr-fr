---
title: "Résumé du chapitre 11. L’infrastructure pouvant être liée"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 34671C48-0ED4-4B76-A33D-D6505390DC5B
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: ccae97021e86eb1375f948c5ad126253c6088037
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/15/2018
---
# <a name="summary-of-chapter-11-the-bindable-infrastructure"></a>Résumé du chapitre 11. L’infrastructure pouvant être liée

Tous les programmeurs c# est familiarisé avec c# *propriétés*. Les propriétés contiennent un *définir* accesseur et/ou un *obtenir* accesseur. Ils sont souvent appelés *propriétés CLR* pour le Common Language Runtime.

Xamarin.Forms définit une définition de propriété améliorée appelée un *propriété pouvant être liée* encapsulé par le [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) de classe et la prise en charge par le [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/)classe. Ces classes sont associées mais assez distinctes : le `BindableProperty` est utilisé pour définir la propriété proprement dite ; `BindableObject` est similaire à `object` dans la mesure où il s’agit d’une classe de base pour les classes qui définissent les propriétés pouvant être liées.

## <a name="the-xamarinforms-class-hierarchy"></a>La hiérarchie de classes Xamarin.Forms

Le [ **ClassHierarchy** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/ClassHierarchy) exemple utilise la réflexion pour présenter une hiérarchie de classe de Xamarin.Forms et illustrer le rôle crucial joué par `BindableObject` dans cette hiérarchie. `BindableObject` dérive de `Object` et est la classe parente pour [ `Element` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Element/) à partir de laquelle [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) dérive. C’est la classe parente pour [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) et [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/), qui est la classe parente pour [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/):

[![Capture d’écran de triple de la hiérarchie de classes partage](images/ch11fg01-small.png "la hiérarchie de la classe partage")](images/ch11fg01-large.png#lightbox "partage de hiérarchie de classe")

## <a name="a-peek-into-bindableobject-and-bindableproperty"></a>Un coup de œil dans BindableObject et BindableProperty

Dans les classes qui dérivent de `BindableObject` de nombreuses propriétés CLR sont dits « reposer sur « Propriétés pouvant être liées. Par exemple, le [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) propriété de la `Label` classe est une propriété CLR, mais la `Label` classe définit également un champ statique public en lecture seule nommé [ `TextProperty` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextProperty/) de type `BindableProperty`.

Une application peut définir ou obtenir la `Text` propriété du `Label` normalement, ou l’application peut définir le `Text` en appelant le [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) méthode définie par `BindableObject` avec un `Label.TextProperty` argument. De même, une application peut obtenir la valeur de la `Text` propriété en appelant le [ `GetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.GetValue/p/Xamarin.Forms.BindableProperty/) (méthode), en utilisant un `Label.TextProperty` argument. Cela est illustré par le [ **PropertySettings** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/PropertySettings) exemple.

En effet, le `Text` propriété CLR est implémentée entièrement à l’aide de la `SetValue` et `GetValue` méthodes définies par `BindableObject` conjointement avec la `Label.TextProperty` propriété statique.

`BindableObject` et `BindableProperty` prennent en charge :

- En donnant des valeurs de propriétés par défaut
- Le stockage de leurs valeurs actuelles
- Offrant des mécanismes de valider les valeurs de propriété
- Maintenir la cohérence entre les propriétés associées dans une classe unique
- Répondre aux modifications de propriétés
- Déclencher des notifications lorsqu’une propriété est sur le point de changer ou a changé
- Prise en charge de la liaison de données
- Prise en charge des styles
- Prise en charge des ressources dynamiques

Chaque fois qu’une propriété qui est soutenue par une propriété pouvant être liée est modifiée, `BindableObject` se déclenche un [ `PropertyChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.BindableObject.PropertyChanged/) événement qui identifie la propriété qui a changé. Cet événement n’est pas déclenché lorsque la propriété est définie sur la même valeur.

Certaines propriétés ne sont pas soutenues par les propriétés pouvant être liées et certaines classes Xamarin.Forms &mdash; comme `Span` &mdash; ne dérivent pas de `BindableObject`. Une classe qui dérive de `BindableObject` peut prendre en charge les propriétés pouvant être liées, car `BindableObject` définit le `SetValue` et `GetValue` méthodes.

Étant donné que `Span` ne dérive pas de `BindableObject`, aucune de ses propriétés &mdash; comme `Text` &mdash; bénéficient d’une propriété pouvant être liée. C’est pourquoi un `DynamicResource` définition sur le `Text` propriété du `Span` lève une exception dans le [ **DynamicVsStatic** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/DynamicVsStatic) présenté dans le chapitre précédent. Le [ **DynamicVsStaticCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/DynamicVsStaticCode) exemple montre comment définir une ressource dynamique dans un code utilisant la [ `SetDynamicResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Element.SetDynamicResource/p/Xamarin.Forms.BindableProperty/System.String/) méthode définie par `Element`. Le premier argument est un objet de type `BindableProperty`.

De même, la [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) méthode définie par `BindableObject` a un premier argument de type `BindableProperty`.

## <a name="defining-bindable-properties"></a>Définition des propriétés pouvant être liées

Vous pouvez définir vos propres propriétés pouvant être liées à l’aide de la méthode statique [ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) le légèrement plus court ou méthode [ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/) surcharge pour créer un champ en lecture seule statique de type `BindableProperty`.

Cela est illustré dans le [ `AltLabel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AltLabel.cs) classe dans le [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) bibliothèque. Dérive de la classe `Label` et vous permet de spécifier une taille de police en points. Il est présenté dans le [ **PointSizedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/PointSizedText) exemple.

Quatre arguments de la `BindableProperty.Create` méthode sont requises :

- `propertyName`: le nom de la propriété (le même que le nom de propriété CLR)
- `returnType`: le type de la propriété CLR
- `declaringType`: le type de la classe déclarant la propriété
- `defaultValue`: la valeur de propriété par défaut

Étant donné que `defaultValue` est de type `object`, le compilateur doit être en mesure de déterminer le type de la valeur par défaut. Par exemple, si le `returnType` est `double`, la `defaultValue` doit être défini comme 0.0 plutôt que simplement 0, ou l’incompatibilité de type déclenchera une exception lors de l’exécution.

Il est également très courant pour une propriété pouvant être liée à inclure :

- `propertyChanged`: une méthode statique appelée lorsque la propriété change de valeur. Le premier argument est l’instance de la classe dont la propriété a été modifiée.

Les autres arguments de `BindableProperty.Create` ne sont pas aussi courantes :

- `defaultBindingMode`: utilisé pour la liaison de données (comme indiqué dans [ **chapitre 16. Liaison de données**](chapter16.md))
- `validateValue`: un rappel pour rechercher une valeur valide
- `propertyChanging`: un rappel pour indiquer le moment où la propriété va être modifiée
- `coerceValue`: un rappel pour forcer une valeur définie à une autre valeur
- `defaultValueCreate`: un rappel pour créer une valeur par défaut qui ne peut pas être partagée entre des instances de la classe (par exemple, une collection)

### <a name="the-read-only-bindable-property"></a>La propriété pouvant être liée en lecture seule

Une propriété peut être en lecture seule. Création d’une propriété en lecture seule, vous devez appeler la méthode statique [ `BindableProperty.CreateReadOnly` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.CreateReadOnly/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) ou un plus court [ `BindableProperty.CreateReadOnly` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.CreateReadOnly/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/) pour définir un champ statique privé en lecture seule de type [ `BindablePropertyKey` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindablePropertyKey/).

Ensuite, définissez la propriété CLR `set` accesseur d’en tant que `private` pour appeler un [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindablePropertyKey/System.Object/) surcharger avec la `BindablePropertyKey` objet. Ainsi, la propriété est définie en dehors de la classe.

Cela est illustré dans le [ `CountedLabel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CountedLabel.cs) classe utilisée dans le [ **BaskervillesCount** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/BaskervillesCount) exemple.



## <a name="related-links"></a>Liens associés

- [Recherche en texte intégral chapitre 11 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch11-Apr2016.pdf)
- [Exemples du chapitre 11](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11)
