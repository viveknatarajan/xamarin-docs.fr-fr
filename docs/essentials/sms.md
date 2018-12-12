---
title: 'Xamarin.Essentials : SMS'
description: La classe Sms de Xamarin.Essentials permet à une application d’ouvrir l’application SMS par défaut avec un message spécifié à envoyer à un destinataire.
ms.assetid: 81A757F2-6F2A-458F-B9BE-770ADEBFAB58
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
ms.openlocfilehash: d0655f3e687750e0fca626fb017096a946c0abb3
ms.sourcegitcommit: 01f93a34b466f8d4043cef68fab9b35cd8decee6
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2018
ms.locfileid: "52898769"
---
# <a name="xamarinessentials-sms"></a>Xamarin.Essentials : SMS

La classe **Sms** permet à une application d’ouvrir l’application SMS par défaut avec un message spécifié à envoyer à un destinataire.

## <a name="get-started"></a>Prise en main

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-sms"></a>Utiliser Sms

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

Pour utiliser la fonctionnalité SMS, appelez la méthode `ComposeAsync` avec un `SmsMessage` contenant le destinataire et le corps du message, tous deux facultatifs.

```csharp
public class SmsTest
{
    public async Task SendSms(string messageText, string recipient)
    {
        try
        {
            var message = new SmsMessage(messageText, recipient);
            await Sms.ComposeAsync(message);
        }
        catch (FeatureNotSupportedException ex)
        {
            // Sms is not supported on this device.
        }
        catch (Exception ex)
        {
            // Other error has occurred.
        }
    }
}
```

Vous pouvez également transmettre plusieurs destinataires à un `SmsMessage` :

```csharp
public class SmsTest
{
    public async Task SendSms(string messageText, string[] recipients)
    {
        try
        {
            var message = new SmsMessage(messageText, recipients);
            await Sms.ComposeAsync(message);
        }
        catch (FeatureNotSupportedException ex)
        {
            // Sms is not supported on this device.
        }
        catch (Exception ex)
        {
            // Other error has occurred.
        }
    }
}
```

## <a name="api"></a>API

- [Code source de Sms](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Sms)
- [Documentation de l’API Sms](xref:Xamarin.Essentials.Sms)
