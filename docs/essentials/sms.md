---
title: Xamarin.Essentials SMS
description: La classe Sms permet à une application ouvrir l’application de SMS par défaut avec un message spécifié à envoyer à un destinataire.
ms.assetid: 81A757F2-6F2A-458F-B9BE-770ADEBFAB58
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 5baeee03626ba659ac7e5c06be40039476a67e08
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2018
---
# <a name="xamarinessentials-sms"></a>Xamarin.Essentials SMS

![Version préliminaire de NuGet](~/media/shared/pre-release.png)

Le **Sms** classe permet à une application ouvrir l’application de SMS par défaut avec un message spécifié à envoyer à un destinataire.

## <a name="using-sms"></a>À l’aide de Sms

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

La fonctionnalité SMS fonctionne en appelant le `ComposeAsync` méthode un `SmsMessage` qui contient le destinataire et le corps du message, qui sont facultatifs.

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

## <a name="api"></a>API

- [Code source de SMS](https://github.com/xamarin/Essentials/tree/master/Essentials/Sms)
- [Documentation de l’API de SMS](xref:Xamarin.Essentials.Sms)
