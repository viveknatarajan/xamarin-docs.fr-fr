---
title: CallKit dans Xamarin.iOS
description: Cet article traite de la nouvelle API CallKit par Apple publié dans iOS 10 et comment l’implémenter dans les applications Xamarin.iOS VOIP.
ms.prod: xamarin
ms.assetid: 738A142D-FFD2-4738-B3ED-57C273179848
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/15/2017
ms.openlocfilehash: 40d4a6cbd3bb8e3bd4c55c50c69f85f91d94feac
ms.sourcegitcommit: 2f6a5c1abf90fbdb0475fd8a3ce6de3cd7c7d575
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52459926"
---
# <a name="callkit-in-xamarinios"></a>CallKit dans Xamarin.iOS

La nouvelle API CallKit dans iOS 10 offre un moyen pour les applications VOIP intégrer l’interface utilisateur de l’iPhone et fournir une interface familière et à l’utilisateur final. Avec cette API les utilisateurs peuvent afficher et interagir avec les appels VOIP à partir de l’écran de verrouillage de l’appareil iOS et gérer des contacts à l’aide de l’application de téléphone **favoris** et **récents** vues.

## <a name="about-callkit"></a>À propos de CallKit

En fonction d’Apple, CallKit est une nouvelle infrastructure qui élève les applications de voix sur IP (VOIP) tiers 3e un 1er tiers expérience sur iOS 10. L’API CallKit permet aux applications VOIP intégrer l’interface utilisateur de l’iPhone et fournir une interface familière et à l’utilisateur final. Tout comme l’application de téléphone intégrée, un utilisateur peut afficher et interagir avec les appels VOIP à partir de l’écran de verrouillage de l’appareil iOS et gérer des contacts à l’aide de l’application de téléphone **favoris** et **récents** vues.

En outre, l’API CallKit offre la possibilité de créer des Extensions d’application qui peut associer un numéro de téléphone avec un nom (ID de l’appelant), ou indiquer le système lorsqu’un nombre doit être bloqué (appel bloquant).

### <a name="the-existing-voip-app-experience"></a>L’expérience d’application VOIP existant

Avant d’aborder la nouvelle API CallKit et ses capacités, examinez l’expérience utilisateur en cours avec un 3e partie application VOIP dans iOS 9 (et inférieure) à l’aide d’une application VOIP fictive appelée MonkeyCall. MonkeyCall est une application simple qui permet à l’utilisateur envoyer et recevoir des appels VOIP à l’aide de l’API pour iOS existants.

Actuellement, si l’utilisateur reçoit un appel entrant sur MonkeyCall et leur iPhone est verrouillé, la notification reçue sur l’écran de verrouillage est indiscernable à partir de n’importe quel autre type de notification (comme ceux des Messages ou les applications de messagerie par exemple).

Si l’utilisateur souhaite répondez à l’appel, ils devaient faire disparaître la notification MonkeyCall pour ouvrir l’application et de saisir leur mot de passe (ou l’utilisateur Touch ID) pour déverrouiller le téléphone avant qu’ils peuvent accepter l’appel et démarrer la conversation.

L’expérience est tout aussi lourde si le téléphone est déverrouillé. Là encore, l’appel entrant MonkeyCall s’affiche comme une bannière de notification standard dans les diapositives à partir du haut de l’écran. Étant donné que la notification est temporaire, il peut être omis facilement par l’utilisateur les forcer à ouvrir le centre de Notification et de trouver la notification spécifique pour répondre ensuite appeler ou rechercher et lancer l’application MonkeyCall manuellement.

### <a name="the-callkit-voip-app-experience"></a>L’expérience d’application CallKit VOIP

En implémentant les nouvelles APIs CallKit dans l’application MonkeyCall, l’expérience utilisateur avec un appel VOIP entrant peut être considérablement améliorée dans iOS 10. Prenons l’exemple de l’utilisateur reçoit un appel VOIP lors de leur numéro de téléphone est verrouillé ci-dessus. En implémentant CallKit, l’appel s’affiche sur l’écran de verrouillage de l’iPhone, comme il le ferait si l’appel a été reçu à partir de l’application de téléphone intégrée, les plein écran, une interface utilisateur native et les fonctionnalités standard de balayage à répondre.

Là encore, si l’iPhone est déverrouillé lors de la réception d’un appel MonkeyCall VOIP, le même plein écran, une interface utilisateur native et fonctionnalité standard de balayage à répondre et tap-refuser des intégrés Phone application est présentée et MonkeyCall a la possibilité de lire une sonnerie personnalisée .

CallKit fournit des fonctionnalités supplémentaires à MonkeyCall, ce qui permet son VOIP appelle pour interagir avec d’autres types d’appels, apparaissent dans le récents intégré et répertorie les favoris, pour utiliser les fonctionnalités intégrées de bloc et de ne pas déranger, démarrez MonkeyCall appels à partir de Siri et offre la possibilité pour les utilisateurs affecter des appels MonkeyCall aux personnes de l’application Contacts.

Les sections suivantes décrit l’architecture CallKit, entrants et sortants appellent des flux et l’API CallKit en détail.

## <a name="the-callkit-architecture"></a>L’architecture CallKit

Dans iOS 10, Apple a adopté CallKit dans tous les Services système tels que les appels effectués sur CarPlay, par exemple, sont connus à l’interface utilisateur système via CallKit. Dans l’exemple ci-dessous, dans la mesure où MonkeyCall adopte CallKit, il est inconnu du système dans la même façon que ces Services système intégrés et tous les mêmes fonctionnalités :

[![](callkit-images/callkit01.png "La pile CallKit Service")](callkit-images/callkit01.png#lightbox)

Examinons plus en détail l’application MonkeyCall dans le diagramme ci-dessus. L’application contient tout le code pour communiquer avec son propre réseau et contient ses propres Interfaces utilisateur. Il est lié dans CallKit pour communiquer avec le système :

[![](callkit-images/callkit02.png "Architecture d’application MonkeyCall")](callkit-images/callkit02.png#lightbox)

Il existe deux interfaces principales dans CallKit qui utilise l’application :

- `CXProvider` -Cela permet à l’application MonkeyCall informer le système de toutes les notifications hors-bande qui peuvent se produire.
- `CXCallController` -Permet à l’application MonkeyCall informer le système d’actions de l’utilisateur local.

### <a name="the-cxprovider"></a>Le CXProvider

Comme indiqué ci-dessus, `CXProvider` permet à une application indiquer au système de toutes les notifications hors-bande qui peuvent se produire. Il s’agit de notification qui n’ont pas lieu en raison d’actions de l’utilisateur local, mais se produire en raison d’événements externes tels que les appels entrants.

Une application doit utiliser le `CXProvider` pour les éléments suivants :

- Signaler un appel entrant sur le système.
- Signaler un sortant d’appel s’est connecté au système.
- L’utilisateur distant se terminant par l’appel au système de rapports.

Lorsque l’application souhaite communiquer avec le système, il utilise le `CXCallUpdate` classe et lorsque le système a besoin communiquer avec l’application, il utilise le `CXAction` classe :

[![](callkit-images/callkit03.png "Communiquer avec le système via une CXProvider")](callkit-images/callkit03.png#lightbox)

### <a name="the-cxcallcontroller"></a>Le CXCallController

Le `CXCallController` permet à une application indiquer au système local d’actions des utilisateurs telles que l’utilisateur à partir d’un appel VOIP. En implémentant un `CXCallController` Obtient de l’application et interagissent avec d’autres types d’appels dans le système. Par exemple, s’il existe déjà un appel de téléphonie active en cours d’exécution, `CXCallController` peut permettre à l’application VOIP placer cet appel en attente et de démarrer ou de répondre à un appel VOIP.

Une application doit utiliser le `CXCallController` pour les éléments suivants :

- Rapport lorsque l’utilisateur a commencé à un appel sortant sur le système.
- Rapport lorsque l’utilisateur répond à un appel entrant sur le système.
- Rapport lorsque l’utilisateur termine un appel au système.

Lorsque l’application souhaite communiquer les actions utilisateur local pour le système, il utilise le `CXTransaction` classe :

[![](callkit-images/callkit04.png "Création de rapports dans le système à l’aide d’un CXCallController")](callkit-images/callkit04.png#lightbox)

## <a name="implementing-callkit"></a>Implémentation CallKit

Les sections suivantes seront montrent comment implémenter CallKit dans une application Xamarin.iOS VOIP. Par exemple, ce document utilisera code à partir de l’application MonkeyCall VOIP fictive. Le code présenté ici représente plusieurs classes de prise en charge, le CallKit des parties spécifiques seront traitées en détail dans les sections suivantes.

### <a name="the-activecall-class"></a>La classe ActiveCall

Le `ActiveCall` classe est utilisée par l’application MonkeyCall pour contenir toutes les informations relatives à un appel VOIP qui est actuellement actif comme suit :

```csharp
using System;
using CoreFoundation;
using Foundation;

namespace MonkeyCall
{
    public class ActiveCall
    {
        #region Private Variables
        private bool isConnecting;
        private bool isConnected;
        private bool isOnhold;
        #endregion

        #region Computed Properties
        public NSUuid UUID { get; set; }
        public bool isOutgoing { get; set; }
        public string Handle { get; set; }
        public DateTime StartedConnectingOn { get; set;}
        public DateTime ConnectedOn { get; set;}
        public DateTime EndedOn { get; set; }

        public bool IsConnecting {
            get { return isConnecting; }
            set {
                isConnecting = value;
                if (isConnecting) StartedConnectingOn = DateTime.Now;
                RaiseStartingConnectionChanged ();
            }
        }

        public bool IsConnected {
            get { return isConnected; }
            set {
                isConnected = value;
                if (isConnected) {
                    ConnectedOn = DateTime.Now;
                } else {
                    EndedOn = DateTime.Now;
                }
                RaiseConnectedChanged ();
            }
        }

        public bool IsOnHold {
            get { return isOnhold; }
            set {
                isOnhold = value;
            }
        }
        #endregion

        #region Constructors
        public ActiveCall ()
        {
        }

        public ActiveCall (NSUuid uuid, string handle, bool outgoing)
        {
            // Initialize
            this.UUID = uuid;
            this.Handle = handle;
            this.isOutgoing = outgoing;
        }
        #endregion

        #region Public Methods
        public void StartCall (ActiveCallbackDelegate completionHandler)
        {
            // Simulate the call starting successfully
            completionHandler (true);

            // Simulate making a starting and completing a connection
            DispatchQueue.MainQueue.DispatchAfter (new DispatchTime(DispatchTime.Now, 3000), () => {
                // Note that the call is starting
                IsConnecting = true;

                // Simulate pause before connecting
                DispatchQueue.MainQueue.DispatchAfter (new DispatchTime (DispatchTime.Now, 1500), () => {
                    // Note that the call has connected
                    IsConnecting = false;
                    IsConnected = true;
                });
            });
        }

        public void AnswerCall (ActiveCallbackDelegate completionHandler)
        {
            // Simulate the call being answered
            IsConnected = true;
            completionHandler (true);
        }

        public void EndCall (ActiveCallbackDelegate completionHandler)
        {
            // Simulate the call ending
            IsConnected = false;
            completionHandler (true);
        }
        #endregion

        #region Events
        public delegate void ActiveCallbackDelegate (bool successful);
        public delegate void ActiveCallStateChangedDelegate (ActiveCall call);

        public event ActiveCallStateChangedDelegate StartingConnectionChanged;
        internal void RaiseStartingConnectionChanged ()
        {
            if (this.StartingConnectionChanged != null) this.StartingConnectionChanged (this);
        }

        public event ActiveCallStateChangedDelegate ConnectedChanged;
        internal void RaiseConnectedChanged ()
        {
            if (this.ConnectedChanged != null) this.ConnectedChanged (this);
        }
        #endregion
    }
}
```

`ActiveCall` contient plusieurs propriétés qui définissent l’état de l’appel et deux événements qui peuvent être déclenchés lorsque l’état d’appel change. Dans la mesure où il s’agit d’un exemple uniquement, il existe trois méthodes permettant de simulée de démarrage, répondre et se terminant par un appel.

### <a name="the-startcallrequest-class"></a>La classe StartCallRequest

Le `StartCallRequest` classe statique, fournit quelques méthodes d’assistance qui seront utilisés lors de l’utilisation de sortant appelle :

```csharp
using System;
using Foundation;
using Intents;

namespace MonkeyCall
{
    public static class StartCallRequest
    {
        public static string URLScheme {
            get { return "monkeycall"; }
        }

        public static string ActivityType {
            get { return INIntentIdentifier.StartAudioCall.GetConstant ().ToString (); }
        }

        public static string CallHandleFromURL (NSUrl url)
        {
            // Is this a MonkeyCall handle?
            if (url.Scheme == URLScheme) {
                // Yes, return host
                return url.Host;
            } else {
                // Not handled
                return null;
            }
        }

        public static string CallHandleFromActivity (NSUserActivity activity)
        {
            // Is this a start call activity?
            if (activity.ActivityType == ActivityType) {
                // Yes, trap any errors
                try {
                    // Get first contact
                    var interaction = activity.GetInteraction ();
                    var startAudioCallIntent = interaction.Intent as INStartAudioCallIntent;
                    var contact = startAudioCallIntent.Contacts [0];

                    // Get the person handle
                    return contact.PersonHandle.Value;
                } catch {
                    // Error, report null
                    return null;
                }
            } else {
                // Not handled
                return null;
            }
        }
    }
}
```

Le `CallHandleFromURL` et `CallHandleFromActivity` classes sont utilisées dans l’AppDelegate pour obtenir le handle de contact de la personne qui est appelé dans un appel sortant. Pour plus d’informations, consultez le [gère les appels sortants](#Handling-Outgoing-Calls) section ci-dessous.

### <a name="the-activecallmanager-class"></a>La classe ActiveCallManager

Le `ActiveCallManager` classe gère tous les appels dans l’application MonkeyCall.

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using CallKit;

namespace MonkeyCall
{
    public class ActiveCallManager
    {
        #region Private Variables
        private CXCallController CallController = new CXCallController ();
        #endregion

        #region Computed Properties
        public List<ActiveCall> Calls { get; set; }
        #endregion

        #region Constructors
        public ActiveCallManager ()
        {
            // Initialize
            this.Calls = new List<ActiveCall> ();
        }
        #endregion

        #region Private Methods
        private void SendTransactionRequest (CXTransaction transaction)
        {
            // Send request to call controller
            CallController.RequestTransaction (transaction, (error) => {
                // Was there an error?
                if (error == null) {
                    // No, report success
                    Console.WriteLine ("Transaction request sent successfully.");
                } else {
                    // Yes, report error
                    Console.WriteLine ("Error requesting transaction: {0}", error);
                }
            });
        }
        #endregion

        #region Public Methods
        public ActiveCall FindCall (NSUuid uuid)
        {
            // Scan for requested call
            foreach (ActiveCall call in Calls) {
                if (call.UUID.Equals(uuid)) return call;
            }

            // Not found
            return null;
        }

        public void StartCall (string contact)
        {
            // Build call action
            var handle = new CXHandle (CXHandleType.Generic, contact);
            var startCallAction = new CXStartCallAction (new NSUuid (), handle);

            // Create transaction
            var transaction = new CXTransaction (startCallAction);

            // Inform system of call request
            SendTransactionRequest (transaction);
        }

        public void EndCall (ActiveCall call)
        {
            // Build action
            var endCallAction = new CXEndCallAction (call.UUID);

            // Create transaction
            var transaction = new CXTransaction (endCallAction);

            // Inform system of call request
            SendTransactionRequest (transaction);
        }

        public void PlaceCallOnHold (ActiveCall call)
        {
            // Build action
            var holdCallAction = new CXSetHeldCallAction (call.UUID, true);

            // Create transaction
            var transaction = new CXTransaction (holdCallAction);

            // Inform system of call request
            SendTransactionRequest (transaction);
        }

        public void RemoveCallFromOnHold (ActiveCall call)
        {
            // Build action
            var holdCallAction = new CXSetHeldCallAction (call.UUID, false);

            // Create transaction
            var transaction = new CXTransaction (holdCallAction);

            // Inform system of call request
            SendTransactionRequest (transaction);
        }
        #endregion
    }
}
```

À nouveau, car il s’agit d’une simulation uniquement, le `ActiveCallManager` uniquement gère une collection de `ActiveCall` objets et a une routine pour la recherche d’un appel donné par son `UUID` propriété. Il inclut également des méthodes pour démarrer, de se terminer et de modifier l’état en attente d’un appel sortant. Pour plus d’informations, consultez le [gère les appels sortants](#Handling-Outgoing-Calls) section ci-dessous.

### <a name="the-providerdelegate-class"></a>La classe ProviderDelegate

Comme indiqué ci-dessus, un `CXProvider` fournit une communication bidirectionnelle entre l’application et le système pour les notifications hors-bande. Le développeur doit fournir un personnalisé `CXProviderDelegate` et l’attacher à la `CXProvider` pour l’application gérer les événements de CallKit hors-bande. MonkeyCall utilise les éléments suivants `CXProviderDelegate`:

```csharp
using System;
using Foundation;
using CallKit;
using UIKit;

namespace MonkeyCall
{
    public class ProviderDelegate : CXProviderDelegate
    {
        #region Computed Properties
        public ActiveCallManager CallManager { get; set;}
        public CXProviderConfiguration Configuration { get; set; }
        public CXProvider Provider { get; set; }
        #endregion

        #region Constructors
        public ProviderDelegate (ActiveCallManager callManager)
        {
            // Save connection to call manager
            CallManager = callManager;

            // Define handle types
            var handleTypes = new [] { (NSNumber)(int)CXHandleType.PhoneNumber };

            // Get Image Template
            var templateImage = UIImage.FromFile ("telephone_receiver.png");

            // Setup the initial configurations
            Configuration = new CXProviderConfiguration ("MonkeyCall") {
                MaximumCallsPerCallGroup = 1,
                SupportedHandleTypes = new NSSet<NSNumber> (handleTypes),
                IconTemplateImageData = templateImage.AsPNG(),
                RingtoneSound = "musicloop01.wav"
            };

            // Create a new provider
            Provider = new CXProvider (Configuration);

            // Attach this delegate
            Provider.SetDelegate (this, null);

        }
        #endregion

        #region Override Methods
        public override void DidReset (CXProvider provider)
        {
            // Remove all calls
            CallManager.Calls.Clear ();
        }

        public override void PerformStartCallAction (CXProvider provider, CXStartCallAction action)
        {
            // Create new call record
            var activeCall = new ActiveCall (action.CallUuid, action.CallHandle.Value, true);

            // Monitor state changes
            activeCall.StartingConnectionChanged += (call) => {
                if (call.isConnecting) {
                    // Inform system that the call is starting
                    Provider.ReportConnectingOutgoingCall (call.UUID, call.StartedConnectingOn.ToNSDate());
                }
            };

            activeCall.ConnectedChanged += (call) => {
                if (call.isConnected) {
                    // Inform system that the call has connected
                    provider.ReportConnectedOutgoingCall (call.UUID, call.ConnectedOn.ToNSDate ());
                }
            };

            // Start call
            activeCall.StartCall ((successful) => {
                // Was the call able to be started?
                if (successful) {
                    // Yes, inform the system
                    action.Fulfill ();

                    // Add call to manager
                    CallManager.Calls.Add (activeCall);
                } else {
                    // No, inform system
                    action.Fail ();
                }
            });
        }

        public override void PerformAnswerCallAction (CXProvider provider, CXAnswerCallAction action)
        {
            // Find requested call
            var call = CallManager.FindCall (action.CallUuid);

            // Found?
            if (call == null) {
                // No, inform system and exit
                action.Fail ();
                return;
            }

            // Attempt to answer call
            call.AnswerCall ((successful) => {
                // Was the call successfully answered?
                if (successful) {
                    // Yes, inform system
                    action.Fulfill ();
                } else {
                    // No, inform system
                    action.Fail ();
                }
            });
        }

        public override void PerformEndCallAction (CXProvider provider, CXEndCallAction action)
        {
            // Find requested call
            var call = CallManager.FindCall (action.CallUuid);

            // Found?
            if (call == null) {
                // No, inform system and exit
                action.Fail ();
                return;
            }

            // Attempt to answer call
            call.EndCall ((successful) => {
                // Was the call successfully answered?
                if (successful) {
                    // Remove call from manager's queue
                    CallManager.Calls.Remove (call);

                    // Yes, inform system
                    action.Fulfill ();
                } else {
                    // No, inform system
                    action.Fail ();
                }
            });
        }

        public override void PerformSetHeldCallAction (CXProvider provider, CXSetHeldCallAction action)
        {
            // Find requested call
            var call = CallManager.FindCall (action.CallUuid);

            // Found?
            if (call == null) {
                // No, inform system and exit
                action.Fail ();
                return;
            }

            // Update hold status
            call.isOnHold = action.OnHold;

            // Inform system of success
            action.Fulfill ();
        }

        public override void TimedOutPerformingAction (CXProvider provider, CXAction action)
        {
            // Inform user that the action has timed out
        }

        public override void DidActivateAudioSession (CXProvider provider, AVFoundation.AVAudioSession audioSession)
        {
            // Start the calls audio session here
        }

        public override void DidDeactivateAudioSession (CXProvider provider, AVFoundation.AVAudioSession audioSession)
        {
            // End the calls audio session and restart any non-call
            // related audio
        }
        #endregion

        #region Public Methods
        public void ReportIncomingCall (NSUuid uuid, string handle)
        {
            // Create update to describe the incoming call and caller
            var update = new CXCallUpdate ();
            update.RemoteHandle = new CXHandle (CXHandleType.Generic, handle);

            // Report incoming call to system
            Provider.ReportNewIncomingCall (uuid, update, (error) => {
                // Was the call accepted
                if (error == null) {
                    // Yes, report to call manager
                    CallManager.Calls.Add (new ActiveCall (uuid, handle, false));
                } else {
                    // Report error to user here
                    Console.WriteLine ("Error: {0}", error);
                }
            });
        }
        #endregion
    }
}
```

Lorsqu’une instance de ce délégué est créée, il est passé le `ActiveCallManager` qu’il utilisera pour gérer toute activité d’appel. Ensuite, il définit les types de handle (`CXHandleType`) qui le `CXProvider` répondra à :

```csharp
// Define handle types
var handleTypes = new [] { (NSNumber)(int)CXHandleType.PhoneNumber };
```

Et il obtient l’image de modèle qui est appliqué à l’icône de l’application lorsqu’un appel est en cours :

```csharp
// Get Image Template
var templateImage = UIImage.FromFile ("telephone_receiver.png");
```

Ces valeurs obtient regroupées dans un `CXProviderConfiguration` qui sera utilisé pour configurer le `CXProvider`:

```csharp
// Setup the initial configurations
Configuration = new CXProviderConfiguration ("MonkeyCall") {
    MaximumCallsPerCallGroup = 1,
    SupportedHandleTypes = new NSSet<NSNumber> (handleTypes),
    IconTemplateImageData = templateImage.AsPNG(),
    RingtoneSound = "musicloop01.wav"
};
```

Le délégué crée ensuite un nouveau `CXProvider` avec ces configurations et s’attache à celui-ci :

```csharp
// Create a new provider
Provider = new CXProvider (Configuration);

// Attach this delegate
Provider.SetDelegate (this, null);
```

Lorsque vous utilisez CallKit, l’application n’est plus crée et gère ses propres sessions audio, au lieu de cela, il doit configurer et utiliser une session audio que le système de créer et gérer pour celui-ci. 

S’il s’agissait d’une application réelle, les `DidActivateAudioSession` méthode doit être utilisée pour démarrer l’appel avec un préconfiguré `AVAudioSession` que le système fourni :

```csharp
public override void DidActivateAudioSession (CXProvider provider, AVFoundation.AVAudioSession audioSession)
{
    // Start the call's audio session here...
}
```

Il utilise également le `DidDeactivateAudioSession` méthode pour finaliser et sa connexion au système de la version fournie de session audio :

```csharp
public override void DidDeactivateAudioSession (CXProvider provider, AVFoundation.AVAudioSession audioSession)
{
    // End the calls audio session and restart any non-call
    // releated audio
}
```

Le reste du code est abordé en détail dans les sections qui suivent.

### <a name="the-appdelegate-class"></a>La classe AppDelegate

MonkeyCall utilise l’AppDelegate pour contenir des instances de la `ActiveCallManager` et `CXProviderDelegate` qui sera utilisé tout au long de l’application :

```csharp
using Foundation;
using UIKit;
using Intents;
using System;

namespace MonkeyCall
{
    [Register ("AppDelegate")]
    public class AppDelegate : UIApplicationDelegate
    {
        #region Constructors
        public override UIWindow Window { get; set; }
        public ActiveCallManager CallManager { get; set; }
        public ProviderDelegate CallProviderDelegate { get; set; }
        #endregion

        #region Override Methods
        public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
        {
            // Initialize the call handlers
            CallManager = new ActiveCallManager ();
            CallProviderDelegate = new ProviderDelegate (CallManager);

            return true;
        }

        public override bool OpenUrl (UIApplication app, NSUrl url, NSDictionary options)
        {
            // Get handle from url
            var handle = StartCallRequest.CallHandleFromURL (url);

            // Found?
            if (handle == null) {
                // No, report to system
                Console.WriteLine ("Unable to get call handle from URL: {0}", url); 
                return false;
            } else {
                // Yes, start call and inform system
                CallManager.StartCall (handle);
                return true;
            }
        }

        public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
        {
            var handle = StartCallRequest.CallHandleFromActivity (userActivity);

            // Found?
            if (handle == null) {
                // No, report to system
                Console.WriteLine ("Unable to get call handle from User Activity: {0}", userActivity);
                return false;
            } else {
                // Yes, start call and inform system
                CallManager.StartCall (handle);
                return true;
            }
        }

        ...
        #endregion
    }
}
```

Le `OpenUrl` et `ContinueUserActivity` remplacement de méthodes sont utilisées lorsque l’application traite un appel sortant. Pour plus d’informations, consultez le [gère les appels sortants](#Handling-Outgoing-Calls) section ci-dessous.

## <a name="handling-incoming-calls"></a>Gestion des appels entrants

Il existe plusieurs États et les processus qui un appel VOIP entrant peut franchir pendant un flux de travail appel entrant classiques tels que :

- Pour informer l’utilisateur (et le système) qu’il existe un appel entrant.
- Réception d’une notification lorsque l’utilisateur veut pour répondre à l’appel et l’initialisation de l’appel avec un autre utilisateur.
- Informe le système et le réseau de Communication lorsque l’utilisateur souhaite mettre fin à l’appel actuel.

Les sections suivantes prendra un examen détaillé de la façon dont une application peut utiliser CallKit pour gérer le flux de travail de l’appel entrant, à nouveau à l’aide de l’application MonkeyCall VOIP, par exemple.

### <a name="informing-user-of-incoming-call"></a>Pour informer l’utilisateur d’appel entrant

Lorsqu’un utilisateur distant a démarré une conversation de VOIP avec l’utilisateur local, les événements suivants se produisent :

[![](callkit-images/callkit05.png "Un utilisateur distant a démarré une conversation VOIP")](callkit-images/callkit05.png#lightbox)

1. L’application reçoit une notification à partir de son réseau de Communications qu’il existe un appel VOIP entrant.
2. L’application utilise le `CXProvider` pour envoyer un `CXCallUpdate` au système informant de l’appel.
3. Le système publie l’appel à l’interface utilisateur système, les Services système et toutes les autres applications VOIP à l’aide de CallKit.

Par exemple, dans le `CXProviderDelegate`:

```csharp
public void ReportIncomingCall (NSUuid uuid, string handle)
{
    // Create update to describe the incoming call and caller
    var update = new CXCallUpdate ();
    update.RemoteHandle = new CXHandle (CXHandleType.Generic, handle);

    // Report incoming call to system
    Provider.ReportNewIncomingCall (uuid, update, (error) => {
        // Was the call accepted
        if (error == null) {
            // Yes, report to call manager
            CallManager.Calls.Add (new ActiveCall (uuid, handle, false));
        } else {
            // Report error to user here
            Console.WriteLine ("Error: {0}", error);
        }
    });
}
```

Ce code crée un nouveau `CXCallUpdate` instance et attache un handle qui identifiera l’appelant. Ensuite, il utilise le `ReportNewIncomingCall` méthode de la `CXProvider` classe pour indiquer au système de l’appel. Si elle réussit, l’appel est ajouté à la collection de l’application des appels actives, si elle n’est pas le cas, l’erreur doit être signalé à l’utilisateur.

### <a name="user-answering-incoming-call"></a>Appel entrant réponse utilisateur

Si l’utilisateur souhaite recevoir l’appel VOIP entrant, les événements suivants se produisent :

[![](callkit-images/callkit06.png "L’utilisateur répond à l’appel VOIP entrant")](callkit-images/callkit06.png#lightbox)

1. L’interface utilisateur système informe le système que l’utilisateur souhaite répondre à l’appel VOIP.
2. Le système envoie un `CXAnswerCallAction` à l’application `CXProvider` informant de l’intention de réponse.
3. L’application informe son réseau de Communication que l’utilisateur est à répondre à l’appel et l’appel VOIP continue comme d’habitude.

Par exemple, dans le `CXProviderDelegate`:

```csharp
public override void PerformAnswerCallAction (CXProvider provider, CXAnswerCallAction action)
{
    // Find requested call
    var call = CallManager.FindCall (action.CallUuid);

    // Found?
    if (call == null) {
        // No, inform system and exit
        action.Fail ();
        return;
    }

    // Attempt to answer call
    call.AnswerCall ((successful) => {
        // Was the call successfully answered?
        if (successful) {
            // Yes, inform system
            action.Fulfill ();
        } else {
            // No, inform system
            action.Fail ();
        }
    });
}
```

Ce code commence par rechercher l’appel donné dans sa liste d’appels actifs. Si l’appel ne peut pas être trouvée, le système est informé et la méthode s’arrête. S’il est trouvé, le `AnswerCall` méthode de la `ActiveCall` classe est appelée pour démarrer l’appel et le système est plus d’informations, si elle réussit ou échoue.

### <a name="user-ending-incoming-call"></a>Utilisateur se terminant par appel entrant

Si l’utilisateur souhaite mettre fin à l’appel à partir de l’interface utilisateur de l’application, les événements suivants se produisent :

[![](callkit-images/callkit07.png "L’utilisateur met fin à l’appel à partir de l’interface utilisateur de l’application")](callkit-images/callkit07.png#lightbox)

1. L’application crée `CXEndCallAction` qui obtient regroupé dans un `CXTransaction` qui est envoyé au système qu’il l’informe que l’appel se termine.
2. Le système vérifie l’intention d’appeler de fin et envoie le `CXEndCallAction` à l’application via le `CXProvider`.
3. L’application informe ensuite son réseau de Communication que l’appel se termine.

Par exemple, dans le `CXProviderDelegate`:

```csharp
public override void PerformEndCallAction (CXProvider provider, CXEndCallAction action)
{
    // Find requested call
    var call = CallManager.FindCall (action.CallUuid);

    // Found?
    if (call == null) {
        // No, inform system and exit
        action.Fail ();
        return;
    }

    // Attempt to answer call
    call.EndCall ((successful) => {
        // Was the call successfully answered?
        if (successful) {
            // Remove call from manager's queue
            CallManager.Calls.Remove (call);

            // Yes, inform system
            action.Fulfill ();
        } else {
            // No, inform system
            action.Fail ();
        }
    });
}
```

Ce code commence par rechercher l’appel donné dans sa liste d’appels actifs. Si l’appel ne peut pas être trouvée, le système est informé et la méthode s’arrête. S’il est trouvé, le `EndCall` méthode de la `ActiveCall` classe est appelée pour mettre fin à l’appel et le système est plus d’informations, si elle réussit ou échoue. En cas de réussite, l’appel est supprimé de la collection d’appels actifs.

## <a name="managing-multiple-calls"></a>La gestion de plusieurs appels

La plupart des applications VOIP peut gérer plusieurs appels à la fois. Par exemple, s’il existe actuellement un appel VOIP actif et la notification d’application obtient qu’il existe un nouvel appel entrant, l’utilisateur peut mettre en pause ou raccrocher sur le premier appel à répondre à l’autre.

Dans le champ de la situation ci-dessus, le système vous communiquera un `CXTransaction` à l’application qui inclut une liste de plusieurs actions (telles que la `CXEndCallAction` et le `CXAnswerCallAction`). Toutes ces actions devrez remplir individuellement, afin que le système peut mettre à jour l’interface utilisateur en conséquence.

## <a name="handling-outgoing-calls"></a>Gestion des sortant appelle

Si l’utilisateur appuie sur une entrée dans la liste de documents récents (dans l’application de téléphone), par exemple, c’est à partir d’un appel appartenant à l’application, il recevront un _démarrer les intention appeler_ par le système :

[![](callkit-images/callkit08.png "Réception d’une intention d’appel de début")](callkit-images/callkit08.png#lightbox)

1. L’application créera une _Action appeler un Démarrer_ en fonction de la démarrer appeler intention qu’il a reçu à partir du système. 
2. L’application utilisera le `CXCallController` pour demander l’Action appeler un démarrage à partir du système.
3. Si le système accepte l’Action, elle s’affichera à l’application via le `XCProvider` déléguer.
4. L’application démarre l’appel sortant avec son réseau de Communication.

Pour plus d’informations sur les intentions, veuillez consulter notre [Intents et Intents UI Extensions](~/ios/platform/sirikit/understanding-sirikit.md) documentation. 

### <a name="the-outgoing-call-lifecycle"></a>Le cycle de vie appel sortant

Lorsque vous travaillez avec CallKit et un appel sortant, l’application doit informer le système des événements de cycle de vie suivants :

1. **Démarrage** -indiquer au système d’appel sortant est prêt à démarrer.
2. **Démarrer** -indiquer au système d’appel sortant a démarré.
3. **Connexion** -informer le système que l’appel sortant se connecte.
4. **Connecté** -informer sortant appel s’est connecté et que les deux parties peuvent parler maintenant.

Par exemple, le code suivant démarre un appel sortant :

```csharp
private CXCallController CallController = new CXCallController ();
...

private void SendTransactionRequest (CXTransaction transaction)
{
    // Send request to call controller
    CallController.RequestTransaction (transaction, (error) => {
        // Was there an error?
        if (error == null) {
            // No, report success
            Console.WriteLine ("Transaction request sent successfully.");
        } else {
            // Yes, report error
            Console.WriteLine ("Error requesting transaction: {0}", error);
        }
    });
}

public void StartCall (string contact)
{
    // Build call action
    var handle = new CXHandle (CXHandleType.Generic, contact);
    var startCallAction = new CXStartCallAction (new NSUuid (), handle);

    // Create transaction
    var transaction = new CXTransaction (startCallAction);

    // Inform system of call request
    SendTransactionRequest (transaction);
}
```

Il crée un `CXHandle` et l’utilise pour configurer un `CXStartCallAction` qui est fourni dans un `CXTransaction` qui est envoyé au système en utilisant le `RequestTransaction` méthode de la `CXCallController` classe. En appelant le `RequestTransaction` (méthode), le système peut placer n’importe quel existant appels en attente, quelle que soit la source (application de téléphone, FaceTime, VOIP, etc.), avant le démarrage de l’appel de nouveau.

La demande de démarrage d’un appel VOIP peut provenir de différentes sources, telles que Siri, une entrée sur une carte de Contact (dans l’application Contacts) ou à partir de la liste des documents récents (dans l’application de téléphone). Dans ces situations, l’application sera envoyée une intention démarrer les appeler à l’intérieur d’un `NSUserActivity` et l’AppDelegate devra gérer :

```csharp
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{
    var handle = StartCallRequest.CallHandleFromActivity (userActivity);

    // Found?
    if (handle == null) {
        // No, report to system
        Console.WriteLine ("Unable to get call handle from User Activity: {0}", userActivity);
        return false;
    } else {
        // Yes, start call and inform system
        CallManager.StartCall (handle);
        return true;
    }
}
```

Ici le `CallHandleFromActivity` méthode de la classe d’assistance `StartCallRequest` sert à obtenir le handle à la personne qui est appelée (consultez [la classe StartCallRequest](#The-StartCallRequest-Class) ci-dessus). 

Le `PerformStartCallAction` méthode de la [ProviderDelegate classe](#The-ProviderDelegate-Class) est utilisé pour démarrer l’appel sortant réel enfin et d’informer le système de son cycle de vie :

```csharp
public override void PerformStartCallAction (CXProvider provider, CXStartCallAction action)
{
    // Create new call record
    var activeCall = new ActiveCall (action.CallUuid, action.CallHandle.Value, true);

    // Monitor state changes
    activeCall.StartingConnectionChanged += (call) => {
        if (call.IsConnecting) {
            // Inform system that the call is starting
            Provider.ReportConnectingOutgoingCall (call.UUID, call.StartedConnectingOn.ToNSDate());
        }
    };

    activeCall.ConnectedChanged += (call) => {
        if (call.IsConnected) {
            // Inform system that the call has connected
            Provider.ReportConnectedOutgoingCall (call.UUID, call.ConnectedOn.ToNSDate ());
        }
    };

    // Start call
    activeCall.StartCall ((successful) => {
        // Was the call able to be started?
        if (successful) {
            // Yes, inform the system
            action.Fulfill ();

            // Add call to manager
            CallManager.Calls.Add (activeCall);
        } else {
            // No, inform system
            action.Fail ();
        }
    });
}
```

Il crée une instance de la `ActiveCall` classe (pour conserver les informations sur l’appel en cours) et la remplit avec la personne qui est appelée. Le `StartingConnectionChanged` et `ConnectedChanged` événements sont utilisés pour contrôler et signaler le cycle de vie d’appel sortant. L’appel est démarré et le système informé que l’Action a été exécutée.

### <a name="ending-an-outgoing-call"></a>Se terminant par un appel sortant

Lorsque l’utilisateur a fini avec un appel sortant et qu’elle souhaite y mettre fin, le code suivant peut être utilisé :

```csharp
private CXCallController CallController = new CXCallController ();
...

private void SendTransactionRequest (CXTransaction transaction)
{
    // Send request to call controller
    CallController.RequestTransaction (transaction, (error) => {
        // Was there an error?
        if (error == null) {
            // No, report success
            Console.WriteLine ("Transaction request sent successfully.");
        } else {
            // Yes, report error
            Console.WriteLine ("Error requesting transaction: {0}", error);
        }
    });
}

public void EndCall (ActiveCall call)
{
    // Build action
    var endCallAction = new CXEndCallAction (call.UUID);

    // Create transaction
    var transaction = new CXTransaction (endCallAction);

    // Inform system of call request
    SendTransactionRequest (transaction);
}
```

Si crée un `CXEndCallAction` avec l’UUID de l’appel à la fin, les regroupe dans un `CXTransaction` qui est envoyé au système en utilisant le `RequestTransaction` méthode de la `CXCallController` classe. 

## <a name="additional-callkit-details"></a>Détails supplémentaires CallKit

Cette section décrit certains détails supplémentaires que le développeur doit prendre en considération lorsque vous travaillez avec CallKit telles que :

- Configuration du fournisseur
- Erreurs d’action
- Restrictions du système
- Données Audio

### <a name="provider-configuration"></a>Configuration du fournisseur

La configuration du fournisseur permet à une application VOIP iOS 10 personnaliser l’expérience utilisateur (à l’intérieur de l’appel en une interface utilisateur native) lorsque l’utilisation de CallKit.

Une application peut effectuer les types de personnalisations suivants :

- Afficher un nom localisé.
- Activer la prise en charge de l’appel vidéo.
- Personnaliser les boutons de l’interface utilisateur en appel à l’aide de sa propre icône d’image de modèle. Interaction utilisateur avec des boutons personnalisés est envoyée directement à l’application à traiter. 

### <a name="action-errors"></a>Erreurs d’action

les applications VOIP iOS 10 à l’aide de CallKit doivent gérer les Actions échouent de manière appropriée et tenir l’utilisateur informé de l’état de l’Action à tout moment. 

Prenons l’exemple suivant en considération :

1. L’application a reçu une Action appeler un démarrer et a commencé le processus d’initialisation d’un nouvel appel VOIP avec son réseau de Communication.
2. En raison de limitée ou aucune fonctionnalité de communication réseau, cette connexion échoue.
3. L’application *doit* envoyer le **échouer** à l’Action appeler un démarrage du message (`Action.Fail()`) pour indiquer au système de l’échec.
4. Cela permet au système d’informer l’utilisateur de l’état de l’appel. Par exemple, pour afficher l’interface utilisateur de défaillance appeler.

En outre, une application VOIP iOS 10 sera doivent pouvoir répondre aux _les erreurs de délai d’expiration_ qui peut se produire lorsqu’une Action attendue ne peut pas être traitée dans un laps de temps donné. Chaque Type d’Action fournis par CallKit a une valeur de délai d’attente maximale associée. Ces valeurs de délai d’attente vous assurer que toute Action CallKit demandée par l’utilisateur est gérée de manière réactive, évitant ainsi le système d’exploitation fluides et réactives également.

Il existe plusieurs méthodes sur le délégué de fournisseur (`CXProviderDelegate`) qui doit être substituée pour gérer correctement les ainsi ce situations de délai d’attente.

### <a name="system-restrictions"></a>Restrictions du système

Selon l’état actuel de l’appareil iOS exécutant l’application VOIP iOS 10, certaines restrictions du système peuvent être appliquées.

Par exemple, un appel VOIP entrant peut être restreint par le système si :

1. La personne qui appelle est sur la liste l’utilisateur bloqué appelant.
2. APPAREIL iOS de l’utilisateur est en mode-ne pas déranger.

Si un appel VOIP est limité par une de ces situations, utilisez le code suivant à la pour gérer :

```csharp
public class ProviderDelegate : CXProviderDelegate
{
...

    public void ReportIncomingCall (NSUuid uuid, string handle)
    {
        // Create update to describe the incoming call and caller
        var update = new CXCallUpdate ();
        update.RemoteHandle = new CXHandle (CXHandleType.Generic, handle);
    
        // Report incoming call to system
        Provider.ReportNewIncomingCall (uuid, update, (error) => {
            // Was the call accepted
            if (error == null) {
                // Yes, report to call manager
                CallManager.Calls.Add (new ActiveCall (uuid, handle, false));
            } else {
                // Report error to user here
                if (error.Code == (int)CXErrorCodeIncomingCallError.CallUuidAlreadyExists) {
                    // Handle duplicate call ID
                } else if (error.Code == (int)CXErrorCodeIncomingCallError.FilteredByBlockList) {
                    // Handle call from blocked user
                } else if (error.Code == (int)CXErrorCodeIncomingCallError.FilteredByDoNotDisturb) {
                    // Handle call while in do-not-disturb mode
                } else {
                    // Handle unknown error
                }
            }
        });
    }

}
```

### <a name="voip-audio"></a>Données audio

CallKit offre plusieurs avantages pour la gestion des ressources audio qui nécessite une application VOIP iOS 10 pendant un appel VOIP en direct. Un des avantages majeurs est l’application session audio est élevés priorités lors de l’exécution dans iOS 10. Il s’agit du même niveau de priorité que le téléphone intégré et FaceTime applications et ce niveau de priorité améliorée empêche les autres applications en cours d’exécution interrompre la session audio de l’application VOIP.

En outre, CallKit a accès à d’autres indicateurs de routage audio qui peut améliorer les performances et diriger intelligemment les données audio pour les périphériques de sortie spécifique pendant un appel dynamique selon les préférences de l’utilisateur et les États de l’appareil. Par exemple, basée sur des périphériques connectés tels que les casques bluetooth, une connexion CarPlay en direct ou des paramètres d’accessibilité.

Pendant le cycle de vie d’un VOIP classique appeler à l’aide de CallKit, l’application devra configurer le Stream de Audio CallKit il fournira. Jetez un coup de œil à l’exemple suivant :

[![](callkit-images/callkit09.png "La séquence d’Action de démarrage appel")](callkit-images/callkit09.png#lightbox)

1. Une Action appeler un Démarrer est reçue par l’application pour répondre à un appel entrant.
2. Avant que cette Action est remplie par l’application, il fournit la configuration nécessitera pour son `AVAudioSession`.
3. L’application informe le système que l’Action a été exécutée.
4. Avant l’appel se connecte, CallKit fournit une haute priorité `AVAudioSession` correspondant à la configuration de l’application a demandé. L’application est avertie par le `DidActivateAudioSession` méthode de son `CXProviderDelegate`.

## <a name="working-with-call-directory-extensions"></a>Utilisation des extensions d’annuaire appel

Lorsque vous travaillez avec CallKit, _Extensions d’annuaire appeler_ fournissent un moyen d’ajouter des numéros d’appel bloqués et identifier des nombres qui sont spécifiques à une application VOIP donnée aux contacts dans l’application de Contact sur l’appareil iOS.

### <a name="implementing-a-call-directory-extension"></a>Implémentation d’une extension de répertoire d’appel

Pour implémenter une Extension de répertoire appeler dans une application Xamarin.iOS, procédez comme suit :

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

1. Ouvrez la solution de l’application dans Visual Studio pour Mac.
2. Avec le bouton droit sur le nom de la Solution dans le **l’Explorateur de solutions** et sélectionnez **ajouter** > **ajouter un nouveau projet**.
3. Sélectionnez **iOS** > **Extensions** > **Extensions d’annuaire appeler** et cliquez sur le **suivant** bouton : 

    [![](callkit-images/calldir01.png "Création d’une nouvelle Extension de répertoire appeler")](callkit-images/calldir01.png#lightbox)
4. Entrez un **nom** pour l’extension et cliquez sur le **suivant** bouton : 

    [![](callkit-images/calldir02.png "Entrez un nom pour l’extension")](callkit-images/calldir02.png#lightbox)
5. Ajuster la **nom_projet** et/ou **nom de la Solution** si nécessaire et cliquez sur le **créer** bouton : 

    [![](callkit-images/calldir03.png "Création du projet")](callkit-images/calldir03.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Ouvrez la solution de l’application dans Visual Studio.
2. Avec le bouton droit sur le nom de la Solution dans le **l’Explorateur de solutions** et sélectionnez **ajouter** > **ajouter un nouveau projet**.
3. Sélectionnez **iOS** > **Extensions** > **Extensions d’annuaire appeler** et cliquez sur le **suivant** bouton : 

    [![](callkit-images/calldir01w.png "Création d’une nouvelle Extension de répertoire appeler")](callkit-images/calldir01.png#lightbox)
4. Entrez un **nom** pour l’extension et cliquez sur le **OK** bouton

-----

Cela ajoutera un `CallDirectoryHandler.cs` classe au projet qui ressemble à ceci :

```csharp
using System;

using Foundation;
using CallKit;

namespace MonkeyCallDirExtension
{
    [Register ("CallDirectoryHandler")]
    public class CallDirectoryHandler : CXCallDirectoryProvider, ICXCallDirectoryExtensionContextDelegate
    {
        #region Constructors
        protected CallDirectoryHandler (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void BeginRequest (CXCallDirectoryExtensionContext context)
        {
            context.Delegate = this;

            if (!AddBlockingPhoneNumbers (context)) {
                Console.WriteLine ("Unable to add blocking phone numbers");
                var error = new NSError (new NSString ("CallDirectoryHandler"), 1, null);
                context.CancelRequest (error);
                return;
            }

            if (!AddIdentificationPhoneNumbers (context)) {
                Console.WriteLine ("Unable to add identification phone numbers");
                var error = new NSError (new NSString ("CallDirectoryHandler"), 2, null);
                context.CancelRequest (error);
                return;
            }

            context.CompleteRequest (null);
        }
        #endregion

        #region Private Methods
        private bool AddBlockingPhoneNumbers (CXCallDirectoryExtensionContext context)
        {
            // Retrieve phone numbers to block from data store. For optimal performance and memory usage when there are many phone numbers,
            // consider only loading a subset of numbers at a given time and using autorelease pool(s) to release objects allocated during each batch of numbers which are loaded.
            //
            // Numbers must be provided in numerically ascending order.

            long [] phoneNumbers = { 14085555555, 18005555555 };

            foreach (var phoneNumber in phoneNumbers)
                context.AddBlockingEntry (phoneNumber);

            return true;
        }

        private bool AddIdentificationPhoneNumbers (CXCallDirectoryExtensionContext context)
        {
            // Retrieve phone numbers to identify and their identification labels from data store. For optimal performance and memory usage when there are many phone numbers,
            // consider only loading a subset of numbers at a given time and using autorelease pool(s) to release objects allocated during each batch of numbers which are loaded.
            //
            // Numbers must be provided in numerically ascending order.

            long [] phoneNumbers = { 18775555555, 18885555555 };
            string [] labels = { "Telemarketer", "Local business" };

            for (var i = 0; i < phoneNumbers.Length; i++) {
                long phoneNumber = phoneNumbers [i];
                string label = labels [i];
                context.AddIdentificationEntry (phoneNumber, label);
            }

            return true;
        }
        #endregion

        #region Public Methods
        public void RequestFailed (CXCallDirectoryExtensionContext extensionContext, NSError error)
        {
            // An error occurred while adding blocking or identification entries, check the NSError for details.
            // For Call Directory error codes, see the CXErrorCodeCallDirectoryManagerError enum.
            //
            // This may be used to store the error details in a location accessible by the extension's containing app, so that the
            // app may be notified about errors which occurred while loading data even if the request to load data was initiated by
            // the user in Settings instead of via the app itself.
        }
        #endregion
    }
}
```

Le `BeginRequest` méthode dans le Gestionnaire de répertoire appeler doit être modifiée pour fournir la fonctionnalité requise. Dans le cas de l’exemple ci-dessus, il tente de définir la liste des numéros disponibles et bloquées dans la base de données de contacts de l’application VOIP. Si une demande échoue pour une raison quelconque, créer un `NSError` pour décrire l’échec et lui passer le `CancelRequest` méthode de la `CXCallDirectoryExtensionContext` classe.

Pour définir l’utilisation de nombres bloqué la `AddBlockingEntry` méthode de la `CXCallDirectoryExtensionContext` classe. Les valeurs fournies à la méthode _doit_ être par ordre numérique croissant. Pour des performances optimales et d’utilisation de la mémoire lorsqu’il existe que plusieurs numéros de téléphone, envisagez uniquement le chargement d’un sous-ensemble de nombres à un moment donné et à l’aide de pools d’autorelease pour libérer les objets alloués lors de chaque lot de nombres qui sont chargés.

Pour indiquer à l’application de Contact des numéros de contact connus à l’application VOIP, utilisez le `AddIdentificationEntry` méthode de la `CXCallDirectoryExtensionContext` classe et fournissez le nombre et une étiquette d’identification. Là encore, les valeurs fournies à la méthode _doit_ être par ordre numérique croissant. Pour des performances optimales et d’utilisation de la mémoire lorsqu’il existe que plusieurs numéros de téléphone, envisagez uniquement le chargement d’un sous-ensemble de nombres à un moment donné et à l’aide de pools d’autorelease pour libérer les objets alloués lors de chaque lot de nombres qui sont chargés.

## <a name="summary"></a>Récapitulatif

Cet article a présenté la nouvelle API CallKit par Apple publié dans iOS 10 et comment l’implémenter dans les applications Xamarin.iOS VOIP. Il vous a montré comment CallKit permet à une application à intégrer dans le système iOS, comment il assure la parité de fonctionnalité avec des applications intégrées (par exemple, Phone) et comment elle augmente la visibilité d’une application tout au long d’iOS dans des emplacements tels que le verrou et les écrans d’accueil, via les interactions de Siri et les applications de Contacts.

## <a name="related-links"></a>Liens connexes

- [Exemples iOS 10](https://developer.xamarin.com/samples/ios/iOS10/)
