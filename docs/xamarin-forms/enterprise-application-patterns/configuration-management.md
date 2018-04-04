---
title: Gestion des configurations
ms.prod: xamarin
ms.assetid: 50d6e780-e768-47f8-9361-3af11e56b87b
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 9318f86077c9cdb98e073e4816dae4fdabbfe568
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="configuration-management"></a>Gestion des configurations

Les paramètres permettent la séparation des données qui configure le comportement d’une application à partir du code, ce qui permet le comportement d’être modifiée sans la reconstruction de l’application. Il existe deux types de paramètres : paramètres de l’application et les paramètres de l’utilisateur.

Paramètres d’application sont des données qu’une application crée et gère. Il peut inclure des données telles que les points de terminaison de service web fixe, les clés de l’API et état d’exécution. Paramètres de l’application sont liées à l’existence de l’application et sont uniquement significatives pour cette application.

Paramètres utilisateur sont les paramètres personnalisables d’une application qui affectent le comportement de l’application et n’ont pas besoin de fréquent ajustement de nouveau. Par exemple, une application peut permettent l’utilisateur de spécifier où récupérer les données à partir d’et comment l’afficher à l’écran.

Xamarin.Forms contient un dictionnaire persistant qui peut être utilisé pour stocker les données de paramètres. Ce dictionnaire est accessible à l’aide de la [ `Application.Current.Properties` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.Properties/) propriété et toutes les données qui sont placées dans celui-ci est enregistré lorsque l’application passe en état de veille et est restaurée lors de l’application reprend ou redémarre. En outre, le [ `Application` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Application/) classe a également un [ `SavePropertiesAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.SavePropertiesAsync()/) méthode qui permet à une application possède ses paramètres enregistrés à la demande. Pour plus d’informations sur ce dictionnaire, consultez [dictionnaire de propriétés](~/xamarin-forms/app-fundamentals/application-class.md#Properties_Dictionary).

Le stockage des données à l’aide du dictionnaire persistant Xamarin.Forms présente un inconvénient est qu’il n’est pas facilement liées aux données. Par conséquent, l’application mobile eShopOnContainers utilise la bibliothèque Xam.Plugins.Settings, disponible à partir de [NuGet](https://www.nuget.org/packages/Xam.Plugins.Settings/). Cette bibliothèque fournit une approche cohérente, de type sécurisé et d’inter-plateformes pour conserver et récupérer des paramètres d’application et utilisateur, lors de l’utilisation de la gestion de paramètres natif fournie par chaque plateforme. En outre, il est simple à utiliser la liaison de données pour accéder aux données de paramètres exposées par la bibliothèque.

> [!NOTE]
> Alors que la bibliothèque Xam.Plugin.Settings peut stocker des paramètres d’application et utilisateur, il fait aucune distinction entre les deux.

## <a name="creating-a-settings-class"></a>Création d’une classe de paramètres

Lorsque vous utilisez la bibliothèque Xam.Plugins.Settings, une seule classe statique doit être créée qui contient les paramètres d’application et d’utilisateur requis par l’application. L’exemple de code suivant illustre la classe de paramètres dans l’application mobile eShopOnContainers :

```csharp
public static class Settings  
{  
    private static ISettings AppSettings  
    {  
        get  
        {  
            return CrossSettings.Current;  
        }  
    }  
    ...  
}
```

Les paramètres pouvant être lus et écrits via le `ISettings` API, qui est fournie par la bibliothèque Xam.Plugins.Settings. Cette bibliothèque fournit un singleton qui peut être utilisé pour accéder à l’API, `CrossSettings.Current`, et de la classe de paramètres d’une application doit exposer ce singleton via un `ISettings` propriété.

> [!NOTE]
> À l’aide des directives pour les espaces de noms Plugin.Settings et Plugin.Settings.Abstractions doit être ajouté à une classe qui requiert l’accès aux types de bibliothèque Xam.Plugins.Settings.

## <a name="adding-a-setting"></a>Ajout d’un paramètre

Chaque paramètre se compose d’une clé, une valeur par défaut et une propriété. L’exemple de code suivant montre les trois éléments d’un paramètre utilisateur qui représente l’URL de base pour les services en ligne l’application mobile eShopOnContainers se connecte à :

```csharp
public static class Settings  
{  
    ...  
    private const string IdUrlBase = "url_base";  
    private static readonly string UrlBaseDefault = GlobalSetting.Instance.BaseEndpoint;  
    ...  

    public static string UrlBase  
    {  
        get  
        {  
            return AppSettings.GetValueOrDefault<string>(IdUrlBase, UrlBaseDefault);  
        }  
        set  
        {  
            AppSettings.AddOrUpdateValue<string>(IdUrlBase, value);  
        }  
    }  
}
```

La clé est toujours une chaîne constante qui définit le nom de clé, la valeur par défaut pour le paramètre étant une valeur readonly statique du type requis. En fournissant une valeur par défaut garantit qu’une valeur valide est disponible si un paramètre non défini est récupéré.

Le `UrlBase` propriété statique utilise deux méthodes à partir de la `ISettings` API pour lire ou écrire la valeur du paramètre. Le `ISettings.GetValueOrDefault` méthode est utilisée pour récupérer une valeur de paramètre à partir du stockage de plateforme spécifique. Si aucune valeur n’est définie pour le paramètre, sa valeur par défaut est récupérée à la place. De même, la `ISettings.AddOrUpdateValue` méthode est utilisée pour conserver une valeur de paramètre vers le stockage spécifique à la plateforme.

Au lieu de cela, qui définissent une valeur par défaut à l’intérieur de la `Settings` (classe), le `UrlBaseDefault` chaîne obtienne sa valeur de la `GlobalSetting` classe. Montre l’exemple de code suit le `BaseEndpoint` propriété et `UpdateEndpoint` méthode dans cette classe :

```csharp
public class GlobalSetting  
{  
    ...  
    public string BaseEndpoint  
    {  
        get { return _baseEndpoint; }  
        set  
        {  
            _baseEndpoint = value;  
            UpdateEndpoint(_baseEndpoint);  
        }  
    }  
    ...  

    private void UpdateEndpoint(string baseEndpoint)  
    {  
        RegisterWebsite = string.Format("{0}:5105/Account/Register", baseEndpoint);  
        CatalogEndpoint = string.Format("{0}:5101", baseEndpoint);  
        OrdersEndpoint = string.Format("{0}:5102", baseEndpoint);  
        BasketEndpoint = string.Format("{0}:5103", baseEndpoint);  
        IdentityEndpoint = string.Format("{0}:5105/connect/authorize", baseEndpoint);  
        UserInfoEndpoint = string.Format("{0}:5105/connect/userinfo", baseEndpoint);  
        TokenEndpoint = string.Format("{0}:5105/connect/token", baseEndpoint);  
        LogoutEndpoint = string.Format("{0}:5105/connect/endsession", baseEndpoint);  
        IdentityCallback = string.Format("{0}:5105/xamarincallback", baseEndpoint);  
        LogoutCallback = string.Format("{0}:5105/Account/Redirecting", baseEndpoint);  
    }  
}
```

Chaque fois que le `BaseEndpoint` propriété est définie, la `UpdateEndpoint` méthode est appelée. Cette méthode met à jour une série de propriétés, qui sont basées sur les `UrlBase` paramètre utilisateur fourni par le `Settings` classe qui représentent des points de terminaison différents pour l’application mobile eShopOnContainers se connecte à.

## <a name="data-binding-to-user-settings"></a>Liaison de données aux paramètres utilisateur

Dans l’application mobile eShopOnContainers, le `SettingsView` expose deux paramètres d’utilisateur. Ces paramètres permettent la configuration de si l’application doit récupérer des microservices sont déployés en tant que conteneurs Docker, ou si l’application doit récupérer des données à partir des services fictives qui ne nécessitent pas une connexion internet. Lorsque vous choisissez récupérer des données à partir de microservices en conteneur, vous devez spécifier une URL de point de terminaison de base pour le microservices. Figure 7-1 indique la `SettingsView` lorsque l’utilisateur a choisi récupérer des données à partir de microservices en conteneur.

![](configuration-management-images/settings-endpoint.png "Paramètres utilisateur exposés par l’application mobile eShopOnContainers")

**Figure 7-1**: les paramètres utilisateur exposés par l’application mobile eShopOnContainers

Liaison de données peut être utilisée pour récupérer et définir les paramètres exposés par la `Settings` classe. Pour cela, les contrôles sur la liaison de vue pour afficher les propriétés de modèle qui à son tour accéder aux propriétés dans la `Settings` classe et le déclenchement d’une propriété modifiée notification si la valeur de paramètres a été modifié. Pour plus d’informations sur la façon dont l’application mobile eShopOnContainers construit vue modèles et les associe aux vues, consultez [créant automatiquement un modèle d’affichage avec un localisateur de modèle de vue](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically_creating_a_view_model_with_a_view_model_locator).

Ce qui suit montre l’exemple de code la [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) contrôle depuis la `SettingsView` qui permet à l’utilisateur à entrer une URL de point de terminaison de base pour le microservices en conteneur :

```xaml
<Entry Text="{Binding Endpoint, Mode=TwoWay}" />
```

Cela [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) contrôle est lié à la `Endpoint` propriété de la `SettingsViewModel` classe, à l’aide d’une liaison bidirectionnelle. L’exemple de code suivant montre la propriété de point de terminaison :

```csharp
public string Endpoint  
{  
    get { return _endpoint; }  
    set  
    {  
        _endpoint = value;  

        if(!string.IsNullOrEmpty(_endpoint))  
        {  
            UpdateEndpoint(_endpoint);  
        }  

        RaisePropertyChanged(() => Endpoint);  
    }  
}
```

Lorsque le `Endpoint` est définie la `UpdateEndpoint` est appelée, à condition que la valeur fournie est valide, et de modifier la propriété notification est déclenchée. Le code suivant exemple illustre la `UpdateEndpoint` méthode :

```csharp
private void UpdateEndpoint(string endpoint)  
{  
    Settings.UrlBase = endpoint;  
}
```

Cette méthode met à jour la `UrlBase` propriété dans la `Settings` classe avec la valeur d’URL de point de terminaison de base entré par l’utilisateur, de sorte qu’il soit rendu persistant dans le stockage de plateforme spécifique.

Lorsque le `SettingsView` est accédé, le `InitializeAsync` méthode dans la `SettingsViewModel` classe est exécutée. L’exemple de code suivant illustre cette méthode :

```csharp
public override Task InitializeAsync(object navigationData)  
{  
    ...  
    Endpoint = Settings.UrlBase;  
    ...  
}
```

La méthode affecte le `Endpoint` propriété à la valeur de la `UrlBase` propriété dans la `Settings` classe. L’accès à la `UrlBase` la propriété entraîne la bibliothèque Xam.Plugins.Settings extraire la valeur de paramètres de stockage spécifiques de la plateforme. Pour plus d’informations sur la façon dont `InitializeAsync` méthode est appelée, consultez [en passant les paramètres au cours de Navigation](~/xamarin-forms/enterprise-application-patterns/navigation.md#passing_parameters_during_navigation).

Ce mécanisme garantit que chaque fois qu’un utilisateur accède à la SettingsView, les paramètres utilisateur sont récupérées à partir de stockage spécifiques de la plate-forme et présentées via la liaison de données. Ensuite, si l’utilisateur modifie les valeurs de paramètres, la liaison de données permet de s’assurer qu’ils sont répercutées immédiatement à stockage spécifiques de la plateforme.

## <a name="summary"></a>Récapitulatif

Les paramètres permettent la séparation des données qui configure le comportement d’une application à partir du code, ce qui permet le comportement d’être modifiée sans la reconstruction de l’application. Paramètres d’application sont des données qu’une application crée et gère et paramètres utilisateur sont les paramètres personnalisables d’une application qui affectent le comportement de l’application et n’ont pas besoin de fréquent ajustement de nouveau.

La bibliothèque Xam.Plugins.Settings fournit un cohérent, de type sécurisé, approche multiplateforme pour conserver et récupérer l’application et les paramètres utilisateur et la liaison de données peut être utilisé pour accéder aux paramètres créés avec la bibliothèque.


## <a name="related-links"></a>Liens associés

- [Téléchargez le livre électronique (PDF 2 Mo)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (sample)](https://github.com/dotnet-architecture/eShopOnContainers)
