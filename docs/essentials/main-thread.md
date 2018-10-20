---
title: 'Xamarin.Essentials : MainThread'
description: La classe MainThread permet aux applications d’exécuter du code sur le thread principal de l'application. Elle permet également de déterminer si un bloc de code particulier est en cours d’exécution sur le thread principal ou non.
ms.assetid: CD6D51E7-D933-4FE7-A7F7-392EF27812E1
author: charlespetzold
ms.author: chape
ms.date: 06/26/2018
ms.openlocfilehash: e07d36d3e9a5492e6e170b62dbacb36be44dbfa9
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38831423"
---
# <a name="xamarinessentials-mainthread"></a>Xamarin.Essentials : MainThread

![Version préliminaire NuGet](~/media/shared/pre-release.png)

La classe **MainThread** permet aux applications d’exécuter du code sur le thread principal de l'application. Elle permet également de déterminer si un bloc de code particulier est en cours d’exécution sur le thread principal ou non.

## <a name="background"></a>Présentation

La plupart des systèmes d’exploitation, y compris iOS, Android et la plateforme Windows universelle, utilisent un modèle de thread unique pour le code de l’interface utilisateur. Ce modèle est nécessaire pour sérialiser les événements d’interface utilisateur, y compris les séquences de touches et d'entrées tactiles. Ce thread est souvent appelé le _thread principal_ ou _thread d’interface utilisateur_ ou _thread d’interface utilisateur_. L’inconvénient de ce modèle est que tout le code qui accède aux éléments d’interface utilisateur doit s’exécuter sur le thread principal de l’application.

Les applications doivent parfois d’utiliser des événements qui appellent le Gestionnaire d’événements sur un thread secondaire de l’exécution. (Les classes Xamarin.Essentials [`Accelerometer`](accelerometer.md), [`Compass`](compass.md), [`Gyroscope`](gyroscope.md), [`Magnetometer`](magnetometer.md), et [`OrientationSensor`](orientation-sensor.md) peuvent retourner des informations sur des threads secondaires lorsqu’ils sont très utilisés.) Si le Gestionnaire d’événements doit accéder aux éléments d’interface utilisateur, il doit exécuter ce code sur le thread principal. La classe **MainThread** permet à l’application d’exécuter ce code sur le thread principal.

## <a name="running-code-on-the-main-thread"></a>Code en cours d’exécution sur le Thread principal

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

Pour exécuter du code sur le thread principal, appelez la méthode statique `MainThread.BeginInvokeOnMainThread` avec un argument de type [`Action`](xref:System.Action), qui est simplement une méthode sans arguments et aucune valeur de retour :

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

Vous pouvez ensuite exécuter cette méthode sur le thread principal en le référençant dans le `BeginInvokeOnMainThread` méthode :

```csharp
MainThread.BeginInvokeOnMainThread(MyMainThreadCode);
```

> [!NOTE]
> Xamarin.Forms a une méthode appelée [`Device.BeginInvokeOnMainThread(Action)`](https://docs.microsoft.com/dotnet/api/xamarin.forms.device.begininvokeonmainthread) qui effectue la même chose que `MainThread.BeginInvokeOnMainThread(Action)`. Si le code passé à la fonction InvokeOnMainThread a besoin d’une dépendance de Xamarin.Forms il est préférable d'utiliser [`Device.BeginInvokeOnMainThread(Action)`]. Si ce n’est pas le cas, `MainThread.BeginInvokeOnMainThread(Action)` est probablement une meilleure option.

## <a name="determining-if-code-is-running-on-the-main-thread"></a>Déterminer si le Code s’exécute sur le Thread principal

La classe `MainThread` permet également à une application de déterminer si un bloc de code particulier est en cours d’exécution sur le thread principal. Le `IsMainThread` retourne de la propriété `true` si le code appelant la propriété est en cours d’exécution sur le thread principal. Un programme peut utiliser cette propriété pour exécuter un code différent pour le thread principal ou un thread secondaire :

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

Vous vous demandez peut-être si vous devez vérifier si le code s’exécute sur un thread secondaire avant d’appeler `BeginInvokeOnMainThread`, par exemple, comme suit :

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

Vous pouvez l’imaginer que cette vérification peut améliorer les performances si le bloc de code est déjà en cours d’exécution sur le thread principal.

_Toutefois, cette vérification n’est pas nécessaire._ Les implémentations de plateforme de `BeginInvokeOnMainThread` vérifient elles-même si l’appel est effectué sur le thread principal. La perte de performances est négligeable si vous appelez `BeginInvokeOnMainThread` depuis le thread principal.

## <a name="api"></a>API

- [Code source de MainThread](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/MainThread)
- [Documentation de l’API de MainThread](xref:Xamarin.Essentials.MainThread)
