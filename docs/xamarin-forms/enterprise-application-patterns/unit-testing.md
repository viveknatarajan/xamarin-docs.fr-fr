---
title: Applications d’entreprise de tests unitaires
description: Ce chapitre explique comment effectuer des tests unitaires sont effectuée dans l’application mobile eShopOnContainers.
ms.prod: xamarin
ms.assetid: 4af82e52-f99b-4cad-b278-1745f190c240
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 02aeedd5498c47950e2fbc0d218de05bc0bb3204
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998681"
---
# <a name="unit-testing-enterprise-apps"></a>Applications d’entreprise de tests unitaires

Les applications mobiles ont des problèmes spécifiques qui bureau et les applications basées sur le web n’ont pas à vous soucier. Les utilisateurs mobiles diffèrent par les appareils qu’ils utilisent, par une connectivité réseau, par la disponibilité des services et une plage d’autres facteurs. Par conséquent, les applications mobiles doivent être testées car ils seront utilisés dans le monde réel pour améliorer leur qualité, la fiabilité et performances. Il existe de nombreux types de tests qui doivent être effectuées sur une application, y compris les tests unitaires, tests d’intégration et l’interface utilisateur de test, avec les tests en cours de la forme la plus courante des tests unitaires.

Un test unitaire prend une petite unité de l’application, généralement une méthode, il isole le reste du code et vérifie qu’il se comporte comme prévu. Son objectif consiste à vérifier que chaque unité de fonctionnalité s’exécute comme prévu, afin que les erreurs ne pas propager dans l’application. La détection d’un bogue dans lequel il se produit est plus efficace que l’effet d’un bogue indirectement à un point de défaillance de secondaire en observant.

Test unitaire a la plus grande incidence sur la qualité du code lorsqu’il fait partie intégrante du flux de travail de développement logiciel. Dès qu’une méthode a été écrite, tests unitaires doivent être écrites pour vérifier le comportement de la méthode en réponse aux cas standard, limite et incorrects des données d’entrée et cette vérification les hypothèses explicites ou implicites par le code. Vous pouvez également, avec le développement piloté par test, les tests unitaires sont écrits avant le code. Dans ce scénario, les tests unitaires agissent comme documentation de conception et spécifications fonctionnelles.

> [!NOTE]
> Tests unitaires sont très efficaces par rapport à la régression : autrement dit, les fonctionnalités qui fonctionnent alors qu’elle a été perturbée par une mise à jour défectueuse.

Tests unitaires utilisent généralement le modèle de disposition act-assert :

-   Le *réorganiser* section de la méthode de test unitaire initialise les objets et définit la valeur des données qui sont passées à la méthode testée.
-   Le *agissent* section appelle la méthode testée avec les arguments requis.
-   Le *assert* section vérifie que l’action de la méthode testée se comporte comme prévu.

En suivant ce modèle permet de s’assurer que les tests unitaires sont lisibles et cohérente.

## <a name="dependency-injection-and-unit-testing"></a>L’Injection de dépendances et les tests unitaires

Une des motivations d’adoption d’une architecture faiblement couplée est qu’il facilite le test unitaire. Un des types inscrits avec Autofac est la `OrderService` classe. L’exemple de code suivant montre un contour de cette classe :

```csharp
public class OrderDetailViewModel : ViewModelBase  
{  
    private IOrderService _ordersService;  

    public OrderDetailViewModel(IOrderService ordersService)  
    {  
        _ordersService = ordersService;  
    }  
    ...  
}
```

Le `OrderDetailViewModel` classe a une dépendance sur le `IOrderService` de type qui le conteneur est résolue quand il instancie un `OrderDetailViewModel` objet. Toutefois, au lieu de créer un `OrderService` objet des tests unitaires sur le `OrderDetailViewModel` (classe), au lieu de cela, remplacez le `OrderService` objet avec un simulacre à des fins de tests. Figure 10-1 illustre cette relation.

![](unit-testing-images/unittesting.png "Classes qui implémentent l’interface IOrderService")

**Figure 10-1 :** les Classes qui implémentent l’interface IOrderService

Cette approche permet la `OrderService` objet à passer dans le `OrderDetailViewModel` de classe lors de l’exécution et dans un souci de testabilité, il permet la `OrderMockService` classe à passer dans le `OrderDetailViewModel` classe au moment du test. Le principal avantage de cette approche est qu’elle permet les tests unitaires pour être exécutés sans nécessiter des ressources difficile à gérer, telles que les services web ou des bases de données.

## <a name="testing-mvvm-applications"></a>Tester les Applications MVVM

Test des modèles et des modèles de vue à partir des applications MVVM est identique au test de toutes les autres classes et les mêmes outils et techniques, telles que des tests unitaires et la simulation, peuvent être utilisés. Toutefois, il existe quelques modèles qui typiques d’un modèle et les classes de modèle de vue, qui peuvent tirer parti des techniques de test d’unité spécifique.

> [!TIP]
> Tester une chose avec chaque test unitaire. Ne soyez pas tenté de rendre une unité à tester exercice plusieurs aspects du comportement de l’unité. Cela conduit à des tests qui sont difficiles à lire et mettre à jour. Il peut également entraîner une certaine confusion lors de l’interprétation d’un échec.

L’application mobile d’eShopOnContainers utilise [xUnit](https://xunit.github.io/) pour effectuer des tests unitaires, qui prend en charge deux types de tests unitaires :

-   Les faits sont des tests qui sont toujours trues, le test des conditions d’invariant.
-   Théories sont des tests qui sont uniquement remplies pour un jeu de données particulier.

Les tests unitaires inclus avec l’application mobile eShopOnContainers sont des tests de faits, et par conséquent, chaque méthode de test unitaire est décorée avec le `[Fact]` attribut.

> [!NOTE]
> tests xUnit sont exécutées par un test runner. Pour exécuter le test runner, exécutez le projet de eShopOnContainers.TestRunner pour la plateforme nécessaire.

### <a name="testing-asynchronous-functionality"></a>Test de la fonctionnalité asynchrone

Lorsque vous implémentez le modèle MVVM, modèles de vue général, l’appel des opérations sur les services, souvent de façon asynchrone. Tests pour le code qui appelle ces opérations généralement utilisent simulacres comme remplacements pour les services réels. L’exemple de code suivant montre le test des fonctionnalités asynchrones en transmettant un service factice dans un modèle de vue :

```csharp
[Fact]  
public async Task OrderPropertyIsNotNullAfterViewModelInitializationTest()  
{  
    var orderService = new OrderMockService();  
    var orderViewModel = new OrderDetailViewModel(orderService);  

    var order = await orderService.GetOrderAsync(1, GlobalSetting.Instance.AuthToken);  
    await orderViewModel.InitializeAsync(order);  

    Assert.NotNull(orderViewModel.Order);  
}
```

Ce test unitaire vérifie que le `Order` propriété de la `OrderDetailViewModel` instance a une valeur après la `InitializeAsync` méthode a été appelée. Le `InitializeAsync` méthode est appelée lors de la vue correspondante du modèle de vue cible. Pour plus d’informations sur la navigation, consultez [Navigation](~/xamarin-forms/enterprise-application-patterns/navigation.md).

Lorsque le `OrderDetailViewModel` instance est créée, elle attend un `OrderService` instance à être spécifié en tant qu’argument. Toutefois, le `OrderService` récupère les données à partir d’un service web. Par conséquent, un `OrderMockService` instance, ce qui est une version fictive de la `OrderService` class, est spécifiée comme argument à la `OrderDetailViewModel` constructeur. Ensuite, lorsque le modèle de vue `InitializeAsync` méthode est appelée, qui appelle `IOrderService` opérations, des données fictives sont récupérée plutôt que communique avec un service web.

### <a name="testing-inotifypropertychanged-implementations"></a>Test des implémentations de INotifyPropertyChanged

Implémentation de la `INotifyPropertyChanged` interface permet de réagir aux modifications qui proviennent d’affichage de vues modèles et les modèles. Ces modifications ne sont pas limitées aux données affichées dans les contrôles : ils sont également utilisés pour contrôler l’affichage, telles que les États de modèle d’affichage qui provoquent des animations à démarrer ou des contrôles doit être désactivée.

Les propriétés pouvant être mis à jour directement par le test unitaire peuvent être testées en attachant un gestionnaire d’événements pour le `PropertyChanged` événement et en vérifiant si l’événement est déclenché après la définition d’une nouvelle valeur pour la propriété. L’exemple de code suivant montre un test de ce type :

```csharp
[Fact]  
public async Task SettingOrderPropertyShouldRaisePropertyChanged()  
{  
    bool invoked = false;  
    var orderService = new OrderMockService();  
    var orderViewModel = new OrderDetailViewModel(orderService);  

    orderViewModel.PropertyChanged += (sender, e) =>  
    {  
        if (e.PropertyName.Equals("Order"))  
            invoked = true;  
    };  
    var order = await orderService.GetOrderAsync(1, GlobalSetting.Instance.AuthToken);  
    await orderViewModel.InitializeAsync(order);  

    Assert.True(invoked);  
}
```

Ce test unitaire appelle les `InitializeAsync` méthode de la `OrderViewModel` classe, ce qui conduit sa `Order` propriété à mettre à jour. Le test unitaire passera, à condition que le `PropertyChanged` événement est déclenché pour la `Order` propriété.

### <a name="testing-message-based-communication"></a>Communication basée sur le Message de test

Vue modèles qui utilisent la [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) classe pour communiquer entre les classes faiblement couplés peut être unité testée en vous abonnant au message envoyé par le code sous test, comme illustré dans l’exemple de code suivant :

```csharp
[Fact]  
public void AddCatalogItemCommandSendsAddProductMessageTest()  
{  
    bool messageReceived = false;  
    var catalogService = new CatalogMockService();  
    var catalogViewModel = new CatalogViewModel(catalogService);  

    Xamarin.Forms.MessagingCenter.Subscribe<CatalogViewModel, CatalogItem>(  
        this, MessageKeys.AddProduct, (sender, arg) =>  
    {  
        messageReceived = true;  
    });  
    catalogViewModel.AddCatalogItemCommand.Execute(null);  

    Assert.True(messageReceived);  
}
```

Ce test unitaire vérifie que le `CatalogViewModel` publie le `AddProduct` message en réponse à son `AddCatalogItemCommand` en cours d’exécution. Étant donné que le [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) classe prend en charge les abonnements aux messages de multidiffusion, le test unitaire peut s’abonner à la `AddProduct` du message et d’exécuter un délégué de rappel en réponse à recevoir. Ce délégué de rappel, spécifié comme une expression lambda, définit un `boolean` champ qui est utilisé par le `Assert` instruction pour vérifier le comportement du test.

### <a name="testing-exception-handling"></a>Test de la gestion des exceptions

Tests unitaires peuvent également être écrit cette vérification spécifiques exceptions sont levées pour des actions non valides ou entrées, comme illustré dans l’exemple de code suivant :

```csharp
[Fact]  
public void InvalidEventNameShouldThrowArgumentExceptionText()  
{  
    var behavior = new MockEventToCommandBehavior  
    {  
        EventName = "OnItemTapped"  
    };  
    var listView = new ListView();  

    Assert.Throws<ArgumentException>(() => listView.Behaviors.Add(behavior));  
}
```

Ce test unitaire lève une exception, car le [ `ListView` ](xref:Xamarin.Forms.ListView) contrôle n’a pas d’un événement nommé `OnItemTapped`. Le `Assert.Throws<T>` méthode est une méthode générique où `T` est le type de l’exception attendue. L’argument passé à la `Assert.Throws<T>` méthode est une expression lambda qui lève l’exception. Par conséquent, le test unitaire passera autant que l’expression lambda lève une `ArgumentException`.

>💡 **Conseil**: éviter d’écrire des tests unitaires qui examinent des chaînes de message d’exception. Chaînes de message d’exception peuvent changer au fil du temps, et par conséquent, les tests unitaires qui s’appuient sur leur présence sont considérés comme fragile.

### <a name="testing-validation"></a>Test de la Validation

Il existe deux aspects au test de l’implémentation de la validation : test que les règles de validation sont correctement implémentés et les tests qui le `ValidatableObject<T>` classe fonctionne comme prévu.

Logique de validation est généralement simple tester, car il est généralement un processus autonome où le résultat dépend de l’entrée. Il doit y avoir des tests sur les résultats de l’appel de la `Validate` méthode sur chaque propriété qui possède au moins une règle de validation associé, comme illustré dans l’exemple de code suivant :

```csharp
[Fact]  
public void CheckValidationPassesWhenBothPropertiesHaveDataTest()  
{  
    var mockViewModel = new MockViewModel();  
    mockViewModel.Forename.Value = "John";  
    mockViewModel.Surname.Value = "Smith";  

    bool isValid = mockViewModel.Validate();  

    Assert.True(isValid);  
}
```

Ce test unitaire vérifie que la validation réussit lorsque les deux `ValidatableObject<T>` propriétés dans le `MockViewModel` instance les deux avoir des données.

Ainsi que la vérification de la réussite de la validation, les tests unitaires de validation doivent vérifier également les valeurs de la `Value`, `IsValid`, et `Errors` propriété de chaque `ValidatableObject<T>` instance, pour vérifier que la classe fonctionne comme prévu. L’exemple de code suivant montre un test unitaire qui s’en charge :

```csharp
[Fact]  
public void CheckValidationFailsWhenOnlyForenameHasDataTest()  
{  
    var mockViewModel = new MockViewModel();  
    mockViewModel.Forename.Value = "John";  

    bool isValid = mockViewModel.Validate();  

    Assert.False(isValid);  
    Assert.NotNull(mockViewModel.Forename.Value);  
    Assert.Null(mockViewModel.Surname.Value);  
    Assert.True(mockViewModel.Forename.IsValid);  
    Assert.False(mockViewModel.Surname.IsValid);  
    Assert.Empty(mockViewModel.Forename.Errors);  
    Assert.NotEmpty(mockViewModel.Surname.Errors);  
}
```

Ce test unitaire vérifie que la validation échoue lorsque le `Surname` propriété de la `MockViewModel` n’a pas toutes les données et le `Value`, `IsValid`, et `Errors` propriété de chaque `ValidatableObject<T>` instance sont correctement définies.

## <a name="summary"></a>Récapitulatif

Un test unitaire prend une petite unité de l’application, généralement une méthode, il isole le reste du code et vérifie qu’il se comporte comme prévu. Son objectif consiste à vérifier que chaque unité de fonctionnalité s’exécute comme prévu, afin que les erreurs ne pas propager dans l’application.

Le comportement d’un objet en cours de test peut être isolé en remplaçant les objets dépendants avec des objets fictifs qui simulent le comportement des objets dépendants. Ainsi, les tests unitaires pour être exécutés sans nécessiter des ressources difficile à gérer, telles que les services web ou des bases de données.

Test des modèles et des modèles de vue à partir des applications MVVM est identique au test de toutes les autres classes et les mêmes outils et techniques peuvent être utilisés.


## <a name="related-links"></a>Liens associés

- [Téléchargez le livre électronique (PDF de 2 Mo)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (exemple)](https://github.com/dotnet-architecture/eShopOnContainers)
