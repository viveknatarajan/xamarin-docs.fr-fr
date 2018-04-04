---
title: Cycle de vie
description: Comment répondre à un cycle de vie des applications
ms.prod: xamarin
ms.assetid: 69B416CF-B243-4790-AB29-F030B32465BE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/19/2016
ms.openlocfilehash: 511591482a0e7512be34f6a210c6f44a1826be24
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="app-lifecycle"></a>Cycle de vie

La `Application` classe de base offre les fonctionnalités suivantes :

* [Les méthodes de cycle de vie](#Lifecycle_Methods) `OnStart`, `OnSleep`, et `OnResume`.
* [Événements de navigation modale](#modal) `ModalPushing`, `ModalPushed`, `ModalPopping`, et `ModalPopped`.

<a name="Lifecycle_Methods" />

## <a name="lifecycle-methods"></a>Méthodes de cycle de vie

La `Application` classe contient trois méthodes virtuelles qui peuvent être substituées pour gérer les méthodes de cycle de vie :

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

<a name="modal" />

## <a name="modal-navigation-events"></a>Événements de Navigation modale

Il existe quatre nouveaux événements sur le `Application` classe dans Xamarin.Forms 1.4, chacun ayant leurs propres arguments d’événement :

* **ModalPushing** - `ModalPushingEventArgs`
* **ModalPushed** - `ModalPushedEventArgs`
* **ModalPopping** - le `ModalPoppingEventArgs` classe contient un `Cancel` propriété. Lorsque `Cancel` a la valeur `true` la fenêtre contextuelle modale est annulée.
* **ModalPopped** - `ModalPoppedEventArgs`

Ces événements vous permettra de mieux gérer le cycle de vie de votre application, en vous permettant de vous répondez aux pages modales qui est indiqué et sera ignoré.

> [!NOTE]
> Pour implémenter les méthodes de cycle de vie des applications et les événements de navigation modale, avant tout`Application` méthodes de création d’une application Xamarin.Forms (ie. les applications écrites dans la version 1.2 ou antérieure qui utilisent un statique `GetMainPage` méthode) ont été mis à jour pour créer un par défaut `Application` qui est défini comme parent de `MainPage`.
>
> Xamarin.Forms les applications qui utilisent ce comportement hérité doivent être mis à jour vers une `Application` sous-classe comme décrit dans la [classe Application](~/xamarin-forms/app-fundamentals/application-class.md) page.
