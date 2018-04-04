---
title: Test unitaire
ms.prod: xamarin
ms.assetid: 4af82e52-f99b-4cad-b278-1745f190c240
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 57201a32f5ffc0ae962f6db851a25a737e1cb17d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="unit-testing"></a>Test unitaire

Les applications mobiles ont des problèmes spécifiques qui bureau et des applications basées sur le web n’ont pas à vous soucier. Les utilisateurs mobiles diffèrent par les appareils qu’ils utilisent, par une connectivité réseau, à la disponibilité des services et une plage d’autres facteurs. Par conséquent, les applications mobiles doivent être testées car ils seront utilisés dans le monde réel pour améliorer leur qualité, la fiabilité et les performances. Il existe de nombreux types de tests qui doivent être effectuées sur une application, y compris les tests unitaires, tests d’intégration et l’interface utilisateur de test, avec les tests en cours de la forme la plus commune de tests unitaires.

Un test unitaire prend une petite unité de l’application, en général, une méthode, il isole le reste du code et vérifie qu’elle se comporte comme prévu. Son objectif est de vérifier que chaque unité de fonctionnalité effectue correctement, afin que les erreurs ne se propagent dans toute l’application. Détection d’un bogue dans lequel il se produit est plus efficace que l’effet d’un bogue indirectement à un point de défaillance de secondaire observation.

Les tests unitaires ont la plus grande incidence sur la qualité du code lorsqu’il fait partie intégrante du flux de travail du développement logiciel. Dès qu’une méthode a été écrit, tests unitaires doivent être écrits pour vérifier le comportement de la méthode en réponse aux cas standard, limite et incorrects des données d’entrée et cette vérification les hypothèses explicites ou implicites par le code. Vous pouvez également, avec le développement piloté par test, les tests unitaires sont écrits avant le code. Dans ce scénario, les tests unitaires agissent comme documentation de conception et spécifications fonctionnelles.

> [!NOTE]
> Les tests unitaires sont très efficaces contre régression : autrement dit, les fonctionnalités qui permettant d’utiliser, mais a été déplacée par une mise à jour est défectueux.

Tests unitaires utilisent généralement le modèle de disposition act-assert :

-   Le *réorganiser* section de la méthode de test unitaire initialise les objets et définit la valeur des données qui sont passées à la méthode de test.
-   Le *agissent* section appelle la méthode de test avec les arguments requis.
-   Le *assert* section vérifie que l’action de la méthode testée se comporte comme prévu.

Ce modèle garantit que les tests unitaires sont lisibles et cohérent.

## <a name="dependency-injection-and-unit-testing"></a>Injection de dépendance et des tests unitaires

Un des intérêts de l’adoption d’une architecture faiblement couplée est qu’il facilite le test unitaire. Un des types inscrits avec Autofac est la `OrderService` classe. L’exemple de code suivant montre un plan de cette classe :

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

Le `OrderDetailViewModel` classe a une dépendance sur le `IOrderService` de type qui le conteneur est résolue quand il instancie une `OrderDetailViewModel` objet. Toutefois, plutôt que de créer un `OrderService` objet au test unitaire la `OrderDetailViewModel` (classe), au lieu de cela, remplacez le `OrderService` objet avec un fictifs à des fins de tests. Figure 10-1 illustre cette relation.

![](unit-testing-images/unittesting.png "Classes qui implémentent l’interface IOrderService")

**Figure 10-1 :** les Classes qui implémentent l’interface IOrderService

Cette approche permet la `OrderService` objet d’être passé dans le `OrderDetailViewModel` classe lors de l’exécution et dans un souci de testabilité, il permet la `OrderMockService` classe à transmettre la `OrderDetailViewModel` classe au moment du test. Le principal avantage de cette approche est qu’il permet des tests unitaires à exécuter sans nécessiter des ressources lourde, telles que les services web ou des bases de données.

## <a name="testing-mvvm-applications"></a>Test d’Applications MVVM

Test de modèles et afficher les modèles à partir d’applications de MVVM est identique à d’autres classes de test et les mêmes outils et techniques, telles que le test unitaire et simulation, peuvent être utilisés. Toutefois, il existe certains modèles sont classiques au modèle et les classes de modèle de vue, qui peuvent tirer parti des techniques de test unitaire spécifique.

> [!TIP]
> Tester une chose avec chaque test unitaire. Ne pas être tenté de rendre une unité test exercice plusieurs aspects du comportement de l’unité. Cela conduit à des tests qui sont difficiles à lire et mettre à jour. Il peut également porter à confusion lors de l’interprétation d’une défaillance.

Les utilisations d’application mobile eShopOnContainers [xUnit](https://xunit.github.io/) pour effectuer des tests unitaires, qui prend en charge deux types de tests unitaires :

-   Les faits sont des tests qui sont toujours la valeur est trues, le test des conditions invariantes.
-   Théories sont les tests qui ne sont pas trues pour un jeu de données particulier.

Les tests unitaires inclus avec l’application mobile eShopOnContainers sont des tests de faits, et par conséquent, chaque méthode de test unitaire est décorée avec le `[Fact]` attribut.

> [!NOTE]
> xUnit tests sont exécutés par un test runner. Pour exécuter test runner, exécutez le projet de eShopOnContainers.TestRunner pour la plateforme nécessaire.

### <a name="testing-asynchronous-functionality"></a>Test de la fonctionnalité asynchrone

Lorsque vous implémentez le modèle MVVM, afficher les modèles généralement appellent des opérations sur les services, souvent de façon asynchrone. Tests pour le code qui appelle ces opérations généralement utilisent mocks comme remplacements pour les services réels. L’exemple de code suivant montre le test des fonctionnalités asynchrones en passant d’un service fictif dans un modèle d’affichage :

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

Ce test unitaire vérifie que le `Order` propriété de la `OrderDetailViewModel` instance aura une valeur après la `InitializeAsync` méthode a été appelée. Le `InitializeAsync` méthode est appelée lorsque l’accès à la vue de correspondante du modèle de vue. Pour plus d’informations sur la navigation, consultez [Navigation](~/xamarin-forms/enterprise-application-patterns/navigation.md).

Lorsque le `OrderDetailViewModel` instance est créée, elle attend un `OrderService` instance en tant qu’argument. Toutefois, le `OrderService` récupère les données à partir d’un service web. Par conséquent, un `OrderMockService` instance, qui est une version fictive de la `OrderService` de classe, est spécifiée comme argument à la `OrderDetailViewModel` constructeur. Ensuite, lorsque le modèle d’affichage `InitializeAsync` méthode est appelée, qui appelle `IOrderService` opérations, des données fictives sont récupérée plutôt qu’il communique avec un service web.

### <a name="testing-inotifypropertychanged-implementations"></a>Test des implémentations de INotifyPropertyChanged

Mise en œuvre le `INotifyPropertyChanged` interface permet de réagir aux modifications qui proviennent d’affichage de vues modèles et les modèles. Ces modifications ne sont pas limitées à des données affichées dans les contrôles, ils sont également utilisés pour contrôler l’affichage, telles que les États de modèle d’affichage qui provoquent des animations à démarrer ou des contrôles doit être désactivée.

Les propriétés qui peuvent être mis à jour directement par le test unitaire peuvent être testées en attachant un gestionnaire d’événements pour le `PropertyChanged` événement et en vérifiant si l’événement est déclenché après la définition d’une nouvelle valeur pour la propriété. L’exemple de code suivant montre un test de ce type :

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

Ce test unitaire appelle les `InitializeAsync` méthode de la `OrderViewModel` classe, ce qui entraîne son `Order` propriété à mettre à jour. Le test unitaire passera, à condition que la `PropertyChanged` événement est déclenché pour les `Order` propriété.

### <a name="testing-message-based-communication"></a>Communication basée sur le Message de test

Affichage des modèles qui utilisent la [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) classe pour communiquer entre faiblement couplées de classes peut être unité testée en vous abonnant au message envoyé par le code sous test, comme illustré dans l’exemple de code suivant :

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

Ce test unitaire vérifie que le `CatalogViewModel` publie la `AddProduct` message en réponse à son `AddCatalogItemCommand` en cours d’exécution. Étant donné que la [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) classe prend en charge les abonnements aux messages de multidiffusion, le test unitaire peut s’abonner à la `AddProduct` le message et exécuter un délégué de rappel en réponse à la réception. Ce délégué de rappel spécifié comme une expression lambda, définit un `boolean` champ qui est utilisé par la `Assert` instruction pour vérifier le comportement du test.

### <a name="testing-exception-handling"></a>Test de la gestion des exceptions

Tests unitaires peuvent également être écrites cette vérification levées d’exceptions spécifiques pour les actions non valides ou des entrées, comme illustré dans l’exemple de code suivant :

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

Ce test unitaire lève une exception, car le [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) contrôle n’a pas d’un événement nommé `OnItemTapped`. Le `Assert.Throws<T>` méthode est une méthode générique où `T` est le type de l’exception attendue. L’argument passé à la `Assert.Throws<T>` méthode est une expression lambda qui lève l’exception. Par conséquent, le test unitaire passera autant que l’expression lambda lève une `ArgumentException`.

>💡 **Conseil**: Évitez d’écrire des tests unitaires qui examinent des chaînes de message d’exception. Chaînes de message d’exception peuvent changer au fil du temps, et par conséquent, les tests unitaires qui s’appuient sur leur présence sont considérés comme fragile.

### <a name="testing-validation"></a>Test de la Validation

Il existe deux aspects de l’implémentation de la validation de test : test que les règles de validation sont correctement implémentés et de test qui le `ValidatableObject<T>` classe fonctionne comme prévu.

La logique de validation est généralement simple à tester, car il s’agit généralement d’un processus autonome où le résultat dépend de l’entrée. Il doit y avoir des tests sur les résultats de l’appel de la `Validate` méthode sur chaque propriété qui possède au moins une règle de validation associé, comme illustré dans l’exemple de code suivant :

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

Ce test unitaire vérifie que la validation réussit lorsque les deux `ValidatableObject<T>` propriétés dans le `MockViewModel` instance les deux ont des données.

Ainsi que la vérification que la validation réussit, les tests unitaires de validation doivent vérifier également les valeurs de la `Value`, `IsValid`, et `Errors` propriété de chaque `ValidatableObject<T>` instance, pour vérifier que la classe fonctionne comme prévu. L’exemple de code suivant montre un test unitaire qui effectue l’opération :

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

Ce test unitaire vérifie que la validation échoue lorsque le `Surname` propriété de la `MockViewModel` n’a pas de données et le `Value`, `IsValid`, et `Errors` propriété de chaque `ValidatableObject<T>` instance sont correctement définies.

## <a name="summary"></a>Récapitulatif

Un test unitaire prend une petite unité de l’application, en général, une méthode, il isole le reste du code et vérifie qu’elle se comporte comme prévu. Son objectif est de vérifier que chaque unité de fonctionnalité effectue correctement, afin que les erreurs ne se propagent dans toute l’application.

Le comportement d’un objet de test peut être isolé en remplaçant les objets dépendants avec des objets fictifs qui simulent le comportement des objets dépendants. Ainsi, les tests unitaires pour être exécutée sans nécessiter des ressources lourde, telles que les services web ou des bases de données.

Test de modèles et afficher les modèles à partir d’applications de MVVM est identique à d’autres classes de test et les mêmes outils et techniques qui peuvent être utilisés.


## <a name="related-links"></a>Liens associés

- [Téléchargez le livre électronique (PDF 2 Mo)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (sample)](https://github.com/dotnet-architecture/eShopOnContainers)
