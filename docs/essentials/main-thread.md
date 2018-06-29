---
title: 'Xamarin.Essentials : MainThread'
description: La classe MainThread permet aux applications d’exécuter du code sur le thread d’exécution principale.
ms.assetid: CD6D51E7-D933-4FE7-A7F7-392EF27812E1
author: charlespetzold
ms.author: chape
ms.date: 06/26/2018
ms.openlocfilehash: e07d36d3e9a5492e6e170b62dbacb36be44dbfa9
ms.sourcegitcommit: 72450a6a29599fa133ff4f16fb0b1f443d89f9dc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37080451"
---
# <a name="xamarinessentials-mainthread"></a>Xamarin.Essentials : MainThread

![Version préliminaire de NuGet](~/media/shared/pre-release.png)

Le **MainThread** classe permet aux applications d’exécuter du code sur le thread principal de l’exécution, et pour déterminer si un bloc de code particulier est en cours d’exécution sur le thread principal.

## <a name="background"></a>Présentation

La plupart des systèmes d’exploitation, y compris iOS, Android et la plateforme Windows universelle : utiliser un modèle de modèle de thread unique pour le code de l’interface utilisateur. Ce modèle est nécessaire pour sérialiser des événements d’interface utilisateur, y compris les séquences de touches et entrée tactile. Ce thread est souvent appelé le _thread principal_ ou _thread d’interface utilisateur_ ou _thread d’interface utilisateur_. L’inconvénient de ce modèle est que tout le code qui accède à des éléments d’interface utilisateur doit être exécutée sur le thread principal de l’application. 

Les applications doivent parfois utiliser des événements qui appellent le Gestionnaire d’événements sur un thread secondaire de l’exécution. (Les classes Xamarin.Essentials [ `Accelerometer` ](accelerometer.md), [ `Compass` ](compass.md), [ `Gyroscope` ](gyroscope.md), [ `Magnetometer` ](magnetometer.md), et [ `OrientationSensor` ](orientation-sensor.md) tous peuvent retourner des informations sur un thread secondaire lorsqu’il est utilisé avec des débits plus importants.) Si le Gestionnaire d’événements doit accéder aux éléments d’interface utilisateur, il doit exécuter ce code sur le thread principal. Le **MainThread** classe permet à l’application d’exécuter ce code sur le thread principal.

## <a name="running-code-on-the-main-thread"></a>Code en cours d’exécution sur le Thread principal

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

Pour exécuter du code sur le thread principal, appelez la méthode statique `MainThread.BeginInvokeOnMainThread` (méthode). L’argument est un [ `Action` ](xref:System.Action) objet, qui est simplement une méthode sans arguments et aucune valeur de retour :

```csharp
MainThread.BeginInvokeOnMainThread(() =>
{
    // Code to run on the main thread
});
```

Il est également possible de définir une méthode distincte pour le code qui doit s’exécuter sur le thread principal :

```csharp
void MyMainThreadCode()
{
    // Code to run on the main thread
}
```

Vous pouvez ensuite exécuter cette méthode sur le thread principal en référençant dans le `BeginInvokeOnMainThread` méthode :

```csharp
MainThread.BeginInvokeOnMainThread(MyMainThreadCode);
```

> [!NOTE]
> Xamarin.Forms possède une méthode appelée [ `Device.BeginInvokeOnMainThread(Action)` ](https://docs.microsoft.com/dotnet/api/xamarin.forms.device.begininvokeonmainthread) qui produit la même chose que `MainThread.BeginInvokeOnMainThread(Action)`. Si vous pouvez utiliser soit la méthode dans une application de Xamarin.Forms, considérez si le code appelant a besoin d’une dépendance sur Xamarin.Forms. Si ce n’est pas le cas, `MainThread.BeginInvokeOnMainThread(Action)` est probablement une meilleure option.

## <a name="determining-if-code-is-running-on-the-main-thread"></a>Déterminer si le Code s’exécute sur le Thread principal

La `MainThread` classe permet également à une application déterminer si un bloc de code particulier est en cours d’exécution sur le thread principal. Le `IsMainThread` propriété renvoie `true` si le code d’appel de la propriété est en cours d’exécution sur le thread principal. Un programme peut utiliser cette propriété pour exécuter un code différent pour le thread principal ou un thread secondaire :

```csharp
if (MainThread.IsMainThread)
{
    // Code to run if this is the main thread
}
else
{
    // Code to run if this is a secondary thread
}
```

Vous pouvez vous demander si vous devez vérifier si le code s’exécute sur un thread secondaire avant d’appeler `BeginInvokeOnMainThread`, par exemple, comme suit :

```csharp
if (MainThread.IsMainThread)
{
    MyMainThreadCode();
}
else
{
    MainThread.BeginInvokeOnMainThread(MyMainThreadCode);
}
```

Vous pouvez donc supposer que cette vérification peut améliorer les performances si le bloc de code est déjà en cours d’exécution sur le thread principal.

_Toutefois, cette vérification n’est pas nécessaire._ Les implémentations de plateforme de `BeginInvokeOnMainThread` eux-mêmes vérifier si l’appel est effectué sur le thread principal. Il existe très peu baisse des performances si vous appelez `BeginInvokeOnMainThread` lorsqu’il n’est pas vraiment nécessaire.

## <a name="api"></a>API

- [Code source de MainThread](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/MainThread)
- [Documentation de l’API de MainThread](xref:Xamarin.Essentials.MainThread)
