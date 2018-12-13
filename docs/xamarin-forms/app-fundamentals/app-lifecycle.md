---
title: Cycle de vie des applications Xamarin.Forms
description: Cet article explique comment répondre au cycle de vie des applications, notamment concernant les méthodes de cycle de vie, les événements de navigation de page et les événements de navigation modale.
ms.prod: xamarin
ms.assetid: 69B416CF-B243-4790-AB29-F030B32465BE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/31/2018
ms.openlocfilehash: 5bdce8d1752b3d7273ffec233b120266909999c4
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50675118"
---
# <a name="xamarinforms-app-lifecycle"></a>Cycle de vie des applications Xamarin.Forms

La classe de base [`Application`](xref:Xamarin.Forms.Application) offre les fonctionnalités suivantes :

* [Méthodes de cycle de vie](#Lifecycle_Methods) `OnStart`, `OnSleep` et `OnResume`.
* [Événements de navigation de page](#page) [`PageAppearing`](xref:Xamarin.Forms.Application.PageAppearing), [`PageDisappearing`](xref:Xamarin.Forms.Application.PageDisappearing).
* [Événements de navigation modale](#modal) `ModalPushing`, `ModalPushed`, `ModalPopping` et `ModalPopped`.

<a name="Lifecycle_Methods" />

## <a name="lifecycle-methods"></a>Méthodes de cycle de vie

La classe [`Application`](xref:Xamarin.Forms.Application) contient trois méthodes virtuelles qui peuvent être substituées pour gérer les méthodes de cycle de vie :

* **OnStart** : appelé au démarrage de l’application.

* **OnSleep** : appelé chaque fois que l’application passe à l’arrière-plan.

* **OnResume** : appelé lors de la reprise de l’application, après son envoi en arrière-plan.

Notez qu’il n’existe *aucune* méthode pour arrêter l’application.
Dans des circonstances normales (c’est-à-dire, en dehors d’un plantage), l’arrêt de l’application est effectué à partir de l’état *OnSleep*, sans autre notification de votre code.

Pour savoir à quel moment ces méthodes sont appelées, implémentez un appel `WriteLine` dans chacune d’elle (comme indiqué ci-dessous) et testez-les sur chaque plateforme.

```csharp
protected override void OnStart()
{
    Debug.WriteLine ("OnStart");
}
protected override void OnSleep()
{
    Debug.WriteLine ("OnSleep");
}
protected override void OnResume()
{
    Debug.WriteLine ("OnResume");
}
```

Lorsque vous mettez à jour des applications Xamarin.Forms *plus anciennes* (c’est-à-dire, créées avec Xamarin.Forms 1.3 ou une version antérieure), vérifiez que l’activité principale Android comprend `ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation` dans l’attribut `[Activity()]`. Si ce n’est pas le cas, vous verrez que la méthode `OnStart` est appelée au moment de la rotation et lors du premier démarrage de l’application. Cet attribut est automatiquement inclus dans les modèles d’application Xamarin.Forms actuels.

<a name="page" />

## <a name="page-navigation-events"></a>Événements de navigation de page

Dans la classe [`Application`](xref:Xamarin.Forms.Application), deux événements fournissent une notification d’apparition et de disparition de pages :

- [`PageAppearing`](xref:Xamarin.Forms.Application.PageAppearing) : déclenché lorsqu’une page est sur le point de s’afficher à l’écran.
- [`PageDisappearing`](xref:Xamarin.Forms.Application.PageDisappearing) : déclenché lorsqu’une page est sur le point de disparaître de l’écran.

Ces événements peuvent être utilisés lorsque vous souhaitez effectuer le suivi des pages qui s’affichent à l’écran.

> [!NOTE]
> Les événements [`PageAppearing`](xref:Xamarin.Forms.Application.PageAppearing) et [`PageDisappearing`](xref:Xamarin.Forms.Application.PageDisappearing) sont déclenchés à partir de la classe de base [`Page`](xref:Xamarin.Forms.Page) immédiatement après les événements [`Page.Appearing`](xref:Xamarin.Forms.Page.Appearing) et [`Page.Disappearing`](xref:Xamarin.Forms.Page.Disappearing), respectivement.

<a name="modal" />

## <a name="modal-navigation-events"></a>Événements de navigation modale

La classe [`Application`](xref:Xamarin.Forms.Application) comprend quatre événements qui ont chacun leurs propres arguments d’événement, et qui vous permettent de répondre aux pages modales affichées et ignorées :

* **ModalPushing** - `ModalPushingEventArgs`
* **ModalPushed** - `ModalPushedEventArgs`
* **ModalPopping** : la classe `ModalPoppingEventArgs` contient une propriété `Cancel`. Lorsque `Cancel` est défini sur `true`, la fenêtre contextuelle modale est annulée.
* **ModalPopped** - `ModalPoppedEventArgs`

> [!NOTE]
> Pour implémenter les méthodes de cycle de vie des applications et les événements de navigation modale, toutes les méthodes de création d’applications Xamarin.Forms pré-`Application` (autrement dit, les applications écrites avec une version égale ou antérieure à la version 1.2 et qui utilisent une méthode statique `GetMainPage`) ont été mises à jour pour créer un `Application` par défaut qui est défini comme parent de `MainPage`.
>
> Les applications Xamarin.Forms qui utilisent ce comportement hérité doivent être mises à jour vers une sous-classe `Application`, comme décrit dans la page [Classe Application](~/xamarin-forms/app-fundamentals/application-class.md).
