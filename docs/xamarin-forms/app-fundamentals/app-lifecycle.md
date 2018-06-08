---
title: Cycle de vie
description: Comment répondre à un cycle de vie des applications
ms.prod: xamarin
ms.assetid: 69B416CF-B243-4790-AB29-F030B32465BE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/31/2018
ms.openlocfilehash: a22ad8f3f272212f5c7f088ba2112f2771ff4a7f
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34846342"
---
# <a name="app-lifecycle"></a>Cycle de vie

Le [ `Application` ](xref:Xamarin.Forms.Application) classe de base offre les fonctionnalités suivantes :

* [Les méthodes de cycle de vie](#Lifecycle_Methods) `OnStart`, `OnSleep`, et `OnResume`.
* [Page événements de navigation](#page) [ `PageAppearing` ](xref:Xamarin.Forms.Application.PageAppearing), [ `PageDisappearing` ](xref:Xamarin.Forms.Application.PageDisappearing).
* [Événements de navigation modale](#modal) `ModalPushing`, `ModalPushed`, `ModalPopping`, et `ModalPopped`.

<a name="Lifecycle_Methods" />

## <a name="lifecycle-methods"></a>Méthodes de cycle de vie

Le [ `Application` ](xref:Xamarin.Forms.Application) classe contient trois méthodes virtuelles qui peuvent être substituées pour gérer les méthodes de cycle de vie :

* **OnStart** -appelée lorsque l’application démarre.

* **OnSleep** -appelée chaque fois que l’application passe à l’arrière-plan.

* **OnResume** -appelée lors de la reprise de l’application, après l’envoi à l’arrière-plan.

Notez qu’il existe *aucune* méthode pour l’arrêt de l’application.
Dans des circonstances normales (ie. pas un incident) arrêt de l’application se produit à partir de la *OnSleep* état, sans les notifications supplémentaires à votre code.

Pour observer lorsque ces méthodes sont appelées, implémentez un `WriteLine` appeler dans chaque (comme indiqué ci-dessous) et de test sur chaque plateforme.

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

Lors de la mise à jour *antérieure* applications Xamarin.Forms (par exemple). créer avec Xamarin.Forms 1.3 ou antérieure), vérifiez que l’activité principale Android inclut `ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation` dans le `[Activity()]` attribut. Si ce n’est pas présent, vous pourrez observer la `OnStart` méthode est appelée sur la rotation ainsi qu’au premier démarrage de l’application. Cet attribut est automatiquement inclus dans les modèles d’application Xamarin.Forms actuels.

<a name="page" />

## <a name="page-navigation-events"></a>Événements de Navigation de page

Il existe deux événements sur le [ `Application` ](xref:Xamarin.Forms.Application) classe qui fournissent une notification de pages apparaît et disparaît :

- [`PageAppearing`](xref:Xamarin.Forms.Application.PageAppearing) -déclenché lorsqu’une page est sur le point de s’affichent à l’écran.
- [`PageDisappearing`](xref:Xamarin.Forms.Application.PageDisappearing) -déclenché lorsqu’une page est sur le point de disparaître de l’écran.

Ces événements peuvent être utilisés dans les scénarios où vous souhaitez effectuer le suivi des pages comme ils apparaissent sur l’écran.

> [!NOTE]
> Le [ `PageAppearing` ](xref:Xamarin.Forms.Application.PageAppearing) et [ `PageDisappearing` ](xref:Xamarin.Forms.Application.PageDisappearing) sont déclenchés à partir de la [ `Page` ](xref:Xamarin.Forms.Page) classe de base immédiatement après le [ `Page.Appearing` ](xref:Xamarin.Forms.Page.Appearing) et [ `Page.Disappearing` ](xref:Xamarin.Forms.Page.Disappearing) événements, respectivement.

<a name="modal" />

## <a name="modal-navigation-events"></a>Événements de Navigation modale

Il existe quatre événements sur le [ `Application` ](xref:Xamarin.Forms.Application) (classe), chacun ayant leurs propres arguments d’événement, qui vous permettent de répondre aux pages modales qui est indiqué et sera ignoré :

* **ModalPushing** - `ModalPushingEventArgs`
* **ModalPushed** - `ModalPushedEventArgs`
* **ModalPopping** - le `ModalPoppingEventArgs` classe contient un `Cancel` propriété. Lorsque `Cancel` a la valeur `true` la fenêtre contextuelle modale est annulée.
* **ModalPopped** - `ModalPoppedEventArgs`

> [!NOTE]
> Pour implémenter les méthodes de cycle de vie des applications et les événements de navigation modale, avant tout`Application` méthodes de création d’une application Xamarin.Forms (ie. les applications écrites dans la version 1.2 ou antérieure qui utilisent un statique `GetMainPage` méthode) ont été mis à jour pour créer un par défaut `Application` qui est défini comme parent de `MainPage`.
>
> Xamarin.Forms les applications qui utilisent ce comportement hérité doivent être mis à jour vers une `Application` sous-classe comme décrit dans la [classe Application](~/xamarin-forms/app-fundamentals/application-class.md) page.
