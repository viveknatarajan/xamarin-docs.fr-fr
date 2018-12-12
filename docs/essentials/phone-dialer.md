---
title: 'Xamarin.Essentials : clavier téléphonique'
description: La classe PhoneDialer de Xamarin.Essentials permet à une application de composer un numéro de téléphone sur le clavier.
ms.assetid: E7457942-4D7B-4195-A2FF-417919B9537F
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
ms.openlocfilehash: 8d4b0cdcae5e33ac2c48baa0b7749597314eae8c
ms.sourcegitcommit: 01f93a34b466f8d4043cef68fab9b35cd8decee6
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2018
ms.locfileid: "52898266"
---
# <a name="xamarinessentials-phone-dialer"></a>Xamarin.Essentials : clavier téléphonique

La classe **PhoneDialer** permet à une application de composer un numéro de téléphone sur le clavier.

## <a name="get-started"></a>Prise en main

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-phone-dialer"></a>Utilisation du clavier téléphonique

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

La fonctionnalité de clavier téléphonique appelle la méthode `Open` pour composer un numéro de téléphone sur le clavier. Quand `Open` est demandé, l’API tente automatiquement d’appliquer le format approprié au numéro en fonction de l’indicatif téléphonique international, s’il est spécifié.

```csharp
public class PhoneDialerTest
{
    public async Task PlacePhoneCall(string number)
    {
        try
        {
            PhoneDialer.Open(number);
        }
        catch (ArgumentNullException anEx)
        {
            // Number was null or white space
        }
        catch (FeatureNotSupportedException ex)
        {
            // Phone Dialer is not supported on this device.
        }
        catch (Exception ex)
        {
            // Other error has occurred.
        }
    }
}
```

## <a name="api"></a>API

- [Code source de la fonctionnalité de clavier téléphonique](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/PhoneDialer)
- [Documentation sur l’API de clavier téléphonique](xref:Xamarin.Essentials.PhoneDialer)
