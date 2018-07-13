---
title: Le modèle Model-View-ViewModel
description: Ce chapitre explique comment l’application mobile eShopOnContainers utilise le modèle MVVM pour différencier distinctement la logique métier et présentation de l’application à partir de son interface utilisateur.
ms.prod: xamarin
ms.assetid: dd8c1813-df44-4947-bcee-1a1ff2334b87
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: c947ec0c2fffbd9038ee58211c77bd947c445b6e
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998440"
---
# <a name="the-model-view-viewmodel-pattern"></a>Le modèle Model-View-ViewModel

L’expérience de développement Xamarin.Forms implique généralement la création d’une interface utilisateur dans XAML et l’ajout de code-behind qui fonctionne sur l’interface utilisateur. Comme les applications sont modifiées et croître en taille et la portée, des problèmes complexes de maintenance peuvent se produire. Ces problèmes incluent le couplage étroit entre les contrôles d’interface utilisateur et la logique métier, ce qui augmente le coût d’effectuer les modifications de l’interface utilisateur et la difficulté de ce code de test unitaire.

Le modèle Model-View-ViewModel (MVVM) permet de séparer clairement la logique métier et de présentation d’une application à partir de son interface utilisateur (IU). Maintenir une séparation nette entre la logique d’application et l’interface utilisateur vous aide à résoudre de nombreux problèmes liés au développement et peut rendre plus faciles à tester, gérer et faire évoluer une application. Il peut améliorer considérablement les opportunités de réutilisation de code et permet aux développeurs et concepteurs d’interfaces utilisateur plus collaborent aisément lors du développement de leurs éléments respectifs d’une application.

## <a name="the-mvvm-pattern"></a>Le modèle MVVM

Il existe trois composants principaux dans le modèle MVVM : le modèle, la vue et le modèle de vue. Chacun a un objectif de distinct. Figure 2-1 montre les relations entre les trois composants.

![](mvvm-images/mvvm.png "Le modèle MVVM")

**Figure 2-1**: le modèle MVVM

Outre les responsabilités de chaque composant, il est également important de comprendre la façon dont ils interagissent entre eux. À un niveau élevé, la vue « connaît » le modèle de vue, le modèle de vue « connaît » du modèle, mais le modèle n’a pas connaissance du modèle de vue et le modèle de vue n’a pas connaissance de la vue. Par conséquent, le modèle de vue isole la vue à partir du modèle et permet au modèle d’évoluer indépendamment de la vue.

Les avantages de l’utilisation du modèle MVVM sont les suivantes :

-   S’il existe une implémentation de modèle existante qui encapsule la logique métier existante, il peut être difficile, voire à risque pour la modifier. Dans ce scénario, le modèle de vue agit comme un adaptateur pour les classes de modèle et vous permet d’éviter de faire aucune modification importante pour le code du modèle.
-   Les développeurs peuvent créer des tests unitaires pour le modèle de vue et le modèle, sans utiliser la vue. Les tests unitaires pour le modèle de vue peuvent exercer exactement les mêmes fonctionnalités que celles utilisées par la vue.
-   L’interface utilisateur de l’application peut être repensée sans toucher au code, à condition que la vue est entièrement implémentée dans XAML. Par conséquent, une nouvelle version de la vue doit fonctionner avec le modèle de vue existant.
-   Concepteurs et développeurs peuvent travailler indépendamment et simultanément sur leurs composants pendant le processus de développement. Concepteurs peuvent se concentrer sur la vue, tandis que les développeurs peuvent travailler sur le modèle de vue et les composants de modèle.

La clé à l’aide de MVVM efficacement repose comprendre comment factoriser le code d’application dans les classes correctes et comprendre l’interagissent entre les classes. Les sections suivantes décrivent les responsabilités de chacune des classes dans le modèle MVVM.

### <a name="view"></a>Vue

La vue est chargée de définir la structure, la disposition et l’apparence de ce que voit l’utilisateur sur l’écran. Dans l’idéal, chaque vue est définie dans XAML, avec un code-behind limité qui ne contient-elle pas de logique métier. Toutefois, dans certains cas, le code-behind peut contenir la logique de l’interface utilisateur qui implémente le comportement visuel qui est difficile à exprimer dans XAML, tels que les animations.

Dans une application Xamarin.Forms, une vue est généralement un [ `Page` ](xref:Xamarin.Forms.Page)-dérivée ou [ `ContentView` ](xref:Xamarin.Forms.ContentView)-classe dérivée. Toutefois, les vues peuvent également être représentées par un modèle de données, qui spécifie les éléments d’interface utilisateur à utiliser pour représenter visuellement un objet lorsqu’il est affiché. Un modèle de données en tant que vue n’a pas de n’importe quel code-behind et est conçu pour lier à un type de modèle de vue spécifique.

> [!TIP]
> Évitez d’activation et désactivation des éléments d’interface utilisateur dans le code-behind. Assurez-vous que les modèles de vue sont responsables de la définition des changements d’état logique qui affectent certains aspects de l’affichage de la vue, telles que si une commande est disponible, ou une indication qu’une opération est en attente. Par conséquent, activer et désactiver des éléments d’interface utilisateur en le liant à afficher les propriétés du modèle, plutôt que l’activation et désactivation code-behind.

Il existe plusieurs options pour exécuter le code sur le modèle de vue en réponse aux interactions de la vue, comme un clic de bouton ou la sélection d’éléments. Si un contrôle prend en charge les commandes, le contrôle `Command` propriété peut être lié aux données à un `ICommand` propriété sur le modèle de vue. Lorsque la commande du contrôle est appelée, le code dans le modèle de vue est exécuté. Outre les commandes, les comportements peuvent être attachés à un objet dans la vue et peuvent écouter pour une commande à appeler ou d’un événement à déclencher. En réponse, le comportement peut ensuite appeler une `ICommand` sur le modèle de vue ou une méthode sur le modèle de vue.

### <a name="viewmodel"></a>ViewModel

Le modèle de vue implémente des propriétés et commandes dans lequel la vue peut lier les données à et en avertit l’affichage des modifications d’état via les événements de notification de modification. Les propriétés et les commandes qui fournit le modèle d’affichage à définir les fonctionnalités offerts par l’interface utilisateur, mais la vue détermine la façon dont cette fonctionnalité doit être affichée.

> [!TIP]
> Maintenir la réactivité des opérations asynchrones de l’interface utilisateur. Les applications mobiles doivent conserver le thread d’interface utilisateur débloqué pour améliorer la perception des performances. Par conséquent, dans le modèle de vue, utilisez les méthodes asynchrones pour les opérations d’e/s et déclencher des événements pour notifier les vues des modifications de propriétés de façon asynchrone.

Le modèle de vue est également chargé de coordonner les interactions de la vue avec toutes les classes de modèle qui sont nécessaires. Il est généralement une relation un-à-plusieurs entre le modèle de vue et les classes de modèle. Le modèle de vue peut choisir d’exposer des classes de modèle directement à la vue afin que les contrôles dans la vue peuvent lier les données directement. Dans ce cas, les classes de modèle devrez être conçues pour prendre en charge la liaison de données et les événements de notification de modification.

Chaque modèle de vue fournit des données à partir d’un modèle dans un formulaire de la vue peut facilement consommer. Pour ce faire, le modèle de vue effectue parfois la conversion de données. Placer cette conversion de données dans le modèle de vue d’est une bonne idée, car elle fournit des propriétés que la vue peut lier à. Par exemple, le modèle de vue peut combiner les valeurs des deux propriétés pour le rendre plus facile pour l’affichage par la vue.

> [!TIP]
> Centraliser les conversions de données dans une couche de conversion. Il est également possible d’utiliser des convertisseurs comme une couche de conversion de données distinct qui se trouve entre le modèle de vue et la vue. Cela peut être nécessaire, par exemple, lors de données nécessitent une mise en forme spéciale qui ne fournit pas le modèle de vue.

Dans l’ordre pour le modèle de vue de participer à la liaison de données bidirectionnelle avec la vue, ses propriétés doivent déclencher la `PropertyChanged` événement. Afficher les modèles de satisfont cette exigence en implémentant la `INotifyPropertyChanged` d’interface et de déclencher le `PropertyChanged` événement lorsqu’une propriété est modifiée.

Pour les collections, la vue conviviale `ObservableCollection<T>` est fourni. Cette collection implémente la notification de la collection modifiée, ce qui décharge le développeur de devoir implémenter le `INotifyCollectionChanged` interface sur les collections.

### <a name="model"></a>Modèle

Classes de modèle sont des classes non visuels qui encapsulent les données de l’application. Par conséquent, le modèle peut être considéré comme représentant le modèle de domaine de l’application, qui comprend généralement un modèle de données, ainsi que la logique métier et de validation. Objets de transfert de données (DTO), Plain Old CLR Objects (OCT), entité générée et des objets proxy sont des exemples d’objets de modèle.

Classes de modèle sont généralement utilisés conjointement avec les services ou les référentiels qui encapsulent l’accès aux données et la mise en cache.

## <a name="connecting-view-models-to-views"></a>Modèles de vue qui se connecte aux vues

Modèles de vue peuvent être connectés à des vues en utilisant les fonctionnalités de liaison de données de Xamarin.Forms. Il existe de nombreuses approches qui peuvent être utilisées pour construire des vues et consulter les modèles et les associer à l’exécution. Ces approches se répartissent en deux catégories, appelés composition de la première vue et composition premier modèle de vue. Choix entre la composition de la première vue et vue composition premier modèle est un problème de préférence et de complexité. Toutefois, toutes les approches partagent le même but, ce qui concerne l’affichage de disposer d’un modèle de vue assigné à sa propriété de contexte BindingContext.

Avec la vue première composition de l’application est sur le plan conceptuel composée de vues qui se connectent à elles varient selon les modèles de vue. Le principal avantage de cette approche est qu’il rend faciles à construire faiblement couplés, applications testables unité étant donné que les modèles de vue n’ont aucune dépendance vis-à-vis sur les vues eux-mêmes. Il est également facile de comprendre la structure de l’application en suivant sa structure visuelle, plutôt que d’avoir à effectuer le suivi de l’exécution de code pour comprendre comment les classes sont créés et associés. En outre, construction première vue s’aligne sur le système de navigation Xamarin.Forms qui est responsable de la construction des pages lors de la navigation se produit, ce qui rend une composition de la première vue modèle complexes et non alignés avec la plateforme.

Avec la vue composition première de modèles à l’application est sur le plan conceptuel composée de modèles de vue, avec un service qui est chargé de rechercher la vue pour un modèle de vue. Composition de première vue modèles semble plus naturelle pour certains développeurs, étant donné que la création de la vue peut être disparaît, ce qui leur permet de se concentrer sur la structure logique non-interface utilisateur de l’application. En outre, il permet des modèles de vue doit être créé par d’autres modèles de vue. Toutefois, cette approche est souvent complexe et il peut devenir difficile à comprendre comment les différentes parties de l’application sont créés et associés.

> [!TIP]
> Conserver les modèles de vue et les vues indépendantes. La liaison des vues à une propriété dans une source de données doit être de dépendance de principal de la vue sur son modèle de vue correspondante. Plus précisément, ne font pas référence types d’affichages, comme [ `Button` ](xref:Xamarin.Forms.Button) et [ `ListView` ](xref:Xamarin.Forms.ListView), à partir de modèles de vue. En suivant les principes présentés ici, les modèles de vue peuvent être testées de manière isolée, ce qui réduit la probabilité d’erreurs logicielles en limitant l’étendue.

Les sections suivantes décrivent les approches principales pour la connexion des modèles de vue aux vues.

### <a name="creating-a-view-model-declaratively"></a>Créez un modèle de vue de façon déclarative

L’approche la plus simple est pour l’affichage de manière déclarative d’instancier son modèle de vue correspondant dans XAML. Lorsque la vue est construite, l’objet de modèle de vue correspondant sera également être construite. Cette approche est illustrée dans l’exemple de code suivant :

```xaml
<ContentPage ... xmlns:local="clr-namespace:eShop">  
    <ContentPage.BindingContext>  
        <local:LoginViewModel />  
    </ContentPage.BindingContext>  
    ...  
</ContentPage>
```

Lorsque le [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) est créé, une instance de la `LoginViewModel` est automatiquement construite et défini en tant que la vue [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext).

Cette construction déclarative et l’assignation de modèle de vue par la vue présente l’avantage qu’il est simple, mais présente les inconvénients qu’il nécessite le constructeur par défaut (sans paramètre) dans le modèle de vue.

### <a name="creating-a-view-model-programmatically"></a>Création d’un modèle de vue par programmation

Une vue peut avoir un code dans le fichier code-behind qui résulte dans le modèle de vue assigné à son [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) propriété. Souvent cela dans le constructeur de la vue, comme indiqué dans l’exemple de code suivant :

```csharp
public LoginView()  
{  
    InitializeComponent();  
    BindingContext = new LoginViewModel(navigationService);  
}
```

La construction de programmation et l’affectation du modèle de vue dans le code-behind de la vue présente l’avantage qu’il est simple. Toutefois, le principal inconvénient de cette approche est que la vue doit fournir le modèle de vue avec les dépendances requises. À l’aide d’un conteneur d’injection de dépendance peut contribuer à maintenir couplage faible entre la vue et le modèle de vue. Pour plus d’informations, consultez [l’Injection de dépendances](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md).

### <a name="creating-a-view-defined-as-a-data-template"></a>Création d’un affichage défini comme un modèle de données

Une vue peut être définie comme un modèle de données et associée à un type de modèle de vue. Modèles de données peuvent être définis en tant que ressources, ou ils peuvent être définies inline dans le contrôle qui affiche le modèle de vue. Le contenu du contrôle est l’instance de modèle de vue et le modèle de données est utilisé pour représenter visuellement. Cette technique est un exemple d’une situation dans laquelle le modèle de vue est instancié tout d’abord, suivie de la création de la vue.

<a name="automatically_creating_a_view_model_with_a_view_model_locator" />

### <a name="automatically-creating-a-view-model-with-a-view-model-locator"></a>Création automatique d’un modèle de vue avec un localisateur de modèle de vue

Un localisateur de modèle de vue est une classe personnalisée qui gère l’instanciation des modèles de vue et leur association à des vues. Dans l’application mobile eShopOnContainers, le `ViewModelLocator` classe a une propriété jointe, `AutoWireViewModel`, qui est utilisé pour associer des modèles de vue à des vues. Dans XAML la vue, cette propriété jointe est définie sur true pour indiquer que le modèle de vue doit être connecté automatiquement à la vue, comme indiqué dans l’exemple de code suivant :

```xaml
viewModelBase:ViewModelLocator.AutoWireViewModel="true"
```

Le `AutoWireViewModel` propriété est une propriété pouvant être liée qui est initialisé sur false et lorsque sa valeur change le `OnAutoWireViewModelChanged` Gestionnaire d’événements est appelé. Cette méthode résout le modèle de vue pour la vue. L’exemple de code suivant montre comment cela :

```csharp
private static void OnAutoWireViewModelChanged(BindableObject bindable, object oldValue, object newValue)  
{  
    var view = bindable as Element;  
    if (view == null)  
    {  
        return;  
    }  

    var viewType = view.GetType();  
    var viewName = viewType.FullName.Replace(".Views.", ".ViewModels.");  
    var viewAssemblyName = viewType.GetTypeInfo().Assembly.FullName;  
    var viewModelName = string.Format(  
        CultureInfo.InvariantCulture, "{0}Model, {1}", viewName, viewAssemblyName);  

    var viewModelType = Type.GetType(viewModelName);  
    if (viewModelType == null)  
    {  
        return;  
    }  
    var viewModel = _container.Resolve(viewModelType);  
    view.BindingContext = viewModel;  
}
```

Le `OnAutoWireViewModelChanged` méthode essaie de résoudre le modèle de vue à l’aide d’une approche basée sur une convention. Cette convention suppose que :

-   Modèles de vue sont dans le même assembly en tant que types d’affichage.
-   Les vues sont dans un. Espace de noms enfant vues.
-   Modèles de vue sont dans un. Espace de noms enfant ViewModels.
-   Afficher les noms de modèle correspondent aux noms de vues et se terminent par « ViewModel ».

Enfin, le `OnAutoWireViewModelChanged` méthode indique le [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) du type de vue pour le type de modèle de vue résolu. Pour plus d’informations sur la résolution du type de modèle de vue, consultez [résolution](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#resolution).

Cette approche présente l’avantage qu’une application a une classe unique qui est responsable de l’instanciation des modèles de vue et de leur connexion à des vues.

> [!TIP]
> Utilisez un localisateur de modèle de vue pour faciliter la substitution. Un localisateur de modèle de vue peut également servir en tant que point de substitution pour les autres mises en œuvre des dépendances, comme pour les données de l’unité de test ou de la conception du temps.

## <a name="updating-views-in-response-to-changes-in-the-underlying-view-model-or-model"></a>Vues de la mise à jour en réponse aux modifications dans sous-jacent afficher le modèle ou modèle

Tous les modèle de vue et les classes de modèle qui sont accessibles à une vue doivent implémenter le `INotifyPropertyChanged` interface. Qui implémente cette interface dans un modèle de vue ou de la classe de modèle permet à la classe de fournir des notifications de modification à tous les contrôles liés aux données dans la vue lorsque la valeur de propriété sous-jacente change.

Applications doivent être mise en œuvre pour l’utilisation correcte de notification de modification de propriété, en respectant les exigences suivantes :

-   Toujours déclencher un `PropertyChanged` événement si la valeur d’une propriété publique change. Ne supposez pas que déclenchant le `PropertyChanged` événement peut être ignoré en raison de la base de connaissances de la liaison de XAML se produit.
-   Toujours déclencher un `PropertyChanged` événement pour tout calculé propriétés dont les valeurs sont utilisées par d’autres propriétés dans la vue de modèle ou modèle.
-   Toujours déclencher le `PropertyChanged` événement à la fin de la méthode qui prend une propriété modifier, ou lorsque l’objet est connu pour être dans un état stable. Déclenche l’événement interrompt l’opération en appelant les gestionnaires de l’événement de façon synchrone. Si cela se produit au milieu d’une opération, il peut exposer l’objet aux fonctions de rappel lorsqu’il est dans un état non sécurisé, partiellement mis à jour. En outre, il est possible que les modifications en cascade déclenchée par `PropertyChanged` événements. Modifications en cascade nécessitent généralement être terminées avant que la modification en cascade est sécurisée exécuter les mises à jour.
-   Déclenchement jamais un `PropertyChanged` événement si la propriété ne change pas. Cela signifie que vous devez comparer les valeurs anciennes et nouvelles avant de déclencher le `PropertyChanged` événement.
-   Jamais déclenchant le `PropertyChanged` événement pendant le constructeur d’un modèle de vue si vous initialisez une propriété. Les contrôles liés aux données dans la vue seront abonneront pas pour recevoir des notifications de modification à ce stade.
-   Jamais déclencher plusieurs `PropertyChanged` événement avec le même argument de nom de propriété dans un seul appel synchrone d’une méthode publique d’une classe. Par exemple, étant donné un `NumberOfItems` propriété dont le magasin de stockage est le `_numberOfItems` champ, si une méthode incrémente `_numberOfItems` cinquante fois pendant l’exécution d’une boucle, il doit uniquement déclencher notification de modification de propriété sur le `NumberOfItems` une seule fois, propriété une fois que tout le travail est terminé. Pour les méthodes asynchrones, déclencher le `PropertyChanged` événement pour un nom de la propriété donnée dans chaque segment synchrone d’une chaîne de continuation asynchrone.

L’application mobile d’eShopOnContainers utilise la `ExtendedBindableObject` classe pour fournir des notifications de changement, ce qui est indiqué dans l’exemple de code suivant :

```csharp
public abstract class ExtendedBindableObject : BindableObject  
{  
    public void RaisePropertyChanged<T>(Expression<Func<T>> property)  
    {  
        var name = GetMemberInfo(property).Name;  
        OnPropertyChanged(name);  
    }  

    private MemberInfo GetMemberInfo(Expression expression)  
    {  
        ...  
    }  
}
```

De Xamarin.Form [ `BindableObject` ](xref:Xamarin.Forms.BindableObject) la classe implémente le `INotifyPropertyChanged` interface et fournit un [ `OnPropertyChanged` ](xref:Xamarin.Forms.BindableObject.OnPropertyChanged(System.String)) (méthode). Le `ExtendedBindableObject` classe fournit le `RaisePropertyChanged` méthode à appeler la propriété notification de modification et ce faisant, utilise la fonctionnalité fournie par la `BindableObject` classe.

Chaque classe de modèle de vue dans l’application mobile eShopOnContainers dérive le `ViewModelBase` classe qui dérive à son tour la `ExtendedBindableObject` classe. Par conséquent, chaque classe de modèle de vue utilise le `RaisePropertyChanged` méthode dans la `ExtendedBindableObject` classe pour fournir une notification de modification de propriété. L’exemple de code suivant montre comment l’application mobile eShopOnContainers appelle la notification de modification de propriété à l’aide d’une expression lambda :

```csharp
public bool IsLogin  
{  
    get  
    {  
        return _isLogin;  
    }  
    set  
    {  
        _isLogin = value;  
        RaisePropertyChanged(() => IsLogin);  
    }  
}
```

Notez qu’à l’aide d’une expression lambda de cette façon implique légèrement des coûts, car l’expression lambda doit être évaluée pour chaque appel. Bien que le coût de performances est petit et n’affecte pas normalement d’une application, les coûts peuvent provisionner lorsqu’il existe que plusieurs notifications de modifications. Toutefois, l’avantage de cette approche est qu’elle fournit la sécurité de type de compilation et la refactorisation de prise en charge lorsque vous renommez des propriétés.

## <a name="ui-interaction-using-commands-and-behaviors"></a>Interaction de l’interface utilisateur à l’aide des commandes et des comportements

Dans les applications mobiles, les actions sont généralement appelées en réponse à une action de l’utilisateur, tels qu’un clic de bouton, ce qui peut être implémentée en créant un gestionnaire d’événements dans le fichier code-behind. Toutefois, dans le modèle MVVM, la responsabilité pour l’implémentation de l’action se situe dans le modèle de vue et code mis en place dans le code-behind doit être évitée.

Les commandes fournissent un moyen pratique pour représenter les actions qui peuvent être liées aux contrôles dans l’interface utilisateur. Ils encapsulent le code qui implémente l’action et vous aider à conserver il découplée à partir de sa représentation visuelle dans la vue. Xamarin.Forms inclut des contrôles qui peuvent être connectés de façon déclarative à une commande, et ces contrôles appelle la commande lorsque l’utilisateur interagit avec le contrôle.

Comportements permettent également de contrôles à être connecté de façon déclarative à une commande. Toutefois, les comportements peuvent être utilisés pour appeler une action qui est associé à une plage d’événements déclenchés par un contrôle. Par conséquent, comportements résolvent des scénarios de contrôles prenant en charge de commande, tout en offrant une plus grande flexibilité et de contrôle. En outre, les comportements peuvent également être utilisés pour associer des objets de commande ou de méthodes avec des contrôles qui n’ont pas été spécifiquement conçues pour interagir avec les commandes.

### <a name="implementing-commands"></a>Implémentation de commandes

En règle générale, les modèles de vue exposent les propriétés de commande, pour la liaison à partir de la vue, qui sont des instances d’objets qui implémentent le `ICommand` interface. Un nombre de contrôles Xamarin.Forms fournir un `Command` propriété, qui peut être des données lié à un `ICommand` objet fourni par le modèle de vue. Le `ICommand` interface définit un `Execute` (méthode), qui encapsule l’opération elle-même, un `CanExecute` (méthode), ce qui indique si la commande peut être appelée, ainsi qu’un `CanExecuteChanged` événement qui se produit lorsque des modifications qui affectent si la commande doit s’exécuter. Le [ `Command` ](xref:Xamarin.Forms.Command) et [ `Command<T>` ](xref:Xamarin.Forms.Command) fournies par Xamarin.Forms, les classes implémentent le `ICommand` interface, où `T` est le type des arguments de `Execute`et `CanExecute`.

Au sein d’un modèle de vue, il doit être un objet de type [ `Command` ](xref:Xamarin.Forms.Command) ou [ `Command<T>` ](xref:Xamarin.Forms.Command) pour chaque propriété publique dans le modèle de vue de type `ICommand`. Le `Command` ou `Command<T>` constructeur requiert une `Action` objet de rappel qui est appelée lorsque le `ICommand.Execute` méthode est appelée. Le `CanExecute` (méthode) est un paramètre de constructeur facultatif, qui est un `Func` qui retourne un `bool`.

Le code suivant montre comment un [ `Command` ](xref:Xamarin.Forms.Command) instance, ce qui représente une commande d’inscription, est construit en spécifiant un délégué à la `Register` afficher la méthode de modèle :

```csharp
public ICommand RegisterCommand => new Command(Register);
```

La commande est exposée à la vue via une propriété qui retourne une référence à un `ICommand`. Lorsque le `Execute` méthode est appelée sur le [ `Command` ](xref:Xamarin.Forms.Command) de l’objet, il transfère simplement l’appel à la méthode dans le modèle de vue via le délégué a été spécifié dans le `Command` constructeur.

Une méthode asynchrone peut être appelée par une commande à l’aide de la `async` et `await` mots clés lors de la spécification de la commande `Execute` déléguer. Cela indique que le rappel est un `Task` et doit être attendue. Par exemple, le code suivant montre comment un [ `Command` ](xref:Xamarin.Forms.Command) instance, ce qui représente une commande de connexion, est construit en spécifiant un délégué à la `SignInAsync` afficher la méthode de modèle :

```csharp
public ICommand SignInCommand => new Command(async () => await SignInAsync());
```

Paramètres peuvent être passés à la `Execute` et `CanExecute` actions à l’aide de la [ `Command<T>` ](xref:Xamarin.Forms.Command) classe à instancier la commande. Par exemple, le code suivant montre comment un `Command<T>` instance est utilisée pour indiquer que le `NavigateAsync` méthode nécessite un argument de type `string`:

```csharp
public ICommand NavigateCommand => new Command<string>(NavigateAsync);
```

À la fois dans le [ `Command` ](xref:Xamarin.Forms.Command) et [ `Command<T>` ](xref:Xamarin.Forms.Command) des classes, le délégué à la `CanExecute` dans chaque constructeur est facultative. Si un délégué n’est pas spécifié, le `Command` retournera `true` pour `CanExecute`. Toutefois, le modèle de vue peut indiquer une modification dans la commande `CanExecute` état en appelant le `ChangeCanExecute` méthode sur le `Command` objet. Cela entraîne le `CanExecuteChanged` déclenchement d’événement. Tous les contrôles dans l’interface utilisateur qui sont liés à la commande met ensuite à jour leur état est activée afin de refléter la disponibilité de la commande lié aux données.

#### <a name="invoking-commands-from-a-view"></a>Appel des commandes à partir d’une vue

Le code suivant montre l’exemple comment un [ `Grid` ](xref:Xamarin.Forms.Grid) dans le `LoginView` lie à la `RegisterCommand` dans le `LoginViewModel` classe à l’aide un [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) instance :

```xaml
<Grid Grid.Column="1" HorizontalOptions="Center">  
    <Label Text="REGISTER" TextColor="Gray"/>  
    <Grid.GestureRecognizers>  
        <TapGestureRecognizer Command="{Binding RegisterCommand}" NumberOfTapsRequired="1" />  
    </Grid.GestureRecognizers>  
</Grid>
```

Un paramètre de commande peut également être éventuellement défini à l’aide de la [ `CommandParameter` ](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter) propriété. Le type de l’argument attendu est spécifié dans le `Execute` et `CanExecute` cibler des méthodes. Le [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) invoquera automatiquement la commande cible lorsque l’utilisateur interagit avec le contrôle attaché. Le paramètre de commande, si fourni, sera être passé comme argument à la commande `Execute` déléguer.

<a name="implementing_behaviors" />

### <a name="implementing-behaviors"></a>Implémenter des comportements

Comportements permettent de fonctionnalités à ajouter à des contrôles d’interface utilisateur sans devoir sous-classe les. Au lieu de cela, la fonctionnalité est implémentée dans une classe de comportement et rattachée au contrôle comme s’il faisait partie du contrôle lui-même. Comportements permettent d’implémenter le code que vous devriez normalement écrire sous forme de code-behind, car il interagit directement avec l’API du contrôle, de sorte qu’il peut être plus concise liée au contrôle et empaquetée pour une réutilisation sur plus d’une vue ou une application. Dans le contexte du modèle MVVM, les comportements sont une approche utile pour la connexion des contrôles à des commandes.

Un comportement est attaché à un contrôle via les propriétés jointes est appelé un *attaché comportement*. Le comportement pouvez ensuite utiliser l’API exposée de l’élément auquel il est connecté pour ajouter des fonctionnalités à ce contrôle, ou d’autres contrôles, dans l’arborescence visuelle de la vue. L’application mobile eShopOnContainers contient le `LineColorBehavior` (classe), qui est un comportement attaché. Pour plus d’informations sur ce comportement, consultez [affichage des erreurs de Validation](~/xamarin-forms/enterprise-application-patterns/validation.md#displaying_validation_errors).

Un comportement de Xamarin.Forms est une classe qui dérive de la [ `Behavior` ](xref:Xamarin.Forms.Behavior) ou [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) (classe), où `T `est le type du contrôle auquel le comportement doit s’appliquer. Ces classes fournissent `OnAttachedTo` et `OnDetachingFrom` méthodes, qui doivent être substituées pour fournir une logique qui sera exécutée lorsque le comportement est attaché au et détaché à partir de contrôles.

Dans l’application mobile eShopOnContainers, le `BindableBehavior<T>` classe dérive de la [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) classe. L’objectif de la `BindableBehavior<T>` classe est de fournir une classe de base pour les comportements de Xamarin.Forms qui nécessitent le [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) du comportement à définir le contrôle attaché.

Le `BindableBehavior<T>` classe fournit un substituable `OnAttachedTo` méthode qui définit la [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) du comportement et un substituable `OnDetachingFrom` méthode nettoie les `BindingContext`. En outre, la classe stocke une référence au contrôle attaché dans le `AssociatedObject` propriété.

L’application mobile eShopOnContainers inclut un `EventToCommandBehavior` (classe), qui exécute une commande en réponse à un événement qui se produisent. Cette classe est dérivée de la `BindableBehavior<T>` classe afin que le comportement peut être liés à et exécuter un `ICommand` spécifié par un `Command` propriété lorsque le comportement est consommé. L’exemple de code suivant illustre la classe `EventToCommandBehavior` :

```csharp
public class EventToCommandBehavior : BindableBehavior<View>  
{  
    ...  
    protected override void OnAttachedTo(View visualElement)  
    {  
        base.OnAttachedTo(visualElement);  

        var events = AssociatedObject.GetType().GetRuntimeEvents().ToArray();  
        if (events.Any())  
        {  
            _eventInfo = events.FirstOrDefault(e => e.Name == EventName);  
            if (_eventInfo == null)  
                throw new ArgumentException(string.Format(  
                        "EventToCommand: Can't find any event named '{0}' on attached type",   
                        EventName));  

            AddEventHandler(_eventInfo, AssociatedObject, OnFired);  
        }  
    }  

    protected override void OnDetachingFrom(View view)  
    {  
        if (_handler != null)  
            _eventInfo.RemoveEventHandler(AssociatedObject, _handler);  

        base.OnDetachingFrom(view);  
    }  

    private void AddEventHandler(  
            EventInfo eventInfo, object item, Action<object, EventArgs> action)  
    {  
        ...  
    }  

    private void OnFired(object sender, EventArgs eventArgs)  
    {  
        ...  
    }  
}
```

Le `OnAttachedTo` et `OnDetachingFrom` méthodes sont utilisées pour vous inscrire et annuler l’inscription d’un gestionnaire d’événements pour l’événement défini dans le `EventName` propriété. Ensuite, lorsque l’événement se déclenche, le `OnFired` méthode est appelée, qui exécute la commande.

L’avantage d’utiliser le `EventToCommandBehavior` pour exécuter une commande lorsqu’un événement se déclenche, est que les commandes peuvent être associés à des contrôles qui n’ont pas été conçus pour interagir avec les commandes. En outre, cette opération déplace code de gestion des événements pour afficher les modèles, où elle peut être le test unitaire.

#### <a name="invoking-behaviors-from-a-view"></a>Appel de comportements à partir d’une vue

Le `EventToCommandBehavior` est particulièrement utile pour l’attachement d’une commande à un contrôle qui ne prend pas en charge les commandes. Par exemple, le `ProfileView` utilise le `EventToCommandBehavior` pour exécuter le `OrderDetailCommand` lorsque le [ `ItemTapped` ](xref:Xamarin.Forms.ListView.ItemTapped) événement est déclenché sur le [ `ListView` ](xref:Xamarin.Forms.ListView) qui répertorie les commandes de l’utilisateur, comme indiqué dans le code suivant :

```xaml
<ListView>  
    <ListView.Behaviors>  
        <behaviors:EventToCommandBehavior             
            EventName="ItemTapped"  
            Command="{Binding OrderDetailCommand}"  
            EventArgsConverter="{StaticResource ItemTappedEventArgsConverter}" />  
    </ListView.Behaviors>  
    ...  
</ListView>
```

Lors de l’exécution, le `EventToCommandBehavior` répondra à l’interaction avec le [ `ListView` ](xref:Xamarin.Forms.ListView). Lorsqu’un élément est sélectionné dans le `ListView`, le [ `ItemTapped` ](xref:Xamarin.Forms.ListView.ItemTapped) événement se déclenche, qui s’exécute le `OrderDetailCommand` dans le `ProfileViewModel`. Par défaut, les arguments d’événement pour l’événement sont passés à la commande. Ces données sont converties en tant qu’elle est transmise entre source et cible par le convertisseur spécifié dans le `EventArgsConverter` propriété, qui retourne le [ `Item` ](xref:Xamarin.Forms.ItemTappedEventArgs.Item) de la `ListView` à partir de la [ `ItemTappedEventArgs` ](xref:Xamarin.Forms.ItemTappedEventArgs). Par conséquent, lorsque le `OrderDetailCommand` est exécutée, le texte sélectionné `Order` est passé en tant que paramètre à l’Action inscrite.

Pour plus d’informations sur les comportements, consultez [comportements](~/xamarin-forms/app-fundamentals/behaviors/index.md).

## <a name="summary"></a>Récapitulatif

Le modèle Model-View-ViewModel (MVVM) permet de séparer clairement la logique métier et de présentation d’une application à partir de son interface utilisateur (IU). Maintenir une séparation nette entre la logique d’application et l’interface utilisateur vous aide à résoudre de nombreux problèmes liés au développement et peut rendre plus faciles à tester, gérer et faire évoluer une application. Il peut améliorer considérablement les opportunités de réutilisation de code et permet aux développeurs et concepteurs d’interfaces utilisateur plus collaborent aisément lors du développement de leurs éléments respectifs d’une application.

À l’aide de la MVVM de modèle, l’interface utilisateur de l’application et la logique de présentation et métier sous-jacente est divisée en trois classes distinctes : la vue, qui encapsule l’interface utilisateur et l’interface utilisateur logique ; le modèle de vue, qui encapsule la logique de présentation et d’état ; et le modèle, qui encapsule la logique métier et les données de l’application.


## <a name="related-links"></a>Liens associés

- [Téléchargez le livre électronique (PDF de 2 Mo)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (exemple)](https://github.com/dotnet-architecture/eShopOnContainers)
