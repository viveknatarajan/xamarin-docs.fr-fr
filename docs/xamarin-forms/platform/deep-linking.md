---
title: Indexation de l’application et lien profond
description: Cet article explique comment utiliser l’indexation de l’application et lien profond pour rendre le contenu de l’application Xamarin.Forms recherche sur des appareils iOS et Android.
ms.prod: xamarin
ms.assetid: 410C5D19-AA3C-4E0D-B799-E288C5803226
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 07/11/2016
ms.openlocfilehash: 9cc5177a585af1569385840ab8c370993984ca2b
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35242490"
---
# <a name="application-indexing-and-deep-linking"></a>Indexation de l’application et lien profond

_Indexation de l’application permet aux applications seraient autrement être oubliées après quelques utilise pour rester en apparaissant dans les résultats de la recherche. Lien profond permet à un résultat de recherche qui contient les données d’application, généralement en accédant à une page référencée à partir d’un lien vers des applications. Cet article explique comment utiliser l’indexation de l’application et lien profond pour rendre le contenu de l’application Xamarin.Forms recherche sur des appareils iOS et Android._

> [!VIDEO https://youtube.com/embed/UJv4jUs7cJw]

**Profondeur en liaison avec Xamarin.Forms et Azure, [University de Xamarin](https://university.xamarin.com/)**


L’indexation d’application Xamarin.Forms et lien profond fournissent une API pour la publication des métadonnées pour l’indexation des applications que les utilisateurs accèdent via des applications. Contenu indexé peut ensuite être recherché dans la recherche Spotlight, dans la recherche de Google ou dans une recherche sur le web. En appuyant sur un résultat de recherche qui contient un lien ciblé se déclenche un événement qui peut être géré par une application et est généralement utilisé pour accéder à la page référencée à partir du lien profond.

L’exemple d’application illustre une application de liste Todo où les données sont stockées dans une base de données SQLite locale, comme indiqué dans les captures d’écran suivants :

![](deep-linking-images/screenshots.png "Application de la liste des tâches")

Chaque `TodoItem` instance créée par l’utilisateur est indexé. Recherches spécifiques à une plateforme peut ensuite servir pour rechercher des données indexées à partir de l’application. Lorsque l’utilisateur appuie sur un élément de résultat de recherche pour l’application, l’application est lancée, le `TodoItemPage` cible et le `TodoItem` référencé à partir de la profondeur lien s’affiche.

Pour plus d’informations sur l’utilisation d’une base de données SQLite, consultez [fonctionne avec une base de données locale](~/xamarin-forms/app-fundamentals/databases.md).

> [!NOTE]
> L’indexation d’application Xamarin.Forms approfondie des fonctionnalités de liaison est uniquement disponible sur la plateforme iOS et Android utilisent et nécessite d’iOS 9 et API 23 respectivement.

## <a name="setup"></a>Installation

Les sections suivantes fournissent les instructions d’installation supplémentaires pour l’utilisation de cette fonctionnalité sur la plateforme iOS et Android utilisent.

### <a name="ios"></a>iOS

Sur la plateforme iOS, il n’existe aucune configuration supplémentaire nécessaire pour utiliser cette fonctionnalité.

### <a name="android"></a>Android

Sur la plateforme Android, il existe un nombre de conditions préalables qui doivent être remplies pour utiliser l’indexation de l’application et la profondeur des fonctionnalités de liaison :

1. Une version de votre application doit être active sur Google Play.
1. Un site Web d’accompagnement doit être inscrite auprès de l’application dans la Console de développement de Google. Une fois que l’application est associée à un site Web, l’URL peuvent être indexée qui fonctionnent pour le site Web et l’application, qui peut ensuite être traitée dans les résultats de la recherche. Pour plus d’informations, consultez [application l’indexation de recherche Google](https://support.google.com/googleplay/android-developer/answer/6041489) sur le site Web de Google.
1. Votre application doit prendre en charge les modes d’URL HTTP sur le `MainActivity` (classe), qui indiquent application indexation quels types de jeux de données d’URL de l’application peut répondre à. Pour plus d’informations, consultez [configuration du filtre intention](~/android/platform/app-linking.md#configure-intent-filter).

Lorsque ces conditions préalables sont réunies, le programme d’installation supplémentaire suivant est requis pour utiliser l’indexation d’application Xamarin.Forms et lien profond sur la plateforme Android :

1. Installer le [Xamarin.Forms.AppLinks](https://www.nuget.org/packages/Xamarin.Forms.AppLinks/) package NuGet dans le projet d’application Android.
1. Dans le `MainActivity.cs` de fichiers, importez le `Xamarin.Forms.Platform.Android.AppLinks` espace de noms.
1. Dans le `MainActivity.OnCreate` remplacer, ajoutez la ligne suivante de code sous `Forms.Init(this, bundle)`:

```csharp
AndroidAppLinks.Init (this);
```

Pour plus d’informations, consultez [approfondie contenu lien avec la Navigation dans les URL Xamarin.Forms](https://blog.xamarin.com/deep-link-content-with-xamarin-forms-url-navigation/) sur le blog de Xamarin.

## <a name="indexing-a-page"></a>L’indexation d’une Page

Le processus d’indexation d’une page et de l’exposer à la recherche de Google et Spotlight est comme suit :

1. Créer un [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) qui contient les métadonnées nécessaires à la page, avec un lien ciblé pour revenir à la page lorsque l’utilisateur sélectionne le contenu indexé dans les résultats de la recherche d’index.
1. Inscrire le [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) instance à l’index de recherche.

L’exemple de code suivant montre comment créer un [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) instance :

```csharp
AppLinkEntry GetAppLink (TodoItem item)
{
  var pageType = GetType ().ToString ();
  var pageLink = new AppLinkEntry {
    Title = item.Name,
    Description = item.Notes,
    AppLinkUri = new Uri (string.Format ("http://{0}/{1}?id={2}",
      App.AppName, pageType, WebUtility.UrlEncode (item.ID)), UriKind.RelativeOrAbsolute),
    IsLinkActive = true,
    Thumbnail = ImageSource.FromFile ("monkey.png")
  };

  return pageLink;
}
```

Le [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) instance contient un nombre de propriétés dont les valeurs sont requises pour la page d’index et de créer un lien ciblé. Le [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.Title/), [ `Description` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.Description/), et [ `Thumbnail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.Thumbnail/) propriétés sont utilisées pour identifier le contenu indexé lorsqu’il apparaît dans les résultats de la recherche. Le [ `IsLinkActive` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.IsLinkActive/) est définie sur `true` pour indiquer que le contenu indexé est actuellement affiché. Le [ `AppLinkUri` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.AppLinkUri/) propriété est un `Uri` qui contient les informations requises pour retourner à la page active et afficher actuel `TodoItem`. L’exemple suivant montre un exemple `Uri` pour l’exemple d’application :

```csharp
http://deeplinking/DeepLinking.TodoItemPage?id=ec38ebd1-811e-4809-8a55-0d028fce7819
```

Cela `Uri` contient toutes les informations requises pour lancer le `deeplinking` application, accédez à la `DeepLinking.TodoItemPage`et afficher le `TodoItem` qui a un `ID` de `ec38ebd1-811e-4809-8a55-0d028fce7819`.

## <a name="registering-content-for-indexing"></a>L’inscription de contenu pour l’indexation

Une fois un [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) instance a été créée, elle doit être inscrite pour l’indexation doit s’afficher dans les résultats de la recherche. Pour cela, avec la [ `RegisterLink` ](https://developer.xamarin.com/api/member/Xamarin.Forms.IAppLinks.RegisterLink/p/Xamarin.Forms.IAppLinkEntry/) méthode, comme illustré dans l’exemple de code suivant :

```csharp
Application.Current.AppLinks.RegisterLink (appLink);
```

Cette opération ajoute le [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) instance à l’application [ `AppLinks` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.AppLinks/) collection.

> [!NOTE]
> Le `RegisterLink` méthode peut également être utilisée pour mettre à jour le contenu indexé pour la page.

Une fois un [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) instance a été inscrit pour l’indexation, elle peut apparaître dans les résultats de la recherche. La capture d’écran suivante montre le contenu indexé apparaissant dans les résultats de recherche sur la plateforme iOS :

![](deep-linking-images/ios-search.png "Contenu indexé dans les résultats de recherche sur iOS")

## <a name="de-registering-indexed-content"></a>Annuler l’inscription d’un contenu indexé

Le [ `DeregisterLink` ](https://developer.xamarin.com/api/member/Xamarin.Forms.IAppLinks.DeregisterLink/p/Xamarin.Forms.IAppLinkEntry/) méthode est utilisée pour supprimer le contenu indexé à partir des résultats de recherche, comme illustré dans l’exemple de code suivant :

```csharp
Application.Current.AppLinks.DeregisterLink (appLink);
```

Cette opération supprime le [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) instance à partir de l’application [ `AppLinks` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.AppLinks/) collection.

> [!NOTE]
> Sur Android, il n’est pas possible de supprimer le contenu indexé à partir des résultats de la recherche.

<a name="responding" />

## <a name="responding-to-a-deep-link"></a>Réponse à un lien ciblé

Lorsque le contenu indexé apparaît dans les résultats de la recherche et est sélectionné par un utilisateur, le `App` de classe pour l’application reçoit une demande pour gérer les `Uri` contenus dans le contenu indexé. Cette demande peut être traitée dans le [ `OnAppLinkRequestReceived` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.OnAppLinkRequestReceived/p/System.Uri/) remplacer, comme illustré dans l’exemple de code suivant :

```csharp
public class App : Application
{
  ...

  protected override async void OnAppLinkRequestReceived (Uri uri)
  {
    string appDomain = "http://" + App.AppName.ToLowerInvariant () + "/";
    if (!uri.ToString ().ToLowerInvariant ().StartsWith (appDomain)) {
      return;
    }

    string pageUrl = uri.ToString ().Replace (appDomain, string.Empty).Trim ();
    var parts = pageUrl.Split ('?');
    string page = parts [0];
    string pageParameter = parts [1].Replace ("id=", string.Empty);

    var formsPage = Activator.CreateInstance (Type.GetType (page));
    var todoItemPage = formsPage as TodoItemPage;
    if (todoItemPage != null) {
      var todoItem = App.Database.Find (pageParameter);
      todoItemPage.BindingContext = todoItem;
      await MainPage.Navigation.PushAsync (formsPage as Page);
    }

    base.OnAppLinkRequestReceived (uri);
  }
}
```

Le [ `OnAppLinkRequestReceived` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.OnAppLinkRequestReceived/p/System.Uri/) méthode vérifie que les données reçues `Uri` est destiné à l’application, avant d’analyser le `Uri` dans la page pour être accédé et le paramètre à passer à la page. Une instance de la page pour permettre une navigation est créé et le `TodoItem` représenté par la page de paramètre est récupéré. Le [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) de la page pour être la cible de la navigation est ensuite affectée à la `TodoItem`. Cela garantit que quand le `TodoItemPage` est affiché par le [ `PushAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushAsync/p/Xamarin.Forms.Page/) (méthode), il sera affiché le `TodoItem` dont `ID` est contenue dans le lien ciblé.

## <a name="making-content-available-for-search-indexing"></a>Rendre le contenu disponible pour l’indexation de recherche

Chaque fois que la page représentée par un lien ciblé s’affiche, le [ `AppLinkEntry.IsLinkActive` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.IsLinkActive/) propriété peut être définie `true`. Sur iOS et Android, cela rend le [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) instance disponible pour l’indexation de recherche et iOS uniquement, en rendant le `AppLinkEntry` instance disponible pour la transférer. Pour plus d’informations sur la procédure de transfert, consultez [Introduction à la procédure de transfert](~/ios/platform/handoff.md).

L’exemple de code suivant illustre la définition du [ `AppLinkEntry.IsLinkActive` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.IsLinkActive/) propriété `true` dans les [ `Page.OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing()/) remplacer :

```csharp
protected override void OnAppearing ()
{
  appLink = GetAppLink (BindingContext as TodoItem);
  if (appLink != null) {
    appLink.IsLinkActive = true;
  }
}
```

De même, lorsque la page représentée par un lien ciblé utilisateur quitte, le [ `AppLinkEntry.IsLinkActive` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.IsLinkActive/) propriété peut être définie `false`. Sur iOS et Android, cette commande arrête le [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) instance publié également pour l’indexation de recherche et sur iOS que, elle arrête la publicité la `AppLinkEntry` instance pour la transférer. Cela peut être accompli dans le [ `Page.OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing()/) remplacer, comme illustré dans l’exemple de code suivant :

```csharp
protected override void OnDisappearing ()
{
  if (appLink != null) {
    appLink.IsLinkActive = false;
  }
}
```

## <a name="providing-data-to-handoff"></a>Fournir des données à transférer

Sur iOS, les données spécifiques à l’application peuvent être stockées lors de l’indexation de la page. Pour cela, l’ajout de données à la [ `KeyValues` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.KeyValues/) collection, qui est un `Dictionary<string, string>` pour stocker les paires clé-valeur qui sont utilisés dans la procédure de transfert. Procédure de transfert est un moyen de l’utilisateur démarrer une activité sur l’un de leurs appareils et continuer à cette activité sur un autre de leurs appareils (tel qu’identifié par le compte d’utilisateur iCloud). Le code suivant montre un exemple de stocker des paires clé-valeur de spécifiques à l’application :

```csharp
var pageLink = new AppLinkEntry {
  ...  
};
pageLink.KeyValues.Add("appName", App.AppName);
pageLink.KeyValues.Add("companyName", "Xamarin");
```

Les valeurs stockées dans le [ `KeyValues` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.KeyValues/) collection seront stockée dans les métadonnées pour la page indexée et sera restaurée lorsque l’utilisateur appuie sur un résultat de recherche qui contient un lien ciblé (ou lorsque la remise est utilisé pour afficher le contenu sur un autre signature de périphérique).

En outre, les valeurs pour les clés suivantes peuvent être spécifiées :

- `contentType` – un `string` qui spécifie l’identificateur de type uniforme du contenu indexé. La convention recommandée à utiliser pour cette valeur est le nom de type de la page contenant le contenu indexé.
- `associatedWebPage` – un `string` qui représente la page web à visiter si le contenu indexé peut également être affiché sur le web, ou si l’application prend en charge des liens profonds de Safari.
- `shouldAddToPublicIndex` – un `string` du `true` ou `false` qui contrôle ou non ajouter le contenu indexé à index de cloud public d’Apple, qui peut ensuite être présenté aux utilisateurs qui n’ont pas installé l’application sur son appareil iOS. Toutefois, parce que le contenu a été défini pour l’indexation de public, cela ne signifie pas qu’il sera être ajouté automatiquement à index de cloud public d’Apple. Pour plus d’informations, consultez [Public l’indexation de recherche](~/ios/platform/search/nsuseractivity.md). Notez que cette clé doit être définie sur `false` lors de l’ajout des données personnelles le [ `KeyValues` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.KeyValues/) collection.

> [!NOTE]
> Le `KeyValues` collection n’est pas utilisé sur la plateforme Android.

Pour plus d’informations sur la procédure de transfert, consultez [Introduction à la procédure de transfert](~/ios/platform/handoff.md).

## <a name="summary"></a>Récapitulatif

Cet article a montré comment utiliser l’indexation de l’application et lien profond pour rendre le contenu de l’application Xamarin.Forms recherche sur des appareils iOS et Android. Indexation de l’application permet de rester par apparaissant dans les résultats de recherche qui seraient sinon être oubliés après quelques utilise des applications.


## <a name="related-links"></a>Liens associés

- [Création de liens complète (exemple)](https://developer.xamarin.com/samples/xamarin-forms/deeplinking/)
- [iOS API de recherche](~/ios/platform/search/index.md)
- [La liaison App dans Android 6.0](~/android/platform/app-linking.md)
- [AppLinkEntry](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/)
- [IAppLinkEntry](https://developer.xamarin.com/api/type/Xamarin.Forms.IAppLinkEntry/)
- [IAppLinks](https://developer.xamarin.com/api/type/Xamarin.Forms.IAppLinks/)
