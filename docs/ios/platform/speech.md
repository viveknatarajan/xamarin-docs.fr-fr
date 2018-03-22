---
title: Reconnaissance vocale
description: "Cet article présente la nouvelle API de reconnaissance vocale et montre comment l’implémenter dans une application Xamarin.iOS pour prendre en charge la reconnaissance vocale continue et transcription de la reconnaissance vocale (direct ou enregistrés flux audio) dans le texte."
ms.topic: article
ms.prod: xamarin
ms.assetid: 64FED50A-6A28-4833-BEAE-63CEC9A09010
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: e868c0ee71688e208c5217d9f5a89ea3acec988c
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
---
# <a name="speech-recognition"></a>Reconnaissance vocale

_Cet article présente la nouvelle API de reconnaissance vocale et montre comment l’implémenter dans une application Xamarin.iOS pour prendre en charge la reconnaissance vocale continue et transcription de la reconnaissance vocale (direct ou enregistrés flux audio) dans le texte._

Nouveau pour iOS 10, Apple a libérer de l’API de reconnaissance vocale qui permet à une application iOS pour la reconnaissance vocale continue de prendre en charge et la transcription de la reconnaissance vocale (direct ou enregistrés flux audio) dans le texte.

Apple, en fonction de l’API de reconnaissance vocale a les fonctionnalités et les avantages suivants :

- Une grande précision
- État de l’Art
- Facile à utiliser
- Rapide
- Prend en charge plusieurs langues
- Confidentialité de l’utilisateur respecte

## <a name="how-speech-recognition-works"></a>Fonctionne de la reconnaissance vocale

La reconnaissance vocale est implémentée dans une application iOS en direct ou pré-enregistrée audio (dans un des langages parlés prenant en charge l’API) lors de l’acquisition et en le passant à un module de reconnaissance vocale qui retourne une transcription en texte brut des mots parlés.

[![](speech-images/speech01.png "Fonctionne de la reconnaissance vocale")](speech-images/speech01.png#lightbox)

### <a name="keyboard-dictation"></a>Dictée de clavier

Lorsque la plupart des utilisateurs de réflexion de la reconnaissance vocale sur un appareil iOS, ils considérer l’assistant vocal Siri intégrée, qui a été publiée en même temps que le clavier dictée dans iOS 5 avec la 4 s iPhone.

Clavier dictée est pris en charge par n’importe quel élément d’interface qui prend en charge TextKit (tel que `UITextField` ou `UITextArea`) et est activé par l’utilisateur en cliquant sur le bouton dictée (directement à gauche de la barre d’espace) sur le clavier virtuel iOS.

Apple a publié les statistiques dictée de clavier suivantes (collectées depuis 2011) :

- Clavier dictée a été largement utilisée, car il a été publié dans iOS 5.
- Environ 65 000 applications l’utilisation par jour.
- Sur un tiers d’e/s de tous les dictée est effectuée dans une application de partie 3.

Clavier dictée est très facile à utiliser, car il ne nécessite aucun effort de part du développeur, autre qu’à l’aide d’un élément d’interface TextKit dans la conception de l’interface utilisateur de l’application. Clavier dictée a également l’avantage de ne pas nécessiter toutes les demandes de privilège spécial à partir de l’application avant de pouvoir être utilisé.

Applications qui utilisent les nouvelles API de reconnaissance vocale nécessite des autorisations spéciales pour pouvoir être accordé par l’utilisateur, étant donné que la reconnaissance vocale nécessite la transmission et le stockage temporaire des données sur les serveurs Apple. Veuillez consulter notre [sécurité et confidentialité améliorations](~/ios/app-fundamentals/security-privacy.md) pour plus d’informations.

Lors de la dictée du clavier est facile à implémenter, il n’est fourni avec plusieurs limitations et des inconvénients :

- Elle nécessite l’utilisation d’un champ d’entrée de texte et d’affichage d’un clavier.
- Il fonctionne avec audio en direct d’entrée uniquement, et l’application n’a aucun contrôle sur le processus d’enregistrement audio.
- Il ne fournit aucun contrôle sur le langage utilisé pour interpréter la reconnaissance vocale de l’utilisateur.
- Il n’existe aucun moyen de l’application de savoir si le bouton dictée est encore disponible pour l’utilisateur.
- L’application ne peut pas personnaliser le processus d’enregistrement audio.
- Il fournit un ensemble de résultats très superficiel qui ne dispose pas des informations telles que la synchronisation et la confiance.

### <a name="speech-recognition-api"></a>API de reconnaissance vocale

Nouveau pour iOS 10, Apple a publié l’API de reconnaissance vocale qui offre un moyen plus puissant pour une application iOS implémenter la reconnaissance vocale. Cette API méthode est le même que celui de l’alimentation Siri et clavier dictée par Apple et il est capable de fournir une transcription rapide avec une précision de pointe.

Les résultats fournis par l’API de reconnaissance vocale sont personnalisées en toute transparence à des utilisateurs individuels, sans l’application ayant collecter ou accéder aux données utilisateur privées.

L’API de reconnaissance vocale fournit les résultats vers l’application appelante en temps réel ou presque que l’utilisateur est élevé, et il fournit plus d’informations sur les résultats de la traduction à uniquement du texte. Elles incluent notamment :

- Interprétée de plusieurs façons de ce que l’utilisateur a répondu.
- Niveaux de confiance pour les traductions individuelles.
- Informations de minutage.

Comme indiqué ci-dessus, l’audio pour la traduction peut être fourni par un flux en direct, ou à partir de l’enregistrement source et dans les dialectes pris en charge par iOS 10 et plus de 50 langues.

L’API de reconnaissance vocale peut être utilisé sur n’importe quel appareil iOS exécutant iOS 10 et dans la plupart des cas, requiert une connexion internet active dans la mesure où la majorité des traductions a lieu sur les serveurs Apple. Ceci dit, certaines plus récente iOS périphériques prennent en charge toujours sur, la traduction sur le périphérique de langues spécifiques.

Apple a inclus une API de disponibilité pour déterminer si une langue donnée est disponible pour la traduction au moment actuel. L’application doit utiliser cette API plutôt que de tester votre connexion internet lui-même directement.

Comme indiqué ci-dessus dans la section clavier dictée, la reconnaissance vocale nécessite la transmission et le stockage temporaire des données sur les serveurs Apple sur internet et par conséquent, l’application _doit_ demander l’autorisation de l’utilisateur à effectuer reconnaissance en incluant le `NSSpeechRecognitionUsageDescription` clé dans son `Info.plist` fichier et en appelant le `SFSpeechRecognizer.RequestAuthorization` (méthode). 

En fonction de la source des données audio utilisé pour la reconnaissance vocale, autres modifications à l’application `Info.plist` fichier peut être nécessaire. Veuillez consulter notre [sécurité et confidentialité améliorations](~/ios/app-fundamentals/security-privacy.md) pour plus d’informations.

## <a name="adopting-speech-recognition-in-an-app"></a>Adoption de la reconnaissance vocale dans une application

Il existe quatre étapes majeures, le développeur doit prendre d’adopter la reconnaissance vocale dans une application iOS :

- Fournissez une description de l’utilisation de l’application `Info.plist` de fichiers à l’aide de la `NSSpeechRecognitionUsageDescription` clé. Par exemple, une application de caméra peut inclure la description suivante, _« Cela vous permet de prendre une photo en indiquant que le mot « cheese ». »_
- Demande d’autorisation en appelant le `SFSpeechRecognizer.RequestAuthorization` méthode pour présenter une explication (fourni dans le `NSSpeechRecognitionUsageDescription` clé ci-dessus) de la raison pour laquelle l’application veut vocale reconnaissance de l’accès à l’utilisateur dans une boîte de dialogue et les autoriser à accepter ou refuser.
- Créer une demande de reconnaissance vocale :
    * Pour un fichier audio préenregistré sur le disque, utilisez la `SFSpeechURLRecognitionRequest` classe.
    * Pour l’audio (ou audio à partir de la mémoire), utilisez la `SFSPeechAudioBufferRecognitionRequest` classe.
- Transmettre la demande de reconnaissance vocale à un module de reconnaissance vocale (`SFSpeechRecognizer`) pour commencer la reconnaissance. L’application peut éventuellement contenir sur retourné `SFSpeechRecognitionTask` pour surveiller et suivre les résultats de la reconnaissance.

Ces étapes seront abordées en détail ci-dessous.

### <a name="providing-a-usage-description"></a>Fournir une Description de l’utilisation

Pour fournir le texte requis `NSSpeechRecognitionUsageDescription` clé dans le `Info.plist` de fichiers, procédez comme suit :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1. Double-cliquez sur le `Info.plist` fichier à ouvrir pour le modifier.
2. Basculez vers le **Source** vue : 

    [![](speech-images/speech02.png "La vue de Source")](speech-images/speech02.png#lightbox)
3. Cliquez sur **ajouter une nouvelle entrée**, entrez `NSSpeechRecognitionUsageDescription` pour le **propriété**, `String` pour le **Type** et un **Description d’utilisation** comme le **valeur**. Exemple : 

    [![](speech-images/speech03.png "Ajout de NSSpeechRecognitionUsageDescription")](speech-images/speech03.png#lightbox)
4. Si l’application doit gérer la transcription audio en direct, elle nécessite également une Description de l’utilisation du Microphone. Cliquez sur **ajouter une nouvelle entrée**, entrez `NSMicrophoneUsageDescription` pour le **propriété**, `String` pour le **Type** et un **Description d’utilisation** comme le **valeur**. Exemple : 

    [![](speech-images/speech04.png "Ajout de NSMicrophoneUsageDescription")](speech-images/speech04.png#lightbox)
4. Enregistrez les modifications dans le fichier.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Double-cliquez sur le `Info.plist` fichier à ouvrir pour le modifier.
3. Cliquez sur **ajouter une nouvelle entrée**, entrez `NSSpeechRecognitionUsageDescription` pour le **propriété**, `String` pour le **Type** et un **Description d’utilisation** comme le **valeur**. Exemple : 

    [![](speech-images/speech03w.png "Ajout de NSSpeechRecognitionUsageDescription")](speech-images/speech03w.png#lightbox)
4. Si l’application doit gérer la transcription audio en direct, elle nécessite également une Description de l’utilisation du Microphone. Cliquez sur **ajouter une nouvelle entrée**, entrez `NSMicrophoneUsageDescription` pour le **propriété**, `String` pour le **Type** et un **Description d’utilisation** comme le **valeur**. Exemple : 

    [![](speech-images/speech04w.png "Ajout de NSMicrophoneUsageDescription")](speech-images/speech04w.png#lightbox)
4. Enregistrez les modifications dans le fichier.

-----

> [!IMPORTANT]
> Échec d’un des éléments ci-dessus fournissent `Info.plist` clés (`NSSpeechRecognitionUsageDescription` ou `NSMicrophoneUsageDescription`) peut entraîner l’application échoue sans avertissement lors de la tentative d’accéder à la reconnaissance vocale ou sur le microphone audio en direct.




### <a name="requesting-authorization"></a>Demande d’autorisation

Pour demander l’autorisation d’utilisateur requis qui autorise l’application à accéder à la reconnaissance vocale, modifiez la classe de contrôleur de la vue principale et ajoutez le code suivant :

```csharp
using System;
using UIKit;
using Speech;

namespace MonkeyTalk
{
    public partial class ViewController : UIViewController
    {
        protected ViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }

        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Request user authorization
            SFSpeechRecognizer.RequestAuthorization ((SFSpeechRecognizerAuthorizationStatus status) => {
                // Take action based on status
                switch (status) {
                case SFSpeechRecognizerAuthorizationStatus.Authorized:
                    // User has approved speech recognition
                    ...
                    break;
                case SFSpeechRecognizerAuthorizationStatus.Denied:
                    // User has declined speech recognition
                    ...
                    break;
                case SFSpeechRecognizerAuthorizationStatus.NotDetermined:
                    // Waiting on approval
                    ...
                    break;
                case SFSpeechRecognizerAuthorizationStatus.Restricted:
                    // The device is not permitted
                    ...
                    break;
                }
            });
        }
    }
}
```

Le `RequestAuthorization` méthode de la `SFSpeechRecognizer` classe sera demander l’autorisation de l’utilisateur pour la reconnaissance vocale accès à l’aide de la raison pour laquelle le développeur fourni dans le `NSSpeechRecognitionUsageDescription` clé de le `Info.plist` fichier.

A `SFSpeechRecognizerAuthorizationStatus` résultat est retourné à le `RequestAuthorization` la routine de rappel de la méthode qui peut être utilisée pour prendre des mesures en fonction de l’autorisation de l’utilisateur. 

> [!IMPORTANT]
> Apple suggère en attente jusqu'à ce que l’utilisateur a démarré une action dans l’application qui nécessite la reconnaissance vocale avant de demander cette autorisation.

### <a name="recognizing-pre-recorded-speech"></a>Reconnaissance vocale pré-enregistré

Si l’application souhaite voix à partir d’un fichier WAV ou MP3 pré-enregistré, il peut utiliser le code suivant :

```csharp
using System;
using UIKit;
using Speech;
using Foundation;
...

public void RecognizeFile (NSUrl url)
{
    // Access new recognizer
    var recognizer = new SFSpeechRecognizer ();

    // Is the default language supported?
    if (recognizer == null) {
        // No, return to caller
        return;
    }

    // Is recognition available?
    if (!recognizer.Available) {
        // No, return to caller
        return;
    }

    // Create recognition task and start recognition
    var request = new SFSpeechUrlRecognitionRequest (url);
    recognizer.GetRecognitionTask (request, (SFSpeechRecognitionResult result, NSError err) => {
        // Was there an error?
        if (err != null) {
            // Handle error
            ...
        } else {
            // Is this the final translation?
            if (result.Final) {
                Console.WriteLine ("You said, \"{0}\".", result.BestTranscription.FormattedString);
            }
        }
    });
}
```

Examinez ce code en détail, tout d’abord, il tente de créer un module de reconnaissance vocale (`SFSpeechRecognizer`). Si la langue par défaut n’est pas pris en charge pour la reconnaissance vocale, `null` est retournée et les sorties de fonctions.

Si le module de reconnaissance vocale n’est disponible pour la langue par défaut, l’application vérifie s’il est actuellement disponible pour l’utilisation de la reconnaissance du `Available` propriété. Par exemple, reconnaissance ne peut pas être disponible si l’appareil n’a pas une connexion internet active.

A `SFSpeechUrlRecognitionRequest` est créé à partir de la `NSUrl` emplacement du fichier vidéo sur l’appareil iOS et il est transmis au module de reconnaissance vocale pour traiter une routine de rappel.

Lorsque le rappel est appelé, si le `NSError` n’est pas `null` a eu une erreur qui doit être gérée. Étant donné que la reconnaissance vocale est effectuée de façon incrémentielle, la routine de rappel peut être appelée plusieurs fois pour que le `SFSpeechRecognitionResult.Final` propriété est testée pour voir si la traduction est terminée et meilleure version de la traduction est écrit (`BestTranscription`).

### <a name="recognizing-live-speech"></a>Reconnaissance vocale dynamique

Si l’application souhaite voix en direct, le processus est très similaire à la reconnaissance vocale préenregistré. Exemple :

```csharp
using System;
using UIKit;
using Speech;
using Foundation;
using AVFoundation;
...

#region Private Variables
private AVAudioEngine AudioEngine = new AVAudioEngine ();
private SFSpeechRecognizer SpeechRecognizer = new SFSpeechRecognizer ();
private SFSpeechAudioBufferRecognitionRequest LiveSpeechRequest = new SFSpeechAudioBufferRecognitionRequest ();
private SFSpeechRecognitionTask RecognitionTask;
#endregion
...

public void StartRecording ()
{
    // Setup audio session
    var node = AudioEngine.InputNode;
    var recordingFormat = node.GetBusOutputFormat (0);
    node.InstallTapOnBus (0, 1024, recordingFormat, (AVAudioPcmBuffer buffer, AVAudioTime when) => {
        // Append buffer to recognition request
        LiveSpeechRequest.Append (buffer);
    });

    // Start recording
    AudioEngine.Prepare ();
    NSError error;
    AudioEngine.StartAndReturnError (out error);

    // Did recording start?
    if (error != null) {
        // Handle error and return
        ...
        return;
    }

    // Start recognition
    RecognitionTask = SpeechRecognizer.GetRecognitionTask (LiveSpeechRequest, (SFSpeechRecognitionResult result, NSError err) => {
        // Was there an error?
        if (err != null) {
            // Handle error
            ...
        } else {
            // Is this the final translation?
            if (result.Final) {
                Console.WriteLine ("You said \"{0}\".", result.BestTranscription.FormattedString);
            }
        }
    });
}

public void StopRecording ()
{
    AudioEngine.Stop ();
    LiveSpeechRequest.EndAudio ();
}

public void CancelRecording ()
{
    AudioEngine.Stop ();
    RecognitionTask.Cancel ();
}
```

Examinez ce code en détail, il crée plusieurs variables privées pour gérer le processus de reconnaissance :

```csharp
private AVAudioEngine AudioEngine = new AVAudioEngine ();
private SFSpeechRecognizer SpeechRecognizer = new SFSpeechRecognizer ();
private SFSpeechAudioBufferRecognitionRequest LiveSpeechRequest = new SFSpeechAudioBufferRecognitionRequest ();
private SFSpeechRecognitionTask RecognitionTask;
```

Il utilise AV Foundation enregistrement audio qui sera passée à une `SFSpeechAudioBufferRecognitionRequest` pour traiter la demande de reconnaissance :

```csharp
var node = AudioEngine.InputNode;
var recordingFormat = node.GetBusOutputFormat (0);
node.InstallTapOnBus (0, 1024, recordingFormat, (AVAudioPcmBuffer buffer, AVAudioTime when) => {
    // Append buffer to recognition request
    LiveSpeechRequest.Append (buffer);
});
```

L’application tente de démarrer l’enregistrement et toutes les erreurs sont gérées Si l’enregistrement ne peut pas être démarré :

```csharp
AudioEngine.Prepare ();
NSError error;
AudioEngine.StartAndReturnError (out error);

// Did recording start?
if (error != null) {
    // Handle error and return
    ...
    return;
}
```

Démarrage de la tâche de reconnaissance et un handle est conservé à la tâche de reconnaissance (`SFSpeechRecognitionTask`) :

```csharp
RecognitionTask = SpeechRecognizer.GetRecognitionTask (LiveSpeechRequest, (SFSpeechRecognitionResult result, NSError err) => {
    ...
});
```

Le rappel est utilisé de façon similaire à celle utilisée ci-dessus sur vocale préenregistré.

Si l’enregistrement est arrêté par l’utilisateur, le moteur Audio et la demande de reconnaissance vocale sont informés :

```csharp
AudioEngine.Stop ();
LiveSpeechRequest.EndAudio ();
```

Si l’utilisateur annule la reconnaissance, le moteur Audio et la tâche de reconnaissance sont informés :

```csharp
AudioEngine.Stop ();
RecognitionTask.Cancel ();
```

Il est important d’appeler `RecognitionTask.Cancel` si l’utilisateur annule la traduction pour libérer de la mémoire et processeur de l’appareil.

> [!IMPORTANT]
> Ne parvient pas à fournir le `NSSpeechRecognitionUsageDescription` ou `NSMicrophoneUsageDescription` `Info.plist` clés peuvent entraîner l’application échoue sans avertissement lors de la tentative d’accéder à la reconnaissance vocale ou sur le microphone dynamique audio (`var node = AudioEngine.InputNode;`). Consultez le **fournir une Description de l’utilisation de** section ci-dessus pour plus d’informations.

## <a name="speech-recognition-limits"></a>Limites de reconnaissance vocale

Apple impose les limitations suivantes lorsque vous travaillez avec la reconnaissance vocale dans une application iOS :

- La reconnaissance vocale est libre de toutes les applications, mais son utilisation n’est pas illimitée :
    - Les appareils iOS individuels ont un nombre limité de reconnaissance qui peuvent être effectuées par jour.
    - Les applications sont limitées globalement sur une base de la demande par jour.
- L’application doit être préparée à gérer les échecs de limite de taux d’utilisation et de connexion réseau de la reconnaissance vocale.
- La reconnaissance vocale peut avoir un coût élevé de décharge de la batterie et un trafic réseau élevé sur son appareil iOS, pour cette raison, Apple impose une limite de durée audio strict d’environ une minute de la parole max.

Si une application sollicite régulièrement ses limites de limitation de la vitesse, Apple vous demande que le développeur les contacter.

## <a name="privacy-and-usability-considerations"></a>Considérations relatives à la facilité d’utilisation et confidentialité

Apple a la suggestion suivante pour transparentes et en respectant la confidentialité quand vous incluez la reconnaissance vocale dans une application iOS :

- Lors de l’enregistrement vocal de l’utilisateur, veillez à indiquer clairement que l’enregistrement se déroule dans l’Interface utilisateur de l’application. Par exemple, l’application peut lire un « enregistrement » son et affiche un indicateur d’enregistrement.
- N’utilisez pas la reconnaissance vocale pour des informations utilisateur sensibles telles que les mots de passe, les données d’intégrité ou des informations financières.
- Afficher les résultats de la reconnaissance _avant_ agissant sur ces derniers. Cela fournit non seulement un commentaire quant à ce que l’application effectue, mais permet à l’utilisateur gérer les erreurs de reconnaissance comme elles ont été effectuées.

## <a name="summary"></a>Récapitulatif

Cet article a présenté la nouvelle API de reconnaissance vocale et a montré comment l’implémenter dans une application Xamarin.iOS pour prendre en charge la reconnaissance vocale continue et transcription de la reconnaissance vocale (direct ou enregistrés flux audio) dans le texte. 



## <a name="related-links"></a>Liens associés

- [SpeakToMe (sample)](https://developer.xamarin.com/samples/monotouch/ios10/SpeakToMe/)
