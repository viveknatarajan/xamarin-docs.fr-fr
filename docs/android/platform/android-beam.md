---
title: Android Beam
ms.prod: xamarin
ms.assetid: 4172A798-89EC-444D-BC0C-0A7DD67EF98C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/06/2017
ms.openlocfilehash: 9fcabc90875dda28ecdd5d94f1ca2f263ffe4886
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57670846"
---
# <a name="android-beam"></a>Android Beam

FAISCEAU Android est une technologie de Communication NFC (Near Field) introduite dans Android 4.0 qui permet aux applications de partager des informations sur NFC lorsqu’à proximité.

[![Diagramme illustrant les deux périphériques à proximité de partage d’informations](android-beam-images/androidbeam.png)](android-beam-images/androidbeam.png#lightbox)

FAISCEAU Android fonctionne en envoyant des messages sur NFC lorsque deux périphériques sont à portée. Appareils environ 4cm entre eux peuvent partager des données à l’aide de faisceau Android. Une activité sur un périphérique crée un message et spécifie une activité (ou activités) qui peut gérer le placer. Lorsque l’activité spécifiée est au premier plan et les appareils se trouvent dans la plage, faisceau Android envoie le message au deuxième appareil. Sur l’appareil de réception, une intention est appelée contenant les données du message.

Android prend en charge deux méthodes de définition de messages avec faisceau Android :

-   `SetNdefPushMessage` -Avant Android faisceau est lancée, une application peut appeler SetNdefPushMessage pour spécifier un NdefMessage à transmettre via NFC et l’activité qui est l’envoi. Ce mécanisme est utilisé lorsqu’un message ne change pas pendant une application est en cours d’utilisation.

-   `SetNdefPushMessageCallback` -Lorsque le faisceau Android est lancée, une application peut gérer un rappel pour créer un NdefMessage. Ce mécanisme permet la création de messages peut être différé jusqu'à ce que les appareils se trouvent dans la plage. Il prend en charge les scénarios où le message peut varier en fonction de ce qui se passe dans l’application.


Dans les deux cas, pour envoyer des données avec faisceau Android, une application envoie un `NdefMessage`, empaqueter les données dans plusieurs `NdefRecords`. Jetons un œil sur les points clés qui doivent être traitées avant que nous pouvons déclencher faisceau Android. Tout d’abord, nous allons utiliser le style de rappel de la création d’un `NdefMessage`.


## <a name="creating-a-message"></a>Création d’un Message

Nous pouvons enregistrer des rappels avec un `NfcAdapter` dans l’activité `OnCreate` (méthode). Par exemple, en supposant un `NfcAdapter` nommé `mNfcAdapter` est déclarée comme une variable de classe dans l’activité, nous pouvons écrire le code suivant pour créer le rappel qui construira le message :

```csharp
mNfcAdapter = NfcAdapter.GetDefaultAdapter (this);
mNfcAdapter.SetNdefPushMessageCallback (this, this);
```

L’activité, qui implémente `NfcAdapter.ICreateNdefMessageCallback`, est passé à la `SetNdefPushMessageCallback` méthode ci-dessus. Lorsque le faisceau Android est lancée, le système appelle `CreateNdefMessage`, à partir de laquelle l’activité peut construire un `NdefMessage` comme indiqué ci-dessous :

```csharp
public NdefMessage CreateNdefMessage (NfcEvent evt)
{
    DateTime time = DateTime.Now;
    var text = ("Beam me up!\n\n" + "Beam Time: " +
        time.ToString ("HH:mm:ss"));
    NdefMessage msg = new NdefMessage (
        new NdefRecord[]{ CreateMimeRecord (
            "application/com.example.android.beam",
            Encoding.UTF8.GetBytes (text)) });
        } };
    return msg;
}

public NdefRecord CreateMimeRecord (String mimeType, byte [] payload)
{
    byte [] mimeBytes = Encoding.UTF8.GetBytes (mimeType);
    NdefRecord mimeRecord = new NdefRecord (
        NdefRecord.TnfMimeMedia, mimeBytes, new byte [0], payload);
    return mimeRecord;
}
```


## <a name="receiving-a-message"></a>Réception d’un Message

Sur le côté réception, le système appelle une intention avec le `ActionNdefDiscovered` action, à partir de laquelle nous pouvons extraire le NdefMessage comme suit :

```csharp
IParcelable [] rawMsgs = intent.GetParcelableArrayExtra (NfcAdapter.ExtraNdefMessages);
NdefMessage msg = (NdefMessage) rawMsgs [0];
```

Pour obtenir un exemple de code complet qui utilise le faisceau Android, indiqué en cours d’exécution dans la capture d’écran ci-dessous, consultez le [démonstration de faisceau Android](https://developer.xamarin.com/samples/monodroid/AndroidBeamDemo/) dans la galerie d’exemples.

[![Captures d’écran de l’exemple à partir de la démo de faisceau Android](android-beam-images/24.png)](android-beam-images/24.png#lightbox)



## <a name="related-links"></a>Liens associés

- [Démonstration de faisceau Android (exemple)](https://developer.xamarin.com/samples/monodroid/AndroidBeamDemo/)
- [Présentation d’Ice Cream Sandwich](http://www.android.com/about/ice-cream-sandwich/)
- [Plateforme 4.0 Android](https://developer.android.com/sdk/android-4.0.html)
