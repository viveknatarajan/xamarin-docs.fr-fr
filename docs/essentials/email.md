---
title: 'Xamarin.Essentials : courrier électronique'
description: La classe de courrier électronique dans Xamarin.Essentials permet à une application ouvrir l’application de messagerie par défaut avec une informations spécifiées, y compris le sujet, corps et les destinataires (à, CC, Cci).
ms.assetid: 5FBB6FF0-0E7B-4C29-8F06-91642AF12629
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: aea2f429126180ae3d98bc665bed5574f416ea53
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782435"
---
# <a name="xamarinessentials-email"></a>Xamarin.Essentials : courrier électronique

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
- [Documentation de l’API de messagerie](xref:Xamarin.Essentials.Email)
