---
title: Xamarin.Forms multi-écran - En profondeur
description: Cet article présente la navigation entre les pages et la liaison de données dans une application Xamarin.Forms, et illustre leur utilisation dans une application multiplateforme et multi-écran.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: e4faa36c-6600-48c0-94c4-b4431103a4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/02/2016
ms.openlocfilehash: 1c7edff3c71b9d7530b2acf21acaa06149156d43
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35242233"
---
# <a name="xamarinforms-multiscreen-deep-dive"></a>Xamarin.Forms multi-écran - En profondeur

Dans [Xamarin.Forms multi-écran - Démarrage rapide](~/xamarin-forms/get-started/hello-xamarin-forms-multiscreen/quickstart.md), l’application Phoneword a été étendue pour inclure un second écran qui effectue le suivi de l’historique des appels pour l’application. Cet article passe en revue les éléments générés afin de comprendre la navigation entre les pages et la liaison de données dans une application Xamarin.Forms.

## <a name="navigation"></a>Navigation

Xamarin.Forms fournit un modèle de navigation intégré qui gère la navigation et l’expérience utilisateur avec une pile de pages. Ce modèle implémente une pile d’objets `Page` LIFO (dernier entré, premier sorti). Pour passer d’une page à une autre, une application envoie une nouvelle page dans cette pile. Pour revenir à la page précédente, l’application dépile la page actuelle.

Xamarin.Forms a une classe [`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) qui gère la pile d’objets [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/). La classe `NavigationPage` ajoute également une barre de navigation en haut de la page qui affiche un titre et un bouton <span class="uiitem">Précédent</span> approprié à la plateforme qui permet de revenir à la page précédente. L’exemple de code suivant montre comment envelopper une classe `NavigationPage` autour de la première page dans une application :

```csharp
public App ()
{
    ...
    MainPage = new NavigationPage (new MainPage ());
}
```

Toutes les instances de [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) ont une propriété [`Navigation`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) qui expose des méthodes pour modifier la pile de pages. Ces méthodes ne doivent être appelées que si l’application inclut une classe [`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/). Pour accéder à `CallHistoryPage`, il est nécessaire d’appeler la méthode [`PushAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PushAsync/p/Xamarin.Forms.Page/) comme illustré dans l’exemple de code ci-dessous :

```csharp
async void OnCallHistory(object sender, EventArgs e)
{
    await Navigation.PushAsync (new CallHistoryPage ());
}
```

Le nouvel objet `CallHistoryPage` est ainsi envoyé dans la pile de navigation. Pour retourner par programmation à la page d’origine, l’objet `CallHistoryPage` doit appeler la méthode [`PopAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopAsync()/), comme illustré dans l’exemple de code ci-dessous :

```csharp
await Navigation.PopAsync();
```

Toutefois, dans l’application Phoneword, ce code n’est pas nécessaire parce que la classe [`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) ajoute une barre de navigation en haut de la page qui comprend un bouton <span class="uiitem">Précédent</span> approprié à la plateforme qui permet de revenir à la page précédente.

## <a name="data-binding"></a>Liaison de données

La liaison de données permet de simplifier la façon dont une application Xamarin.Forms affiche ses données et interagit avec elles. Elle établit une connexion entre l’interface utilisateur et l’application sous-jacente. La classe [`BindableObject`](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/) contient une grande partie de l’infrastructure pour prendre en charge la liaison de données.

La liaison de données définit la relation entre deux objets. L’objet *source* fournit des données. L’objet *cible* consomme (et affiche souvent) les données de l’objet source. Dans l’application Phoneword, la cible de liaison est le contrôle [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) qui affiche les numéros de téléphone, tandis que la collection `PhoneNumbers` est la source de liaison.

La collection `PhoneNumbers` est déclarée et initialisée dans la classe `App`, comme indiqué dans l’exemple de code suivant :

```csharp
public partial class App : Application
{
   public static List<string> PhoneNumbers { get; set; }

   public App ()
   {
     PhoneNumbers = new List<string>();
     ...
   }
   ...
}
```

L’instance de [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) est déclarée et initialisée dans la classe `CallHistoryPage`, comme indiqué dans l’exemple de code suivant :

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage ...
             xmlns:local="clr-namespace:Phoneword;assembly=Phoneword"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             ...>
    ...
    <ContentPage.Content>
       ...
       <ListView ItemsSource="{x:Static local:App.PhoneNumbers}" />
       ...
    </ContentPage.Content>
</ContentPage>
```

Dans cet exemple, le contrôle [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) affiche la collection `IEnumerable` de données à laquelle la propriété [`ItemsSource`](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView.ItemsSource/) est liée. La collection de données peut être des objets de tout type mais, par défaut, `ListView` utilise la méthode `ToString` de chaque élément pour afficher cet élément. L’extension de balisage [`x:Static`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.StaticExtension/) est utilisée pour indiquer que la propriété `ItemsSource` sera liée à la propriété `PhoneNumbers` statique de la classe `App`, qui se trouve dans l’espace de noms `local`.

Pour plus d’informations sur la liaison de données, consultez [Notions de base de la liaison de données](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md). Pour plus d’informations sur les extensions de balisage XAML, consultez [Extensions de balisage XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md).

## <a name="additional-concepts-introduced-in-phoneword"></a>Autres concepts introduits dans Phoneword

Le contrôle [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) est chargé de l’affichage d’une collection d’éléments à l’écran. Chaque élément dans `ListView` est contenu dans une cellule unique. Pour plus d’informations sur l’utilisation du contrôle `ListView`, consultez [ListView](~/xamarin-forms/user-interface/listview/index.md).

## <a name="summary"></a>Récapitulatif

Cet article a présenté la navigation entre les pages et la liaison de données dans une application Xamarin.Forms, et illustré leur utilisation dans une application multiplateforme et multi-écran.
