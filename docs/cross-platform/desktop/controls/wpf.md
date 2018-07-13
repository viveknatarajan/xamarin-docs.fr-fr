---
ms.assetid: 1BB412D1-FC3D-4E69-8B01-B976A3DB6328
title: 'Visual Studio WPF. Xamarin.Forms : Similitudes et différences'
description: Ce document compare et contraste WPF de Xamarin.Forms. Il aborde les modèles de contrôle, XAML, infrastructure de liaison, les modèles de données, ItemsControl, UserControl, navigation et navigation dans les URL.
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: 4d6585715b2fc118bb350c242abccbc68791ec0b
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998516"
---
# <a name="wpf-vs-xamarinforms-similarities--differences"></a>Visual Studio WPF. Xamarin.Forms : Similitudes et différences

## <a name="control-templates"></a>Modèles de contrôle

WPF prend en charge le concept de *modèles de contrôle* qui fournissent les instructions de visualisation pour un contrôle (`Button`, `ListBox`, etc..). Comme mentionné ci-dessus, Xamarin.Forms utilise concret _rendu_ pour cela, les classes qui interagissent avec la plateforme native (iOS, Android, etc.) pour visualiser le contrôle.

Toutefois, Xamarin.Forms _est_ ont un `ControlTemplate` type - il est utilisé pour le thème `Page` objets. Il fournit une définition pour un `Page` qui fournit un contenu homogène, mais permet à l’utilisateur de la page Modifier les couleurs, polices, etc. et même ajouter des éléments pour le rendre unique à l’application.

Utilisations courantes de ce sont des choses telles que des boîtes de dialogue d’authentification, à l’écran et pour fournir une page standard, mais thémables apparence qui peuvent être personnalisée dans l’application. Dans le cadre de cette prise en charge, de nombreux contrôles de WPF-nommé familiers sont utilisés :

1. `ContentPage`
2. `ContentView`
3. `ContentPresenter`
4. `TemplateBinding`

Mais il est important de savoir qu’il s’agit _pas_ poursuit le même objectif dans Xamarin.Forms. Pour plus d’informations sur cette fonctionnalité, consultez le [page de documentation](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md).

## <a name="xaml"></a>XAML

XAML est utilisé comme langage de balisage déclaratif pour WPF et Xamarin.Forms. La plupart du temps, la syntaxe est identique, la principale différence réside dans les objets qui sont définis/créés par les graphiques XAML.

- Xamarin.Forms prend en charge la [spécification de XAML 2009](/dotnet/framework/xaml-services/xaml-2009-language-features/); cela rend plus facile de définir des données telles que `string`s, `int`s, etc., ainsi que la définition des types génériques et passage des arguments aux constructeurs.

- Il n’existe actuellement aucun moyen de charger dynamiquement XAML, telles que WPF possible avec `XamlReader`. Vous pouvez obtenir les mêmes fonctionnalités de base avec un [package NuGet](https://www.nuget.org/packages/Xamarin.Forms.Dynamic/) si.

### <a name="markup-extensions"></a>Extensions de balisage

Xamarin.Forms prend en charge l’extension XAML par le biais des extensions de balisage, comme WPF. Dès le départ, il possède les mêmes blocs de construction de base :

1. `{x:Array}`
2. `{Binding}`
3. `{DynamicResource}`
4. `{x:Null}`
5. `{x:Static}`
6. `{StaticResource}`
7. `{x:Type}`

En outre, il inclut `{x:Reference}` à partir de la spécification de XAML 2009 et un `{TemplateBinding}` extension de balisage qui est utilisée pour la version spécialisée de `ControlTemplate` pris en charge par Xamarin.Forms.

> [!WARNING]
> Le `ControlTemplate` prise en charge n’est pas le même - même si elle a le même nom.

Xamarin.Forms prend en charge les extensions de balisage personnalisée - mais l’implémentation est légèrement différente. Dans WPF, vous devez dériver de `MarkupExtension` -classe de base abstraite. Dans Xamarin.Forms, qui est remplacée par une interface `IMarkupExtension` ou `IMarkupExtension<T>` qui est plus flexible.

Comme WPF, la seule méthode requise est un `ProvideValue` méthode pour retourner la valeur à partir de l’extension de balisage.

## <a name="binding-infrastructure"></a>Infrastructure de liaison

Un des principaux concepts reportés est une infrastructure de liaison de données pour se connecter de propriétés visuelles aux propriétés de données .NET. Ainsi, les modèles architecturaux tels que MVVM. La conception de base est identique : vous avez une classe de base peut être liée [BindableObject](xref:Xamarin.Forms.BindableObject), dans WPF, c’est le [DependencyObject](https://msdn.microsoft.com/en-us/library/system.windows.dependencyobject(v=vs.110).aspx) classe. Cette classe de base est utilisée comme l’ancêtre de racine pour tous les objets qui participeront en tant que cibles de liaison de données. Les classes dérivées exposent puis [BindableProperty](xref:Xamarin.Forms.BindableProperty) objets qui agissent en tant que le stockage de sauvegarde pour les valeurs de propriété (elles sont définies comme [DependencyProperty](https://msdn.microsoft.com/library/system.windows.dependencyproperty(v=vs.110).aspx) objets dans WPF).

### <a name="defining-bindable-properties"></a>Définition des propriétés pouvant être liées

La définition d’une propriété pouvant être liée dans Xamarin.Forms est le même que WPF :
1. L’objet doit dériver de `BindableObject`.
2. Il doit y avoir un champ statique public de type `BindableProperty` déclaré pour définir la clé de stockage de sauvegarde pour la propriété.
3. Il doit y avoir un wrapper de propriété d’instance public qui utilise `GetValue` et `SetValue` pour récupérer et modifier la valeur de propriétés.

Pour obtenir un exemple complet, consultez [propriétés pouvant être liées dans Xamarin.Forms](~/xamarin-forms/xaml/bindable-properties.md).

### <a name="attached-properties"></a>Propriétés jointes

Propriétés jointes sont un sous-ensemble de la propriété peut être liée et ils fonctionnent de la même façon que dans WPF. La principale différence est que le wrapper de propriété est dans ce cas, aucun et remplacé par un ensemble de méthodes statiques get/set sur la classe propriétaire. Consultez [propriétés rattaché dans Xamarin.Forms](~/xamarin-forms/xaml/attached-properties.md) pour plus d’informations.

### <a name="using-the-binding-engine"></a>En utilisant le moteur de liaison

Le processus permettant d’utiliser le moteur de liaison est le même comme il se trouve dans WPF. Il peut être utilisé dans le code-behind en créant un `Binding` objet lié à un objet de source (n’importe quel type .NET) et une valeur de propriété facultatif (si aucun, il traite l’objet source en tant que la propriété elle-même, comme WPF). Vous pouvez ensuite utiliser `SetBinding` sur n’importe quel `BindableObject` pour associer la liaison à un `BindableProperty`.

Vous pouvez également définir la relation de liaison dans XAML à l’aide du `BindingExtension`. Il a les mêmes valeurs de base que l’extension dans WPF.

La prise en charge de la liaison et le moteur sont plus similaires à l’implémentation Silverlight que WPF. Il existe plusieurs fonctionnalités manquantes qui n’étaient pas implémentées dans Xamarin.Forms :

- Il n’existe aucune prise en charge pour les fonctionnalités suivantes dans les liaisons :
    - BindingGroupName
    - BindsDirectlyToSource
    - IsAsync
    - MultiBinding
    - NotifyOnSourceUpdated
    - NotifyOnTargetUpdated
    - NotifyOnValidationError
    - UpdateSourceTrigger
    - UpdateSourceExceptionFilter
    - ValidatesOnDataErrors
    - ValidatesOnExceptions
    - Collection de ValidationRules
    - XPath
    - XmlNamespaceManager
- `Binding.Mode` ne prend pas en charge `OneTime`, utiliser simplement `OneWay`.

#### <a name="relativesource"></a>RelativeSource

Il n’existe aucune prise en charge pour `RelativeSource` liaisons. Dans WPF, permettent de lier à d’autres éléments visuels définis dans XAML. Dans Xamarin.Forms, cette même fonctionnalité peut être obtenue à l’aide de la `{x:Reference}` extension de balisage. Par exemple, en supposant que nous avons un contrôle avec le nom « otherControl » qui a une propriété de texte, nous pouvons lier comme suit :

**WPF**

```xaml
Text={Binding RelativeSource={RelativeSource otherControl}, Path=Text}
```

**Xamarin.Forms**

```xaml
Text={Binding Source={x:Reference otherControl}, Path=Text}
```

La même fonctionnalité peut être utilisée pour le `{RelativeSource Self}` fonctionnalité. Il n’existe cependant aucune prise en charge pour la localisation des ancêtres par type (`{RelativeSource FindAncestor}`).

#### <a name="binding-context"></a>Contexte de liaison

Dans WPF, vous pouvez définir un `DataContext` valeur de la propriété qui reprents la source de liaison par défaut. Si la source d’une liaison n’est pas définie, cette valeur de propriété est utilisée. La valeur est héritée dans l’arborescence d’éléments visuels, ce qui lui permet d’être définis à un niveau supérieur, puis utilisé par les enfants.

Dans Xamarin.Forms, cette même fonctionnalité est disponible, mais le nom de propriété est `BindingContext`.

#### <a name="value-converters"></a>convertisseurs de valeurs ;

Convertisseurs de valeur sont entièrement pris en charge dans Xamarin.Forms - comme WPF. La même forme d’interface est utilisée, mais Xamarin.Forms a l’interface définie dans le `Xamarin.Forms` espace de noms.

### <a name="model-view-viewmodel"></a>Model-View-ViewModel

MVVM est complètement pris en charge par WPF et Xamarin.Forms.

WPF inclut un intégré dans `RoutedCommand` qui est parfois utilisé ; Xamarin.Forms n’a aucune prise en charge de commande intégrée au-delà de la `ICommand` définition d’interface. Vous pouvez inclure plusieurs infrastructures MVVM pour ajouter les classes de base nécessaires pour implémenter le modèle MVVM.

#### <a name="inotifypropertychanged-and-inotifycollectionchanged"></a>INotifyPropertyChanged et INotifyCollectionChanged

Les deux interfaces sont entièrement pris en charge dans les liaisons de Xamarin.Forms. Contrairement à nombreuses infrastructures XAML, les notifications de modification de propriété peuvent être déclenchées sur les threads d’arrière-plan dans Xamarin.Forms (comme WPF) et le moteur de liaison correctement entraîne le passage au thread d’interface utilisateur.

En outre, les deux environnements prennent en charge `SynchronziationContext` et `async` / `await` pour effectuer le marshaling de thread approprié. WPF inclut la `Dispatcher` classe sur tous les éléments visuels, Xamarin.Forms a une méthode statique `Device.BeginInvokeOnMainThread` qui peut également être utilisé (bien que `SynchronizationContext` est recommandée pour le codage inter-plateformes).

- Xamarin.Forms inclut un `ObservableCollection<T>` qui prend en charge les notifications de modification de collection.
- Vous pouvez utiliser `BindingBase.EnableCollectionSynchronization` pour activer les mises à jour inter-threads pour une collection. L’API est légèrement différente de la variante de WPF, [vérifier la documentation pour plus d’informations](xref:Xamarin.Forms.BindingBase.EnableCollectionSynchronization*).

## <a name="data-templates"></a>Modèles de données

Modèles de données sont pris en charge dans Xamarin.Forms pour personnaliser le rendu d’un `ListView` ligne (cellule). Contrairement à WPF pouvant utiliser `DataTemplate`s pour n’importe quel contrôle orienté sur le contenu, Xamarin.Forms actuellement uniquement les utilise pour `ListView`. La définition du modèle peut être définies inline (affectée à la `ItemTemplate` propriété), ou en tant que ressource dans un `ResourceDictionary`.

En outre, ils ne sont pas tout à fait aussi flexibles que leur équivalent WPF.

1. L’élément racine de la `DataTemplate` doit _toujours_ être un `ViewCell` objet.
2. Déclencheurs de données sont entièrement pris en charge dans un modèle de données, mais doit inclure un `DataType` indiquant le type de la propriété qui est associé le déclencheur de propriété.
3. `DataTemplateSelector` est également pris en charge, mais dérive `DataTemplate` est donc simplement affecter directement à la `ItemTemplate` propriété (et `ItemTemplateSelector` dans WPF).

## <a name="itemscontrol"></a>ItemsControl

Il n’existe aucun équivalent intégré à un `ItemsControl` dans Xamarin.Forms ; mais il existe un [personnalisé un pour Xamarin.Forms disponible ici](https://github.com/xamarinhq/xamu-infrastructure/blob/master/src/XamU.Infrastructure/Controls/ItemsControl.cs).

## <a name="user-controls"></a>Contrôles utilisateur

Dans WPF, `UserControl`s sont utilisés pour fournir une section d’interface utilisateur qui est associé à comportement. Dans Xamarin.Forms, nous utilisons le `ContentView` dans le même but. Les deux prennent en charge de liaison et inclusion dans XAML.

## <a name="navigation"></a>Navigation

WPF inclut un rarement utilisés `NavigationService` qui peut être utilisé pour fournir une fonctionnalité de navigation de type « navigateur ». La plupart des applications n’a pas s’embêter avec ce toutefois et à la place utiliser différents `Window` éléments ou des différentes sections de la fenêtre pour afficher des données.

Sur les téléphones, différents _écrans_ sont souvent la solution et par conséquent, Xamarin.Forms inclut la prise en charge de plusieurs formes de navigation :

| Style de navigation | Type de page |
|--- |--- |
|Basée sur pile (push/pop)|NavigationPage|
|Maître/détail|MasterDetailPage|
|Tabulations|TabbedPage|
|Balayez vers la gauche/droite|CarouselView|

Le `NavigationPage` est l’approche la plus courante, et chaque page comporte un `Navigation` propriété qui peut être utilisée pour envoyer ou afficher des pages et de la pile de navigation. C’est l’équivalent le plus proche pour le `NavigationService` trouvé dans WPF.

### <a name="url-navigation"></a>Navigation dans les URL

WPF est une technologie orientée bureau et peut accepter des paramètres de ligne de commande pour indiquer le comportement au démarrage. Xamarin.Forms peut utiliser [lien profond de URL](https://blog.xamarin.com/deep-link-content-with-xamarin-forms-url-navigation/) pour accéder à une page de démarrage.
