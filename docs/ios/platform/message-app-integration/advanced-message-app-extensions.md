---
title: "Extensions d’application Message avancés"
description: "Cet article montre des techniques avancées pour travailler avec les Extensions d’application de Message dans une solution Xamarin.iOS qui s’intègre à l’application des Messages et présente les nouvelles fonctionnalités à l’utilisateur."
ms.topic: article
ms.prod: xamarin
ms.assetid: 394A1FDA-AF70-4493-9B2C-4CFE4BE791B6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: fcfd1fd2ec9271bb5e8d9e09b43b7dc4cf3b3f12
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="advanced-message-app-extensions"></a>Extensions d’application Message avancés

_Cet article montre des techniques avancées pour travailler avec les Extensions d’application de Message dans une solution Xamarin.iOS qui s’intègre à l’application des Messages et présente les nouvelles fonctionnalités à l’utilisateur._


Nouveau pour iOS 10, une Extension d’application Message intègre le **Messages** application et affiche le nombre de nouvelles fonctionnalités à l’utilisateur. L’extension peut envoyer des messages interactifs, texte, autocollants et fichiers multimédias.

## <a name="about-message-app-extensions"></a>Sur les Extensions d’application de Message

Comme indiqué ci-dessus, une Extension d’application Message intègre le **Messages** application et affiche le nombre de nouvelles fonctionnalités à l’utilisateur. L’extension peut envoyer des messages interactifs, texte, autocollants et fichiers multimédias. Deux types d’Extension d’application de Message sont disponibles :

- **Les packs d’autocollant** -contient une collection d’autocollants l’utilisateur peut ajouter à un message. Packs d’étiquette peuvent être créés sans écrire de code.
- **iMessage application** -peut présenter une Interface utilisateur personnalisée au sein de l’application de Messages pour la sélection des autocollants, saisie de texte, y compris les fichiers de support (avec des conversions de type facultatif) et création, modification et envoyer des messages de l’interaction.

Extensions des applications Message fournissent trois principaux types de contenu :

- **Messages interactives** -sont un type de contenu de message personnalisé qui génère d’une application, lorsque l’utilisateur appuie sur le message, l’application est lancée au premier plan.
- **Autocollants** -sont des images générées par l’application qui peut être incluse dans les messages envoyés entre les utilisateurs. Consultez notre [le Générateur de glace](https://developer.xamarin.com/samples/monotouch/ios10/IceCreamBuilder/) exemple d’application pour un exemple d’implémentation d’une application de l’autocollant du Pack.
- **Tout autre contenu pris en charge** -l’application peut fournir le contenu comme des photos, vidéos, texte ou des liens du type qui a toujours été pris en charge par l’application des Messages.

Nouveau pour iOS 10, l’application Message inclut désormais son propre magasin d’applications dédié, intégrés. Toutes les applications qui incluent les Extensions d’applications de Message seront affiche et promues dans ce magasin. Le tiroir application de nouveaux Messages affiche toutes les applications qui ont été téléchargées à partir du Store de Messages pour fournir un accès rapide aux utilisateurs.

Également nouveau dans iOS 10, Apple a ajouté les Attribution application Inline qui lui permet de découvrir facilement une application. Par exemple, si un utilisateur envoie le contenu vers un autre à partir d’une application que l’utilisateur 2 n’est installé (par exemple, une vignette par exemple), le nom de l’application émettrice est répertorié sous le contenu de l’historique des messages. Si l’utilisateur appuie sur l’application un nom, le Message App Store nous série ouvert et l’application sélectionnée dans le magasin.

Extensions des applications de message sont semblables à des applications iOS existantes que le développeur est familier à la création et ils ont accès à toutes les infrastructures standards et les fonctionnalités d’une application iOS standard. Exemple :

- Ils ont accès dans l’application fournisseur.
- Ils ont accès à payer par Apple.
- Ils ont accès au matériel de périphérique tels que l’appareil photo.

Extensions des applications de message sont uniquement pris en charge sur iOS 10, mais le contenu de l’envoi de ces Extensions est visible sur les appareils watchOS et macOS. La nouvelle _récents Page_ ajouté à watchOS 3, affichera autocollants récents qui ont été envoyés à partir du téléphone, y compris celles des Extensions d’applications de Message, et l’utilisateur d’envoyer ces vignettes à partir de la surveillance.

## <a name="about-interactive-messages"></a>À propos des Messages interactifs

Messages interactifs présentent une bulle de Message personnalisé et sont fournies par une Extension d’application de Message. Ils permettent à l’utilisateur créer le contenu du Message interactif, insérez-le dans le champ d’entrée de Message et l’envoient.

[![](advanced-message-app-extensions-images/interactive01.png "Création de contenu de Message interactif")](advanced-message-app-extensions-images/interactive01.png#lightbox)

L’utilisateur destinataire peut répondre à un Message interactif en appuyant sur ses bulles de Message de l’historique des messages pour charger l’Extension d’application de Message qui l’a créée. L’Extension sera être lancé plein écran et autoriser l’utilisateur à composer une réponse et l’envoyer à l’utilisateur d’origine.

[![](advanced-message-app-extensions-images/interactive02.png "L’Extension lancé plein écran")](advanced-message-app-extensions-images/interactive02.png#lightbox)


Les rubriques suivantes sont traitées en détail ci-dessous :

- Vue d’ensemble des API de messages
- Le cycle de vie d’Extension
- Composer un Message
- Envoi d’un Message

## <a name="messages-api-overview"></a>Vue d’ensemble des API de messages

Lorsqu’elle est appelée par l’utilisateur, une Extension d’application Message s’affichera au bas de l’historique du Message dans le mode d’affichage compact :

[![](advanced-message-app-extensions-images/interactive03.png "Vue d’ensemble des API de messages")](advanced-message-app-extensions-images/interactive03.png#lightbox)

1. Le `MSMessageAppViewController` objet dans l’Extension d’application de Message est la classe principale qui est appelée lors de l’affichage de l’extension à l’utilisateur.
2. La conversation est présentée à l’utilisateur comme un `MSConversation` instance d’objet.
3. La `MSMessage` classe représente une bulle de Message donné dans la conversation.
4. `MSSession` contrôle la façon dont un message est envoyé.
5. `MSMessageTemplateLayout` contrôle la façon dont le message s’affiche

## <a name="the-extension-lifecycle"></a>Le cycle de vie d’Extension

Examinez le processus d’une Extension d’application Message devient active :

[![](advanced-message-app-extensions-images/interactive04.png "Le processus d’une Extension d’application Message devient active")](advanced-message-app-extensions-images/interactive04.png#lightbox)

1. Lorsqu’une extension est lancée (par exemple sur le contenu de l’application), l’application Message lance un processus.
2. Le `DidBecomeActive` méthode est appelée et reçoit un `MSConversation` qui représente la conversation dont l’Extension d’application de Message est en cours d’exécution dans.
3. Étant donné que l’extension est basée dérivé d’un `UIViewController` les deux `ViewWillAppear` et `ViewDidAppear` sont appelées.

Ensuite, examinez le processus d’une Extension d’application Message devient désactivé :

[![](advanced-message-app-extensions-images/interactive05.png "Le processus d’une Extension d’application Message devient désactivé")](advanced-message-app-extensions-images/interactive05.png#lightbox)

1. Lors de l’Extension d’application de Message est en cours de désactivation, le `ViewWillDisappear` méthode est appelée en premier.
2. Le `ViewDidDisappear` méthode sera appelée.
3. Le `WillResignActive` méthode est appelée et reçoit un `MSConversation` qui représente la conversation dont l’Extension d’application de Message est en cours d’exécution dans. À ce stade, la connexion entre l’application des Messages et l’extension sont sur le point d’être publié.
4. À un stade ultérieur, le processus se termine par l’application des Messages.

Dans la mesure où une extension est un processus de courte durée, il est arrêté efficacement par le système pour économiser de l’énergie de traitement et la batterie. Le développeur doit Gardez cela à l’esprit lorsque vous concevez et implémentez une Extension d’application de Message.

## <a name="composing-a-message"></a>Composer un Message

Une fois l’Extension d’application de Message est en cours d’exécution dans un processus et a affiché son Interface utilisateur, le code suivant peut être utilisé pour composer un nouveau message :

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

Ce code crée un nouveau `MSMessage` et définir plusieurs propriétés (telles que `Url`). Pendant que le message peut uniquement être créé sur iOS, il peut être envoyé sur iOS et macOS à afficher.

Si l’utilisateur clique sur la bulle de Message dans la conversation sur macOS, les Mac essaiera d’ouvrir l’adresse spécifiée dans l’URL dans le navigateur web. Par conséquent, le site Web des développeurs doit être en mesure d’afficher qu'une représentation du message dans le navigateur web de macOS en fonction des ordinateurs.

Le `AccessibilityLabel` propriété est utilisée par les lecteurs d’écran pour lire la transcription de la conversation à l’utilisateur. Le `Layout` propriété spécifie la façon dont le message s’affichera, actuellement uniquement la `MSMessageTemplateLayout` est pris en charge et se présente comme suit :

[![](advanced-message-app-extensions-images/interactive06.png "Le modèle MSMessageTemplateLayout")](advanced-message-app-extensions-images/interactive06.png#lightbox)

Le `Image` propriété de la `MSMessageTemplateLayout` fournit du contenu pour le corps principal de la MessageBubble sur l’écran. Le `MediaFileUrl` propriété également fournit du contenu pour le corps de la bulle de Message, mais autorise pour le contenu qui n’est pas pris en charge par `UIImage` (par exemple, un fichier vidéo qui effectuerait une boucle en arrière-plan). Si les deux le `Image` et `MediaFileUrl` propriétés sont fournies, le `Image` propriété aura la priorité. Le `MediaFileUrl` prend en charge PNG, JPEG, GIF et vidéo (dans n’importe quel format qui peut être lu par l’infrastructure de lecteur) formats multimédias.

La taille du média recommandée est pixels de 300 x 300 à la résolution 3 x. Ressources légèrement supérieure et inférieure sont également acceptées et Apple suggère de test avec quelques différentes tailles pour obtenir de meilleurs résultats. L’application Message sera bas-exemple et mettre à l’échelle de ce média selon les besoins.

Lorsque les ressources sont envoyés au récepteur, tous les médias attachés seront automatiquement transcodé par l’application Messages pour optimiser les à partir de transfert sur les réseaux. Pour cette raison, Apple déconseille notamment le texte dans le média qui est joint au message, car le support sera mis à l’échelle vers le bas et compressé pour la transmission, potentiellement rendant ainsi le texte illisible.

Le `ImageTitle` et `ImageSubtitle` propriétés fournissent une description pour le média qui est présentée dans la bulle de Message. Ces propriétés recevront sous forme de texte pour le périphérique de réception où ils seront rendus précisément dans le coin inférieur gauche de l’image.

Le `Caption`, `SubCaption`, `TrailingCaption` et `TrailingSubcaption` propriétés décrivent l’image supplémentaire et est restituées dans une section sous l’image. Le paramétrage de ces propriétés pour `null` créera une bulle de Message sans la zone de légende :

[![](advanced-message-app-extensions-images/interactive07.png "Une bulle de Message sans la zone de légende")](advanced-message-app-extensions-images/interactive07.png#lightbox)

La dernière chose à noter est que l’application Messages dessinera icône de l’Extension application de messages dans le coin supérieur gauche de la bulle de Message.

## <a name="sending-a-message"></a>Envoi d’un Message

Une fois un `MSMessage` a été composé, le code suivant peut être utilisé pour envoyer des :

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

Le `ActiveConversation` propriété de la `MSMessagesAppViewController` contiendront la conversation actuelle l’Extension d’application de Message a été lancée dans.

Appelez le `InsertMessage` de la `MSConversation` pour inclure le message dans la conversation et de gérer les erreurs qui peuvent se produire. Si le message est inclus avec succès, la bulle de Message s’affichera dans le champ d’entrée.

En outre, l’extension peut envoyer des différents types de données à la conversation telles que :

- **Text** - `ActiveConversation.InsertText ("Message", (error) => {...});`
- **Attachments** - `ActiveConversation.InsertAttachment (new NSUrl ("path"), "filename", (error) => {...});`
- **Autocollants**  -  `ActiveConversation.InsertSticker (sticker, (obj) => {...});` où `sticker` est un `MSSticker`.

Une fois le nouveau contenu dans le champ d’entrée, l’utilisateur est en mesure d’envoyer le message en cliquant sur le bleu **envoyer** bouton (comme n’importe quel message classique). Il n’existe aucun moyen pour l’Extension d’application de Message envoyer automatiquement le contenu, ce processus est totalement sous le contrôle de l’utilisateur.

## <a name="handling-the-compact-and-expanded-modes"></a>Gérer les Modes Compact et développés

Une Extension d’application de Message peut être affichée dans un des deux modes d’affichage différents :

[![](advanced-message-app-extensions-images/interactive08.png "Une Extension d’application Message affiché dans les deux modes d’affichage différents : Compact et développée")](advanced-message-app-extensions-images/interactive08.png#lightbox)

- **Compact** -il s’agit du mode par défaut où l’Extension d’application Message occupe de 25 % en bas de l’affichage du Message. En mode réduit, l’application n’a pas d’accès pour le clavier, le défilement horizontal ou reconnaissances de mouvement de balayage. L’application a accès au champ d’entrée et appelle à `InsertMessage` s’affichera instantanément à l’utilisateur.
- **Développé** -l’Extension d’application Message remplit toute la vue de Message. Il n’a pas accès au champ d’entrée, mais n’a pas accès au clavier, défilement horizontal et reconnaissances de mouvement de balayage.

Une Extension d’application de Message est possible entre ces modes par programme ou manuellement par l’utilisateur à tout moment et doit être immédiatement réactive à aucun mode est modifié dans la vue.

Examinons l’exemple suivant de gérer le basculement entre les deux modes d’affichage différents. Deux contrôleurs de vue différents sera nécessaire pour chaque état. Le `StickerBrowserViewController` gère la **Compact** vue et la `AddStickerViewController` gère la **développée** vue :

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

Le `DidTransition` méthode est substituée pour gérer le basculement entre les deux modes :

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

Si vous le souhaitez, l’application pourrait avoir utilisé le `WillTransition` méthode pour gérer le changement de mode d’affichage avant qu’il est présenté à l’utilisateur (comme dans l’exemple de générateur de Icecream ci-dessus). Pour plus d’informations, consultez notre [une personnalisation plus poussée autocollant](~/ios/platform/message-app-integration/intro-to-message-app-extensions.md) documentation.

## <a name="replying-to-a-message"></a>Réponse à un Message

Il existe deux cas nécessitant une Extension d’application Message gérer lors de la réponse à un message :

[![](advanced-message-app-extensions-images/interactive09.png "L’Extension d’application de Message dans les modes inactif et actif")](advanced-message-app-extensions-images/interactive09.png#lightbox)

- **L’extension est inactif** -il est une des bulles de Message de l’Extension application de messages dans la transcription de Message que l’utilisateur peut appuyer pour activer les extensions et poursuivre la conversation interactive.
- **Extension est Active** -l’utilisateur peut appuyer à bulles de Message de l’Extension d’application Message dans la transcription de Message pour entrer le mode d’affichage développé et continuer le processus interactif à partir de leur point d’interruption.

### <a name="the-extension-is-inactive"></a>L’Extension est inactif

Lorsque l’utilisateur clique sur une bulle de Message par l’utilisateur dans la transcription de Message et l’Extension d’application de Message est inactive, le processus suivant se produit :

[![](advanced-message-app-extensions-images/interactive10.png "Gestion d’une bulle de Message inactif")](advanced-message-app-extensions-images/interactive10.png#lightbox)

1. L’utilisateur appuie sur la bulle de Message de l’extension.
2. Lorsqu’une extension est lancée, l’application Message lance un processus.
3. Le `DidBecomeActive` méthode est appelée et reçoit un `MSConversation` qui représente la conversation dont l’Extension d’application de Message est en cours d’exécution dans.
4. Étant donné que l’extension est basée dérivé d’un `UIViewController` les deux `ViewWillAppear` et `ViewDidAppear` sont appelées.

Lorsque le processus est terminé, l’Extension d’application de Message s’affiche dans le mode d’affichage développé.

### <a name="the-extension-is-active"></a>L’Extension est actif.

Lorsqu’une bulle de Message est tapée par l’utilisateur dans la transcription de Message et l’Extension d’application de Message est active, le processus suivant se produit :

[![](advanced-message-app-extensions-images/interactive11.png "Gestion d’une bulle de Message active")](advanced-message-app-extensions-images/interactive11.png#lightbox)

1. L’utilisateur appuie sur la bulle de Message de l’extension.
2. Étant donné que l’Extension d’application de Message est déjà active, le `WillTransition` méthode de la `MSMessagesAppViewController` est appelé pour gérer le basculement à partir du CD-ROM vers le mode d’affichage développé.
3. Le `DidSelectMessage` méthode de la `MSMessagesAppViewController` est appelée et reçoit le `MSMessage` et `MSConversation` auquel appartient la bulle de Message.
4. Le `DidTransition` méthode de la `MSMessagesAppViewController` est appelé pour gérer le basculement à partir du CD-ROM vers le mode d’affichage développé.

Là encore, lorsque le processus est terminé, l’Extension d’application de Message s’affiche dans le mode d’affichage développé.

### <a name="accessing-the-selected-message"></a>L’accès au Message sélectionné

Dans les deux cas, lorsque l’utilisateur appuie sur une bulle de Message appartenant à l’Extension d’application de Message, elle devra accéder à la `MSMessage` qui a été tapée à l’aide de la `SelectedMessage` propriété de la `MSConversation`.

Exemple :

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

Le message sélectionné doit être indiqué dans l’interface utilisateur de l’Extension application de Message et l’utilisateur doit être autorisé à composer une réponse.

## <a name="removing-partially-completed-messages"></a>Suppression partiellement terminée des Messages

Lors de l’envoi des différentes étapes d’interactif d’une conversation entre deux l’utilisateur de la conversation, les bulles de Message partiellement terminée peut commencer à encombrer la transcription de Message :

[![](advanced-message-app-extensions-images/interactive12.png "Les bulles de Message partiellement terminée peuvent encombrer la transcription de Message")](advanced-message-app-extensions-images/interactive12.png#lightbox)

Au lieu de cela, l’Extension d’application de Message doit réduire le précédent Message remonte dans un commentaire concis dans la transcription de Message :

[![](advanced-message-app-extensions-images/interactive13.png "Réduction des bulles de Message précédent dans la transcription de Message")](advanced-message-app-extensions-images/interactive13.png#lightbox)

Ceci est géré à l’aide un `MSSession` pour réduire toutes les étapes existants. Par conséquent, le `DidSelectMessage` méthode de la `MSMessagesAppViewController` classe peut être modifiée pour ressembler à ce qui suit :

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

Si le message sélectionné a déjà une sortie `MSSession`, il est utilisé sans quoi une nouvelle `MSSession` est créé. Le `SummaryText` propriété de la `MSMessage` est utilisée pour ajouter une légende pour les étapes précédentes réduits. Si le `SummaryText` est définie sur `null`, les étapes précédentes de la conversation seront supprimés en totalité à partir de la transcription de la Conversation.

## <a name="advanced-message-api-features"></a>Message API fonctionnalités avancées

Avec les fonctionnalités de base de la nouvelle API Message abordée en détail ci-dessus, allons examiner les certaines des fonctionnalités plus avancées qui Apple a intégré dans l’infrastructure.

Tout d’abord, il existe plusieurs autres méthodes de remplacement dans la `MSMessagesAppViewController` classe qui fournissent un accès plus détaillé à la conversation :

- `DidStartSendingMessage` -Ceci est appelé lorsque l’utilisateur appuie sur le bouton Envoyer. Cela ne signifie pas que le message a été réellement remise au destinataire, simplement que le processus d’envoi a été démarré.
- `DidCancelSendingMessage` -Cela se produit lorsque l’utilisateur appuie sur le *X* situé dans l’angle supérieur droit de la bulle de Message dans la transcription de la Conversation.
- `DidReceiveMessage` -Cette méthode est appelée lorsque l’Extension d’application de messages est active un un nouveau message a été reçu à partir d’un des participants à la conversation.

### <a name="group-conversations"></a>Conversations de groupe

Une Extension d’application Message utilisable pendant que les utilisateurs sont impliqués dans des conversations de groupe (avec des personnes de 3 ou plus), et cela aura prendre en considération lorsque vous concevez et implémentez une Extension d’application de Message.

Examinons l’interaction suivante dans une conversation de groupe avec trois utilisateurs :

[![](advanced-message-app-extensions-images/interactive14.png "Interaction dans une conversation de groupe avec trois utilisateurs")](advanced-message-app-extensions-images/interactive14.png#lightbox)

1. L’utilisateur 1 envoie un Message interactif de groupe demandant l’utilisateur 2 et 3 de l’utilisateur de choisir un remplissage d’un hamburger.
2. L’utilisateur 2 choisit tomatoes.
3. L’utilisateur 3 choisit cornichons.
4. Choix de l’utilisateur 2 et 3 utilisateur arrive à utilisateur 1 presque simultanément. Par conséquent, les choix de l’utilisateur 2 est réduite en une ligne de résumé et n’est pas disponible. Cet incident pourrait ont également été retourné, avec le choix de l’utilisateur 2 est affiché et l’utilisateur 3 de réduit.

Dans les deux cas, ce comportement est indésirable 1 de l’utilisateur doit pouvoir accéder aux options d’utilisateur 2 et 3 utilisateur. Pour gérer cette situation, Apple suggère que l’Extension d’application Message stocke l’état du message dans le cloud et utilisez la propriété URL de la `MSMessage` (qui est envoyée entre les utilisateurs) pour accéder à cet état.

Lorsque l’utilisateur envoie un message, un jeton de session est généré et envoyé vers le cloud avec l’état actuel du message. Lorsqu’un utilisateur clique sur une bulle de Message dans la transcription de la Conversation, le jeton de session est utilisé pour récupérer l’état de session en cours à partir du cloud.

### <a name="sender-identifiers"></a>Identificateurs de l’émetteur

Pour discuter de l’accès à l’identificateur de l’expéditeur d’un message, prenons l’exemple d’une conversation de groupe ci-dessus :

[![](advanced-message-app-extensions-images/interactive15.png "Envoi d’identificateurs de conversation de groupe")](advanced-message-app-extensions-images/interactive15.png#lightbox)

1. Là encore, utilisateur 1 envoie un Message interactif groupe demandant l’utilisateur 2 et 3 de l’utilisateur de choisir un remplissage d’un hamburger.
2. L’utilisateur 3 choisit cornichons.
3. Choix de l’utilisateur 3 arrive à utilisateur 1 et 2 de l’utilisateur n’a pas encore répondu.
4. Apple étant très concernée par la confidentialité des utilisateurs, l’Extension d’application Message connaît uniquement un identificateur Unique (comme un `NSUUID`) qui est assigné à chaque participant de la conversation. Sur l’appareil local, seul l’identificateur de l’utilisateur actuel est connu.
5. Le `MSMessage` a un `SenderIdentifier` propriété qui correspond à l’un de l’utilisateur dans la liste des participants connue de l’extension.
6. Chaque périphérique de l’utilisateur possède sa propre copie de la liste des participants où là encore, l’identificateur de l’utilisateur local uniquement est connu.
7. Lorsqu’un message est envoyé, son `SenderIdentifier` propriété est également connue de l’utilisateur local.

Les identificateurs de l’expéditeur peuvent être utilisés comme suit :

- En examinant la liste des participants l’extension peut obtenir le nombre d’utilisateurs dans la conversation.
- Lorsque l’extension reçoit un message à partir d’un utilisateur, il peut conserver le suivi de l’identificateur d’expéditeur. S’il reçoit un autre message avec le même identificateur de l’expéditeur, l’extension sait qu’il est dans le même utilisateur.
- Ils peuvent être utilisés pour identifier un utilisateur spécifique dans la conversation.

L’identificateur d’expéditeur peut être utilisé dans tous les champs de texte de la `MSMessageTemplateLayout` en lui attribuant un signe dollar (`$`). Exemple :

```csharp
// Pass along the sender identifier
var layout = new MSMessageTemplateLayout()
layout.Caption = string.format("${0} wants pickles.",Conversation.LocalParticipantIdentifier.UuidString);
```

Lorsque l’application Messages affiche une bulle de Message avec ce type de mise en forme, il remplacera le `$uuid...` avec le nom de contact du participant à la conversation qui a envoyé le message.

L’identificateur d’expéditeur étant unique sur chaque appareil, examinez le diagramme ci-dessus, notez de que 1 utilisateur et périphérique de l’utilisateur 3 a un identificateur d’expéditeur unique différent pour chaque participant à la conversation.

Les identificateurs de l’expéditeur sont limités à l’installation de l’Extension d’application de Message. Par conséquent, si un utilisateur désinstalle et réinstalle l’Extension d’application Message nouveaux identificateurs expéditeur sera générés pour la nouvelle installation.

Pour accéder à l’identificateur, l’extension peut utiliser le code suivant :

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

Les Messages Interactive générés par une Extension d’application Message seront remis sur les plateformes Apple suivantes :

- watchOS 3
- macOS Sierra
- iOS 10

Des trois plateformes, uniquement iOS 10 permet à l’utilisateur Générer un message interactif. Sur macOS Sierra, si l’utilisateur clique sur une bulle de Message interactif, l’URL associée à la `MSMessage` seront ouverts dans Safari et une représentation du message doit être y est affichée.

Sur watchOS, l’application Messages peut transférer un message interactif sur un appareil iOS attaché où l’utilisateur peut créer une réponse.

La nouvelle API de Messages est prise en charge de secours si le message interactif est reçu sur les plateformes plus anciennes Apple :

- watchOS 2 +
- OS X 10.11 +
- iOS 9 +

Ils seront fournis dans un format de secours comme deux messages distincts :

- L’image sera fournie par la mise en page du modèle.
- L’autre est l’URL comme indiqué dans le `MSMessage`.

## <a name="summary"></a>Récapitulatif

Cet article a présenté les techniques avancées pour travailler avec les Extensions d’application de Message dans une solution Xamarin.iOS qui s’intègre à la **Messages** application et présente nouvelles fonctionnalités à l’utilisateur.


## <a name="related-links"></a>Liens associés

- [Générateur de glace (exemple)](https://developer.xamarin.com/samples/monotouch/ios10/IceCreamBuilder/)
- [Référence des messages](https://developer.apple.com/reference/messages)
- [Guide de programmation d’Extension d’application](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214)
