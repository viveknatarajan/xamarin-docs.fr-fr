---
title: 'Xamarin.Essentials : E-mail'
description: La classe Email permet à une application d'ouvrir l’application de messagerie par défaut en lui transmettant des informations sur le message pré-composé, tel que le sujet, le corps et les destinataires (à, CC, Cci).
ms.assetid: 5FBB6FF0-0E7B-4C29-8F06-91642AF12629
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: f113cebfebf4238fd4b75ad8ab248e2abf61efea
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353904"
---
# <a name="xamarinessentials-email"></a>Xamarin.Essentials : E-mail

![Version préliminaire NuGet](~/media/shared/pre-release.png)

La classe **Email** permet à une application d'ouvrir l’application de messagerie par défaut en lui transmettant des informations sur le message pré-composé, tel que le sujet, le corps et les destinataires (à, CC, Cci).

## <a name="using-email"></a>Utilisation de **Email**

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

La fonctionnalité Email appelle la méthode `ComposeAsync` avec `EmailMessage` qui contient des informations sur l'e-mail :

```csharp
public class EmailTest
{
    public async Task SendEmail(string subject, string body, List<string> recipients)
    {
        try
        {
            var message = new EmailMessage
            {
                Subject = subject,
                Body = body,
                To = recipients,
                //Cc = ccRecipients,
                //Bcc = bccRecipients
            };
            await Email.ComposeAsync(message);
        }
        catch (FeatureNotSupportedException fbsEx)
        {
            // Email is not supported on this device
        }
        catch (Exception ex)
        {
            // Some other exception occurred
        }
    }
}
```

## <a name="api"></a>API

- [Code source de messagerie](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Email)
- [Documentation des API d’e-mail](xref:Xamarin.Essentials.Email)
