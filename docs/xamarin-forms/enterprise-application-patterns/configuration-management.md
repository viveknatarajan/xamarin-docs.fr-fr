---
title: Gestion des configurations
description: Ce chapitre explique comment l’application mobile eShopOnContainers implémente la gestion de la configuration afin de fournir des paramètres d’application et les paramètres utilisateur.
ms.prod: xamarin
ms.assetid: 50d6e780-e768-47f8-9361-3af11e56b87b
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 6f32d8f328232bdfc644da57bdb3201c60010063
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61381897"
---
# <a name="configuration-management"></a>Gestion des configurations

Les paramètres permettent la séparation des données qui configure le comportement d’une application à partir du code, ce qui permet le comportement d’être modifiée sans reconstruction de l’application. Il existe deux types de paramètres : paramètres d’application et les paramètres utilisateur.

Paramètres d’application sont des données qu’une application crée et gère. Il peut inclure des données telles que les points de terminaison de service web fixe, les clés d’API et état d’exécution. Paramètres d’application sont liées à l’existence de l’application et sont uniquement significatives pour cette application.

Paramètres utilisateur sont les paramètres personnalisables d’une application qui affectent le comportement de l’application et ne nécessitent pas ré-ajustement fréquente. Par exemple, une application peut permettre à l’utilisateur spécifier où extraire des données et comment l’afficher sur l’écran.

Xamarin.Forms contient un dictionnaire persistant qui peut être utilisé pour stocker les données de paramètres. Ce dictionnaire est accessible à l’aide de la [ `Application.Current.Properties` ](xref:Xamarin.Forms.Application.Properties) propriété et toutes les données qui sont placées dans celle-ci est enregistré lorsque l’application passe en état de veille et est restaurée lors de l’application reprend ou redémarre. En outre, le [ `Application` ](xref:Xamarin.Forms.Application) classe a également un [ `SavePropertiesAsync` ](xref:Xamarin.Forms.Application.SavePropertiesAsync) méthode qui permet à une application pour que ses paramètres enregistrés si nécessaire. Pour plus d’informations sur ce dictionnaire, consultez [dictionnaire de propriétés](~/xamarin-forms/app-fundamentals/application-class.md#Properties_Dictionary).

Un inconvénient de stocker des données avec le dictionnaire persistant Xamarin.Forms est qu’il n’est pas facilement lié aux données. Par conséquent, l’application mobile eShopOnContainers utilise la bibliothèque Xam.Plugins.Settings, disponible à partir de [NuGet](https://www.nuget.org/packages/Xam.Plugins.Settings/). Cette bibliothèque fournit une approche cohérente, de type sécurisé et d’inter-plateformes pour la persistance et la récupération des paramètres d’application et l’utilisateur, lors de l’utilisation de la gestion de paramètres natif fournie par chaque plateforme. En outre, il est simple à utiliser la liaison de données pour accéder aux données de paramètres exposées par la bibliothèque.

> [!NOTE]
> Alors que la bibliothèque de Xam.Plugin.Settings peut stocker des paramètres d’application et utilisateur, il fait aucune distinction entre les deux.

## <a name="creating-a-settings-class"></a>Création d’une classe de paramètres

Lorsque vous utilisez la bibliothèque Xam.Plugins.Settings, une classe statique unique doit être créée qui contient les paramètres d’application et l’utilisateur requis par l’application. L’exemple de code suivant montre la classe de paramètres dans l’application mobile eShopOnContainers :

```csharp
public static class Settings  
{  
    private static ISettings AppSettings  
    {  
        get  
        {  
            return CrossSettings.Current;  
        }  
    }  
    ...  
}
```

Paramètres peuvent être lue et modifiées par le biais du `ISettings` API, qui est fournie par la bibliothèque Xam.Plugins.Settings. Cette bibliothèque fournit un singleton qui peut être utilisé pour accéder à l’API, `CrossSettings.Current`, et de la classe de paramètres d’une application doit exposer ce singleton via un `ISettings` propriété.

> [!NOTE]
> À l’aide des directives pour les espaces de noms Plugin.Settings et Plugin.Settings.Abstractions doit être ajouté à une classe qui requiert l’accès aux types de bibliothèque Xam.Plugins.Settings.

## <a name="adding-a-setting"></a>Ajout d’un paramètre

Chaque paramètre se compose d’une clé, une valeur par défaut et une propriété. L’exemple de code suivant affiche tous les éléments de trois pour un paramètre utilisateur qui représente l’URL de base pour les services en ligne que l’application mobile eShopOnContainers se connecte à :

```csharp
public static class Settings  
{  
    ...  
    private const string IdUrlBase = "url_base";  
    private static readonly string UrlBaseDefault = GlobalSetting.Instance.BaseEndpoint;  
    ...  

    public static string UrlBase  
    {  
        get  
        {  
            return AppSettings.GetValueOrDefault<string>(IdUrlBase, UrlBaseDefault);  
        }  
        set  
        {  
            AppSettings.AddOrUpdateValue<string>(IdUrlBase, value);  
        }  
    }  
}
```

La clé est toujours une chaîne constante qui définit le nom de clé, la valeur par défaut pour le paramètre étant une valeur readonly statique du type requis. En fournissant une valeur par défaut garantit qu’une valeur valide est disponible si un paramètre d’annuler la définition est récupéré.

Le `UrlBase` propriété statique utilise deux méthodes à partir de la `ISettings` API pour lire ou écrire la valeur du paramètre. Le `ISettings.GetValueOrDefault` méthode est utilisée pour récupérer une valeur de paramètre à partir du stockage de la plateforme spécifique. Si aucune valeur n’est définie pour le paramètre, sa valeur par défaut est récupérée à la place. De même, la `ISettings.AddOrUpdateValue` méthode est utilisée pour conserver une valeur de paramètre dans le stockage spécifique à la plateforme.

Plutôt que définir une valeur par défaut à l’intérieur du `Settings` (classe), le `UrlBaseDefault` chaîne obtient sa valeur de la `GlobalSetting` classe. Le code suivant montre l’exemple le `BaseEndpoint` propriété et `UpdateEndpoint` méthode dans cette classe :

```csharp
public class GlobalSetting  
{  
    ...  
    public string BaseEndpoint  
    {  
        get { return _baseEndpoint; }  
        set  
        {  
            _baseEndpoint = value;  
            UpdateEndpoint(_baseEndpoint);  
        }  
    }  
    ...  

    private void UpdateEndpoint(string baseEndpoint)  
    {  
        RegisterWebsite = string.Format("{0}:5105/Account/Register", baseEndpoint);  
        CatalogEndpoint = string.Format("{0}:5101", baseEndpoint);  
        OrdersEndpoint = string.Format("{0}:5102", baseEndpoint);  
        BasketEndpoint = string.Format("{0}:5103", baseEndpoint);  
        IdentityEndpoint = string.Format("{0}:5105/connect/authorize", baseEndpoint);  
        UserInfoEndpoint = string.Format("{0}:5105/connect/userinfo", baseEndpoint);  
        TokenEndpoint = string.Format("{0}:5105/connect/token", baseEndpoint);  
        LogoutEndpoint = string.Format("{0}:5105/connect/endsession", baseEndpoint);  
        IdentityCallback = string.Format("{0}:5105/xamarincallback", baseEndpoint);  
        LogoutCallback = string.Format("{0}:5105/Account/Redirecting", baseEndpoint);  
    }  
}
```

Chaque fois que le `BaseEndpoint` propriété est définie, le `UpdateEndpoint` méthode est appelée. Cette méthode met à jour une série de propriétés, qui sont basées sur le `UrlBase` paramètre utilisateur fourni par le `Settings` classe qui représentent les différents points de terminaison auquel se connecte l’application mobile eShopOnContainers.

## <a name="data-binding-to-user-settings"></a>Liaison de données à des paramètres utilisateur

Dans l’application mobile eShopOnContainers, le `SettingsView` expose deux paramètres de l’utilisateur. Ces paramètres permettent la configuration d’indique si l’application doit récupérer des microservices sont déployés en tant que conteneurs Docker, ou si l’application doit récupérer des services fictifs qui ne nécessitent pas une connexion internet. Lorsque vous choisissez de récupérer des données à partir de microservices en conteneur, vous devez spécifier une URL de point de terminaison de base pour les microservices. Figure 7-1 montre la `SettingsView` lorsque l’utilisateur a choisi récupérer des données à partir de microservices en conteneur.

![](configuration-management-images/settings-endpoint.png "Paramètres utilisateur exposés par l’application mobile eShopOnContainers")

**Figure 7-1**: Paramètres utilisateur exposés par l’application mobile eShopOnContainers

Liaison de données peut être utilisée pour récupérer et définir les paramètres exposés par le `Settings` classe. Pour cela, les contrôles sur la liaison de vue pour afficher les propriétés de modèle qui à son tour accède aux propriétés dans le `Settings` classe et déclencher une propriété modifiée notification si la valeur de paramètres a changé. Pour plus d’informations sur la façon dont l’application mobile eShopOnContainers construit vue modèles et les associe aux vues, consultez [créant automatiquement un modèle de vue avec un localisateur de modèle de vue](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically_creating_a_view_model_with_a_view_model_locator).

Le code suivant montre l’exemple le [ `Entry` ](xref:Xamarin.Forms.Entry) contrôle depuis la `SettingsView` qui permet à l’utilisateur à entrer une URL de point de terminaison de base pour les microservices en conteneur :

```xaml
<Entry Text="{Binding Endpoint, Mode=TwoWay}" />
```

Cela [ `Entry` ](xref:Xamarin.Forms.Entry) contrôle se lie le `Endpoint` propriété de la `SettingsViewModel` classe, à l’aide d’une liaison bidirectionnelle. L’exemple de code suivant montre la propriété de point de terminaison :

```csharp
public string Endpoint  
{  
    get { return _endpoint; }  
    set  
    {  
        _endpoint = value;  

        if(!string.IsNullOrEmpty(_endpoint))  
        {  
            UpdateEndpoint(_endpoint);  
        }  

        RaisePropertyChanged(() => Endpoint);  
    }  
}
```

Lorsque le `Endpoint` propriété est définie la `UpdateEndpoint` est appelée, à condition que la valeur fournie est valide, et de modifier la propriété notification est déclenchée. L’exemple de code suivant montre la méthode `UpdateEndpoint` :

```csharp
private void UpdateEndpoint(string endpoint)  
{  
    Settings.UrlBase = endpoint;  
}
```

Cette méthode met à jour le `UrlBase` propriété dans la `Settings` classe avec la valeur d’URL de point de terminaison base entré par l’utilisateur, ce qui oblige ce dernier à être rendues persistantes dans le stockage spécifique à la plateforme.

Lorsque le `SettingsView` cible, le `InitializeAsync` méthode dans la `SettingsViewModel` classe est exécutée. L’exemple de code suivant illustre cette méthode :

```csharp
public override Task InitializeAsync(object navigationData)  
{  
    ...  
    Endpoint = Settings.UrlBase;  
    ...  
}
```

La méthode indique le `Endpoint` propriété à la valeur de la `UrlBase` propriété dans la `Settings` classe. L’accès à la `UrlBase` propriété entraîne la bibliothèque Xam.Plugins.Settings récupérer la valeur de paramètres à partir du stockage de la plateforme spécifique. Pour plus d’informations sur la façon dont `InitializeAsync` méthode est appelée, consultez [en passant les paramètres au cours de Navigation](~/xamarin-forms/enterprise-application-patterns/navigation.md#passing_parameters_during_navigation).

Ce mécanisme garantit que chaque fois qu’un utilisateur accède à la SettingsView, paramètres utilisateur sont récupérées à partir du stockage de spécifique à la plateforme et présentées via la liaison de données. Ensuite, si l’utilisateur modifie les valeurs de paramètres, la liaison de données permet de s’assurer qu’elles sont conservées immédiatement vers le stockage spécifique à la plateforme.

## <a name="summary"></a>Récapitulatif

Les paramètres permettent la séparation des données qui configure le comportement d’une application à partir du code, ce qui permet le comportement d’être modifiée sans reconstruction de l’application. Paramètres d’application sont des données qu’une application crée et gère et paramètres utilisateur sont les paramètres personnalisables d’une application qui affectent le comportement de l’application et ne nécessitent pas ré-ajustement fréquente.

La bibliothèque Xam.Plugins.Settings fournit une cohérence, de type sécurisé, approche d’inter-plateformes pour conserver et récupérer l’application et de paramètres utilisateur et de liaison de données peut être utilisée pour accéder aux paramètres créés avec la bibliothèque.


## <a name="related-links"></a>Liens associés

- [Téléchargez le livre électronique (PDF de 2 Mo)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (sample)](https://github.com/dotnet-architecture/eShopOnContainers)
