---
title: Extensions d’application Advanced Message dans Xamarin.iOS
description: Cet article montre des techniques avancées pour travailler avec des Extensions d’application de Message dans une solution Xamarin.iOS qui s’intègre à l’application Messages et présente les nouvelles fonctionnalités à l’utilisateur.
ms.prod: xamarin
ms.assetid: 394A1FDA-AF70-4493-9B2C-4CFE4BE791B6
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: baceb59116dd907918b34eca4f44293051190954
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50120519"
---
# <a name="advanced-message-app-extensions-in-xamarinios"></a>Extensions d’application Advanced Message dans Xamarin.iOS

_Cet article montre des techniques avancées pour travailler avec des Extensions d’application de Message dans une solution Xamarin.iOS qui s’intègre à l’application Messages et présente les nouvelles fonctionnalités à l’utilisateur._


Nouveau à iOS 10, une Extension d’application Message intègre le **Messages** application et présente de nouvelles fonctionnalités à l’utilisateur. L’extension peut envoyer des messages interactifs, texte, autocollants et fichiers multimédias.

## <a name="about-message-app-extensions"></a>À propos des Extensions d’application Message

Comme indiqué ci-dessus, une Extension d’application Message intègre le **Messages** application et présente de nouvelles fonctionnalités à l’utilisateur. L’extension peut envoyer des messages interactifs, texte, autocollants et fichiers multimédias. Deux types d’Extension d’application de Message sont disponibles :

- **Packs d’autocollant** -contient une collection d’autocollants de l’utilisateur peut ajouter à un message. Packs d’autocollant peuvent être créés sans écrire de code.
- **iMessage application** -peut présenter une Interface utilisateur personnalisée dans l’application Messages pour la sélection autocollants, saisie de texte, y compris les fichiers de support (avec des conversions de type facultatif) et création, modification et envoi de messages de l’interaction.

Extensions des applications Message fournissent trois principaux types de contenu :

- **Des Messages interactifs** -sont un type de contenu de message personnalisé qui génère une application, lorsque l’utilisateur appuie sur le message, l’application est lancée au premier plan.
- **Autocollants** -sont des images générées par l’application qui peut être incluse dans les messages envoyés entre les utilisateurs. Consultez notre [Ice cream Générateur](https://developer.xamarin.com/samples/monotouch/ios10/IceCreamBuilder/) exemple d’application pour un exemple d’implémentation d’une application du Pack d’autocollant.
- **Autre contenu pris en charge** -l’application peut fournir le contenu tel que des photos, vidéos, texte ou des liens de type qui a toujours été pris en charge par l’application de Messages.

Nouveau à iOS 10, l’application Message inclut désormais son propre Store application dédié, intégrés. Toutes les applications qui incluent des Extensions d’applications de Message seront affiche et promues dans ce magasin. Le tiroir application de nouveaux Messages affiche toutes les applications qui ont été téléchargées à partir du Store application Messages pour fournir un accès rapide aux utilisateurs.

Également nouveau dans iOS 10, Apple a ajouté les Attribution application Inline qui permet à l’utilisateur à découvrir facilement une application. Par exemple, si un utilisateur envoie le contenu vers un autre à partir d’une application qui n’a pas l’utilisateur 2nd installé (par exemple, un autocollant par exemple), le nom de l’application émettrice est répertorié sous le contenu de l’historique des messages. Si l’utilisateur appuie sur l’application nom, le Store application Message nous est ouvert et l’application sélectionnée dans le magasin.

Extensions des applications de message sont similaires aux applications iOS existantes que le développeur est familier à la création et ils ont accès à toutes les infrastructures standards et les fonctionnalités d’une application iOS standard. Exemple :

- Ils ont accès à l’achat dans l’application.
- Ils ont accès à Apple Pay.
- Ils ont accès au matériel tels que l’appareil photo de l’appareil.

Extensions des applications de message sont uniquement pris en charge sur iOS 10, toutefois, le contenu qui envoient ces Extensions est visible sur les appareils watchOS et macOS. La nouvelle _récents Page_ ajouté à watchOS 3, affiche les autocollants récents qui ont été envoyés à partir du téléphone, y compris ceux à partir des Extensions d’applications de Message, et l’utilisateur d’envoyer ces autocollants de l’Apple watch.

## <a name="about-interactive-messages"></a>À propos des Messages interactifs

Des Messages interactifs présentent une bulle de Message personnalisé et sont fournies par une Extension d’application de Message. Ils permettent à l’utilisateur créer le contenu du Message interactif, insérez-le dans le champ d’entrée de Message et envoient.

[![](advanced-message-app-extensions-images/interactive01.png "Création de contenu de Message interactif")](advanced-message-app-extensions-images/interactive01.png#lightbox)

L’utilisateur destinataire peut répondre à un Message interactif en appuyant sur ses bulles de Message dans l’historique des messages pour charger l’Extension d’application de Message qui l’a créée. L’Extension sera être lancé plein écran et autoriser l’utilisateur à rédiger une réponse et le renvoyer à l’utilisateur d’origine.

[![](advanced-message-app-extensions-images/interactive02.png "L’Extension lancé plein écran")](advanced-message-app-extensions-images/interactive02.png#lightbox)


Les rubriques suivantes sont traitées en détail ci-dessous :

- Vue d’ensemble des API de messages
- Le cycle de vie d’Extension
- Composition d’un Message
- Envoi d’un Message

## <a name="messages-api-overview"></a>Vue d’ensemble des API de messages

Lorsqu’elle est appelée par l’utilisateur, une Extension d’application de Message s’affiche en bas de l’historique des messages dans le mode d’affichage compact :

[![](advanced-message-app-extensions-images/interactive03.png "Vue d’ensemble des API de messages")](advanced-message-app-extensions-images/interactive03.png#lightbox)

1. Le `MSMessageAppViewController` objet dans l’Extension d’application de Message est la classe principale qui est appelée lors de l’extension s’affiche à l’utilisateur.
2. La conversation est présentée à l’utilisateur comme un `MSConversation` instance d’objet.
3. Le `MSMessage` classe représente une bulle de Message donné dans la conversation.
4. `MSSession` contrôle la façon dont un message est envoyé.
5. `MSMessageTemplateLayout` contrôle comment le message s’affiche

## <a name="the-extension-lifecycle"></a>Le cycle de vie d’Extension

Examinons le processus d’une Extension d’application Message devient active :

[![](advanced-message-app-extensions-images/interactive04.png "Le processus d’une Extension d’application Message devient active")](advanced-message-app-extensions-images/interactive04.png#lightbox)

1. Lorsqu’une extension est lancée (par exemple dans le tiroir application), l’application Message lance un processus.
2. Le `DidBecomeActive` méthode est appelée et reçoit un `MSConversation` qui représente la conversation à laquelle l’Extension d’application de Message est en cours d’exécution dans.
3. Étant donné que l’extension est basée issu d’un `UIViewController` à la fois `ViewWillAppear` et `ViewDidAppear` sont appelées.

Ensuite, examinons le processus d’une Extension d’application Message devient désactivé :

[![](advanced-message-app-extensions-images/interactive05.png "Le processus d’une Extension d’application Message devient désactivé")](advanced-message-app-extensions-images/interactive05.png#lightbox)

1. Lors de l’Extension d’application de messages est désactivée, le `ViewWillDisappear` méthode est appelée en premier.
2. Le `ViewDidDisappear` méthode sera appelée.
3. Le `WillResignActive` méthode est appelée et reçoit un `MSConversation` qui représente la conversation à laquelle l’Extension d’application de Message est en cours d’exécution dans. À ce stade, la connexion entre l’application Messages et l’extension sont sur le point d’être publié.
4. Ultérieurement, le processus se termine par l’application Messages.

Dans la mesure où une extension est un processus de courte durée de vie, elle est arrêtée efficacement par le système pour économiser l’énergie de traitement et de la batterie. Le développeur doit Gardez cela à l’esprit lorsque vous concevez et implémentez une Extension d’application de Message.

## <a name="composing-a-message"></a>Composition d’un Message

Une fois que l’Extension d’application de Message est en cours d’exécution dans un processus et a affiché son Interface utilisateur, le code suivant peut être utilisé pour composer un nouveau message :

```csharp
MSMessage ComposeMessage (IceCream iceCream, string caption, MSSession session = null)
{
    var components = new NSUrlComponents {
        QueryItems = iceCream.QueryItems
    };

    var layout = new MSMessageTemplateLayout {
        Image = iceCream.RenderSticker (true),
        Caption = caption
    };

    var message = new MSMessage (session ?? new MSSession()) {
        Url = components.Url,
        Layout = layout
    };

    return message;
}
```

Ce code crée un nouveau `MSMessage` et définit plusieurs propriétés (telles que `Url`). Tandis que le message peut uniquement être créé sur iOS, il peut être envoyé pour iOS et macOS à afficher.

Si l’utilisateur clique sur la bulle de Message dans la conversation sur macOS, les Mac essaiera d’ouvrir l’adresse spécifiée dans l’URL dans le navigateur web. Par conséquent, le site Web des développeurs doit être en mesure d’afficher qu'une représentation du message dans le navigateur web sur macOS en fonction des ordinateurs.

Le `AccessibilityLabel` propriété est utilisée par les lecteurs d’écran pour lire la transcription de la conversation à l’utilisateur. Le `Layout` propriété spécifie la façon dont le message s’affichera, actuellement uniquement la `MSMessageTemplateLayout` est pris en charge et ressemble à ceci :

[![](advanced-message-app-extensions-images/interactive06.png "Le modèle MSMessageTemplateLayout")](advanced-message-app-extensions-images/interactive06.png#lightbox)

Le `Image` propriété de la `MSMessageTemplateLayout` fournit du contenu pour le corps principal de la MessageBubble sur l’écran. Le `MediaFileUrl` propriété également fournit du contenu pour le corps de la bulle de Message, mais autorise pour le contenu qui n’est pas pris en charge par `UIImage` (par exemple, un fichier vidéo qui effectuerait une boucle en arrière-plan). Si les deux le `Image` et `MediaFileUrl` propriétés sont fournies, le `Image` propriété est prioritaire. Le `MediaFileUrl` prend en charge PNG, JPEG, GIF et vidéo (dans n’importe quel format qui peut être lu par l’infrastructure Media Player) formats multimédias.

La taille du média recommandée est pixels de 300 x 300 à la résolution de 3 x. Ressources légèrement supérieure et inférieures sont également acceptés et Apple suggère quelques tailles différentes pour obtenir les meilleurs résultats de test. L’application Message sera sous-échantillonner et mettre à l’échelle de ce média en fonction des besoins.

Lorsque les ressources sont envoyés au destinataire, tous les médias attachés sera automatiquement transcodés par l’application Messages pour les optimiser à partir de transfert sur les réseaux. Pour cette raison, Apple vous découragez toute notamment le texte dans le média qui est joint au message, car le support sera mis à l’échelle vers le bas et compressé pour la transmission, potentiellement rendant ainsi le texte illisible.

Le `ImageTitle` et `ImageSubtitle` propriétés fournissent une description pour le média est présenté dans la bulle de Message. Ces propriétés seront envoyées comme texte à l’appareil de réception où ils seront restitués on ne retrouvait pas dans le coin inférieur gauche de l’image.

Le `Caption`, `SubCaption`, `TrailingCaption` et `TrailingSubcaption` propriétés décrivent l’image supplémentaire et s’affichera dans la section sous l’image. Le paramétrage de ces propriétés pour `null` créera une bulle de Message sans la zone de légende :

[![](advanced-message-app-extensions-images/interactive07.png "Une bulle de Message sans la zone de légende")](advanced-message-app-extensions-images/interactive07.png#lightbox)

La dernière chose à noter est que l’application Messages dessinera icône de l’Extension application de Message dans le coin supérieur gauche de la bulle de Message.

## <a name="sending-a-message"></a>Envoi d’un Message

Une fois un `MSMessage` a été composé, le code suivant peut être utilisé pour l’envoyer :

```csharp
public void SendMessage (MSMessage message)
{
    // Insert the message into the conversation
    ActiveConversation.InsertMessage (message, (error) => {
        // Did the message send successfully?
        if (error == null) {
            // Handle successful send
        } else {
            // Report Error
            Console.WriteLine ("Error: {0}", error);
        }
    };

}
```

Le `ActiveConversation` propriété de la `MSMessagesAppViewController` contiendra la conversation actuelle l’Extension d’application de Message a été lancée dans.

Appelez le `InsertMessage` de la `MSConversation` pour inclure le message dans la conversation et de gérer les erreurs qui peuvent se produire. Si le message est inclus avec succès, la bulle de Message s’affichera dans le champ d’entrée.

En outre, l’extension peut envoyer différents types de données à la conversation comme :

- **Texte** - `ActiveConversation.InsertText ("Message", (error) => {...});`
- **Pièces jointes** - `ActiveConversation.InsertAttachment (new NSUrl ("path"), "filename", (error) => {...});`
- **Autocollants**  -  `ActiveConversation.InsertSticker (sticker, (obj) => {...});` où `sticker` est un `MSSticker`.

Une fois le nouveau contenu sur le champ d’entrée, l’utilisateur est en mesure d’envoyer le message en appuyant sur le bleu **envoyer** bouton (comme ils le feraient n’importe quel message classique). Il n’existe aucun moyen pour l’Extension d’application de Message envoyer automatiquement le contenu, ce processus est totalement sous le contrôle de l’utilisateur.

## <a name="handling-the-compact-and-expanded-modes"></a>Gérer les Modes Compact et développées

Une Extension d’application de Message peut être affichée dans un des deux modes d’affichage différents :

[![](advanced-message-app-extensions-images/interactive08.png "Une Extension d’application Message affiché dans les deux modes d’affichage différents : Compact & développé")](advanced-message-app-extensions-images/interactive08.png#lightbox)

- **Compact** -il s’agit du mode par défaut où l’Extension d’application Message prend jusqu'à 25 % en bas de la vue de Message. En mode Compact, l’application n’a pas accès au clavier, le défilement horizontal ou modules de reconnaissance de mouvement de balayage. L’application a accès au champ d’entrée et appelle à `InsertMessage` s’affichera instantanément à l’utilisateur à cet endroit.
- **Développé** -l’Extension d’application Message remplit toute la vue de Message. Il n’a pas accès au champ d’entrée, mais a accès au clavier, le défilement horizontal et modules de reconnaissance de mouvement de balayage.

Une Extension d’application de Message est possible entre ces modes par programme ou manuellement par l’utilisateur à tout moment et doit être instantanément réactif à un mode est modifié dans la vue.

Examinons l’exemple suivant de gérer le basculement entre les deux modes d’affichage différent. Deux contrôleurs d’affichage différentes seront nécessaires pour chaque état. Le `StickerBrowserViewController` gère la **Compact** vue et le `AddStickerViewController` gérera le **développé** vue :

```csharp
using System;

using Messages;
using Foundation;
using UIKit;

namespace MessagesExtension {
    public partial class MessagesViewController : MSMessagesAppViewController, IIceCreamsViewControllerDelegate, IBuildIceCreamViewControllerDelegate {
        public MessagesViewController (IntPtr handle) : base (handle)
        {
        }

        public override void WillBecomeActive (MSConversation conversation)
        {
            base.WillBecomeActive (conversation);

            // Present the view controller appropriate for the conversation and presentation style.
            PresentViewController (conversation, PresentationStyle);
        }

        public override void WillTransition (MSMessagesAppPresentationStyle presentationStyle)
        {
            var conversation = ActiveConversation;
            if (conversation == null)
                throw new Exception ("Expected an active converstation");

            // Present the view controller appropriate for the conversation and presentation style.
            PresentViewController (conversation, presentationStyle);
        }

        void PresentViewController (MSConversation conversation, MSMessagesAppPresentationStyle presentationStyle)
        {
            // Determine the controller to present.
            UIViewController controller;

            if (presentationStyle == MSMessagesAppPresentationStyle.Compact) {
                // Show a list of previously created ice creams.
                controller = InstantiateIceCreamsController ();
            } else {
                var iceCream = new IceCream (conversation.SelectedMessage);
                controller = iceCream.IsComplete ? InstantiateCompletedIceCreamController (iceCream) : InstantiateBuildIceCreamController (iceCream);
            }

            foreach (var child in ChildViewControllers) {
                child.WillMoveToParentViewController (null);
                child.View.RemoveFromSuperview ();
                child.RemoveFromParentViewController ();
            }

            AddChildViewController (controller);
            controller.View.Frame = View.Bounds;
            controller.View.TranslatesAutoresizingMaskIntoConstraints = false;
            View.AddSubview (controller.View);

            controller.View.LeftAnchor.ConstraintEqualTo (View.LeftAnchor).Active = true;
            controller.View.RightAnchor.ConstraintEqualTo (View.RightAnchor).Active = true;
            controller.View.TopAnchor.ConstraintEqualTo (View.TopAnchor).Active = true;
            controller.View.BottomAnchor.ConstraintEqualTo (View.BottomAnchor).Active = true;

            controller.DidMoveToParentViewController (this);
        }

        UIViewController InstantiateIceCreamsController ()
        {
            // Instantiate a `IceCreamsViewController` and present it.
            var controller = Storyboard.InstantiateViewController (IceCreamsViewController.StoryboardIdentifier) as IceCreamsViewController;
            if (controller == null)
                throw new Exception ("Unable to instantiate an IceCreamsViewController from the storyboard");

            controller.Builder = this;
            return controller;
        }

        UIViewController InstantiateBuildIceCreamController (IceCream iceCream)
        {
            // Instantiate a `BuildIceCreamViewController` and present it.
            var controller = Storyboard.InstantiateViewController (BuildIceCreamViewController.StoryboardIdentifier) as BuildIceCreamViewController;
            if (controller == null)
                throw new Exception ("Unable to instantiate an BuildIceCreamViewController from the storyboard");

            controller.IceCream = iceCream;
            controller.Builder = this;

            return controller;
        }

        public UIViewController InstantiateCompletedIceCreamController (IceCream iceCream)
        {
            // Instantiate a `BuildIceCreamViewController` and present it.
            var controller = Storyboard.InstantiateViewController (CompletedIceCreamViewController.StoryboardIdentifier) as CompletedIceCreamViewController;
            if (controller == null)
                throw new Exception ("Unable to instantiate an CompletedIceCreamViewController from the storyboard");

            controller.IceCream = iceCream;
            return controller;
        }

        public void DidSelectAdd (IceCreamsViewController controller)
        {
            Request (MSMessagesAppPresentationStyle.Expanded);
        }

        public void Build (BuildIceCreamViewController controller, IceCreamPart iceCreamPart)
        {
            var conversation = ActiveConversation;
            if (conversation == null)
                throw new Exception ("Expected a conversation");

            var iceCream = controller.IceCream;
            if (iceCream == null)
                throw new Exception ("Expected the controller to be displaying an ice cream");

            string messageCaption = string.Empty;
            var b = iceCreamPart as Base;
            var s = iceCreamPart as Scoops;
            var t = iceCreamPart as Topping;

            if (b != null) {
                iceCream.Base = b;
                messageCaption = "Let's build an ice cream";
            } else if (s != null) {
                iceCream.Scoops = s;
                messageCaption = "I added some scoops";
            } else if (t != null) {
                iceCream.Topping = t;
                messageCaption = "Our finished ice cream";
            } else {
                throw new Exception ("Unexpected type of ice cream part selected.");
            }

            // Create a new message with the same session as any currently selected message.
            var message = ComposeMessage (iceCream, messageCaption, conversation.SelectedMessage?.Session);

            // Add the message to the conversation.
            conversation.InsertMessage (message, null);

            // If the ice cream is complete, save it in the history.
            if (iceCream.IsComplete) {
                var history = IceCreamHistory.Load ();
                history.Append (iceCream);
                history.Save ();
            }

            Dismiss ();
        }

        MSMessage ComposeMessage (IceCream iceCream, string caption, MSSession session = null)
        {
            var components = new NSUrlComponents {
                QueryItems = iceCream.QueryItems
            };

            var layout = new MSMessageTemplateLayout {
                Image = iceCream.RenderSticker (true),
                Caption = caption
            };

            var message = new MSMessage (session ?? new MSSession()) {
                Url = components.Url,
                Layout = layout
            };

            return message;
        }
    }
}
```

Le `DidTransition` méthode est substituée pour gérer le changement entre les deux modes :

```csharp
public override void DidTransition (MSMessagesAppPresentationStyle presentationStyle)
{
    base.DidTransition (presentationStyle);

    // Take action based on style
    switch (presentationStyle) {
    case MSMessagesAppPresentationStyle.Compact:
        PresentStickerBrowser ();
        break;
    case MSMessagesAppPresentationStyle.Expanded:
        PresentAddSticker ();
        break;
    }
}
```

Si vous le souhaitez, l’application aurait pu utiliser le `WillTransition` méthode pour gérer le changement de mode d’affichage avant qu’il est présenté à l’utilisateur (comme dans l’exemple de générateur de rapports Icecream ci-dessus). Pour plus d’informations, consultez notre [une personnalisation plus poussée autocollant](~/ios/platform/message-app-integration/intro-to-message-app-extensions.md) documentation.

## <a name="replying-to-a-message"></a>Réponse à un Message

Il existe deux cas une Extension d’application de Message doit gérer lors de la réponse à un message :

[![](advanced-message-app-extensions-images/interactive09.png "L’Extension d’application de messages dans les modes d’inactif à actif")](advanced-message-app-extensions-images/interactive09.png#lightbox)

- **L’extension est inactif** -il est une des bulles de Message de l’Extension d’application Message dans la transcription de Message que l’utilisateur peut appuyer pour activer les extensions et poursuivre la conversation interactive.
- **Extension est Active** -l’utilisateur peut appuyer la bulle de Message de l’Extension application de messages dans la transcription de Message à entrer le mode d’affichage développé et à continuer le processus interactif à partir de l’endroit où il s’était arrêté.

### <a name="the-extension-is-inactive"></a>L’Extension est inactif

Lors de l’appui sur une bulle de Message par l’utilisateur dans la transcription de Message et l’Extension d’application de Message est inactive, le processus suivant se produit :

[![](advanced-message-app-extensions-images/interactive10.png "Gestion d’une bulle de Message inactif")](advanced-message-app-extensions-images/interactive10.png#lightbox)

1. L’utilisateur appuie sur la bulle de Message de l’extension.
2. Lorsqu’une extension est lancée, l’application Message lance un processus.
3. Le `DidBecomeActive` méthode est appelée et reçoit un `MSConversation` qui représente la conversation à laquelle l’Extension d’application de Message est en cours d’exécution dans.
4. Étant donné que l’extension est basée issu d’un `UIViewController` à la fois `ViewWillAppear` et `ViewDidAppear` sont appelées.

Lorsque le processus est terminé, l’Extension d’application de Message s’affiche dans le mode d’affichage développé.

### <a name="the-extension-is-active"></a>L’Extension est Active

Lors de l’appui sur une bulle de Message par l’utilisateur dans la transcription de Message et l’Extension d’application de Message est active, le processus suivant se produit :

[![](advanced-message-app-extensions-images/interactive11.png "Gestion d’une bulle de Message active")](advanced-message-app-extensions-images/interactive11.png#lightbox)

1. L’utilisateur appuie sur la bulle de Message de l’extension.
2. Étant donné que l’Extension d’application de Message est déjà active, le `WillTransition` méthode de la `MSMessagesAppViewController` est appelé pour gérer le basculement à partir du CD-ROM vers le mode d’affichage développé.
3. Le `DidSelectMessage` méthode de la `MSMessagesAppViewController` est appelée et reçoit le `MSMessage` et `MSConversation` auquel appartient la bulle de Message.
4. Le `DidTransition` méthode de la `MSMessagesAppViewController` est appelé pour gérer le basculement à partir du CD-ROM vers le mode d’affichage développé.

Là encore, lorsque le processus est terminé, l’Extension d’application de Message s’affiche dans le mode d’affichage développé.

### <a name="accessing-the-selected-message"></a>Accédez au Message sélectionné

Dans les deux cas, lorsque l’utilisateur appuie sur une bulle de Message appartenant à l’Extension d’application de Message, elle devra accéder à la `MSMessage` que l'on a cliqué sur à l’aide de la `SelectedMessage` propriété de la `MSConversation`.

Exemple :

```csharp
using System;
using Foundation;
using Messages;
using UIKit;


namespace MessageExtension
{
    public partial class MessagesViewController : MSMessagesAppViewController
    {
        ...

        #region Override Methods
        public override void DidSelectMessage (MSMessage message, MSConversation conversation)
        {
            base.DidSelectMessage (message, conversation);

            // Get selected message
            var selected = conversation.SelectedMessage;

            // Present the user interface to continue editing
            // the conversation
            ...
        }
        #endregion
    }
}
```

Le message sélectionné doit être affiché dans l’interface utilisateur de l’Extension application de Message et l’utilisateur doit être autorisé à composer une réponse.

## <a name="removing-partially-completed-messages"></a>Suppression partiellement terminée des Messages

Lors de l’envoi les différentes étapes d’une conversation interactive entre deux l’utilisateur dans la conversation, le Message remonte ensuite partiellement exécuté peut commencer à encombrer la transcription de Message :

[![](advanced-message-app-extensions-images/interactive12.png "Le Message remonte ensuite partiellement exécuté peuvent encombrer la transcription de Message")](advanced-message-app-extensions-images/interactive12.png#lightbox)

Au lieu de cela, l’Extension d’application de Message doit réduire le précédent Message remonte ensuite dans un commentaire concis dans la transcription de Message :

[![](advanced-message-app-extensions-images/interactive13.png "Réduction des bulles de Message précédent dans la transcription de Message")](advanced-message-app-extensions-images/interactive13.png#lightbox)

Ceci est géré à l’aide un `MSSession` pour réduire toutes les étapes existantes. Par conséquent, le `DidSelectMessage` méthode de la `MSMessagesAppViewController` classe peut être modifiée pour ressembler à ceci :

```csharp
public override void DidSelectMessage (MSMessage message, MSConversation conversation)
{
    base.DidSelectMessage (message, conversation);

    MSSession session;
    var summary = "";

    // Get selected message
    var selected = conversation.SelectedMessage;

    // Does the selected message already have a session?
    if (selected.Session == null) {
        // No, create a new session
        session = new MSSession ();
        summary = "Let's build an ice cream";
    } else {
        // Yes, use the existing session
        session = selected.Session;
        summary = "I added some scoops";
    }

    // Create an instance of the message being composed
    var existingMessage = new MSMessage (session);
    message.SummaryText = summary;
    ...

    // Present the user interface to continue editing
    // the conversation
    ...
}
```

Si le message sélectionné a déjà une sortie `MSSession`, il est utilisé ; sinon un nouveau `MSSession` est créé. Le `SummaryText` propriété de la `MSMessage` est utilisée pour ajouter une légende pour les étapes précédentes réduits. Si le `SummaryText` propriété est définie sur `null`, les étapes précédentes de la conversation va être complètement supprimés de la transcription de Conversation.

## <a name="advanced-message-api-features"></a>Message API fonctionnalités avancées

Avec les fonctionnalités de base de la nouvelle API de Message détaillées ci-dessus, ensuite d’examiner certaines des fonctionnalités plus avancées qui Apple a intégré à l’infrastructure.

Tout d’abord, il existe plusieurs autres méthodes de remplacement dans la `MSMessagesAppViewController` classe qui fournissent un accès plus approfondi à la conversation :

- `DidStartSendingMessage` -Il est appelé lorsque l’utilisateur actionne le bouton Envoyer. Cela ne signifie pas que le message a bien été remise au destinataire, simplement que le processus d’envoi a été démarré.
- `DidCancelSendingMessage` -Cela se produit lorsque l’utilisateur actionne le *X* situé dans l’angle supérieur droit de la bulle de Message dans la transcription de Conversation.
- `DidReceiveMessage` : Cette méthode est appelée lorsque l’Extension d’application de Message est active un un nouveau message a été reçu à partir d’un des participants à la conversation.

### <a name="group-conversations"></a>Conversations de groupe

Une Extension d’application de Message utilisable pendant que les utilisateurs sont impliqués dans des conversations de groupe (avec des personnes de 3 ou plus) et cela aura prendre en considération lorsque vous concevez et implémentez une Extension d’application de Message.

Examinons l’interaction suivante dans une conversation de groupe avec trois utilisateurs :

[![](advanced-message-app-extensions-images/interactive14.png "Interaction dans une conversation de groupe avec trois utilisateurs")](advanced-message-app-extensions-images/interactive14.png#lightbox)

1. L’utilisateur 1 envoie un Message interactif groupe demandant l’utilisateur 2 et 3 de l’utilisateur à choisir un remplissage de votre hamburger.
2. L’utilisateur 2 choisit tomates.
3. Utilisateur 3 choisit les condiments.
4. Choix du utilisateur 2 et 3 utilisateur arrive à utilisateur 1 presque simultanément. Par conséquent, les choix de l’utilisateur 2 est réduite en une ligne de résumé et n’est pas disponible. Ce cas pourrait ont également été retourné, avec choix de l’utilisateur 2 affiché et l’utilisateur 3 du réduit.

Dans les deux cas, ce comportement est indésirable comme utilisateur 1 doit être en mesure d’accéder aux choix de 2 de l’utilisateur et de 3 de l’utilisateur. Pour gérer cette situation, Apple suggère que l’Extension d’application de Message stocke l’état du message dans le cloud et utilisez la propriété URL de la `MSMessage` (qui est envoyée entre les utilisateurs) pour accéder à cet état.

Lorsque l’utilisateur envoie un message, un jeton de session est généré et envoyé vers le cloud avec l’état du message en cours. Lorsqu’un utilisateur appuie sur une bulle de Message dans la transcription de Conversation, le jeton de session est utilisé pour récupérer l’état de session actuel à partir du cloud.

### <a name="sender-identifiers"></a>Identificateurs de l’émetteur

Pour discuter de l’accès à l’identificateur de l’expéditeur d’un message, prenons l’exemple d’une conversation de groupe ci-dessus :

[![](advanced-message-app-extensions-images/interactive15.png "Envoi d’identificateurs de conversation de groupe")](advanced-message-app-extensions-images/interactive15.png#lightbox)

1. Là encore, l’utilisateur 1 envoie un Message interactif de groupe demandant l’utilisateur 2 et 3 de l’utilisateur à choisir un remplissage de votre hamburger.
2. Utilisateur 3 choisit les condiments.
3. Choix de l’utilisateur de 3 arrive à utilisateur 1 et 2 de l’utilisateur n’a pas encore répondu.
4. Étant donné que Apple est très préoccupée par la confidentialité des utilisateurs, l’Extension d’application Message connaît uniquement un identificateur Unique (comme un `NSUUID`) qui est assigné à chaque participant de la conversation. Sur l’appareil local, l’identificateur de l’utilisateur actuel uniquement est connu.
5. Le `MSMessage` a un `SenderIdentifier` propriété qui correspond à celui de l’utilisateur dans la liste des participants connue à l’extension.
6. Chaque appareil de l’utilisateur a sa propre copie de la liste des participants où là encore, l’identificateur de l’utilisateur local uniquement est connu.
7. Lorsqu’un message est envoyé, son `SenderIdentifier` propriété est également connue pour être celui de l’utilisateur local.

Les identificateurs de l’expéditeur peut être utilisés comme suit :

- En examinant la liste des participants, l’extension peut obtenir le nombre d’utilisateurs dans la conversation.
- Lorsque l’extension reçoit un message à partir d’un utilisateur, il peut conserver une trace de l’identificateur d’expéditeur. S’il reçoit un autre message avec le même identificateur de l’expéditeur, l’extension sait qu’il est du même utilisateur.
- Elles peuvent servir pour aider à identifier un utilisateur spécifique dans la conversation.

L’identificateur d’expéditeur peut être utilisé dans tous les champs de texte de la `MSMessageTemplateLayout` en faisant précéder par un signe dollar (`$`). Exemple :

```csharp
// Pass along the sender identifier
var layout = new MSMessageTemplateLayout()
layout.Caption = string.format("${0} wants pickles.",Conversation.LocalParticipantIdentifier.UuidString);
```

Lorsque l’application Messages affiche une bulle de Message avec ce type de mise en forme, il remplacera le `$uuid...` avec le nom de contact du participant à la conversation qui a envoyé le message.

L’identificateur d’expéditeur étant unique sur chaque appareil, examinez le diagramme ci-dessus, notez de que 1 utilisateur et périphérique de l’utilisateur 3 a un autre identificateur unique de l’expéditeur pour chaque participant à la conversation.

Les identificateurs de l’expéditeur sont limitées à l’installation de l’Extension d’application de Message. Par conséquent, si un utilisateur désinstalle et réinstalle l’Extension d’application Message nouveaux identificateurs expéditeur sera générés pour la nouvelle installation.

Pour accéder aux identificateurs de l’expéditeur, l’extension peut utiliser le code suivant :

```csharp
public override void DidStartSendingMessage (MSMessage message, MSConversation conversation)
{
    base.DidStartSendingMessage (message, conversation);

    // Get the sender's participant ID
    var senderID = message.SenderParticipantIdentifier;

    // Get the local participant ID
    var localID = conversation.LocalParticipantIdentifier;

    // Get the remote participant IDs
    var remoteIDs = conversation.RemoteParticipantIdentifiers;
}
```

## <a name="supported-platforms"></a>Plateformes prises en charge

Les Messages interactifs générés par une Extension d’application Message sera livrés sur les plateformes Apple suivantes :

- watchOS 3
- macOS Sierra
- iOS 10

Les trois plateformes, uniquement à iOS 10 permet à l’utilisateur Générer un message interactif. Sur macOS Sierra, si l’utilisateur clique sur une bulle de Message interactif, l’URL associée à la `MSMessage` sera ouvert dans Safari et une représentation du message doit s’afficher ici.

Dans watchOS, l’application Messages pouvez remise d’un message interactif sur un appareil iOS attaché où l’utilisateur peut créer une réponse.

La nouvelle API de Messages a prise en charge de secours si le message interactif est reçu sur les anciennes plateformes Apple :

- watchOS 2 +
- OS X 10.11 +
- iOS 9 +

Ils seront livrés dans un format de secours comme deux messages distincts :

- Une sera l’image fournie par la mise en page du modèle.
- L’autre sera l’URL comme indiqué dans le `MSMessage`.

## <a name="summary"></a>Récapitulatif

Cet article a présenté les techniques avancées pour travailler avec des Extensions d’application de Message dans une solution Xamarin.iOS qui s’intègre à la **Messages** application et les fonctionnalités de nouveau présente à l’utilisateur.


## <a name="related-links"></a>Liens associés

- [Générateur de glace (exemple)](https://developer.xamarin.com/samples/monotouch/ios10/IceCreamBuilder/)
- [Référence des messages](https://developer.apple.com/reference/messages)
- [Guide de programmation d’Extension d’application](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214)
