---
title: 'Xamarin.Essentials: Messagerie'
description: La classe Email de Xamarin.Essentials permet à une application d’ouvrir l’application de messagerie par défaut en utilisant des informations spécifiques, notamment l’objet, le corps et les destinataires (À, CC, CCI).
ms.assetid: 5FBB6FF0-0E7B-4C29-8F06-91642AF12629
author: jamesmontemagno
ms.author: jamont
ms.date: 04/02/2019
ms.openlocfilehash: 5ddbb6f3d1c1ebf16ca5534b7effd1993c3d8b26
ms.sourcegitcommit: 0c823f5439f4279a35af23dd466e7a0483e65d50
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65804913"
---
# <a name="xamarinessentials-email"></a>Xamarin.Essentials: Messagerie

La classe **Email** permet à une application d’ouvrir l’application de messagerie par défaut en utilisant des informations spécifiques, notamment l’objet, le corps et les destinataires (À, CC, CCI).

## <a name="get-started"></a>Prise en main

[!include[](~/essentials/includes/get-started.md)]

> [!TIP]
> Pour utiliser l’API de messagerie sur iOS, vous devez l’exécuter sur un appareil physique, sinon une exception sera levée.

## <a name="using-email"></a>Utilisation de l’e-mail

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

La fonctionnalité d’e-mail appelle la méthode `ComposeAsync` d’un `EmailMessage` contenant des informations sur l’e-mail :

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


## <a name="platform-differences"></a>Différences entre les plateformes

# <a name="androidtabandroid"></a>[Android](#tab/android)

Les clients de messagerie pour Android ne prennent pas tous en charge `Html`, car il n’y a aucun moyen de le détecter. Nous vous recommandons d’utiliser `PlainText` pour envoyer des e-mails.

# <a name="iostabios"></a>[iOS](#tab/ios)

Aucune différence entre les plateformes.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Prend uniquement en charge `PlainText` comme `BodyFormat`, la tentative d’envoi de `Html` lèvera une `FeatureNotSupportedException`.

-----

## <a name="file-attachments"></a>Pièces jointes

![Fonctionnalité d’évaluation](~/media/shared/preview.png)

L’envoi de fichiers par e-mail est disponible en tant que préversion expérimentale dans Xamarin.Essentials version 1.1.0. Cette fonctionnalité permet à une application d’envoyer des fichiers par e-mail dans des clients de messagerie sur l’appareil. Pour activer cette fonctionnalité, définissez la propriété suivante dans le code de démarrage de votre application :

```csharp
ExperimentalFeatures.Enable(ExperimentalFeatures.EmailAttachments);
```

Une fois la fonctionnalité activée, n’importe quel fichier peut être envoyé. Xamarin.Essentials détectera automatiquement le type de fichier (MIME) et demandera que le fichier soit ajouté en tant que pièce jointe. Chaque client de messagerie est différent et susceptible de ne prendre en charge que certaines extensions de fichiers spécifiques, voire aucune.

Voici un exemple d’écriture de texte sur le disque et d’ajout en tant que pièce jointe à un e-mail :

```csharp
var message = new EmailMessage
{
    Subject = "Hello",
    Body = "World",
};

var fn = "Attachment.txt";
var file = Path.Combine(FileSystem.CacheDirectory, fn);
File.WriteAllText(file, "Hello World");

message.Attachments.Add(new EmailAttachment(file));

await Email.ComposeAsync(message);
```

## <a name="api"></a>API

- [Code source de la fonctionnalité d’e-mail](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Email)
- [Documentation sur l’API d’e-mail](xref:Xamarin.Essentials.Email)
