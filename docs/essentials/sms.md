---
title: Xamarin.Essentials SMS
description: La classe Sms permet à une application ouvrir l’application de SMS par défaut avec un message spécifié à envoyer à un destinataire.
ms.assetid: 81A757F2-6F2A-458F-B9BE-770ADEBFAB58
ms.technology: xamarin-crossplatform
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 460aea01381934f1862946c6e17e314560e889f2
ms.sourcegitcommit: 46d3c9daa45350bdd536d9e105517f3c1c753c5b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
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
