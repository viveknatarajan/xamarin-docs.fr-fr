---
title: MVVM
ms.prod: xamarin
ms.assetid: dd8c1813-df44-4947-bcee-1a1ff2334b87
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 32a7a7dd50edcc3eefe76429ddb1e5581447993e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="mvvm"></a>MVVM

L’expérience du développeur Xamarin.Forms implique généralement la création d’une interface utilisateur en XAML et ajout de code-behind qui fonctionne sur l’interface utilisateur. Les applications sont modifiées, et augmente la taille et la portée, des problèmes complexes de maintenance peuvent se produire. Ces problèmes incluent le couplage étroit entre les contrôles d’interface utilisateur et la logique métier, ce qui augmente le coût d’effectuer les modifications de l’interface utilisateur et la difficulté de ce code de test unitaire.

Le modèle Model-View-ViewModel (MVVM) permet de séparer clairement la logique métier et de présentation d’une application à partir de son interface utilisateur (IU). Maintenir une séparation nette entre la logique d’application et l’interface utilisateur permettant de résoudre de nombreux problèmes de développement et peut rendre plus faciles à tester, gérer et faire évoluer une application. Elle peut améliorer considérablement les possibilités de réutilisation de code et permet aux développeurs et concepteurs de l’interface utilisateur à effectuer davantage de collaborent aisément lors du développement de leurs éléments respectifs d’une application.

## <a name="the-mvvm-pattern"></a>Le modèle MVVM

Il existe trois composants principaux dans le modèle MVVM : le modèle, la vue et le modèle d’affichage. Chacune a un objectif de distinct. Figure 2-1 montre les relations entre les trois composants.

![](mvvm-images/mvvm.png "Le modèle MVVM")

**Figure 2-1**: modèle MVVM le

Outre les responsabilités de chaque composant, il est également important de comprendre la façon dont ils interagissent entre eux. À un niveau élevé, la vue « connaît » le modèle d’affichage, le modèle d’affichage « connaît » du modèle, mais le modèle n’a pas connaissance du modèle de vue et le modèle d’affichage n’a pas connaissance de la vue. Par conséquent, le modèle d’affichage isole la vue du modèle et permet au modèle d’évoluer indépendamment de la vue.

Les avantages de l’utilisation du modèle MVVM sont les suivantes :

-   S’il existe une implémentation d’un modèle existant qui encapsule la logique métier existante, il peut être difficile ou présentant un risque pour la modifier. Dans ce scénario, le modèle agit comme un adaptateur pour les classes du modèle et vous permet d’éviter d’effectuer aucune modification importante pour le code du modèle.
-   Développeurs peuvent créer des tests unitaires pour le modèle d’affichage et le modèle, sans l’aide de la vue. Les tests unitaires pour le modèle d’affichage peuvent exercer exactement les mêmes fonctionnalités que celles utilisées par la vue.
-   L’interface utilisateur de l’application peut être modifiée sans toucher au code, pour autant que la vue est entièrement implémentée dans XAML. Par conséquent, une nouvelle version de la vue doit fonctionner avec le modèle existant.
-   Les concepteurs et développeurs peuvent travailler indépendamment et simultanément sur leurs composants pendant le processus de développement. Concepteurs peuvent se concentrer sur la vue, alors que les développeurs peuvent travailler sur le modèle d’affichage et les composants de modèle.

La clé pour une utilisation plus efficace de MVVM se trouve à comprendre la factorisation du code d’application dans les classes correctes et comprendre l’interagissent entre les classes. Les sections suivantes décrivent les responsabilités de chacune des classes dans le modèle MVVM.

### <a name="view"></a>Vue

La vue est chargée de définir la structure, la disposition et l’apparence de ce que l’utilisateur voit à l’écran. Dans l’idéal, chaque vue est définie en XAML, un code-behind limité qui ne contient pas de logique métier. Toutefois, dans certains cas, le code-behind peut contenir la logique de l’interface utilisateur qui implémente le comportement visuel qui est difficile à exprimer dans XAML, tels que les animations.

Dans une application de Xamarin.Forms, une vue est généralement un [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)-dérivée ou [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/)-classe dérivée. Toutefois, les vues peuvent également être représentées par un modèle de données, qui spécifie les éléments d’interface utilisateur à utiliser pour représenter visuellement un objet lorsqu’il est affiché. Un modèle de données en tant que vue n’a pas de n’importe quel code-behind et est conçu pour lier à un type de modèle de vue spécifiques.

> [!TIP]
> Évitez d’activer et désactiver les éléments d’interface utilisateur dans le code-behind. Assurez-vous qu’afficher les modèles sont responsables de la définition des changements d’état logique qui affectent certains aspects de l’affichage de la vue, telles que si une commande est disponible, ou une indication qu’une opération est en attente. Par conséquent, activer et désactiver les éléments d’interface utilisateur en le liant à afficher les propriétés du modèle, plutôt que de l’activation et de les désactiver dans le code-behind.

Il existe plusieurs options pour l’exécution de code sur le modèle d’affichage en réponse à des interactions sur la vue, tel qu’un clic de bouton ou de la sélection d’éléments. Si un contrôle prend en charge les commandes, le contrôle `Command` propriété permettre être liés aux données à un `ICommand` propriété sur le modèle d’affichage. Lorsque la commande du contrôle est appelée, le code dans le modèle d’affichage sera exécuté. En plus de commandes, les comportements peuvent être attachés à un objet dans la vue et peuvent écouter une commande à appeler ou d’un événement soit déclenché. En réponse, le comportement peut ensuite appeler une `ICommand` sur le modèle d’affichage ou d’une méthode sur le modèle d’affichage.

### <a name="viewmodel"></a>ViewModel

Le modèle d’affichage implémente à laquelle la vue peut lier les données à des commandes et les propriétés et notifie l’affichage de toutes les modifications d’état via les événements de notification de modification. Les propriétés et les commandes que le modèle d’affichage fournit à définir les fonctionnalités qui seront offerts par l’interface utilisateur, mais la vue détermine comment cette fonctionnalité doit être affichée.

> [!TIP]
> Maintenir la réactivité des opérations asynchrones de l’interface utilisateur. Les applications mobiles doivent conserver le thread d’interface utilisateur débloqué pour améliorer la perception de l’utilisateur de performances. Par conséquent, dans le modèle d’affichage, utilisez les méthodes asynchrones pour les opérations d’e/s et déclencher des événements pour informer les vues des modifications apportées aux propriétés de façon asynchrone.

Le modèle d’affichage est également responsable de la coordination des interactions de la vue avec toutes les classes de modèle qui sont requis. Il y a généralement une relation un-à-plusieurs entre le modèle d’affichage et les classes du modèle. Le modèle d’affichage peut choisir d’exposer des classes de modèle directement à la vue afin que les contrôles de la vue peuvent lier les données directement. Dans ce cas, les classes du modèle devrez être conçues pour prendre en charge la liaison de données et les événements de notification de modification.

Chaque modèle d’affichage fournit des données à partir d’un modèle dans un formulaire de la vue peut facilement consommer. Pour ce faire, le modèle d’affichage effectue parfois la conversion de données. Placer cette conversion de données dans le modèle d’affichage d’est une bonne idée, car elle fournit des propriétés qui lient à la vue. Par exemple, le modèle d’affichage peut combiner les valeurs des deux propriétés pour le rendre plus facile pour l’affichage par la vue.

> [!TIP]
> Centraliser les conversions de données dans une couche de conversion. Il est également possible d’utiliser des convertisseurs comme une couche de conversion de données distinct qui se situe entre le modèle d’affichage et l’affichage. Cela peut être nécessaire, par exemple, lorsque les données requièrent une mise en forme spéciale qui ne fournit pas le modèle d’affichage.

Dans l’ordre pour le modèle d’affichage devant être inclus dans la liaison de données bidirectionnelle avec la vue, ses propriétés doivent déclencher le `PropertyChanged` événement. Afficher les modèles de satisfont cette exigence en implémentant la `INotifyPropertyChanged` de l’interface et de déclencher la `PropertyChanged` événement lorsqu’une propriété est modifiée.

Pour les collections, la vue conviviale `ObservableCollection<T>` est fourni. Cette collection implémente la notification de la collection a été modifiée, ce qui décharge le développeur d’implémenter la `INotifyCollectionChanged` interface sur des collections.

### <a name="model"></a>Modèle

Classes de modèle sont des classes non visuels qui encapsulent les données d’application. Par conséquent, le modèle peut être considéré comme représentant le modèle de domaine de l’application, qui inclut généralement un modèle de données, ainsi que la logique métier et de validation. Objets de transfert de données (DTO), les Plain Old CLR Objects (POCOs) et les entités générées et les objets proxy sont des exemples d’objets de modèle.

Classes de modèle sont généralement utilisés conjointement avec les référentiels qui encapsulent l’accès aux données et la mise en cache ou des services.

## <a name="connecting-view-models-to-views"></a>Afficher les modèles qui se connecte à des vues

Afficher les modèles peuvent être connectés aux vues en utilisant les fonctionnalités de liaison de données de Xamarin.Forms. Il existe de nombreuses approches qui peuvent être utilisés pour construire des vues et afficher des modèles et les associer à l’exécution. Ces approches entrent dans deux catégories, appelés composition de la première vue et composition premier modèle de vue. Choisir entre la composition de la première vue et la vue de composition de premier modèle est un problème de préférence et de complexité. Toutefois, certaines approches partagent le même objectif, ce qui concerne l’affichage de disposer d’un modèle d’affichage affecté à sa propriété BindingContext.

Avec l’affichage de première composition de l’application est sur le plan conceptuel composée de vues qui se connectent à afficher les modèles que dont elles dépendent. Le principal avantage de cette approche est qu’il rend facile la construction faiblement couplée, applications testable d’unité, car les modèles de vue n’ont aucune dépendance sur la vue à l’autre. Il est également facile à comprendre la structure de l’application en suivant sa structure visual, plutôt que d’avoir à effectuer le suivi de l’exécution du code pour comprendre comment les classes sont créés et associés. En outre, la construction de première vue aligne avec le système de navigation Xamarin.Forms qui est responsable de la construction des pages en cas de navigation, ce qui rend une composition de la première vue modèle complexe et non alignés avec la plateforme.

Avec l’affichage de composition de premier modèle l’application est sur le plan conceptuel composée d’afficher les modèles, avec un service qui est chargé de rechercher la vue pour un modèle d’affichage. Composition de première vue modèle sensation plus naturelle certains développeurs, étant donné que la création de la vue peut être abstrait, leur permettant de se concentrer sur la structure logique pas une interface utilisateur de l’application. En outre, il permet d’afficher les modèles doit être créé par d’autres modèles d’affichage. Toutefois, cette approche est souvent complexe et il peut s’avérer difficile à comprendre comment les différentes parties de l’application sont créés et associés.

> [!TIP]
> Afficher les modèles et vues conserve indépendamment. La liaison des vues à une propriété dans une source de données doit être dépendance principal de la vue de son modèle d’affichage correspondant. En particulier, ne référencez des types d’affichages, tels que [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) et [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/), à partir de modèles d’affichage. En suivant les principes décrites ici, afficher les modèles peuvent être testées de manière isolée, ce qui réduit la probabilité d’erreurs logicielles en limitant la portée.

Les sections suivantes décrivent les principales approches à la connexion d’afficher les modèles à des vues.

### <a name="creating-a-view-model-declaratively"></a>Créez un modèle d’affichage de façon déclarative

L’approche la plus simple est pour la vue de façon déclarative instancier son modèle d’affichage correspondant en XAML. Lorsque la vue est créée, l’objet de modèle de vue correspondant sera également construite. Cette approche est illustrée dans l’exemple de code suivant :

```xaml
<ContentPage ... xmlns:local="clr-namespace:eShop">  
    <ContentPage.BindingContext>  
        <local:LoginViewModel />  
    </ContentPage.BindingContext>  
    ...  
</ContentPage>
```

Lorsque le [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) est créé, une instance de la `LoginViewModel` est automatiquement construite et définir en tant que la vue [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/).

Cette construction déclarative et l’affectation du modèle d’affichage par la vue présente l’avantage qu’il est simple, mais présente l’inconvénient qu’elle requiert le constructeur par défaut (sans paramètre) dans le modèle d’affichage.

### <a name="creating-a-view-model-programmatically"></a>Création d’un modèle d’affichage par programme

Une vue peut avoir un code dans le fichier code-behind qui résulte dans le modèle d’affichage qui sont assigné à son [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) propriété. Cela est souvent accompli dans le constructeur de la vue, comme indiqué dans l’exemple de code suivant :

```csharp
public LoginView()  
{  
    InitializeComponent();  
    BindingContext = new LoginViewModel(navigationService);  
}
```

La construction de programmation et l’affectation du modèle de vue dans le code-behind de la vue présente l’avantage qu’il est simple. Toutefois, le principal inconvénient de cette approche est que la vue doit fournir le modèle d’affichage de toutes les dépendances requises. À l’aide d’un conteneur d’injection de dépendance peut aider à maintenir le faible couplage entre la vue et le modèle d’affichage. Pour plus d’informations, consultez [Injection de dépendance](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md).

### <a name="creating-a-view-defined-as-a-data-template"></a>Création d’un affichage défini comme un modèle de données

Une vue peut être définie comme un modèle de données et associée à un type de modèle de vue. Modèles de données peuvent être définis en tant que ressources, ou ils peuvent être définies inline dans le contrôle qui affiche le modèle d’affichage. Le contenu du contrôle est l’instance de modèle de vue et le modèle de données est utilisé pour représenter visuellement. Cette technique est un exemple d’une situation dans laquelle le modèle est instancié en premier lieu, suivie de la création de la vue.

<a name="automatically_creating_a_view_model_with_a_view_model_locator" />

### <a name="automatically-creating-a-view-model-with-a-view-model-locator"></a>Création automatique d’un modèle d’affichage avec un localisateur de modèle de vue

Un localisateur de modèle de vue est une classe personnalisée qui gère l’instanciation d’afficher les modèles et leur association à des vues. Dans l’application mobile eShopOnContainers, le `ViewModelLocator` classe a une propriété jointe, `AutoWireViewModel`, qui est utilisé pour associer d’afficher les modèles à des vues. Dans XAML la vue, cette propriété jointe est définie sur true pour indiquer que le modèle d’affichage doit être connecté automatiquement à la vue, comme indiqué dans l’exemple de code suivant :

```xaml
viewModelBase:ViewModelLocator.AutoWireViewModel="true"
```

Le `AutoWireViewModel` est une propriété pouvant être liée qui est initialisée à la valeur false, et lorsque sa valeur change la `OnAutoWireViewModelChanged` Gestionnaire d’événements est appelé. Cette méthode résout le modèle d’affichage de la vue. L’exemple de code suivant montre comment cela :

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

Le `OnAutoWireViewModelChanged` méthode tente de résoudre le modèle d’affichage à l’aide d’une approche basée sur une convention. Cette convention suppose que :

-   Afficher les modèles sont dans le même assembly en tant que types d’affichage.
-   Les vues sont dans un. Espace de noms des vues enfants.
-   Afficher les modèles sont dans un. Espace de noms enfant ViewModel.
-   Afficher les noms de modèle correspondant aux noms de vues et se terminent par « View ».

Enfin, le `OnAutoWireViewModelChanged` méthode définit la [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) du type d’affichage pour le type de modèle de vue résolu. Pour plus d’informations sur la résolution du type de modèle de vue, consultez [résolution](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#resolution).

Cette approche présente l’avantage qu’une application possède une seule classe qui est responsable de l’instanciation d’afficher les modèles et leur connexion aux vues.

> [!TIP]
> Utilisez un localisateur de modèle de vue pour faciliter la substitution. Un localisateur de modèle de vue permet également en tant que point de substitution d’autres implémentations de dépendances, telles que des données de l’unité de test ou de la conception du temps.

## <a name="updating-views-in-response-to-changes-in-the-underlying-view-model-or-model"></a>Mise à jour des vues en réponse aux modifications dans sous-jacent afficher le modèle ou modèle

Tous les afficher le modèle et les classes de modèle qui sont accessibles à une vue doivent implémenter le `INotifyPropertyChanged` interface. Implémentation de cette interface dans une classe de modèle ou un modèle d’affichage permet à la classe fournir des notifications de modification à tous les contrôles liés aux données dans la vue lorsque la valeur de propriété sous-jacente change.

L’application doit être mise en œuvre pour l’utilisation correcte de la notification de modification de propriété, en respectant les conditions suivantes :

-   Toujours le déclenchement d’un `PropertyChanged` événement si la valeur d’une propriété publique change. Ne supposez pas que le déclenchement du `PropertyChanged` événement peut être ignoré en raison de la base de connaissances de la liaison XAML se produit.
-   Toujours le déclenchement d’un `PropertyChanged` événement pour tout calculé propriétés dont les valeurs sont utilisées par d’autres propriétés dans la vue de modèle ou modèle.
-   Toujours déclencher la `PropertyChanged` événement à la fin de la méthode qui rend une propriété de modification, ou lorsque l’objet est connu pour être dans un état stable. Déclenche l’événement d’interrompt l’opération à appeler les gestionnaires de l’événement de façon synchrone. Si cela se produit au milieu d’une opération, il peut exposer l’objet à des fonctions de rappel lorsqu’il est dans un état non sécurisé, partiellement mise à jour. En outre, il est possible que les modifications en cascade doit être déclenchée par `PropertyChanged` événements. Modifications en cascade nécessitent généralement des mises à jour pour être terminée avant que la modification en cascade peut exécuter.
-   Jamais le déclenchement d’un `PropertyChanged` événement si la propriété ne change pas. Cela signifie que vous devez comparer les valeurs anciennes et nouvelles avant le déclenchement de le `PropertyChanged` événement.
-   Déclenchement jamais le `PropertyChanged` événement pendant le constructeur d’un modèle de vue si vous initialisez une propriété. Contrôles liés aux données dans la vue ne seront pas êtes abonné pour recevoir des notifications de modifications à ce stade.
-   Le déclenchement de jamais plusieurs `PropertyChanged` événement avec le même argument de nom de propriété dans un seul appel synchrone d’une méthode publique d’une classe. Par exemple, étant donné un `NumberOfItems` propriété dont le magasin de stockage est la `_numberOfItems` champ, si une méthode incrémente `_numberOfItems` 50 fois pendant l’exécution d’une boucle, elle doit uniquement déclencher notification de modification de propriété sur le `NumberOfItems` propriété qu’une seule fois, une fois que tout le travail est terminé. Pour les méthodes asynchrones, déclencher le `PropertyChanged` événement pour un nom de propriété donnée dans chaque segment synchrone d’une chaîne de continuation asynchrone.

Les utilisations d’application mobile eShopOnContainers la `ExtendedBindableObject` classe pour fournir des notifications de modifications, ce qui est indiqué dans l’exemple de code suivant :

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

De Xamarin.Form [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/) la classe implémente le `INotifyPropertyChanged` de l’interface et fournit un [ `OnPropertyChanged` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.OnPropertyChanged/p/System.String/) (méthode). Le `ExtendedBindableObject` classe fournit le `RaisePropertyChanged` méthode à appeler la propriété notification de modification et cela utilise la fonctionnalité fournie par la `BindableObject` classe.

Chaque classe de modèle de vue de l’application mobile eShopOnContainers dérive le `ViewModelBase` (classe), qui à son tour dérive la `ExtendedBindableObject` classe. Par conséquent, chaque classe de modèle d’affichage utilise le `RaisePropertyChanged` méthode dans la `ExtendedBindableObject` classe pour fournir une notification de modification de propriété. L’exemple de code suivant montre comment l’application mobile eShopOnContainers appelle la notification de modification de propriété à l’aide d’une expression lambda :

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

Notez qu’à l’aide d’une expression lambda de cette façon implique un coût, car l’expression lambda doit être évaluée pour chaque appel de performances. Bien que la baisse de performances est petite et n’aurait pas normalement un impact sur une application, les coûts peuvent accumuler lorsqu’il existe que plusieurs notifications de modification. Toutefois, l’avantage de cette approche est qu’il assure la sécurité de type de compilation et la refactorisation de prise en charge lorsque vous renommez des propriétés.

## <a name="ui-interaction-using-commands-and-behaviors"></a>Interaction de l’interface utilisateur à l’aide de commandes et des comportements

Dans les applications mobiles, les actions sont généralement appelées en réponse à une action de l’utilisateur, par exemple un clic de bouton, qui peut être implémentée en créant un gestionnaire d’événements dans le fichier code-behind. Toutefois, dans le modèle MVVM, l’implémentation de l’action incombe avec le modèle d’affichage et code mise dans le code-behind doit être évitée.

Les commandes fournissent un moyen pratique pour représenter les actions qui peuvent être liées à des contrôles dans l’interface utilisateur. Ils encapsulent le code qui implémente l’action et aident pour qu’il soit découplées à partir de sa représentation visuelle de la vue. Xamarin.Forms comprend les contrôles qui peuvent être connectés de façon déclarative à une commande, et ces contrôles appelle la commande lorsque l’utilisateur interagit avec le contrôle.

Comportements permettent également de contrôles de façon déclarative connecté à une commande. Toutefois, les comportements peuvent être utilisés pour appeler une action qui est associé à une plage d’événements déclenchés par un contrôle. Par conséquent, comportements traitent une bonne partie des scénarios de mêmes que les contrôles de commande activée, tout en offrant une plus grande flexibilité et de contrôle. En outre, les comportements peuvent également être utilisés pour associer des objets de commande ou les méthodes avec des contrôles qui n’ont pas été spécifiquement conçus pour interagir avec les commandes.

### <a name="implementing-commands"></a>Implémentation des commandes

Afficher les modèles exposent les propriétés de commande, pour la liaison à partir de la vue, qui sont des instances d’objets qui implémentent la `ICommand` interface. Un nombre de contrôles de Xamarin.Forms fournit un `Command` propriété, qui peut être des données est lié à un `ICommand` objet fourni par le modèle d’affichage. Le `ICommand` interface définit un `Execute` (méthode), qui encapsule l’opération proprement dit, un `CanExecute` (méthode), qui indique si la commande peut être appelée et une `CanExecuteChanged` événement qui se produit lorsque des modifications qui affectent si la commande doit s’exécuter. Le [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) et [ `Command<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) fournies par Xamarin.Forms, les classes implémentent la `ICommand` interface, où `T` est le type des arguments de `Execute`et `CanExecute`.

Dans un modèle de vue, il doit y avoir un objet de type [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) ou [ `Command<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) pour chaque propriété publique dans le modèle d’affichage de type `ICommand`. Le `Command` ou `Command<T>` constructeur requiert une `Action` objet de rappel qui est appelée lorsque le `ICommand.Execute` méthode est appelée. Le `CanExecute` méthode est un paramètre de constructeur facultatif et est un `Func` qui retourne un `bool`.

Le code suivant montre comment un [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) instance, ce qui représente une commande register, est construit en spécifiant un délégué à le `Register` afficher la méthode de modèle :

```csharp
public ICommand RegisterCommand => new Command(Register);
```

La commande est exposée à la vue via une propriété qui retourne une référence à un `ICommand`. Lorsque le `Execute` méthode est appelée sur le [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) de l’objet, il transfère simplement l’appel à la méthode dans le modèle d’affichage via le délégué a été spécifié dans le `Command` constructeur.

Une méthode asynchrone peut être appelée par une commande à l’aide de la `async` et `await` mots clés lors de la spécification de la commande `Execute` déléguer. Cela indique que le rappel est un `Task` et doit être attendue. Par exemple, le code suivant montre comment un [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) instance, ce qui représente une commande de connexion, qui est construit en spécifiant un délégué à le `SignInAsync` afficher la méthode de modèle :

```csharp
public ICommand SignInCommand => new Command(async () => await SignInAsync());
```

Paramètres peuvent être passés à la `Execute` et `CanExecute` actions à l’aide de la [ `Command<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) classe à instancier la commande. Par exemple, le code suivant montre comment un `Command<T>` instance est utilisée pour indiquer que le `NavigateAsync` méthode nécessite un argument de type `string`:

```csharp
public ICommand NavigateCommand => new Command<string>(NavigateAsync);
```

À la fois dans le [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) et [ `Command<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) des classes, le délégué à la `CanExecute` méthode dans chaque constructeur est facultative. Si un délégué n’est pas spécifié, le `Command` retournera `true` pour `CanExecute`. Toutefois, le modèle d’affichage permettre indiquer un changement de la commande `CanExecute` état en appelant le `ChangeCanExecute` méthode sur le `Command` objet. Cela entraîne le `CanExecuteChanged` événement soit déclenché. Tous les contrôles dans l’interface utilisateur qui sont liés à la commande seront puis mettre à jour leur état activé afin de refléter la disponibilité de la commande lié aux données.

#### <a name="invoking-commands-from-a-view"></a>Appel des commandes à partir d’une vue

Le code suivant montre l’exemple comment un [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/) dans le `LoginView` lie à la `RegisterCommand` dans les `LoginViewModel` classe à l’aide un [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/) instance :

```xaml
<Grid Grid.Column="1" HorizontalOptions="Center">  
    <Label Text="REGISTER" TextColor="Gray"/>  
    <Grid.GestureRecognizers>  
        <TapGestureRecognizer Command="{Binding RegisterCommand}" NumberOfTapsRequired="1" />  
    </Grid.GestureRecognizers>  
</Grid>
```

Un paramètre de commande peut également être éventuellement défini à l’aide de la [ `CommandParameter` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TapGestureRecognizer.CommandParameter/) propriété. Le type de l’argument attendu est spécifié dans le `Execute` et `CanExecute` cibler des méthodes. Le [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/) appelle automatiquement la commande de cible lorsque l’utilisateur interagit avec le contrôle attaché. Le paramètre de commande fourni, sera être passé comme argument à la commande `Execute` déléguer.

<a name="implementing_behaviors" />

### <a name="implementing-behaviors"></a>Implémentation de comportements

Comportements permettent de fonctionnalités à ajouter à des contrôles d’interface utilisateur sans devoir sous-classe les. Au lieu de cela, la fonctionnalité est implémentée dans une classe de comportement et liée au contrôle comme s’il faisait partie du contrôle lui-même. Comportements permettent d’implémenter le code qui serait normalement avoir à écrire en tant que code-behind, car il interagit directement avec l’API de contrôle, de sorte qu’il peut être concise liée au contrôle et empaqueté pour une réutilisation sur plus d’une vue ou une application. Dans le contexte de MVVM, les comportements sont une approche très utile pour la connexion des contrôles à des commandes.

Un comportement qui est attaché à un contrôle via les propriétés jointes est appelé un *jointe comportement*. Le comportement peut ensuite utiliser l’API exposée de l’élément auquel il est attaché pour ajouter des fonctionnalités à ce contrôle, ou d’autres contrôles dans l’arborescence d’éléments visuels de la vue. L’application mobile eShopOnContainers contient le `LineColorBehavior` (classe), qui est un comportement attaché. Pour plus d’informations sur ce comportement, consultez [affichage des erreurs de Validation](~/xamarin-forms/enterprise-application-patterns/validation.md#displaying_validation_errors).

Un comportement de Xamarin.Forms est une classe qui dérive de la [ `Behavior` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/) ou [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) (classe), où `T `est le type du contrôle auquel le comportement doit s’appliquer. Ces classes fournissent `OnAttachedTo` et `OnDetachingFrom` méthodes, qui doivent être substituées pour fournir la logique qui sera exécutée lorsque le comportement est attaché à et détaché de contrôles.

Dans l’application mobile eShopOnContainers, le `BindableBehavior<T>` classe dérive de la [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) classe. L’objectif de la `BindableBehavior<T>` classe consiste à fournir une classe de base pour les comportements de Xamarin.Forms qui nécessitent le [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) du comportement à définir le contrôle attaché.

Le `BindableBehavior<T>` classe fournit un substituable `OnAttachedTo` méthode qui définit la [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) le comportement et un substituable `OnDetachingFrom` méthode nettoie les `BindingContext`. En outre, la classe stocke une référence au contrôle attaché dans le `AssociatedObject` propriété.

L’application mobile eShopOnContainers inclut un `EventToCommandBehavior` (classe), qui exécute une commande en réponse à un événement qui se produisent. Cette classe dérive de la `BindableBehavior<T>` classe afin que le comportement peut lier et exécuter un `ICommand` spécifié par un `Command` propriété lorsque le comportement est consommé. L’exemple de code suivant illustre la classe `EventToCommandBehavior` :

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

Le `OnAttachedTo` et `OnDetachingFrom` méthodes sont utilisées pour enregistrer et annuler l’inscription d’un gestionnaire d’événements pour l’événement défini dans le `EventName` propriété. Ensuite, lorsque l’événement se déclenche, le `OnFired` méthode est appelée, qui exécute la commande.

L’avantage d’utiliser le `EventToCommandBehavior` pour exécuter une commande lorsqu’un événement est déclenché, est que les commandes peuvent être associées aux contrôles qui n’ont pas été conçus pour interagir avec les commandes. En outre, cette opération déplace le code de gestion d’événements pour afficher les modèles, où il peut être l’objet de tests unitaires.

#### <a name="invoking-behaviors-from-a-view"></a>Appel des comportements d’une vue

Le `EventToCommandBehavior` est particulièrement utile pour l’attachement d’une commande à un contrôle qui ne prennent pas en charge les commandes. Par exemple, le `ProfileView` utilise le `EventToCommandBehavior` pour exécuter le `OrderDetailCommand` lorsque le [ `ItemTapped` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemTapped/) événement est déclenché sur le [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) qui répertorie les commandes de l’utilisateur, comme indiqué dans le code suivant :

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

Lors de l’exécution, le `EventToCommandBehavior` doit répondre à l’interaction avec les [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/). Lorsqu’un élément est sélectionné dans le `ListView`, le [ `ItemTapped` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemTapped/) événement se déclenche, qui s’exécute le `OrderDetailCommand` dans le `ProfileViewModel`. Par défaut, les arguments d’événement pour l’événement sont passés à la commande. Ces données sont converties comme il est passé entre source et cible par le convertisseur spécifié dans le `EventArgsConverter` propriété, qui retourne le [ `Item` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemTappedEventArgs.Item/) de la `ListView` à partir de la [ `ItemTappedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemTappedEventArgs/). Par conséquent, lorsque le `OrderDetailCommand` est exécutée, le texte sélectionné `Order` est passé en tant que paramètre à l’Action inscrite.

Pour plus d’informations sur les comportements, consultez [comportements](~/xamarin-forms/app-fundamentals/behaviors/index.md).

## <a name="summary"></a>Récapitulatif

Le modèle Model-View-ViewModel (MVVM) permet de séparer clairement la logique métier et de présentation d’une application à partir de son interface utilisateur (IU). Maintenir une séparation nette entre la logique d’application et l’interface utilisateur permettant de résoudre de nombreux problèmes de développement et peut rendre plus faciles à tester, gérer et faire évoluer une application. Elle peut améliorer considérablement les possibilités de réutilisation de code et permet aux développeurs et concepteurs de l’interface utilisateur à effectuer davantage de collaborent aisément lors du développement de leurs éléments respectifs d’une application.

À l’aide de la MVVM de modèle, l’interface utilisateur de l’application et la logique de présentation et entreprise sous-jacente est divisée en trois classes distinctes : la vue, qui encapsule l’interface utilisateur et l’interface utilisateur logique ; le modèle d’affichage, qui encapsule la logique de présentation et d’état ; et le modèle, qui encapsule la logique métier et les données de l’application.


## <a name="related-links"></a>Liens associés

- [Téléchargez le livre électronique (PDF 2 Mo)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (sample)](https://github.com/dotnet-architecture/eShopOnContainers)
