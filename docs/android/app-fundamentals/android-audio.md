---
title: Audio Android
description: "Le système d’exploitation Android prend en charge étendue multimédia, qui comprennent des données audio et vidéo. Ce guide se concentre sur audio dans Android et couvre la lecture et l’enregistrement audio à l’aide du lecteur audio intégré et classes de l’enregistreur, ainsi que l’API de bas niveau audio. Elle traite également travailler avec les événements Audio diffusion par d’autres applications, afin que les développeurs peuvent créer des applications conformes."
ms.topic: article
ms.prod: xamarin
ms.assetid: 646ED563-C34E-256D-4B56-29EE99881C27
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: ea3fd7d73f104f7b9650431a5531fe4399a2630c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="android-audio"></a>Audio Android

_Le système d’exploitation Android prend en charge étendue multimédia, qui comprennent des données audio et vidéo. Ce guide se concentre sur audio dans Android et couvre la lecture et l’enregistrement audio à l’aide du lecteur audio intégré et classes de l’enregistreur, ainsi que l’API de bas niveau audio. Elle traite également travailler avec les événements Audio diffusion par d’autres applications, afin que les développeurs peuvent créer des applications conformes._

<a name="Overview" />

## <a name="overview"></a>Vue d'ensemble

Les appareils mobiles modernes ont adopté des fonctionnalités précédemment aurait requis les parties dédiés du matériel &ndash; caméras, des lecteurs de musique et des enregistreurs vidéo. Pour cette raison, les infrastructures multimédias sont une fonctionnalité de première classe dans les API mobiles.

Android fournit la prise en charge étendue pour multimédia. Cet article examine l’utilisation des données audio dans Android et couvre les rubriques suivantes

1.  **Lecture des données Audio avec MediaPlayer** &ndash; à l’aide de la fonction intégrée `MediaPlayer` classe pour lire des fichiers audio, y compris les fichiers audio locaux et des fichiers audio en continu avec le `AudioTrack` classe.

2.  **Enregistrement Audio** &ndash; à l’aide de la fonction intégrée `MediaRecorder` classe enregistrement audio.

3.  **Utilisation des Notifications Audio** &ndash; à l’aide de notifications audio pour créer des applications conformes répondent correctement aux événements (tels que les appels entrants) par la suspension ou l’annulation de leurs sorties audio.

4.  **Utilisation de bas niveau Audio** &ndash; lecture audio à l’aide de la `AudioTrack` classe en écrivant directement dans les mémoires tampons. L’enregistrement audio à l’aide de la `AudioRecord` classe et de lire directement à partir de mémoires tampons.


## <a name="requirements"></a>Configuration requise

Ce guide nécessite 2.0 Android (API niveau 5) ou une version ultérieure. Notez que le débogage audio sur Android doit être effectué sur un appareil.

Il est nécessaire de demande le `RECORD_AUDIO` autorisations dans **AndroidManifest.XML**:

![Section autorisations de manifeste Android avec enregistrement requise\_AUDIO activé](android-audio-images/image01.png)


<a name="Playing_Audio_with_the_MediaPlayer_Class" />

## <a name="playing-audio-with-the-mediaplayer-class"></a>Lecture Audio avec la classe MediaPlayer.

Le plus simple pour lire un fichier audio dans Android est avec la fonction intégrée [MediaPlayer](https://developer.xamarin.com/api/type/Android.Media.MediaPlayer/) classe.
`MediaPlayer` peut de lire les fichiers locaux ou distants en passant le chemin d’accès de fichier. Toutefois, `MediaPlayer` est très sensible en état et de l’appel de l’une de ses méthodes dans un état incorrect, une exception levée. Il est important d’interagir avec `MediaPlayer` dans l’ordre décrit ci-dessous pour éviter les erreurs.


<a name="Initializing_and_Playing" />

### <a name="initializing-and-playing"></a>L’initialisation et lecture

Lecture de données audio avec `MediaPlayer` requiert la séquence suivante :

1. Instancier un nouvel [MediaPlayer](https://developer.xamarin.com/api/type/Android.Media.MediaPlayer/) objet.

1. Configurer le fichier à lire la [SetDataSource](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.SetDataSource/p/Java.IO.FileDescriptor/) (méthode).

1. Appelez le [préparation](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Prepare/) méthode d’initialisation du lecteur.

1. Appelez le [Démarrer](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Start/) méthode pour démarrer la lecture audio.


L’exemple de code ci-dessous illustre cette utilisation :

```csharp
protected MediaPlayer player;
public void StartPlayer(String  filePath)
{
  if (player == null) {
    player = new MediaPlayer();
  } else {
    player.Reset();
    player.SetDataSource(filePath);
    player.Prepare();
    player.Start();
  }
}
```

<a name="Suspending_and_Resuming_Playback" />

### <a name="suspending-and-resuming-playback"></a>Suspension et reprise de la lecture

La lecture peut être suspendue en appelant le [Pause](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Pause%28%29/) méthode :

```csharp
player.Pause();
```

Pour reprendre la lecture en pause, appelez le [Démarrer](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Start%28%29/) (méthode).
Il reprend à partir de l’emplacement mis en pause dans la lecture :

```csharp
player.Start();
```

Appel de la [arrêter](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Stop%28%29/) méthode sur le lecteur met fin à une lecture en cours :

```csharp
player.Stop();
```

Lorsque le lecteur n’est plus nécessaire, les ressources doivent être libérées en appelant le [version](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Release%28%29/) méthode :

```csharp
player.Release();
```

<a name="Using_the_MediaRecorder_Class_to_Record_Audio" />


## <a name="using-the-mediarecorder-class-to-record-audio"></a>À l’aide de la classe MediaRecorder enregistrement audio

Le corollaire à `MediaPlayer` pour l’enregistrement audio dans Android est le [MediaRecorder](https://developer.xamarin.com/api/type/Android.Media.MediaRecorder/) classe. Comme le `MediaPlayer`, il est dépendant de l’état et passe par plusieurs États pour atteindre le point où il peut démarrer l’enregistrement. Pour enregistrer le contenu audio, la `RECORD_AUDIO` autorisation doit être définie. Pour obtenir des instructions sur la façon de mettre en application les autorisations consultez [utilisation de AndroidManifest.xml](~/android/platform/android-manifest.md).

<a name="Initializing_and_Recording" />

### <a name="initializing-and-recording"></a>L’initialisation et d’enregistrement

Enregistrement audio avec le `MediaRecorder` implique les étapes suivantes :

1. Instancier un nouvel [MediaRecorder](https://developer.xamarin.com/api/type/Android.Media.MediaRecorder/) objet.

2. Spécifiez le périphérique matériel à utiliser pour capturer l’entrée audio via le [SetAudioSource](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.SetAudioSource/p/Android.Media.AudioSource/) (méthode).

3. Définir le format audio de fichier de sortie à l’aide du [SetOutputFormat](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.SetOutputFormat/p/Android.Media.OutputFormat/) (méthode). Pour obtenir la liste des types audio pris en charge, consultez [Android pris en charge des Formats Media](http://developer.android.com/guide/appendix/media-formats.html).

4. Appelez le [SetAudioEncoder](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.SetAudioEncoder/p/Android.Media.AudioEncoder/) pour définir le type de codage audio.

5. Appelez le [SetOutputFile](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.SetOutputFile/p/System.String/) méthode pour spécifier le nom du fichier de sortie figurant dans les données audio.

6. Appelez le [préparation](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.Prepare%28%29/) méthode d’initialisation de l’enregistreur.

7. Appelez le [Démarrer](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.Start%28%29/) méthode pour commencer l’enregistrement.


L’exemple de code suivant illustre cette séquence :

```csharp
protected MediaRecorder recorder;
void RecordAudio (String filePath)
{
  try {
    if (File.Exists (filePath)) {
      File.Delete (filePath);
    }
    if (recorder == null) {
      recorder = new MediaRecorder (); // Initial state.
    } else {
      recorder.Reset ();
      recorder.SetAudioSource (AudioSource.Mic);
      recorder.SetOutputFormat (OutputFormat.ThreeGpp);
      recorder.SetAudioEncoder (AudioEncoder.AmrNb);
      // Initialized state.
      recorder.SetOutputFile (filePath);
      // DataSourceConfigured state.
      recorder.Prepare (); // Prepared state
      recorder.Start (); // Recording state.
    }
  } catch (Exception ex) {
    Console.Out.WriteLine( ex.StackTrace);
  }
}
```

<a name="Stopping_recording" />

### <a name="stopping-recording"></a>Arrêt de l’enregistrement

Pour arrêter l’enregistrement, appelez le `Stop` méthode sur le `MediaRecorder`:

```csharp
recorder.Stop();
```

<a name="Cleaning_up" />


### <a name="cleaning-up"></a>Nettoyage

Une fois la `MediaRecorder` a été arrêté, appelez le [réinitialiser](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.Reset%28%29/) méthode placer de nouveau son état inactif :

```csharp
recorder.Reset();
```

Lorsque le `MediaRecorder` est n’est plus nécessaire, ses ressources doivent être libérées en appelant le [version](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.Release%28%29/) méthode :

```csharp
recorder.Release();
```

<a name="Managing_Audio_Notifications" />

## <a name="managing-audio-notifications"></a>Gestion des Notifications Audio

<a name="The_AudioManager_Class" />


### <a name="the-audiomanager-class"></a>La classe AudioManager

Le [AudioManager](https://developer.xamarin.com/api/type/Android.Media.AudioManager/) classe fournit l’accès à des notifications audio qui permettent aux applications de savoir quand se produisent les événements audio. Ce service fournit également l’accès à d’autres fonctionnalités audio, telles que le contrôle de mode de volume et sonnerie. Le `AudioManager` permet à une application gérer des notifications audio pour contrôler la lecture audio.

<a name="Managing_Audio_Focus" />


### <a name="managing-audio-focus"></a>Gestion du Focus Audio

Les ressources audio de l’appareil (lecteur intégré et enregistreur) sont partagées par toutes les applications en cours d’exécution.

Conceptuellement, cela est similaire aux applications installées sur un ordinateur de bureau où une seule application a le focus clavier : après avoir sélectionné une des applications en cours d’exécution par un clic de la souris, l’entrée au clavier va uniquement à cette application.

Le focus audio est une idée similaire et empêche plusieurs applications de lire ou d’enregistrement audio en même temps. Il est plus complexe que le focus clavier, car il est volontaire &ndash; l’application peut ignorer le fait qu’il ne pas actuellement a le focus audio et lire, &ndash; et parce qu’il existe différents types de concentration audio qui peuvent être demandé. Par exemple, si le demandeur est prévu uniquement pour lire des fichiers audio pour très peu de temps, il peut demander le focus temporaire.

Focus audio peut être accordé immédiatement, ou initialement refusé et accordé ultérieurement. Par exemple, si un application demandes audio le focus lors d’un appel téléphonique, lui sera refusé, mais le focus peut également bénéficier d’une fois que l’appel est terminé. Dans ce cas, un écouteur est inscrit pour réagir en conséquence, si le focus audio n’est pas possible. Demande de focus audio est utilisé pour déterminer si elle est OK pour lire ou enregistrement audio.

Pour plus d’informations sur le focus audio, consultez [la gestion de Focus Audio](http://developer.android.com/training/managing-audio/audio-focus.html).


<a name="Registering_the_Callback_for_Audio_Focus" />

#### <a name="registering-the-callback-for-audio-focus"></a>L’inscription du rappel de Focus Audio

L’inscription du `FocusChangeListener` rappel à partir de la `IOnAudioChangeListener` est une partie importante de l’obtention et la libération de focus audio. Il s’agit, car l’octroi de l’audio focus peut être différée jusqu'à une date ultérieure. Par exemple, une application peut demander à écouter de la musique pendant un appel téléphonique est déjà en cours. Le focus audio ne soient pas accordé jusqu'à la fin de l’appel téléphonique.

Pour cette raison, l’objet de rappel est passé en tant que paramètre dans le `GetAudioFocus` méthode de la `AudioManager`, et il s’agit de cet appel qui enregistre le rappel. Si le focus audio est initialement refusé mais ultérieurement accordé, l’application est informée en appelant `OnAudioFocusChange` sur le rappel. Ce procédé est utilisé pour indiquer que le focus audio est en cours révoquée à l’application.

Lorsque l’application a terminé d’utiliser les ressources audio, il appelle la `AbandonFocus` méthode de la `AudioManager`et le passe à nouveau dans le rappel. Cela annule l’inscription du rappel et libère les ressources audio, afin que les autres applications peuvent obtenir le focus audio.


<a name="Requesting_Audio_Focus" />

#### <a name="requesting-audio-focus"></a>Demande de Focus Audio

Les étapes nécessaires pour demander des ressources audio de l’appareil sont les suivants :

1.  Obtenir un handle vers le `AudioManager` service système.

2.  Créer une instance de la classe de rappel.

3.  Demander les ressources audio de l’appareil en appelant le `RequestAudioFocus` méthode sur le `AudioManager` . Les paramètres sont l’objet de rappel, le type de flux (musique, appel vocal, anneau, etc.) et le type de l’accès demandé à droite (les ressources audio peuvent être demandées momentanément ou pendant une durée indéterminée, par exemple).

4.  Si la demande est accordée, le `playMusic` méthode est appelée immédiatement, et le contenu audio commence à lire.

5.  Si la demande est refusée, aucune action supplémentaire n’est effectuée. Dans ce cas, le contenu audio jouera uniquement si la demande est accordée à une date ultérieure.


L’exemple de code ci-dessous montre comme suit :

```csharp
Boolean RequestAudioResources(INotificationReceiver parent)
{
  AudioManager audioMan = (AudioManager) GetSystemService(Context.AudioService);
  AudioManager.IOnAudioFocusChangeListener listener  = new MyAudioListener(this);
  var ret = audioMan.RequestAudioFocus (listener, Stream.Music, AudioFocus.Gain );
  if (ret == AudioFocusRequest.Granted) {
    playMusic();
    return (true);
  } else if (ret == AudioFocusRequest.Failed) {
    return (false);
  }
  return (false);
}
```

<a name="Releasing_Audio_Focus" />

#### <a name="releasing-audio-focus"></a>Libérer le Focus Audio

Lorsque la lecture de la piste est terminée, le `AbandonFocus` méthode `AudioManager` est appelée. Cela permet d’obtenir les ressources audio de l’appareil d’une autre application. Autres applications reçoit une notification de cette modification de focus audio si elles ont inscrit leurs propres écouteurs.

<a name="Low_Level_Audio_API" />

## <a name="low-level-audio-api"></a>API Audio de niveau bas

L’API de bas niveau audio fournissent un plus grand contrôle sur la lecture et l’enregistrement car elles interagissent directement avec les mémoires tampons au lieu d’utiliser des URI de fichier audio. Il existe certains scénarios dans lesquels cette approche est préférable. Ces scénarios sont les suivantes :

1.  Lors de la lecture à partir de fichiers audio chiffrés.

2.  Lors de la lecture d’une succession de clips courts.

3.  Diffusion audio en continu.


 <a name="AudioTrack_Class" />


### <a name="audiotrack-class"></a>Classe de AudioTrack

Le [AudioTrack](https://developer.xamarin.com/api/type/Android.Media.AudioTrack/) classe utilise l’API de bas niveau audio pour l’enregistrement et est l’équivalent de bas niveau de la `MediaPlayer` classe.

<a name="Initializing_and_Playing" />

#### <a name="initializing-and-playing"></a>L’initialisation et lecture

Pour lire le fichier audio, une nouvelle instance de `AudioTrack` doit être instancié. La liste d’arguments passé dans le [constructeur](https://developer.xamarin.com/api/type/Android.Media.AudioTrack/#memberlist) spécifie comment lire l’échantillonnage audio contenue dans la mémoire tampon. Les arguments sont :

1.  Type de flux &ndash; vocal, sonnerie, musique, système ou alarme.

2.  Fréquence &ndash; le taux d’échantillonnage exprimé en Hz.

3.  Configuration de canal &ndash; Mono ou stéréo.

4.  Le format audio &ndash; 8 bits ou codage 16 bits.

5.  Taille de mémoire tampon &ndash; en octets.

6.  Mode mémoire tampon &ndash; diffusion en continu ou statique.


Après la construction, la [lire](https://developer.xamarin.com/api/member/Android.Media.AudioTrack.Play%28%29/) méthode `AudioTrack` est appelée pour paramétrer pour commencer la lecture. L’écriture de la mémoire tampon audio à le `AudioTrack` démarre la lecture :

```csharp
void PlayAudioTrack(byte[] audioBuffer)
{
  AudioTrack audioTrack = new AudioTrack(
    // Stream type
    Stream.Music,
    // Frequency
    11025,
    // Mono or stereo
    ChannelOut.Mono,
    // Audio encoding
    Android.Media.Encoding.Pcm16bit,
    // Length of the audio clip.
    audioBuffer.Length,
    // Mode. Stream or static.
    AudioTrackMode.Stream);

    audioTrack.Play();
    audioTrack.Write(audioBuffer, 0, audioBuffer.Length);
}
```

<a name="Pausing_and_Stopping_the_Playback" />

#### <a name="pausing-and-stopping-the-playback"></a>Suspension et arrêt de la lecture

Appelez le [suspendre](https://developer.xamarin.com/api/member/Android.Media.AudioTrack.Pause%28%29/) méthode pour suspendre la lecture :

```csharp
audioTrack.Pause();
```

Appel de la [arrêter](https://developer.xamarin.com/api/member/Android.Media.AudioTrack.Stop%28%29/) méthode se termine la lecture définitivement :

```csharp
audioTrack.Stop();
```

<a name="Cleaning_up" />

#### <a name="cleanup"></a>Nettoyage

Lorsque le `AudioTrack` est n’est plus nécessaire, ses ressources doivent être libérées en appelant [version](https://developer.xamarin.com/api/member/Android.Media.AudioTrack.Release%28%29/):

```csharp
audioTrack.Release();
```

<a name="The_AudioRecord_Class" />

### <a name="the-audiorecord-class"></a>La classe AudioRecord

Le [AudioRecord](https://developer.xamarin.com/api/type/Android.Media.AudioRecord/) classe est l’équivalent de `AudioTrack` sur le côté de l’enregistrement. Comme `AudioTrack`, il utilise des mémoires tampons directement, à la place de fichiers et d’URI. Il requiert que le `RECORD_AUDIO` autorisation être défini dans le manifeste.

<a name="Initializing_and_Recording" />

#### <a name="initializing-and-recording"></a>L’initialisation et d’enregistrement

La première étape consiste à construire une nouvelle [AudioRecord](https://developer.xamarin.com/api/type/Android.Media.AudioRecord/) objet. La liste d’arguments passé dans le [constructeur](https://developer.xamarin.com/api/type/Android.Media.AudioRecord/#memberlist) fournit toutes les informations requises pour l’enregistrement. Contrairement à dans `AudioTrack`, où les arguments sont en grande partie des énumérations, les arguments équivalents dans `AudioRecord` sont des entiers. Elles incluent notamment :

1.  Source de d’entrée audio matériel tels que microphone.

2.  Type de flux &ndash; vocal, sonnerie, musique, système ou alarme.

3.  Fréquence &ndash; le taux d’échantillonnage exprimé en Hz.

4.  Configuration de canal &ndash; Mono ou stéréo.

5.  Le format audio &ndash; 8 bits ou codage 16 bits.

6.  Taille en octets de mémoire tampon


Une fois la `AudioRecord` est construite, son [StartRecording](https://developer.xamarin.com/api/member/Android.Media.AudioRecord.StartRecording%28%29/) méthode est appelée. Il est maintenant prêt à commencer l’enregistrement. Le `AudioRecord` en permanence lit la mémoire tampon audio pour l’entrée et écrit des entrées à un fichier audio.

```csharp
void RecordAudio()
{
  byte[] audioBuffer = new byte[100000];
  var audRecorder = new AudioRecord(
    // Hardware source of recording.
    AudioSource.Mic,
    // Frequency
    11025,
    // Mono or stereo
    ChannelIn.Mono,
    // Audio encoding
    Android.Media.Encoding.Pcm16bit,
    // Length of the audio clip.
    audioBuffer.Length
  );
  audRecorder.StartRecording();
  while (true) {
    try
    {
      // Keep reading the buffer while there is audio input.
      audRecorder.Read(audioBuffer, 0, audioBuffer.Length);
      // Write out the audio file.
    } catch (Exception ex) {
      Console.Out.WriteLine(ex.Message);
      break;
    }
  }
}
```

<a name="Stopping_the_Recording" />

#### <a name="stopping-the-recording"></a>Arrêt de l’enregistrement

Appel de la [arrêter](https://developer.xamarin.com/api/member/Android.Media.AudioRecord.Stop%28%29/) méthode met fin à l’enregistrement :

```csharp
audRecorder.Stop();
```

<a name="Clean_Up" />

#### <a name="cleanup"></a>Nettoyage

Lorsque le `AudioRecord` objet n’est plus nécessaire, appeler son [version](https://developer.xamarin.com/api/member/Android.Media.AudioRecord.Release%28%29/) méthode libère toutes les ressources associées :

```csharp
audRecorder.Release();
```

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Le système d’exploitation Android fournit une infrastructure puissante pour lecture, l’enregistrement et la gestion des données audio. Cet article couvert comment lire et enregistrer des données audio à l’aide de la vue `MediaPlayer` et `MediaRecorder` classes. Ensuite, elle a traité l’utilisation des notifications audio pour partager les ressources audio de l’appareil entre différentes applications. Enfin, il traités avec le mode de lecture et d’enregistrement audio à l’aide de l’API de bas niveau, qui directement l’interface avec mémoires tampons.


## <a name="related-links"></a>Liens associés

- [Utilisation avec Audio (exemple)](https://developer.xamarin.com/samples/Example_WorkingWithAudio/)
- [Lecteur multimédia](https://developer.xamarin.com/api/type/Android.Media.MediaPlayer/)
- [Enregistreur de support](https://developer.xamarin.com/api/type/Android.Media.MediaRecorder/)
- [Gestionnaire de audio](https://developer.xamarin.com/api/type/Android.Media.AudioManager/)
- [Piste audio](https://developer.xamarin.com/api/type/Android.Media.AudioTrack/)
- [Enregistreur audio](https://developer.xamarin.com/api/type/Android.Media.AudioRecord/)
