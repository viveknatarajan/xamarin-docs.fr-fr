---
title: Indexation de l’application et lien profond
description: Cet article montre comment utiliser l’indexation de l’application et lien profond pour faciliter la recherche sur des appareils iOS et Android de contenu de l’application Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 410C5D19-AA3C-4E0D-B799-E288C5803226
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 07/11/2016
ms.openlocfilehash: 7a102765a3633b8abaf01b3f090d8253230bc16b
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996094"
---
# <a name="application-indexing-and-deep-linking"></a>Indexation de l’application et lien profond

_Indexation de l’application permet aux applications seraient sinon être oubliées après quelques utilise pour rester à apparaître dans les résultats de recherche. Lien profond permet aux applications répondent à un résultat de recherche qui contient les données d’application, généralement en accédant à une page référencée à partir d’un lien ciblé. Cet article montre comment utiliser l’indexation de l’application et lien profond pour faciliter la recherche sur des appareils iOS et Android de contenu de l’application Xamarin.Forms._

> [!VIDEO https://youtube.com/embed/UJv4jUs7cJw]

**Profondeur établissant une liaison avec Xamarin.Forms et Azure, par [Xamarin University](https://university.xamarin.com/)**


L’indexation de l’application Xamarin.Forms et lien profond fournissent une API pour la publication des métadonnées pour l’indexation de l’application que les utilisateurs naviguent dans les applications. Contenu indexé peut alors être recherché dans la recherche Spotlight, dans la recherche de Google ou dans une recherche sur le web. Appuyant sur un résultat de recherche qui contient un lien profond déclenche un événement qui peut être géré par une application et est généralement utilisé pour accéder à la page référencée à partir du lien profond.

L’exemple d’application illustre une application de liste Todo où les données sont stockées dans une base de données SQLite locale, comme indiqué dans les captures d’écran suivante :

![](deep-linking-images/screenshots.png "Application de la liste des tâches")

Chaque `TodoItem` instance créée par l’utilisateur est indexé. Recherche spécifique à la plateforme peut ensuite servir à localiser les données indexées à partir de l’application. Lorsque l’utilisateur appuie sur un élément de résultat de recherche pour l’application, l’application est lancée, le `TodoItemPage` est accédé et le `TodoItem` référencé à partir de la profondeur lien s’affiche.

Pour plus d’informations sur l’utilisation d’une base de données SQLite, consultez [fonctionne avec une base de données locale](~/xamarin-forms/app-fundamentals/databases.md).

> [!NOTE]
> L’indexation de l’application Xamarin.Forms et la fonctionnalité de lien profond est uniquement disponibles sur les plateformes iOS et Android et nécessite iOS 9 et API 23 respectivement.

## <a name="setup"></a>Installation

Les sections suivantes fournissent les instructions d’installation supplémentaires pour l’utilisation de cette fonctionnalité sur les plateformes iOS et Android.

### <a name="ios"></a>iOS

Sur la plateforme iOS, il n’existe aucune configuration supplémentaire nécessaire pour utiliser cette fonctionnalité.

### <a name="android"></a>Android

Sur la plateforme Android, il existe un nombre de conditions préalables qui doivent être remplies pour utiliser l’indexation de l’application et la fonctionnalité de lien profond :

1. Une version de votre application doit être en direct sur Google Play.
1. Un site Web d’accompagnement doit être inscrit auprès de l’application dans la Console de développeur de Google. Une fois que l’application est associée à un site Web, les URL peuvent être indexés qui fonctionnent pour le site Web et l’application, qui peut ensuite être prises en charge dans les résultats de recherche. Pour plus d’informations, consultez [application l’indexation de la recherche Google](https://support.google.com/googleplay/android-developer/answer/6041489) sur le site Web de Google.
1. Votre application doit prendre en charge les intentions d’URL HTTP sur le `MainActivity` (classe), qui indiquent application indexation quels types de schémas de données d’URL de l’application peut répondre à. Pour plus d’informations, consultez [configuration du filtre d’intention](~/android/platform/app-linking.md#configure-intent-filter).

Une fois ces conditions préalables sont remplies, la configuration supplémentaire suivante est nécessaire pour utiliser l’indexation de l’application Xamarin.Forms et lien profond sur la plateforme Android :

1. Installer le [Xamarin.Forms.AppLinks](https://www.nuget.org/packages/Xamarin.Forms.AppLinks/) package NuGet dans le projet d’application Android.
1. Dans le `MainActivity.cs` fichier, importez le `Xamarin.Forms.Platform.Android.AppLinks` espace de noms.
1. Dans le `MainActivity.OnCreate` substitue, ajoutez la ligne suivante de code sous `Forms.Init(this, bundle)`:

```csharp
AndroidAppLinks.Init (this);
```

Pour plus d’informations, consultez [lien profond contenu avec la Navigation dans les URL Xamarin.Forms](https://blog.xamarin.com/deep-link-content-with-xamarin-forms-url-navigation/) sur le blog de Xamarin.

## <a name="indexing-a-page"></a>L’indexation d’une Page

Le processus d’indexation d’une page et l’exposition de la recherche Google et Spotlight est comme suit :

1. Créer un [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) qui contient les métadonnées nécessaires à la page, ainsi qu’un lien profond pour revenir à la page lorsque l’utilisateur sélectionne le contenu indexé dans les résultats de la recherche d’index.
1. Inscrire le [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) instance à l’index de recherche.

L’exemple de code suivant montre comment créer un [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) instance :

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

Le [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) instance contient un nombre de propriétés dont les valeurs sont requises pour la page d’index et créer un lien ciblé. Le [ `Title` ](xref:Xamarin.Forms.IAppLinkEntry.Title), [ `Description` ](xref:Xamarin.Forms.IAppLinkEntry.Description), et [ `Thumbnail` ](xref:Xamarin.Forms.IAppLinkEntry.Thumbnail) propriétés sont utilisées pour identifier le contenu indexé lorsqu’il apparaît dans les résultats de la recherche. Le [ `IsLinkActive` ](xref:Xamarin.Forms.IAppLinkEntry.IsLinkActive) propriété est définie sur `true` pour indiquer que le contenu indexé est actuellement affiché. Le [ `AppLinkUri` ](xref:Xamarin.Forms.IAppLinkEntry.AppLinkUri) propriété est un `Uri` qui contient les informations requises pour retourner à la page active et afficher les cours `TodoItem`. L’exemple suivant montre un exemple `Uri` pour l’exemple d’application :

```csharp
http://deeplinking/DeepLinking.TodoItemPage?id=ec38ebd1-811e-4809-8a55-0d028fce7819
```

Cela `Uri` contient toutes les informations requises pour lancer le `deeplinking` application, accédez à la `DeepLinking.TodoItemPage`et afficher le `TodoItem` qui a un `ID` de `ec38ebd1-811e-4809-8a55-0d028fce7819`.

## <a name="registering-content-for-indexing"></a>L’enregistrement de contenu pour l’indexation

Une fois un [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) instance a été créée, elle doit être inscrite pour l’indexation doit s’afficher dans les résultats de la recherche. Pour cela, avec la [ `RegisterLink` ](xref:Xamarin.Forms.IAppLinks.RegisterLink(Xamarin.Forms.IAppLinkEntry)) (méthode), comme illustré dans l’exemple de code suivant :

```csharp
Application.Current.AppLinks.RegisterLink (appLink);
```

Cette opération ajoute le [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) instance à l’application [ `AppLinks` ](xref:Xamarin.Forms.Application.AppLinks) collection.

> [!NOTE]
> Le `RegisterLink` méthode peut également être utilisée pour mettre à jour le contenu qui est été indexé pour une page.

Une fois un [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) instance a été inscrit pour l’indexation, elle peut apparaître dans les résultats de la recherche. La capture d’écran suivante montre le contenu indexé apparaître dans les résultats de recherche sur la plateforme iOS :

![](deep-linking-images/ios-search.png "Contenu indexé dans les résultats de recherche sur iOS")

## <a name="de-registering-indexed-content"></a>Annuler l’inscription d’un contenu indexé

Le [ `DeregisterLink` ](xref:Xamarin.Forms.IAppLinks.DeregisterLink(Xamarin.Forms.IAppLinkEntry)) méthode est utilisée pour supprimer le contenu indexé à partir des résultats de recherche, comme illustré dans l’exemple de code suivant :

```csharp
Application.Current.AppLinks.DeregisterLink (appLink);
```

Cette opération supprime le [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) instance à partir de l’application [ `AppLinks` ](xref:Xamarin.Forms.Application.AppLinks) collection.

> [!NOTE]
> Sur Android, il n’est pas possible de supprimer le contenu indexé à partir des résultats de recherche.

<a name="responding" />

## <a name="responding-to-a-deep-link"></a>Répondre à un lien profond

Lorsque le contenu indexé apparaît dans les résultats de recherche et est sélectionné par un utilisateur, le `App` classe pour l’application reçoit une demande pour gérer les `Uri` contenus dans le contenu indexé. Cette demande peut être traitée dans le [ `OnAppLinkRequestReceived` ](xref:Xamarin.Forms.Application.OnAppLinkRequestReceived(System.Uri)) remplacer, comme illustré dans l’exemple de code suivant :

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

Le [ `OnAppLinkRequestReceived` ](xref:Xamarin.Forms.Application.OnAppLinkRequestReceived(System.Uri)) méthode vérifie que la réponse reçue `Uri` est destiné à l’application, avant d’analyser le `Uri` dans la page permet de naviguer et le paramètre à passer à la page. Une instance de la page permet de naviguer est créé et le `TodoItem` représenté par la page paramètre est récupéré. Le [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) de la page permet de naviguer est ensuite définie sur le `TodoItem`. Cela garantit que quand le `TodoItemPage` est affiché par le [ `PushAsync` ](xref:Xamarin.Forms.INavigation.PushAsync(Xamarin.Forms.Page)) (méthode), il sera affiché le `TodoItem` dont `ID` est contenue dans le lien profond.

## <a name="making-content-available-for-search-indexing"></a>Ce qui rend le contenu disponible pour l’indexation de recherche

Chaque fois que la page représentée par un lien profond est affichée, le [ `AppLinkEntry.IsLinkActive` ](xref:Xamarin.Forms.IAppLinkEntry.IsLinkActive) propriété peut être définie sur `true`. Sur iOS et Android ainsi le [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) instance disponible pour l’indexation de recherche et sur iOS uniquement, elle rend également le `AppLinkEntry` instance disponible pour la remise. Pour plus d’informations sur la procédure de transfert, consultez [Introduction à la procédure de transfert](~/ios/platform/handoff.md).

L’exemple de code suivant montre comment définir le [ `AppLinkEntry.IsLinkActive` ](xref:Xamarin.Forms.IAppLinkEntry.IsLinkActive) propriété `true` dans le [ `Page.OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing) remplacer :

```csharp
protected override void OnAppearing ()
{
  appLink = GetAppLink (BindingContext as TodoItem);
  if (appLink != null) {
    appLink.IsLinkActive = true;
  }
}
```

De même, lorsque la page représentée par un lien profond quitte, le [ `AppLinkEntry.IsLinkActive` ](xref:Xamarin.Forms.IAppLinkEntry.IsLinkActive) propriété peut être définie sur `false`. Sur iOS et Android, cette commande arrête le [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) instance annoncé pour l’indexation de recherche et sur it uniquement, iOS également cesse de publicité le `AppLinkEntry` instance pour la remise. Cela peut être accompli dans le [ `Page.OnDisappearing` ](xref:Xamarin.Forms.Page.OnDisappearing) remplacer, comme illustré dans l’exemple de code suivant :

```csharp
protected override void OnDisappearing ()
{
  if (appLink != null) {
    appLink.IsLinkActive = false;
  }
}
```

## <a name="providing-data-to-handoff"></a>Fournir des données à la procédure de transfert

Sur iOS, les données spécifiques à l’application peuvent être stockées lors de l’indexation de la page. Pour cela, l’ajout de données à la [ `KeyValues` ](xref:Xamarin.Forms.IAppLinkEntry.KeyValues) collection, qui est un `Dictionary<string, string>` pour stocker des paires clé-valeur qui sont utilisés dans la procédure de transfert. Procédure de transfert est un moyen pour l’utilisateur de lancer une activité sur l’un de leurs appareils et continuer cette activité sur un autre de leurs appareils (tel qu’il est identifié par le compte d’utilisateur iCloud). Le code suivant montre un exemple de stockage de paires clé-valeur de spécifique à l’application :

```csharp
var pageLink = new AppLinkEntry {
  ...  
};
pageLink.KeyValues.Add("appName", App.AppName);
pageLink.KeyValues.Add("companyName", "Xamarin");
```

Valeurs stockées dans le [ `KeyValues` ](xref:Xamarin.Forms.IAppLinkEntry.KeyValues) collection seront stockée dans les métadonnées pour la page indexée et sera restaurée lorsque l’utilisateur appuie sur un résultat de recherche qui contient un lien ciblé (ou lorsque le transfert est utilisé pour afficher le contenu sur un autre connecté appareil).

En outre, les valeurs pour les clés suivantes peuvent être spécifiées :

- `contentType` – un `string` qui spécifie l’identificateur de type uniforme du contenu indexé. La convention recommandée à utiliser pour cette valeur est le nom de type de la page contenant le contenu indexé.
- `associatedWebPage` – un `string` qui représente la page web à visiter le contenu indexé peut également être affichée sur le web, ou si l’application prend en charge les liens profonds de Safari.
- `shouldAddToPublicIndex` – un `string` du `true` ou `false` qui contrôle s’il faut ajouter le contenu indexé à l’index de cloud public d’Apple, qui peut ensuite être présenté aux utilisateurs qui n’ont pas installé l’application sur son appareil iOS. Toutefois, parce que le contenu a été défini pour l’indexation public, cela ne signifie pas qu’il sera être ajouté automatiquement à l’index de cloud public d’Apple. Pour plus d’informations, consultez [Public l’indexation de recherche](~/ios/platform/search/nsuseractivity.md). Notez que cette clé doit être définie sur `false` lors de l’ajout des données personnelles à la [ `KeyValues` ](xref:Xamarin.Forms.IAppLinkEntry.KeyValues) collection.

> [!NOTE]
> Le `KeyValues` collection n’est pas utilisée sur la plateforme Android.

Pour plus d’informations sur la procédure de transfert, consultez [Introduction à la procédure de transfert](~/ios/platform/handoff.md).

## <a name="summary"></a>Récapitulatif

Cet article a montré comment utiliser l’indexation de l’application et lien profond pour faciliter la recherche sur des appareils iOS et Android de contenu de l’application Xamarin.Forms. Indexation de l’application permet aux applications rester à apparaître dans les résultats de recherche qui seraient sinon être oubliés après quelques utilise.


## <a name="related-links"></a>Liens associés

- [Création de liens approfondie (exemple)](https://developer.xamarin.com/samples/xamarin-forms/deeplinking/)
- [API recherche d’iOS](~/ios/platform/search/index.md)
- [La liaison application dans Android 6.0](~/android/platform/app-linking.md)
- [AppLinkEntry](xref:Xamarin.Forms.AppLinkEntry)
- [IAppLinkEntry](xref:Xamarin.Forms.IAppLinkEntry)
- [IAppLinks](xref:Xamarin.Forms.IAppLinks)
