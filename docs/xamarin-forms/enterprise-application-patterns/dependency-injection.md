---
title: Injection de dépendances
ms.prod: xamarin
ms.assetid: a150f2d1-06f8-4aed-ab4e-7a847d69f103
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 8db8e5b756fe770bdf292ec03c28eb5ed54acf9e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="dependency-injection"></a>Injection de dépendances

En règle générale, un constructeur de classe est appelé lors de l’instanciation d’un objet, et toutes les valeurs qui a besoin de l’objet sont passés comme arguments au constructeur. Ceci est un exemple d’injection de dépendance et en particulier est connu en tant que *injection de constructeur*. Les dépendances de que l’objet doit sont injectées dans le constructeur.

En spécifiant des dépendances en tant que types interface, injection de dépendances permet de découplage des types concrets à partir du code qui dépend de ces types. Il utilise généralement un conteneur qui contient une liste des inscriptions et des mappages entre les interfaces et les types abstraits et les types concrets qui implémentent ou étendent ces types.

Il existe d’autres types d’injection de dépendance, tel que *injection d’accesseur Set de propriété*, et *injection d’appel de méthode*, mais ils sont généralement moins visibles. Par conséquent, ce chapitre concentrerons uniquement sur l’exécution de l’injection de constructeur avec un conteneur d’injection de dépendance.

<a name="introduction_to_dependency_injection" />

## <a name="introduction-to-dependency-injection"></a>Introduction à l’Injection de dépendance

Injection de dépendances est une version spécialisée du modèle Inversion de contrôle (IoC), où le problème est inversé est le processus d’obtention de la dépendance requise. Avec l’injection de dépendance, une autre classe est responsable de l’injection de dépendances dans un objet lors de l’exécution. Le code suivant montre d’exemple comment la `ProfileViewModel` classe est structurée lors de l’utilisation d’injection de dépendances :

```csharp
public class ProfileViewModel : ViewModelBase  
{  
    private IOrderService _orderService;  

    public ProfileViewModel(IOrderService orderService)  
    {  
        _orderService = orderService;  
    }  
    ...  
}
```

Le `ProfileViewModel` constructeur reçoit un `IOrderService` instance en tant qu’argument, injecté par une autre classe. La seule dépendance dans la `ProfileViewModel` classe se trouve sur le type d’interface. Par conséquent, le `ProfileViewModel` classe n’a aucune connaissance de la classe qui est responsable de l’instanciation du `IOrderService` objet. La classe qui est responsable de l’instanciation du `IOrderService` de l’objet et en l’insérant dans le `ProfileViewModel` de classe, est appelé le *conteneur d’injection de dépendance*.

Conteneurs d’injection de dépendance de réduire le couplage entre les objets en fournissant une fonctionnalité pour instancier des instances de classe et de gérer leur durée de vie basée sur la configuration du conteneur. Lors de la création d’objets, le conteneur injecte toutes les dépendances de l’objet requiert dans celui-ci. Si ces dépendances n’ont pas encore été créés, le conteneur crée et résout tout d’abord de leurs dépendances.

> [!NOTE]
> Injection de dépendance peut également être implémentée manuellement à l’aide de fabriques. Toutefois, à l’aide d’un conteneur fournit des fonctionnalités supplémentaires telles que la gestion de la durée de vie et l’inscription via l’assembly de l’analyse.

Il existe plusieurs avantages à l’aide d’un conteneur d’injection de dépendance :

-   Un conteneur supprime la nécessité d’une classe localiser ses dépendances et de gérer leur durée de vie.
-   Un conteneur autorise le mappage des dépendances de mise en œuvre sans affecter la classe.
-   Un conteneur facilite la testabilité en autorisant les dépendances être fictive.
-   Un conteneur augmente la facilité de maintenance en permettant à nouvelles classes à être aisément ajouté pour l’application.

Dans le contexte d’une application de Xamarin.Forms par MVVM, un conteneur d’injection de dépendance généralement servira pour l’inscription et la résolution des modèles d’affichage et pour inscrire les services et les injecte des modèles d’affichage.

Il existe de nombreux conteneurs injection de dépendance, avec l’application mobile eShopOnContainers à l’aide de Autofac pour gérer l’instanciation du modèle d’affichage et des classes dans l’application de service. Autofac facilite la création d’applications faiblement couplées et fournit toutes les fonctionnalités couramment trouvées dans les conteneurs injection de dépendance, y compris les méthodes pour inscrire les mappages de types et instances d’objet, résoudre les objets, gérer la durée de vie des objets et injecter objets dépendants dans des constructeurs d’objets qu’elle résout. Pour plus d’informations sur Autofac, consultez [Autofac](http://autofac.readthedocs.io/en/latest/index.html) sur readthedocs.io.

Dans Autofac, le `IContainer` interface fournit le conteneur d’injection de dépendance. La figure 3-1 montre les dépendances lors de l’utilisation de ce conteneur, qui instancie un `IOrderService` de l’objet et les injecte dans le `ProfileViewModel` classe.

![](dependency-injection-images/dependencyinjection.png "Exemple de dépendances lors de l’utilisation d’injection de dépendance")

**La figure 3-1 :** dépendances lors de l’utilisation d’injection de dépendance

Lors de l’exécution, le conteneur doit savoir quelle implémentation de la `IOrderService` interface à instancier, avant de pouvoir instancier un `ProfileViewModel` objet. Cela implique :

-   Le conteneur de décider comment instancier un objet qui implémente le `IOrderService` interface. Il s’agit en tant que *inscription*.
-   Le conteneur de l’instanciation de l’objet qui implémente le `IOrderService` interface et le `ProfileViewModel` objet. Il s’agit en tant que *résolution*.

Par la suite, l’application se termine à l’aide de la `ProfileViewModel` objet et il devient disponibles pour le garbage collection. À ce stade, le garbage collector doit supprimer de la `IOrderService` si d’autres classes ne partagent pas la même instance de l’instance.

> [!TIP]
> Écrire le code indépendant de conteneur. Toujours essayer d’écrire du code indépendant du conteneur pour découpler l’application à partir du conteneur de dépendance spécifique qui est utilisé.

## <a name="registration"></a>Inscription

Avant de dépendances peuvent être injectées dans un objet, les types des dépendances doivent d’abord être enregistrés avec le conteneur. En général, l’inscription d’un type implique de passer le conteneur une interface et un type concret qui implémente l’interface.

Il existe deux façons d’enregistrer des types et les objets dans le conteneur dans le code :

-   Inscrire un type ou un mappage avec le conteneur. À la demande, le conteneur génère une instance du type spécifié.
-   Enregistrer un objet existant dans le conteneur comme un singleton. À la demande, le conteneur retourne une référence à l’objet existant.

> [!TIP]
> Conteneurs d’injection de dépendance ne sont pas toujours appropriés. Injection de dépendances présente une complexité supplémentaire et les exigences qui peuvent ne pas être utile ou appropriés aux petites applications. Si une classe n’a pas de dépendances ou n’est pas une dépendance pour d’autres types, il ne peut pas être utile à placer dans le conteneur. En outre, si une classe a un seul jeu de dépendances qui font partie intégrante de type et ne doit jamais changer, il ne peut pas être judicieux à placer dans le conteneur.

L’inscription des types qui requièrent l’injection de dépendance doit être exécutée dans une méthode unique dans une application, et cette méthode doit être appelée au début de cycle de vie de l’application pour vous assurer que l’application tient compte des dépendances entre ses classes. Dans l’application mobile eShopOnContainers cette opération est effectuée par le `ViewModelLocator` classe, quelles sont les builds le `IContainer` de l’objet et est la seule classe dans l’application qui conserve une référence à cet objet. L’exemple de code suivant montre comment l’application mobile eShopOnContainers déclare le `IContainer` de l’objet dans la `ViewModelLocator` classe :

```csharp
private static IContainer _container;
```

Types et les instances sont enregistrés dans le `RegisterDependencies` méthode dans la `ViewModelLocator` classe. Pour cela, commencez par créer un `ContainerBuilder` instance, ce qui est illustré dans l’exemple de code suivant :

```csharp
var builder = new ContainerBuilder();
```

Types et les instances sont alors inscrits avec le `ContainerBuilder` objet et l’exemple de code suivant montre la forme la plus courante de l’enregistrement du type :

```csharp
builder.RegisterType<RequestProvider>().As<IRequestProvider>();
```

Le `RegisterType` méthode présentée ici est mappé à un type d’interface à un type concret. Il indique au conteneur pour instancier un `RequestProvider` objet lorsqu’il instancie un objet qui requiert une injection d’une `IRequestProvider` via un constructeur.

Types concrets peuvent également être inscrits directement sans un mappage à partir d’un type d’interface, comme indiqué dans l’exemple de code suivant :

```csharp
builder.RegisterType<ProfileViewModel>();
```

Lorsque le `ProfileViewModel` type est résolu, le conteneur d’injecter des ses dépendances requises.

Autofac autorise également l’inscription d’instance, où le conteneur est chargé de maintenir une référence à une instance de singleton d’un type. Par exemple, l’exemple de code suivant montre comment l’application mobile eShopOnContainers enregistre le type concret à utiliser quand un `ProfileViewModel` instance requiert une `IOrderService` instance :

```csharp
builder.RegisterType<OrderService>().As<IOrderService>().SingleInstance();
```

Le `RegisterType` méthode présentée ici est mappé à un type d’interface à un type concret. Le `SingleInstance` méthode configure l’inscription afin que tous les objets dépendants reçoit la même instance partagée. Par conséquent, un seul `OrderService` instance existe dans le conteneur, qui est partagé par les objets qui nécessitent une injection d’une `IOrderService` via un constructeur.

L’inscription d’instance peut également être effectuée avec la `RegisterInstance` (méthode), qui est présenté dans l’exemple de code suivant :

```csharp
builder.RegisterInstance(new OrderMockService()).As<IOrderService>();
```

Le `RegisterInstance` méthode ci-après crée un nouvel `OrderMockService` de l’instance et l’inscrit avec le conteneur. Par conséquent, un seul `OrderMockService` instance existe dans le conteneur, qui est partagé par les objets qui nécessitent une injection d’une `IOrderService` via un constructeur.

Après l’inscription de type et l’instance, le `IContainer` objet doit être généré, qui est présenté dans l’exemple de code suivant :

```csharp
_container = builder.Build();
```

Appel de la `Build` méthode sur le `ContainerBuilder` instance génère un nouveau conteneur d’injection de dépendance qui contient les enregistrements qui ont été apportées.

>💡 **Conseil**: envisagez un `IContainer` comme étant immuable. Alors que Autofac fournit un `Update` pour mettre à jour des enregistrements dans un conteneur existant, cette méthode doit être évitée lorsque cela est possible. Il existe des risques à la modification d’un conteneur une fois qu’il est générée, en particulier si le conteneur a été utilisé. Pour plus d’informations, consultez [considérez un conteneur comme immuable](http://docs.autofac.org/en/latest/best-practices/#consider-a-container-as-immutable) sur readthedocs.io.

<a name="resolution" />

## <a name="resolution"></a>Résolution

Après l’inscription d’un type, il peut être résolu ou injecté en tant que dépendance. Lorsqu’un type est résolu et le conteneur doit créer une nouvelle instance, elle injecte toutes les dépendances dans l’instance.

En général, lorsqu’un type est résolu, une des trois choses se produit :

1.  Si le type n’a pas été enregistré, le conteneur lève une exception.
1.  Si le type a été enregistré comme un singleton, le conteneur retourne l’instance singleton. S’il s’agit de la première fois que le type est appelé, le conteneur crée si nécessaire et conserve une référence à celui-ci.
1.  Si le type n’a pas été enregistré comme un singleton, le conteneur retourne une nouvelle instance et ne conserve une référence à celui-ci.

Le code suivant montre d’exemple comment la `RequestProvider` type qui a été précédemment inscrits avec Autofac peut être résolu :

```csharp
var requestProvider = _container.Resolve<IRequestProvider>();
```

Dans cet exemple, Autofac est demandé pour résoudre le type concret pour la `IRequestProvider` type, ainsi que toutes les dépendances. En règle générale, le `Resolve` méthode est appelée lorsqu’une instance d’un type spécifique est requise. Pour plus d’informations sur le contrôle de la durée de vie des objets résolus, consultez [la gestion de la durée de vie des objets résolu](#managing_the_lifetime_of_resolved_objects).

L’exemple de code suivant montre comment l’application mobile eShopOnContainers instancie les types de modèle de vue et leurs dépendances :

```csharp
var viewModel = _container.Resolve(viewModelType);
```

Dans cet exemple, Autofac est invité à résoudre le type de modèle d’affichage pour un modèle de vue demandée et le conteneur permet également de résoudre toutes les dépendances. Lors de la résolution du `ProfileViewModel` est de type, la dépendance pour résoudre un `IOrderService` objet. Par conséquent, Autofac construit d’abord un `OrderService` de l’objet et qu’il passe ensuite au constructeur de la `ProfileViewModel` classe. Pour plus d’informations sur la façon dont l’application mobile eShopOnContainers construit vue modèles et les associe aux vues, consultez [créant automatiquement un modèle d’affichage avec un localisateur de modèle de vue](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically_creating_a_view_model_with_a_view_model_locator).

> [!NOTE]
> L’inscription et la résolution des types avec un conteneur a un coût en raison de l’utilisation du conteneur de la réflexion pour la création de chaque type, surtout si les dépendances sont en cours de reconstruction pour la navigation de chaque page dans l’application de performances. S’il existe de nombreuses ou en profondeur des dépendances, le coût de création peut augmenter de manière significative.

<a name="managing_the_lifetime_of_resolved_objects" />

## <a name="managing-the-lifetime-of-resolved-objects"></a>La gestion de la durée de vie des objets résolus

Après avoir inscrit un type, le comportement par défaut pour Autofac est pour créer une nouvelle instance du type inscrit chaque fois que le type est résolu, ou lorsque le mécanisme de dépendance injecte les instances dans les autres classes. Dans ce scénario, le conteneur ne contient une référence à l’objet résolu. Toutefois, lorsque vous inscrivez une instance, le comportement par défaut pour Autofac est pour gérer la durée de vie de l’objet comme un singleton. Par conséquent, l’instance reste dans la portée pendant que le conteneur est dans la portée et est supprimé lorsque le conteneur est hors de portée et est le garbage collector, ou lorsque le code supprime explicitement le conteneur.

Une étendue d’instance Autofac peut être utilisée pour spécifier le comportement de singleton pour un objet Autofac crée à partir d’un type inscrit. Étendues d’instance Autofac gérer les durées de vie objet instanciées par le conteneur. La portée d’instance par défaut pour le `RegisterType` méthode est la `InstancePerDependency` étendue. Toutefois, le `SingleInstance` étendue peut être utilisée avec le `RegisterType` (méthode), afin que le conteneur crée ou retourne une instance singleton de type lors de l’appel du `Resolve` (méthode). L’exemple de code suivant montre comment les Autofac est invité à en créer une instance singleton de la `NavigationService` classe :

```csharp
builder.RegisterType<NavigationService>().As<INavigationService>().SingleInstance();
```

La première fois que le `INavigationService` interface est résolu, le conteneur crée un `NavigationService` de l’objet et conserve une référence à celui-ci. Sur toutes les résolutions suivantes de la `INavigationService` interface, le conteneur retourne une référence à la `NavigationService` objet qui a été créé précédemment.

> [!NOTE]
> L’étendue SingleInstance supprime les objets créés lorsque le conteneur est supprimé.

Autofac comprend des étendues de l’instance supplémentaire. Pour plus d’informations, consultez [étendue de l’Instance](http://autofac.readthedocs.io/en/latest/lifetime/instance-scope.html) sur readthedocs.io.

## <a name="summary"></a>Récapitulatif

Injection de dépendances permet de découpler de types concrets à partir du code qui dépend de ces types. Il utilise généralement un conteneur qui contient une liste des inscriptions et des mappages entre les interfaces et les types abstraits et les types concrets qui implémentent ou étendent ces types.

Autofac facilite la création d’applications faiblement couplées et fournit toutes les fonctionnalités couramment trouvées dans les conteneurs injection de dépendance, y compris les méthodes pour inscrire les mappages de types et instances d’objet, résoudre les objets, gérer la durée de vie des objets et injecter objets dépendants dans des constructeurs d’objets, qu'il est résolu.


## <a name="related-links"></a>Liens associés

- [Téléchargez le livre électronique (PDF 2 Mo)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (sample)](https://github.com/dotnet-architecture/eShopOnContainers)
