---
title: Résumé du chapitre 11. Infrastructure Bindable
description: 'Création d’applications mobiles avec Xamarin.Forms : résumé du chapitre 11. Infrastructure Bindable'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 34671C48-0ED4-4B76-A33D-D6505390DC5B
author: charlespetzold
ms.author: chape
ms.date: 07/19/2018
ms.openlocfilehash: 9f3c077d7bae3557178236b81073afaf4892a272
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/19/2018
ms.locfileid: "39156559"
---
# <a name="summary-of-chapter-11-the-bindable-infrastructure"></a>Résumé du chapitre 11. Infrastructure Bindable

Tous les programmeurs c# est familiarisé avec c# *propriétés*. Les propriétés contiennent une *définir* accesseur et/ou un *obtenir* accesseur. Elles sont souvent appelées *propriétés CLR* pour le Common Language Runtime.

Xamarin.Forms définit une définition de propriété améliorée appelée un *propriété pouvant être liée* encapsulé par le [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) classe et pris en charge par le [ `BindableObject` ](xref:Xamarin.Forms.BindableObject)classe. Ces classes sont associés mais bien distincts : le `BindableProperty` est utilisé pour définir la propriété proprement dite ; `BindableObject` est similaire à `object` : c’est une classe de base des classes qui définissent les propriétés pouvant être liées.

## <a name="the-xamarinforms-class-hierarchy"></a>La hiérarchie de classes de Xamarin.Forms

Le [ **ClassHierarchy** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/ClassHierarchy) exemple utilise la réflexion pour afficher une hiérarchie de classes de Xamarin.Forms et illustrer le rôle crucial joué par `BindableObject` dans cette hiérarchie. `BindableObject` dérive de `Object` et est la classe parente à [ `Element` ](xref:Xamarin.Forms.Element) à partir de laquelle [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) dérive. C’est la classe parent à [ `Page` ](xref:Xamarin.Forms.Page) et [ `View` ](xref:Xamarin.Forms.View), qui est la classe parente à [ `Layout` ](xref:Xamarin.Forms.Layout):

[![Capture d’écran triple de la hiérarchie de classes partage](images/ch11fg01-small.png "partage de hiérarchie de classe")](images/ch11fg01-large.png#lightbox "partage de hiérarchie de classe")

## <a name="a-peek-into-bindableobject-and-bindableproperty"></a>Un coup de œil dans BindableObject et BindableProperty

Dans les classes qui dérivent de `BindableObject` nombreuses propriétés CLR sont dits se reposer » sur « Propriétés pouvant être liées. Par exemple, le [ `Text` ](xref:Xamarin.Forms.Label.Text) propriété de la `Label` classe est une propriété CLR, mais la `Label` classe définit également un champ statique public en lecture seule nommé [ `TextProperty` ](xref:Xamarin.Forms.Label.TextProperty) de type `BindableProperty`.

Une application peut définir ou obtenir le `Text` propriété du `Label` normalement, ou l’application peut définir le `Text` en appelant le [ `SetValue` ](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) méthode définie par `BindableObject` avec un `Label.TextProperty` argument. De même, une application peut obtenir la valeur de la `Text` propriété en appelant le [ `GetValue` ](xref:Xamarin.Forms.BindableObject.GetValue(Xamarin.Forms.BindableProperty)) (méthode), en utilisant un `Label.TextProperty` argument. Cela est illustré par la [ **PropertySettings** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/PropertySettings) exemple.

En effet, le `Text` propriété CLR est entièrement implémentée à l’aide de la `SetValue` et `GetValue` méthodes définies par `BindableObject` conjointement avec le `Label.TextProperty` propriété statique.

`BindableObject` et `BindableProperty` prennent en charge pour :

- Ce qui donne les valeurs de propriétés par défaut
- Stockage de leurs valeurs actuelles
- Offrant des mécanismes de valider les valeurs de propriété
- Maintenir la cohérence entre les propriétés associées dans une classe unique
- Répondre aux modifications de propriétés
- Déclencher des notifications lorsqu’une propriété est sur le point de changer ou a changé
- Prise en charge de la liaison de données
- Prise en charge des styles
- Prise en charge des ressources dynamiques

Chaque fois qu’une propriété qui est associée à la modification d’une propriété pouvant être liée, `BindableObject` se déclenche un [ `PropertyChanged` ](xref:Xamarin.Forms.BindableObject.PropertyChanged) événement qui identifie la propriété qui a changé. Cet événement n’est pas déclenché lorsque la propriété est définie sur la même valeur.

Certaines propriétés ne sont pas soutenues par les propriétés pouvant être liées et certaines classes de Xamarin.Forms &mdash; comme `Span` &mdash; ne dérivent pas de `BindableObject`. Seule une classe qui dérive de `BindableObject` peut prendre en charge les propriétés pouvant être liées, car `BindableObject` définit le `SetValue` et `GetValue` méthodes.

Étant donné que `Span` ne dérive pas de `BindableObject`, aucune de ses propriétés &mdash; comme `Text` &mdash; sont soutenus par une propriété pouvant être liée. C’est pourquoi un `DynamicResource` définition sur le `Text` propriété du `Span` lève une exception dans le [ **DynamicVsStatic** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/DynamicVsStatic) exemple dans le chapitre précédent. Le [ **DynamicVsStaticCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/DynamicVsStaticCode) exemple montre comment définir une ressource dynamique dans le code à l’aide du [ `SetDynamicResource` ](xref:Xamarin.Forms.Element.SetDynamicResource(Xamarin.Forms.BindableProperty,System.String)) méthode définie par `Element`. Le premier argument est un objet de type `BindableProperty`.

De même, le [ `SetBinding` ](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) méthode définie par `BindableObject` a un premier argument de type `BindableProperty`.

## <a name="defining-bindable-properties"></a>Définition des propriétés pouvant être liées

Vous pouvez définir vos propres propriétés pouvant être liées à l’aide de la méthode statique [ `BindableProperty.Create` ](xref:Xamarin.Forms.BindableProperty.Create(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) méthode pour créer un champ en lecture seule statique de type `BindableProperty`.

Cela est illustré dans le [ `AltLabel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AltLabel.cs) classe dans le [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) bibliothèque. La classe dérivée de `Label` et vous permet de spécifier une taille de police en points. Il est montré dans le [ **PointSizedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/PointSizedText) exemple.

Quatre arguments de la `BindableProperty.Create` méthode sont requis :

- `propertyName`: le nom de la propriété (le même que le nom de propriété CLR)
- `returnType`: le type de la propriété CLR
- `declaringType`: le type de la classe déclarant la propriété
- `defaultValue`: la valeur de propriété par défaut

Étant donné que `defaultValue` est de type `object`, le compilateur doit être en mesure de déterminer le type de la valeur par défaut. Par exemple, si le `returnType` est `double`, la `defaultValue` doit être défini sur quelque chose comme 0.0 plutôt que simplement 0, ou l’incompatibilité de type déclenchera une exception lors de l’exécution.

Il est également très courant pour une propriété pouvant être liée à inclure :

- `propertyChanged`: une méthode statique appelée lorsque la propriété change de valeur. Le premier argument est l’instance de la classe dont la propriété a été modifiée.

Les autres arguments de `BindableProperty.Create` ne sont pas aussi courantes :

- `defaultBindingMode`: utilisé dans le cadre de la liaison de données (comme indiqué dans [ **chapitre 16. Liaison de données**](chapter16.md))
- `validateValue`: un rappel pour rechercher une valeur valide
- `propertyChanging`: un rappel pour indiquer le moment où la propriété va être modifiée
- `coerceValue`: un rappel pour forcer une valeur définie à une autre valeur
- `defaultValueCreate`: un rappel pour créer une valeur par défaut qui ne peut pas être partagée entre des instances de la classe (par exemple, une collection)

### <a name="the-read-only-bindable-property"></a>La propriété peut être liée en lecture seule

Une propriété peut être en lecture seule. Création d’une propriété en lecture seule, vous devez appeler la méthode statique [ `BindableProperty.CreateReadOnly` ](xref:Xamarin.Forms.BindableProperty.CreateReadOnly(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) pour définir un champ statique privé en lecture seule de type [ `BindablePropertyKey` ](xref:Xamarin.Forms.BindablePropertyKey).

Ensuite, définissez la propriété CLR `set` accesseur d’en tant que `private` pour appeler un [ `SetValue` ](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindablePropertyKey,System.Object)) surcharge avec le `BindablePropertyKey` objet. Ainsi, la propriété est définie en dehors de la classe.

Cela est illustré dans le [ `CountedLabel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CountedLabel.cs) classe utilisée dans le [ **BaskervillesCount** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/BaskervillesCount) exemple.

## <a name="related-links"></a>Liens connexes

- [Chapitre 11 de texte intégral (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch11-Apr2016.pdf)
- [Exemples de chapitre 11](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11)
- [Propriétés pouvant être liées](~/xamarin-forms/xaml/bindable-properties.md)
