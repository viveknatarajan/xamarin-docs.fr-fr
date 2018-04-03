---
title: Reconnaissance vocale Android
description: Cet article décrit les principes fondamentaux de l’utilisation de l’espace de noms Android.Speech très puissante. Depuis sa création, Android a été en mesure de voix et le résultat sous forme de texte. Il est un processus relativement simple. Pour la synthèse vocale, toutefois, le processus est plus complexe que non seulement le moteur de reconnaissance vocale n’est à prendre en compte, mais également les langues disponibles et installés à partir du système de texte en parole (TTS).
ms.topic: article
ms.prod: xamarin
ms.assetid: FA3B8EC4-34D2-47E3-ACEA-BD34B28115B9
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/02/2018
ms.openlocfilehash: acc64fee37e1a6046991355389a09a29e1889993
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/03/2018
---
# <a name="android-speech"></a>Reconnaissance vocale Android

_Cet article décrit les principes fondamentaux de l’utilisation de l’espace de noms Android.Speech très puissante. Depuis sa création, Android a été en mesure de voix et le résultat sous forme de texte. Il est un processus relativement simple. Pour la synthèse vocale, toutefois, le processus est plus complexe que non seulement le moteur de reconnaissance vocale n’est à prendre en compte, mais également les langues disponibles et installés à partir du système de texte en parole (TTS)._

## <a name="speech-overview"></a>Vue d’ensemble de la reconnaissance vocale

Doté d’un système qui « comprend « voix humaine et enunciates ce qui est en cours tapé : vocale à du texte et texte par synthèse vocale, est une zone dont la croissance dans le développement mobile quand la demande pour la communication naturelle avec nos appareils augmente. Il existe de nombreux cas où ayant une fonctionnalité qui convertit le texte en parole, ou vice versa, est un outil très utile pour incorporer à votre application android.

Par exemple, avec crochet vers le bas sur l’utilisation de téléphone mobile lors de la commande, les utilisateurs souhaitent mains libres de fonctionnement de leurs appareils. La multitude de facteurs de forme Android, telles que l’usure Android, et l’inclusion jamais étendues de ceux en mesure d’utiliser les appareils Android (par exemple, les tablettes et remplit de note), a créé une vue plus grande sur des applications de synthèse.

Google fournit au développeur avec un ensemble rich d’API dans l’espace de noms Android.Speech pour couvrir la plupart des instances de fabrication d’un dispositif « vocale prenant en charge » (par exemple un logiciel conçu pour les aveugles).  L’espace de noms inclut la fonctionnalité pour autoriser le texte sera traduite dans vocale via `Android.Speech.Tts`, un contrôle sur le moteur utilisé pour effectuer la traduction, ainsi que d’un nombre de `RecognizerIntent`s qui permettent la reconnaissance vocale à convertir en texte.

Alors que les installations sont en matière de reconnaissance vocale doit être compréhensible, il existe des limitations en fonction du matériel utilisé. Il est peu probable que l’appareil correctement interprète tout parlée lui dans toutes les langues disponibles.

## <a name="requirements"></a>Spécifications

Il n’existe aucune exigence particulière pour ce guide, autre que votre appareil ayant un microphone et des haut-parleurs.

Le cœur d’un appareil Android interprétation vocale est l’utilisation d’un `Intent` avec correspondante `OnActivityResult`.
Il est important, cependant, de reconnaître que la reconnaissance vocale n’est pas compris, mais interprétées au texte. La différence est importante.

### <a name="the-difference-between-understanding-and-interpreting"></a>La différence entre la présentation et l’interprétation de

Une simple définition de présentation est que vous êtes en mesure de déterminer par ton et le contexte de la signification de ce que dit. Pour interpréter simplement consiste à prendre les mots et les génère dans une autre forme.

Prenons l’exemple simple suivant qui est utilisé dans une conversation de tous les jours : 

<kbd>Bonjour, comment vas-tu ?</kbd>

Sans inflexion (accentuation placée sur des mots spécifiques ou des parties de mots), il est une question simple. Toutefois, si un rythme lent est appliqué à la ligne, la personne à l’écoute détecte que la personne n’est pas trop heureux et peut-être doit t-shirt ou que la personne est car. Si l’accent sur « are », la personne qui est généralement plus intéressée par la réponse.

Sans audio relativement puissant de traitement pour rendre permet de comprendre le contexte de l’inflexion et un degré de l’intelligence artificielle (AI), le logiciel ne peut pas même commencer à comprendre ce qui a été indiqué, la meilleure faire d’un téléphone simple est convertir vocal en texte.

## <a name="setting-up"></a>Configuration

Avant d’utiliser le système de reconnaissance vocale, il est conseillé de vérifier pour vérifier que l’appareil a un microphone. Il y aura peu point vous essayez d’exécuter votre application sur un bloc-notes Kindle ou Google sans microphone installé.

L’exemple de code ci-dessous montre une interrogation si un micro est disponible et dans le cas contraire, pour créer une alerte. Si aucun microphone n’est disponible à ce stade vous serez quitter l’activité soit désactiver la fonctionnalité d’enregistrement vocal.

```csharp
string rec = Android.Content.PM.PackageManager.FeatureMicrophone;
if (rec != "android.hardware.microphone")
{
    var alert = new AlertDialog.Builder(recButton.Context);
    alert.SetTitle("You don't seem to have a microphone to record with");
    alert.SetPositiveButton("OK", (sender, e) =>
    {
        return;
    });
    alert.Show();
}
```

### <a name="creating-the-intent"></a>Création de l’objectif

L’objectif pour le système de reconnaissance vocale utilise un type particulier d’intention appelé le `RecognizerIntent`. Ce mode de contrôle d’un grand nombre de paramètres, y compris le délai d’attente avec latence jusqu'à ce que l’enregistrement est considéré comme sur toutes les langues supplémentaires pour reconnaître et de sortie et le texte à inclure dans le `Intent`de boîte de dialogue modale comme moyen d’instruction. Dans cet extrait de code, `VOICE` est un `readonly int` utilisé pour la reconnaissance dans `OnActivityResult`.

```csharp
var voiceIntent = new Intent(RecognizerIntent.ActionRecognizeSpeech);
voiceIntent.PutExtra(RecognizerIntent.ExtraLanguageModel, RecognizerIntent.LanguageModelFreeForm);
voiceIntent.PutExtra(RecognizerIntent.ExtraPrompt, Application.Context.GetString(Resource.String.messageSpeakNow));
voiceIntent.PutExtra(RecognizerIntent.ExtraSpeechInputCompleteSilenceLengthMillis, 1500);
voiceIntent.PutExtra(RecognizerIntent.ExtraSpeechInputPossiblyCompleteSilenceLengthMillis, 1500);
voiceIntent.PutExtra(RecognizerIntent.ExtraSpeechInputMinimumLengthMillis, 15000);
voiceIntent.PutExtra(RecognizerIntent.ExtraMaxResults, 1);
voiceIntent.PutExtra(RecognizerIntent.ExtraLanguage, Java.Util.Locale.Default);
StartActivityForResult(voiceIntent, VOICE);
```

### <a name="conversion-of-the-speech"></a>Conversion de la parole

Le texte interprété de la reconnaissance vocale sera remis dans le `Intent`, qui est retourné lorsque l’activité est terminée et est accessible `GetStringArrayListExtra(RecognizerIntent.ExtraResults)`. Cette opération retourne un `IList<string>`, dont l’index peut être utilisé et affichée en fonction du nombre de langues demandée dans le but de l’appelant (et spécifié dans le `RecognizerIntent.ExtraMaxResults`). Comme avec n’importe quelle liste, il convient de vérification pour vous assurer qu’il existe des données à afficher.

Lors de l’écoute de la valeur de retour d’un `StartActivityForResult`, le `OnActivityResult` (méthode) doit être fourni.

Dans l’exemple ci-dessous, `textBox` est un `TextBox` utilisée pour la sortie de ce qui a été défini. Il peut également être utilisé pour transférer le texte en une forme de l’interpréteur et à partir de là, l’application peut comparer le texte et une branche à une autre partie de l’application.

```csharp
protected override void OnActivityResult(int requestCode, Result resultVal, Intent data)
{
    if (requestCode == VOICE)
    {
        if (resultVal == Result.Ok)
        {
            var matches = data.GetStringArrayListExtra(RecognizerIntent.ExtraResults);
             if (matches.Count != 0)
             {
                  string textInput = textBox.Text + matches[0];
                  textBox.Text = textInput;
                  switch(matches[0].Substring(0,5).ToLower())
                  {
                     case "north":
                      MovePlayer(0);
                     break;
                   case "south":
                     MovePlayer(1);
                     break;
             }
             else
                  textBox.Text = "No speech was recognised";
        }
   }
    base.OnActivityResult(requestCode, resultVal, data);
}
```

## <a name="text-to-speech"></a>Conversion de texte par synthèse vocale

Texte par synthèse vocale n’est pas tout à fait l’inverse de la parole au texte et s’appuie sur deux composants clés ; un moteur de synthèse vocale en cours d’installation sur l’appareil et un langage en cours d’installation.

En grande partie, les appareils Android fournis avec la valeur par défaut service Google TTS installé et qu’au moins une langue. Cela est établie lorsque le périphérique est tout d’abord configurer et se basera sur où le périphérique est à la fois (par exemple, un téléphone à configurer en Allemagne installera l’allemand, alors qu’un en Amérique aura l’anglais américain).

### <a name="step-1---instantiating-texttospeech"></a>Étape 1 : instanciation de texttospeech avec le

`TextToSpeech` peut prendre jusqu'à 3 paramètres, les deux premiers sont requis et le troisième est facultatif (`AppContext`, `IOnInitListener`, `engine`). L’écouteur est utilisé pour lier vers le service et le test d’échec avec le moteur en cours de n’importe quel nombre de moteurs de disponible Android vocale. Au minimum, le périphérique aura moteur de Google.

### <a name="step-2---finding-the-languages-available"></a>Étape 2 : recherche les langues disponibles

Le `Java.Util.Locale` classe contient une méthode utile appelée `GetAvailableLocales()`. Cette liste de langues prises en charge par le moteur de reconnaissance vocale peut être testée sur les langues installées.

Il est sans importance pour générer la liste des langues de « compris ». Il y aura toujours une langue par défaut (la langue défini de l’utilisateur lorsqu’ils tout d’abord configurent son appareil), c’est le cas dans cet exemple la `List<string>` a « Default » comme premier paramètre, le reste de la liste est rempli en fonction du résultat de la `textToSpeech.IsLanguageAvailable(locale)`.

```csharp
var langAvailable = new List<string>{ "Default" };
var localesAvailable = Java.Util.Locale.GetAvailableLocales().ToList();
foreach (var locale in localesAvailable)
{
    var res = textToSpeech.IsLanguageAvailable(locale);
    switch (res)
    {
        case LanguageAvailableResult.Available:
          langAvailable.Add(locale.DisplayLanguage);
          break;
        case LanguageAvailableResult.CountryAvailable:
          langAvailable.Add(locale.DisplayLanguage);
          break;
        case LanguageAvailableResult.CountryVarAvailable:
          langAvailable.Add(locale.DisplayLanguage);
          break;
    }
}
langAvailable = langAvailable.OrderBy(t => t).Distinct().ToList();
```

Ce code appelle [TextToSpeech.IsLanguageAvailable](https://developer.xamarin.com/api/member/Android.Speech.Tts.TextToSpeech.IsLanguageAvailable/p/Java.Util.Locale/) pour tester si le module linguistique pour les paramètres régionaux donnés est déjà présent sur l’appareil. Cette méthode retourne un [LanguageAvailableResult](https://developer.xamarin.com/api/type/Android.Speech.Tts.LanguageAvailableResult/), ce qui indique si la langue pour les paramètres régionaux passé est disponible. Si `LanguageAvailableResult` indique que la langue est `NotSupported`, alors il n’existe aucun package vocal (même pour le téléchargement) pour cette langue. Si `LanguageAvailableResult` a la valeur `MissingData`, il est possible de télécharger un nouveau package de langue, comme expliqué ci-dessous à l’étape 4.

### <a name="step-3---setting-the-speed-and-pitch"></a>Étape 3 : définition de la vitesse et le pas

Android permet à l’utilisateur de modifier le son de la voix en modifiant le `SpeechRate` et `Pitch` (le taux de vitesse et de la tonalité de la voix). Cela va de 0 à 1, contenant des paroles « normal » à 1 pour les deux.

### <a name="step-4---testing-and-loading-new-languages"></a>Étape 4 : test et le chargement de nouvelles langues

Téléchargement d’un nouveau langage est effectué en utilisant un `Intent`. Le résultat de cette intention entraîne la [OnActivityResult](https://developer.xamarin.com/api/member/Android.App.Activity.OnActivityResult/) méthode à appeler. Contrairement à l’exemple de reconnaissance vocale de texte (qui est utilisé le [RecognizerIntent](https://developer.xamarin.com/api/type/Android.Speech.RecognizerIntent/) comme un `PutExtra` paramètre à la `Intent`), le test et le chargement `Intent`sont `Action`-en fonction :

-   [TextToSpeech.Engine.ActionCheckTtsData](https://developer.xamarin.com/api/field/Android.Speech.Tts.TextToSpeech+Engine.ActionCheckTtsData/) &ndash; démarre une activité à partir de la plate-forme `TextToSpeech` moteur pour vérifier une installation correcte et la disponibilité des ressources de langue sur l’appareil.

-   [TextToSpeech.Engine.ActionInstallTtsData](https://developer.xamarin.com/api/field/Android.Speech.Tts.TextToSpeech+Engine.ActionInstallTtsData/) &ndash; démarre une activité qui invite l’utilisateur à télécharger les langues nécessaires.

L’exemple de code suivant illustre l’utilisation de ces actions pour vérifier la présence de ressources de langue et de télécharger un nouveau langage de :

```csharp
var checkTTSIntent = new Intent();
checkTTSIntent.SetAction(TextToSpeech.Engine.ActionCheckTtsData);
StartActivityForResult(checkTTSIntent, NeedLang);
//
protected override void OnActivityResult(int req, Result res, Intent data)
{
    if (req == NeedLang)
    {
        var installTTS = new Intent();
        installTTS.SetAction(TextToSpeech.Engine.ActionInstallTtsData);
        StartActivity(installTTS);
    }
}
```

`TextToSpeech.Engine.ActionCheckTtsData` vérifie la disponibilité des ressources de langue. `OnActivityResult` est appelé lorsque ce test est terminé. Si les ressources de langue doivent être téléchargés, `OnActivityResult` déclenche le `TextToSpeech.Engine.ActionInstallTtsData` pour démarrer une activité qui permet à l’utilisateur à télécharger les langues nécessaires. Remarque que cette `OnActivityResult` implémentation ne vérifie pas la `Result` de code, car, dans cet exemple simplifié, la détermination a déjà été faite que le package de langue doit être téléchargé.

Le `TextToSpeech.Engine.ActionInstallTtsData` action entraîne le **données vocales de Google TTS** activité à être présentés à l’utilisateur de choisir les langues à télécharger :

![Activité des données Google synthèse vocale](speech-images/01-google-tts-voice-data.png)

Par exemple, l’utilisateur peut choisir le Français et cliquez sur l’icône de téléchargement pour télécharger des données vocales Français :

![Exemple de téléchargement de langue Français](speech-images/02-selecting-french.png)

Installation de ces données s’effectue automatiquement une fois le téléchargement terminé.


### <a name="step-5---the-ioninitlistener"></a>Étape 5 - le IOnInitListener

Une activité peut être en mesure de convertir le texte par synthèse vocale, la méthode d’interface `OnInit` doit être implémentée (Ceci est le deuxième paramètre spécifié pour l’instanciation de la `TextToSpeech` classe). Initialise l’écouteur et le résultat des tests.

L’écouteur doit effectuer un test à la fois `OperationResult.Success` et `OperationResult.Failure` au minimum.
L’exemple suivant montre que :

```csharp
void TextToSpeech.IOnInitListener.OnInit(OperationResult status)
{
    // if we get an error, default to the default language
    if (status == OperationResult.Error)
        textToSpeech.SetLanguage(Java.Util.Locale.Default);
    // if the listener is ok, set the lang
    if (status == OperationResult.Success)
        textToSpeech.SetLanguage(lang);
}
```

## <a name="summary"></a>Récapitulatif

Dans ce guide, nous avons examiné les principes fondamentaux de conversion de texte par synthèse vocale et reconnaissance vocale au texte et les méthodes de la façon de les inclure dans vos propres applications. Pendant qu’ils ne couvrent pas tous les cas particulier, vous disposez à présent une compréhension de base de l’interprétation de la reconnaissance vocale, l’installation de nouvelles langues et comment augmenter l’inclusivity de vos applications.



## <a name="related-links"></a>Liens associés

- [Xamarin.Forms DependencyService](https://developer.xamarin.com/samples/UsingDependencyService/)
- [Texte en parole (exemple)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/TextToSpeech)
- [Reconnaissance vocale à du texte (exemple)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/SpeechToText)
- [Espace de noms Android.Speech](https://developer.xamarin.com/api/namespace/Android.Speech/)
- [Espace de noms Android.Speech.Tts](https://developer.xamarin.com/api/namespace/Android.Speech.Tts/)
