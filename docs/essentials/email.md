---
title: 'Xamarin.Essentials : E-mail'
description: La classe de courrier électronique dans Xamarin.Essentials permet à une application ouvrir l’application de messagerie par défaut avec une informations spécifiées, y compris le sujet, les corps et les destinataires (à, CC, Cci).
ms.assetid: 5FBB6FF0-0E7B-4C29-8F06-91642AF12629
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: aea2f429126180ae3d98bc665bed5574f416ea53
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38848544"
---
# <a name="xamarinessentials-email"></a>Xamarin.Essentials : E-mail

![Version préliminaire NuGet](~/media/shared/pre-release.png)

Le **E-mail** classe permet à une application ouvrir l’application de messagerie par défaut avec une informations spécifiées, y compris le sujet, le corps et les destinataires (à, CC, Cci).

## <a name="using-email"></a>À l’aide de la messagerie

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

La fonctionnalité de courrier électronique fonctionne en appelant le `ComposeAsync` méthode un `EmailMessage` qui contient des informations sur le courrier électronique :

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
            // Some other exception occured
        }
    }
}
```

## <a name="api"></a>API

- [Code source de messagerie](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Email)
- [Documentation des API d’e-mail](xref:Xamarin.Essentials.Email)
