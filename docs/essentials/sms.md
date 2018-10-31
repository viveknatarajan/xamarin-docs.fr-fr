---
title: 'Xamarin.Essentials : SMS'
description: La classe Sms de Xamarin.Essentials permet à une application d’ouvrir l’application SMS par défaut avec un message spécifié à envoyer à un destinataire.
ms.assetid: 81A757F2-6F2A-458F-B9BE-770ADEBFAB58
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: fa55a17e99a11861b98c4d515df882ed3af58a0b
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50102735"
---
# <a name="xamarinessentials-sms"></a>Xamarin.Essentials : SMS

![Préversion NuGet](~/media/shared/pre-release.png)

La classe **Sms** permet à une application d’ouvrir l’application SMS par défaut avec un message spécifié à envoyer à un destinataire.

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
