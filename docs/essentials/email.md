---
title: Par courrier électronique Xamarin.Essentials
description: La classe de messagerie permet à une application ouvrir l’application de messagerie par défaut avec une informations spécifiées, y compris l’objet, le corps et les destinataires (à, CC, Cci).
ms.assetid: 5FBB6FF0-0E7B-4C29-8F06-91642AF12629
ms.technology: xamarin-crossplatform
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: a23d849131372820a73057b752908886f64b4210
ms.sourcegitcommit: 46d3c9daa45350bdd536d9e105517f3c1c753c5b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
---
# <a name="xamarinessentials-email"></a>Par courrier électronique Xamarin.Essentials

![Version préliminaire de NuGet](~/media/shared/pre-release.png)

Le **messagerie** classe permet à une application ouvrir l’application de messagerie par défaut avec une informations spécifiées, y compris l’objet, le corps et les destinataires (à, CC, Cci).

## <a name="using-email"></a>Courrier électronique

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

La fonctionnalité de courrier électronique fonctionne en appelant le `ComposeAsync` méthode un `EmailMessage` qui contient des informations sur l’adresse de messagerie :

```csharp
public class EmailTest
{
    public async Task SendEmail(string subject, string, body, List<string> recipients)
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
            }
            await Email.ComposeAsync(message);
        }
        catch (FeatureNotSupportedException fbsEx)
        {
            // Sms is not supported on this device
        }
        catch (Exception ex)
        {
            // Some other exception occured
        }
    }
}
```

## <a name="api"></a>API

- [Code source de messagerie](https://github.com/xamarin/Essentials/tree/master/Essentials/Email)
- [Documentation de l’API de messagerie](xref:Xamarin.Essentials.Email)
