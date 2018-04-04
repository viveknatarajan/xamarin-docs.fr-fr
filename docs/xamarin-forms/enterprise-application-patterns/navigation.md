---
title: Navigation
ms.prod: xamarin
ms.assetid: 4cad57b5-7fe4-4527-a988-d9b60c9620b4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: aa2e2858e3bb8e435ec3f38bb3d5b249eaa6cba4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="navigation"></a>Navigation

Xamarin.Forms inclut la prise en charge de la navigation entre les pages, ce qui se produit généralement à partir de l’interaction utilisateur avec l’interface utilisateur ou de l’application elle-même à la suite de modifications de l’état interne de piloté par la logique. Toutefois, la navigation peut être complexe à implémenter dans les applications qui utilisent le modèle Model-View-ViewModel (MVVM), comme les problèmes suivants doivent être remplies :

-   Comment identifier la vue à la cible, en utilisant une approche qui n’introduit pas de couplage étroit et les dépendances entre les vues.
-   Comment coordonner le processus par lequel la vue pour permettre une navigation est instanciée et initialisée. Lorsque vous utilisez MVVM, la vue et le modèle de vue doivent être instancié et associé à l’autre via le contexte de liaison de la vue. Quand une application utilise un conteneur d’injection de dépendance, l’instanciation de vues et afficher les modèles peut nécessiter un mécanisme de construction spécifique.
-   Si vous souhaitez effectuer la navigation de la vue en premier ou afficher en premier du modèle de navigation. Avec la navigation de la vue en premier, la page pour accéder au fait référence au nom du type de vue. Lors de la navigation, la vue spécifiée est instanciée, ainsi que son modèle d’affichage correspondant et d’autres services dépendants. Une autre approche consiste à utiliser à afficher en premier du modèle de navigation, d’où la page à atteindre fait référence au nom du type de modèle de vue.
-   Comment à proprement séparer le comportement de l’application de navigation entre les vues et les modèles d’affichage. Le modèle MVVM fournit une séparation entre l’interface utilisateur de l’application et sa présentation et la logique métier. Toutefois, le comportement de navigation d’une application souvent couvrent les parties de l’interface utilisateur et des présentations de l’application. L’utilisateur va lancer souvent de navigation à partir d’une vue, et la vue sera remplacée suite à la navigation. Toutefois, navigation souvent devrez peut-être également être initiée ou coordonnées à partir du modèle de vue.
-   Explique comment passer des paramètres lors de la navigation à des fins d’initialisation. Par exemple, si l’utilisateur accède à une vue pour mettre à jour les détails de la commande, les données de commande aura à passer à la vue pour qu’elle puisse afficher les données correctes.
-   Procédure de navigation de coordination, pour vous assurer que certaines règles d’entreprise sont respectées. Par exemple, les utilisateurs peuvent être invités avant de vous obliger à quitter une vue afin qu’ils peuvent corriger toutes les données non valides ou être invités à envoyer ou ignorer les modifications de données qui ont été apportées dans la vue.

Ce chapitre traite de ces défis en présentant un `NavigationService` classe qui est utilisé pour effectuer la navigation dans les pages de modèle en premier affichage.

> [!NOTE]
> Le `NavigationService` utilisé par l’application est conçue uniquement pour la navigation hiérarchique entre les instances ContentPage. À l’aide du service de naviguer parmi les autres types de page peut entraîner un comportement inattendu.

## <a name="navigating-between-pages"></a>Navigation entre les Pages

Logique de navigation permettre résider dans code-behind d’une vue ou dans les données liées au modèle d’affichage. Alors que placer la logique de navigation dans une vue peut être l’approche la plus simple, il n’est pas facilement testable via les tests unitaires. Placer la logique de la navigation dans l’affichage de classes de modèle signifie que la logique peut être levée par le biais des tests unitaires. En outre, le modèle d’affichage peut ensuite implémenter la logique pour contrôler la navigation pour vous assurer que certaines règles d’entreprise sont appliquées. Par exemple, une application ne peut pas autoriser l’utilisateur de quitter une page sans d’abord s’assurer que les données d’entrée soient valides.

A `NavigationService` de classe est généralement appelée à partir de modèles d’affichage, de promouvoir la testabilité. Toutefois, accédant à des vues à partir de modèles de vue nécessiterait l’afficher les modèles de référence et en particulier les affichages qui le modèle d’affichage actif n’est pas associé, ce qui n’est pas recommandé. Par conséquent, le `NavigationService` présenté ici spécifie le type de modèle de vue en tant que la cible à atteindre.

Les utilisations d’application mobile eShopOnContainers la `NavigationService` classe afin de fournir le modèle d’abord la navigation. Cette classe implémente la `INavigationService` interface, ce qui est indiqué dans l’exemple de code suivant :

```csharp
public interface INavigationService  
{  
    ViewModelBase PreviousPageViewModel { get; }  
    Task InitializeAsync();  
    Task NavigateToAsync<TViewModel>() where TViewModel : ViewModelBase;  
    Task NavigateToAsync<TViewModel>(object parameter) where TViewModel : ViewModelBase;  
    Task RemoveLastFromBackStackAsync();  
    Task RemoveBackStackAsync();  
}
```

Cette interface spécifie qu’une classe d’implémentation doit fournir les méthodes suivantes :

|Méthode|Objectif|
|--- |--- |
|`InitializeAsync`|Effectue la navigation vers un des deux pages lorsque l’application est lancée.|
|`NavigateToAsync`|Effectue la navigation hiérarchique à une page spécifiée.|
|`NavigateToAsync(parameter)`|Effectue la navigation hiérarchique à une page spécifiée, en passant un paramètre.|
|`RemoveLastFromBackStackAsync`|Supprime la page précédente de la pile de navigation.|
|`RemoveBackStackAsync`|Supprime toutes les pages précédentes de la pile de navigation.|

En outre, le `INavigationService` interface spécifie qu’une classe d’implémentation doit fournir un `PreviousPageViewModel` propriété. Cette propriété retourne le type de modèle d’affichage associé à la page précédente dans la pile de navigation.

> [!NOTE]
> Un `INavigationService` interface serait généralement également spécifier un `GoBackAsync` (méthode), qui est utilisée pour retourner par programme à la page précédente dans la pile de navigation. Toutefois, cette méthode est absent de l’application mobile eShopOnContainers, car il n’est pas nécessaire.

### <a name="creating-the-navigationservice-instance"></a>Création de l’Instance NavigationService

Le `NavigationService` classe qui implémente le `INavigationService` l’interface, est enregistré comme un singleton avec le conteneur d’injection de dépendance Autofac, comme illustré dans l’exemple de code suivant :

```csharp
builder.RegisterType<NavigationService>().As<INavigationService>().SingleInstance();
```

Le `INavigationService` interface est résolu dans le `ViewModelBase` constructeur de classe, comme illustré dans l’exemple de code suivant :

```csharp
NavigationService = ViewModelLocator.Resolve<INavigationService>();
```

Cela retourne une référence à la `NavigationService` objet qui est stocké dans le conteneur d’injection de dépendance Autofac, qui est créé par le `InitNavigation` méthode dans la `App` classe. Pour plus d’informations, consultez [naviguer lorsque l’application est lancée](#navigating_when_the_app_is_launched).

Le `ViewModelBase` classe stocke le `NavigationService` d’instance dans un `NavigationService` la propriété de type `INavigationService`. Par conséquent, tous les afficher les classes de modèle, qui dérivent de la `ViewModelBase` de classe, peut utiliser le `NavigationService` propriété pour accéder aux méthodes spécifiés par le `INavigationService` interface. Cela évite d’injecter la `NavigationService` objet à partir du conteneur d’injection de dépendance Autofac dans chaque classe de modèle de vue.

### <a name="handling-navigation-requests"></a>La gestion des requêtes de Navigation

Xamarin.Forms fournit le [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) classe qui implémente une expérience de navigation hiérarchique dans laquelle l’utilisateur est en mesure de parcourir les pages, vers l’avant et vers l’arrière, comme vous le souhaitez. Pour plus d’informations sur la navigation hiérarchique, consultez [Navigation hiérarchique](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md).

Au lieu d’utiliser le [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) classe directement, l’enveloppe d’application eShopOnContainers la `NavigationPage` classe dans le `CustomNavigationView` de classe, comme indiqué dans l’exemple de code suivant :

```csharp
public partial class CustomNavigationView : NavigationPage  
{  
    public CustomNavigationView() : base()  
    {  
        InitializeComponent();  
    }  

    public CustomNavigationView(Page root) : base(root)  
    {  
        InitializeComponent();  
    }  
}
```

L’objectif de ce retour à la ligne est pour faciliter la conception de styles pour le [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) instance à l’intérieur du fichier XAML pour la classe.

Navigation est effectuée à l’intérieur des classes de modèle de vue en appelant une de le `NavigateToAsync` méthodes, en spécifiant le type de modèle de vue de la page cible de la navigation à, comme illustré dans l’exemple de code suivant :

```csharp
await NavigationService.NavigateToAsync<MainViewModel>();
```

Montre l’exemple de code suit le `NavigateToAsync` méthodes fournies par la `NavigationService` classe :

```csharp
public Task NavigateToAsync<TViewModel>() where TViewModel : ViewModelBase  
{  
    return InternalNavigateToAsync(typeof(TViewModel), null);  
}  

public Task NavigateToAsync<TViewModel>(object parameter) where TViewModel : ViewModelBase  
{  
    return InternalNavigateToAsync(typeof(TViewModel), parameter);  
}
```

Chaque méthode permet de toute classe de modèle de vue qui dérive de la `ViewModelBase` classe pour effectuer la navigation hiérarchique en appelant le `InternalNavigateToAsync` (méthode). En outre, la seconde `NavigateToAsync` méthode permet d’être spécifiés comme un argument qui est passé par le modèle d’affichage cible de la navigation, où il est généralement utilisé pour effectuer l’initialisation des données de navigation. Pour plus d’informations, consultez [en passant les paramètres au cours de Navigation](#passing_parameters_during_navigation).

Le `InternalNavigateToAsync` méthode exécute la requête de navigation et est indiqué dans l’exemple de code suivant :

```csharp
private async Task InternalNavigateToAsync(Type viewModelType, object parameter)  
{  
    Page page = CreatePage(viewModelType, parameter);  

    if (page is LoginView)  
    {  
        Application.Current.MainPage = new CustomNavigationView(page);  
    }  
    else  
    {  
        var navigationPage = Application.Current.MainPage as CustomNavigationView;  
        if (navigationPage != null)  
        {  
            await navigationPage.PushAsync(page);  
        }  
        else  
        {  
            Application.Current.MainPage = new CustomNavigationView(page);  
        }  
    }  

    await (page.BindingContext as ViewModelBase).InitializeAsync(parameter);  
}  

private Type GetPageTypeForViewModel(Type viewModelType)  
{  
    var viewName = viewModelType.FullName.Replace("Model", string.Empty);  
    var viewModelAssemblyName = viewModelType.GetTypeInfo().Assembly.FullName;  
    var viewAssemblyName = string.Format(  
                CultureInfo.InvariantCulture, "{0}, {1}", viewName, viewModelAssemblyName);  
    var viewType = Type.GetType(viewAssemblyName);  
    return viewType;  
}  

private Page CreatePage(Type viewModelType, object parameter)  
{  
    Type pageType = GetPageTypeForViewModel(viewModelType);  
    if (pageType == null)  
    {  
        throw new Exception($"Cannot locate page type for {viewModelType}");  
    }  

    Page page = Activator.CreateInstance(pageType) as Page;  
    return page;  
}
```

Le `InternalNavigateToAsync` méthode exécute la navigation vers un modèle d’affichage en appelant d’abord la `CreatePage` (méthode). Cette méthode recherche la vue qui correspond au type de modèle de vue spécifiée et crée et retourne une instance de ce type d’affichage. Localisation de la vue qui correspond au type de modèle de vue utilise une approche basée sur une convention, qui suppose que :

-   Les vues sont dans le même assembly en tant que types de modèle de vue.
-   Les vues sont dans un. Espace de noms des vues enfants.
-   Afficher les modèles sont dans un. Espace de noms enfant ViewModel.
-   Afficher les noms correspondent pour afficher les noms de modèle, avec « Modèle » est supprimé.

Lorsqu’une vue est instanciée, elle a associé à son modèle d’affichage correspondant. Pour plus d’informations sur la manière dont cela se produit, consultez [créant automatiquement un modèle d’affichage avec un localisateur de modèle de vue](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically_creating_a_view_model_with_a_view_model_locator).

Si la vue en cours de création est une `LoginView`, il est encapsulé dans une nouvelle instance de la `CustomNavigationView` classe et assigné à la [ `Application.Current.MainPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.MainPage/) propriété. Dans le cas contraire, le `CustomNavigationView` instance est récupérée et qu’il n’est pas null, le [ `PushAsync` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) méthode est appelée pour transmettre la vue en cours de création dans la pile de navigation. Toutefois, si extraites `CustomNavigationView` instance est `null`, la vue en cours de création est encapsulée à l’intérieur d’une nouvelle instance de la `CustomNavigationView` classe et assigné à la `Application.Current.MainPage` propriété. Ce mécanisme garantit que pendant la navigation, les pages sont correctement ajoutées à la pile de navigation quand elle est vide, et lorsqu’elle contient des données.

> [!TIP]
> Envisagez la mise en cache de pages. Page mise en cache les résultats de la consommation de mémoire pour les vues qui ne sont pas affichées. Toutefois, sans mise en cache de la page, cela ne signifie que l’analyse XAML et la construction de la page et de son modèle d’affichage seront produit chaque fois qu’une nouvelle page est accédée, ce qui peut avoir un impact sur les performances d’une page complexe. Pour une page bien conçue qui n’utilise pas un nombre excessif de contrôles, les performances doivent être suffisantes. Page mise en cache peut toutefois être intéressant si vous rencontrez des temps de chargement lent des pages.

Une fois que la vue est créée et cible, le `InitializeAsync` méthode du modèle de vue associée de la vue est exécutée. Pour plus d’informations, consultez [en passant les paramètres au cours de Navigation](#passing_parameters_during_navigation).

<a name="navigating_when_the_app_is_launched" />

### <a name="navigating-when-the-app-is-launched"></a>Lancée de navigation lorsque application le

Lorsque l’application est lancée, le `InitNavigation` méthode dans le `App` de classe est appelée. L’exemple de code suivant illustre cette méthode :

```csharp
private Task InitNavigation()  
{  
    var navigationService = ViewModelLocator.Resolve<INavigationService>();  
    return navigationService.InitializeAsync();  
}
```

La méthode crée un nouveau `NavigationService` objet dans le conteneur d’injection de dépendance Autofac et retourne une référence à celui-ci, avant d’appeler son `InitializeAsync` (méthode).

> [!NOTE]
> Lorsque le `INavigationService` interface est résolu par le `ViewModelBase` (classe), le conteneur retourne une référence à la `NavigationService` objet qui a été créé lors de la méthode InitNavigation est appelée.

Le code suivant exemple illustre le `NavigationService` `InitializeAsync` méthode :

```csharp
public Task InitializeAsync()  
{  
    if (string.IsNullOrEmpty(Settings.AuthAccessToken))  
        return NavigateToAsync<LoginViewModel>();  
    else  
        return NavigateToAsync<MainViewModel>();  
}
```

Le `MainView` cible si l’application possède un jeton d’accès de mise en cache, qui est utilisé pour l’authentification. Dans le cas contraire, le `LoginView` est la cible de la navigation.

Pour plus d’informations sur le conteneur d’injection de dépendance Autofac, consultez [Introduction à l’Injection de dépendance](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection).

<a name="passing_parameters_during_navigation" />

### <a name="passing-parameters-during-navigation"></a>Passage de paramètres lors de la Navigation

Un de la `NavigateToAsync` méthodes, spécifiés par le `INavigationService` de l’interface, les données de navigation permet d’être spécifiés comme un argument qui est passé par le modèle d’affichage cible de la navigation, où il est généralement utilisé pour effectuer l’initialisation.

Par exemple, le `ProfileViewModel` classe contient un `OrderDetailCommand` qui est exécutée lorsque l’utilisateur sélectionne une commande sur le `ProfileView` page. À son tour, il exécute la `OrderDetailAsync` méthode, qui est indiqué dans l’exemple de code suivant :

```csharp
private async Task OrderDetailAsync(Order order)  
{  
    await NavigationService.NavigateToAsync<OrderDetailViewModel>(order);  
}
```

Cette méthode appelle la navigation vers le `OrderDetailViewModel`, en passant un `Order` instance qui représente l’ordre sélectionné par l’utilisateur sur le `ProfileView` page. Lorsque le `NavigationService` classe crée le `OrderDetailView`, le `OrderDetailViewModel` classe est instanciée et assignée à la vue [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/). Après avoir navigué vers le `OrderDetailView`, le `InternalNavigateToAsync` méthode s’exécute le `InitializeAsync` modèle d’affichage associé à la méthode de la vue.

Le `InitializeAsync` méthode est définie dans le `ViewModelBase` classe comme une méthode qui peut être remplacée. Cette méthode spécifie un `object` argument qui représente les données à passer à un modèle d’affichage pendant une opération de navigation. Par conséquent, les classes de modèle d’affichage que vous souhaitez recevoir des données d’une opération de navigation fournissent leur propre implémentation de la `InitializeAsync` méthode pour effectuer l’initialisation requise. Montre l’exemple de code suit le `InitializeAsync` méthode à partir de la `OrderDetailViewModel` classe :

```csharp
public override async Task InitializeAsync(object navigationData)  
{  
    if (navigationData is Order)  
    {  
        ...  
        Order = await _ordersService.GetOrderAsync(  
                        Convert.ToInt32(order.OrderNumber), authToken);  
        ...  
    }  
}
```

Cette méthode récupère le `Order` détails de l’instance qui a été passée dans le modèle d’affichage lors de l’opération de navigation et l’utilise pour extraire la commande complète à partir de la `OrderService` instance.

<a name="invoking_navigation_using_behaviors" />

### <a name="invoking-navigation-using-behaviors"></a>Appel de Navigation à l’aide de comportements

Navigation est généralement déclenchée à partir d’une vue par une intervention de l’utilisateur. Par exemple, le `LoginView` effectue la navigation après une authentification réussie. L’exemple de code suivant montre comment le volet de navigation est appelée par un comportement :

```xaml
<WebView ...>  
    <WebView.Behaviors>  
        <behaviors:EventToCommandBehavior  
            EventName="Navigating"  
            EventArgsConverter="{StaticResource WebNavigatingEventArgsConverter}"  
            Command="{Binding NavigateCommand}" />  
    </WebView.Behaviors>  
</WebView>
```

Lors de l’exécution, le `EventToCommandBehavior` doit répondre à l’interaction avec les [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/). Lorsque le `WebView` accède à une page web, le [ `Navigating` ](https://developer.xamarin.com/api/event/Xamarin.Forms.WebView.Navigating/) événement se déclenche, qui s’exécute le `NavigateCommand` dans le `LoginViewModel`. Par défaut, les arguments d’événement pour l’événement sont passés à la commande. Ces données sont converties comme il est passé entre source et cible par le convertisseur spécifié dans le `EventArgsConverter` propriété, qui retourne le [ `Url` ](https://developer.xamarin.com/api/property/Xamarin.Forms.WebNavigationEventArgs.Url/) à partir de la [ `WebNavigatingEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebNavigatingEventArgs/). Par conséquent, lorsque le `NavigationCommand` est exécutée, l’Url de la page web est passé en tant que paramètre au nom `Action`.

À son tour, le `NavigationCommand` exécute le `NavigateAsync` (méthode), qui est affichée dans l’exemple de code suivant :

```csharp
private async Task NavigateAsync(string url)  
{  
    ...          
    await NavigationService.NavigateToAsync<MainViewModel>();  
    await NavigationService.RemoveLastFromBackStackAsync();  
    ...  
}
```

Cette méthode appelle la navigation vers le `MainViewModel`, et suivant du volet de navigation, supprime la `LoginView` page à partir de la pile de navigation.

### <a name="confirming-or-cancelling-navigation"></a>Confirmation ou annulation de Navigation

Une application peut doit interagir avec l’utilisateur pendant une opération de navigation, afin que l’utilisateur peut vérifier ou annuler la navigation. Cela peut être nécessaire, par exemple, lorsque l’utilisateur tente d’accéder avant d’avoir entièrement une page d’entrée de données. Dans ce cas, une application doit fournir une notification qui permet à l’utilisateur de quitter la page ou d’annuler l’opération de navigation avant qu’elle se produise. Cela est possible dans une classe de modèle de vue à l’aide de la réponse à partir d’une notification pour contrôler la navigation est appelée ou non.

## <a name="summary"></a>Récapitulatif

Xamarin.Forms inclut la prise en charge de la navigation entre les pages, ce qui se produit généralement à partir de l’interaction utilisateur avec l’interface utilisateur, ou à partir de l’application elle-même, à la suite de modifications de l’état interne de piloté par la logique. Toutefois, la navigation peut être complexe à implémenter dans les applications qui utilisent le modèle MVVM.

Ce chapitre présenté un `NavigationService` (classe), qui est utilisé pour exécuter le modèle d’abord la navigation à partir de modèles d’affichage. Placer la logique de la navigation dans l’affichage de classes de modèle signifie que la logique peut être levée par le biais des tests automatisés. En outre, le modèle d’affichage peut ensuite implémenter la logique pour contrôler la navigation pour vous assurer que certaines règles d’entreprise sont appliquées.


## <a name="related-links"></a>Liens associés

- [Téléchargez le livre électronique (PDF 2 Mo)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (sample)](https://github.com/dotnet-architecture/eShopOnContainers)
