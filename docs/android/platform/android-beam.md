---
title: Faisceaux Android
ms.topic: article
ms.prod: xamarin
ms.assetid: 4172A798-89EC-444D-BC0C-0A7DD67EF98C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/06/2017
ms.openlocfilehash: bea8480c66a2ecf499375636c98511ca55ce7693
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="android-beam"></a>Faisceaux Android

Faisceaux Android est une nouvelle technologie de Communication NFC (Near Field) dans Android 4 qui permet aux applications de partager des informations sur NFC lorsqu’à proximité.

[![Diagramme illustrant les deux périphériques à proximité de partage d’informations](android-beam-images/androidbeam.png)](android-beam-images/androidbeam.png)

Faisceaux Android fonctionne en envoyant des messages via NFC lorsque deux périphériques sont à portée. Périphériques sur 4cm entre eux peuvent partager des données à l’aide de faisceaux Android. Une activité sur un périphérique crée un message et spécifie une activité (ou les activités) qui peut gérer la publier. Lorsque l’activité spécifiée est au premier plan et les périphériques sont à portée, faisceaux Android transmet le message à la deuxième unité. Sur le périphérique de réception, une intention est appelée contenant les données du message.

Android prend en charge deux méthodes de définition de messages avec FAISCEAUX Android :

-   `SetNdefPushMessage` -Avant FAISCEAUX Android est lancée, une application peut appeler SetNdefPushMessage pour spécifier un NdefMessage pour transmettre via NFC, ainsi que l’activité qui est l’envoi. Ce mécanisme est utilisé lorsqu’un message ne change pas pendant une application est en cours d’utilisation.

-   `SetNdefPushMessageCallback` -Lorsque vous FAISCEAUX Android est lancée, une application peut gérer un rappel pour créer un NdefMessage. Ce mécanisme permet de créer de message peut être différé jusqu'à ce que les appareils se trouvent dans la plage. Il prend en charge les scénarios où le message peut varier en fonction de ce qui se passe dans l’application.


Dans les deux cas, pour envoyer des données avec FAISCEAUX Android, une application envoie un `NdefMessage`, empaqueter les données dans plusieurs `NdefRecords`. Jetons un œil sur les points clés qui doivent être résolus avant que nous pouvons déclencher FAISCEAUX Android. Tout d’abord, nous utiliserons avec le style de rappel de création d’un `NdefMessage`.

<a name="Creating_a_Message" />

## <a name="creating-a-message"></a>Création d’un Message

Nous pouvons enregistrer des rappels avec une `NfcAdapter` dans l’activité `OnCreate` (méthode). Par exemple, en supposant une `NfcAdapter` nommé `mNfcAdapter` est déclarée comme une variable de classe dans l’activité, nous pouvons écrire le code suivant pour créer le rappel qui construit le message :

```csharp
mNfcAdapter = NfcAdapter.GetDefaultAdapter (this);
mNfcAdapter.SetNdefPushMessageCallback (this, this);
```

L’activité, qui implémente `NfcAdapter.ICreateNdefMessageCallback`, est passé à la `SetNdefPushMessageCallback` méthode ci-dessus. Lorsque les faisceaux Android est lancée, le système appelle `CreateNdefMessage`, à partir de laquelle l’activité peut construire un `NdefMessage` comme indiqué ci-dessous :

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

<a name="Receiving_a_Message" />

## <a name="receiving-a-message"></a>Réception d’un Message

Sur le côté réception, le système appelle une intention avec le `ActionNdefDiscovered` action, à partir de laquelle nous pouvons extraire le NdefMessage comme suit :

```csharp
IParcelable [] rawMsgs = intent.GetParcelableArrayExtra (NfcAdapter.ExtraNdefMessages);
NdefMessage msg = (NdefMessage) rawMsgs [0];
```

Pour obtenir un exemple de code complet qui utilise des faisceaux Android, indiqué en cours d’exécution dans la capture d’écran ci-dessous, consultez le [démonstration de faisceaux Android](https://developer.xamarin.com/samples/monodroid/AndroidBeamDemo/) dans la galerie d’exemples.

[![Captures d’écran à partir de la démonstration FAISCEAUX Android](android-beam-images/24.png)](android-beam-images/24.png)



## <a name="related-links"></a>Liens associés

- [Démonstration de faisceaux Android (exemple)](https://developer.xamarin.com/samples/monodroid/AndroidBeamDemo/)
- [Présentation de glace Sandwich](http://www.android.com/about/ice-cream-sandwich/)
- [Plateforme 4.0 Android](http://developer.android.com/sdk/android-4.0.html)
