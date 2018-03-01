---
title: "Notifications utilisateur avancé"
description: "Cet article est une présentation approfondie de la nouvelle infrastructure de Notifications à l’utilisateur et comment tirer pleinement parti de celui-ci dans une application Xamarin.iOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4e1ff652-28f0-4566-b383-9d12664401a4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 6408f3b45f93413fa814e410f07e7b71179b7338
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/28/2018
---
# <a name="advanced-user-notifications"></a>Notifications utilisateur avancé

_Cet article est une présentation approfondie de la nouvelle infrastructure de Notifications à l’utilisateur et comment tirer pleinement parti de celui-ci dans une application Xamarin.iOS._

Nouveau pour iOS 10, la Notification utilisateur framework permet la remise et le traitement des notifications locale et distantes. À l’aide de cette infrastructure, une application ou une extension d’application permettre planifier la remise de notifications locales en spécifiant un ensemble de conditions comme emplacement ou l’heure du jour.

## <a name="about-user-notifications"></a>À propos des Notifications à l’utilisateur

Permet à la nouvelle infrastructure de Notification de l’utilisateur pour la remise et le traitement des notifications locale et distantes. À l’aide de cette infrastructure, une application ou une Extension d’application permettre planifier la remise de notifications locales en spécifiant un ensemble de conditions comme emplacement ou l’heure du jour.

En outre, l’application ou une extension peut recevoir (et éventuellement modifier) les notifications à la fois locale et distantes qu’ils sont remis à l’appareil iOS de l’utilisateur.

La nouvelle infrastructure d’interface utilisateur de Notification utilisateur permet à une application ou une Extension d’application pour personnaliser l’apparence des notifications à la fois locale et distantes lorsqu’ils sont présentés à l’utilisateur.

Cette infrastructure offre une application peut fournir les notifications à un utilisateur les domaines suivants :

- **Alertes Visual** - où la notification reporte à partir du haut de l’écran sous forme de bannière.
- **Son et aux Vibrations** -peut être associé à une notification.
- **Marquage d’icône application** : si l’icône de l’application affiche un badge indique que le nouveau contenu est disponible. Comme le nombre de messages électroniques non lus.

En outre, selon le contexte actuel de l’utilisateur, il existe différentes façons d’une notification s’affiche :

- Si l’appareil est déverrouillé, la notification déploie vers le bas à partir du haut de l’écran sous forme de bannière.
- Si l’appareil est verrouillé, la notification s’affichera sur l’écran de verrouillage de l’utilisateur.
- Si l’utilisateur a manqué une notification, ils peuvent ouvrir le centre de notifications et afficher les notifications en attente disponibles, il.

Une application Xamarin.iOS a deux types de Notifications à l’utilisateur qu’il est en mesure d’envoyer :

- **Notifications locales** -ceux-ci sont envoyés par les applications installées localement sur l’appareil de l’utilisateur.
- **Notifications à distance** - sont envoyés à partir d’un serveur distant et soit présentées à l’utilisateur ou déclenche une mise à jour de l’arrière-plan du contenu de l’application.

Pour plus d’informations, consultez notre [Notifications à l’utilisateur améliorée](~/ios/platform/user-notifications/enhanced-user-notifications.md) documentation.

## <a name="the-new-user-notification-interface"></a>La nouvelle Interface de Notification utilisateur

Notifications à l’utilisateur dans iOS 10 sont présentées avec une nouvelle conception de l’interface utilisateur qui fournit davantage de contenu comme un titre, de sous-titre et de pièces jointes Media facultatif qui peut être affiché sur l’écran de verrouillage, sous forme de bannière en haut de l’appareil ou dans le centre de notifications.

Quel que soit l’endroit où une Notification à l’utilisateur s’affiche dans iOS 10, il est présenté avec la même apparence et avec les mêmes fonctionnalités.

Dans iOS 8, Apple a introduit des Notifications où le développeur peut joindre des Actions personnalisées à une Notification et autoriser l’utilisateur à effectuer une opération sur une Notification sans avoir à lancer l’application. Dans iOS 9, Apple améliorée des Notifications utilisables avec une réponse rapide qui permet à l’utilisateur répondre à une Notification de la saisie de texte.

Étant donné que les Notifications à l’utilisateur sont une partie intégrante de plus de l’expérience utilisateur dans iOS 10, Apple a développée davantage de Notifications utilisables pour prendre en charge 3D Touch, où l’utilisateur appuie sur une notification et une interface utilisateur personnalisée est complet pour fournir une interaction riche lors de la notification.

Lorsque l’interface utilisateur de Notification utilisateur personnalisée s’affiche, si l’utilisateur interagit avec toutes les Actions liées à la Notification, l’interface utilisateur personnalisée peut être instantanément mis à jour pour fournir des commentaires concernant ce qui a été modifié.

Nouveau pour iOS 10, l’API de l’interface utilisateur de Notification utilisateur permet à une application Xamarin.iOS facilement tirer parti de ces nouvelles fonctionnalités de l’interface utilisateur de Notification utilisateur.

## <a name="adding-media-attachments"></a>Ajout de pièces jointes de support

L’un des éléments qui obtient partagés entre les utilisateurs plus courants étant photos, iOS 10 ajouté la possibilité de joindre un élément multimédia (par exemple, une photo) directement à une Notification, où il sera présenté et prête à l’utilisateur avec le reste du conte de la Notification NT.

Toutefois, en raison de la taille impliquée dans l’envoi, même une petite image, attachez à une charge utile de Notification à distance devient impossible. Pour gérer cette situation, le développeur peut utiliser la nouvelle Extension de Service dans iOS 10 pour télécharger l’image à partir d’une autre source (par exemple, une banque de données CloudKit) et l’attacher au contenu de la notification avant qu’il est affiché à l’utilisateur.

Pour une Notification à distance doivent être modifiées par une Extension de Service, sa charge utile doit être marqué comme mutable. Exemple :

```csharp
{
    aps : {
        alert : "New Photo Available",
        mutable-content: 1
    },
    my-attachment : "https://example.com/photo.jpg"
}
```

Examinez la présentation suivante du processus :

[ ![](advanced-user-notifications-images/extension02.png "Processus d’ajout des pièces jointes de support")](advanced-user-notifications-images/extension02.png)

Une fois que la Notification à distance est remise à l’appareil (via APNs), l’Extension de Service peuvent télécharger puis de l’image requis par quelque moyen que vous le souhaitez (comme un `NSURLSession`) et qu’il reçoit l’image, il peut modifier le contenu de la Notification et l’affichage Il est à l’utilisateur.

Voici un exemple de comment ce processus peut être géré dans le code :

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;

namespace MonkeyNotification
{
    public class NotificationService : UNNotificationServiceExtension
    {
        #region Constructors
        public NotificationService ()
        {
        }
        #endregion

        #region Override Methods
        public override void DidReceiveNotificationRequest (UNNotificationRequest request, Action<UNNotificationContent> contentHandler)
        {
            // Get file URL
            var attachementPath = request.Content.UserInfo.ObjectForKey (new NSString ("my-attachment"));
            var url = new NSUrl (attachementPath.ToString ());

            // Download the file
            var localURL = new NSUrl ("PathToLocalCopy");

            // Create attachment
            var attachmentID = "image";
            var options = new UNNotificationAttachmentOptions ();
            NSError err;
            var attachment = UNNotificationAttachment.FromIdentifier (attachmentID, localURL, options , out err);

            // Modify contents
            var content = request.Content.MutableCopy() as UNMutableNotificationContent;
            content.Attachments = new UNNotificationAttachment [] { attachment };

            // Display notification
            contentHandler (content);
        }

        public override void TimeWillExpire ()
        {
            // Handle service timing out
        }
        #endregion
    }
}
```

Lorsque la Notification est reçue à partir de l’APNs, l’adresse personnalisée de l’image est en lecture à partir du contenu et le fichier est téléchargé à partir du serveur. Un `UNNotificationAttachement` est créée avec un ID unique et l’emplacement local de l’image (comme un `NSUrl`). Une copie mutable du contenu de la Notification est créée et les pièces jointes de média sont ajoutées. Enfin, la Notification est affichée à l’utilisateur en appelant le `contentHandler`.

Une fois la pièce jointe a été ajoutée à une Notification, le système est sur le déplacement et la gestion du fichier.

En plus des Notifications à distance présentées ci-dessus, à laquelle les médias joints sont également prises en charge des Notifications Local, où le `UNNotificationAttachement` est créé et attaché à la Notification, ainsi que son contenu.

Notification dans iOS 10 prend en charge des pièces jointes du support d’images (statique et GIF), audio ou vidéo et le système seront affiche automatiquement l’interface utilisateur personnalisée correcte pour chacun de ces types de pièces jointes lorsque la Notification est présentée à l’utilisateur.

> [!NOTE]
> **Remarque :** doit veiller à optimiser la taille du média et le temps nécessaire pour télécharger le support à partir du serveur distant (ou pour assembler le support pour les Notifications Local) en tant que le système impose des limites stricts à la fois lors de l’exécution de Service de l’application Extension. Par exemple, envisagez d’envoi d’une version mise à l’échelle vers le bas de l’image ou un petit élément d’une vidéo à être présentés dans la Notification.

## <a name="creating-custom-user-interfaces"></a>Création d’Interfaces utilisateur personnalisées

Pour créer une Interface utilisateur personnalisée pour ses Notifications à l’utilisateur, le développeur doit ajouter une Extension de contenu de Notification (nouveauté d’iOS 10) à la solution de l’application.

L’Extension de contenu de Notification permet au développeur d’ajouter leurs propres vues dans l’interface utilisateur de Notification et dessiner n’importe quel contenu qu'ils souhaitent. Toutefois, les widgets d’interface utilisateur interactives (par exemple, des boutons ou des champs de texte) sont **pas** pris en charge par l’interface utilisateur de Notification et ne doivent jamais être ajoutés à une conception de l’interface utilisateur personnalisée.

Pour prendre en charge l’interaction utilisateur avec une Notification de l’utilisateur, Actions personnalisées doit être créées, inscrit avec le système et attachées à la Notification avant de planifier avec le système. L’Extension de contenu de Notification sera appelée pour gérer le traitement de ces actions. Consultez le [l’utilisation d’Actions de Notification](~/ios/platform/user-notifications/enhanced-user-notifications.md) section de la [Notifications à l’utilisateur améliorée](~/ios/platform/user-notifications/enhanced-user-notifications.md) document pour plus d’informations sur les Actions personnalisées.

Lorsqu’une Notification de l’utilisateur avec une interface utilisateur personnalisée est présentée à l’utilisateur, il possède les éléments suivants :

[ ![](advanced-user-notifications-images/customui01.png "Une Notification de l’utilisateur avec un élément de l’interface utilisateur personnalisée")](advanced-user-notifications-images/customui01.png)

Si l’utilisateur interagit avec les Actions personnalisées (présentés ci-dessous la Notification), l’Interface utilisateur peuvent être mis à jour pour fournir une rétroaction comme la que s’est-il passé lorsqu’ils appelé une action donnée.

### <a name="adding-a-notification-content-extension"></a>Ajout d’une Extension de contenu de Notification

Pour implémenter une interface de Notification utilisateur personnalisé dans une application Xamarin.iOS, procédez comme suit :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1. Ouvrez la solution de l’application dans Visual Studio pour Mac.
2. Avec le bouton droit sur le nom de la Solution dans le **Solution remplissage** et sélectionnez **ajouter** > **ajouter un nouveau projet**.
3. Sélectionnez **iOS** > **Extensions** > **Extensions contenu de la Notification** et cliquez sur le **suivant** bouton : 

    [ ![](advanced-user-notifications-images/notify01.png "Sélectionnez les Extensions de contenu de Notification")](advanced-user-notifications-images/notify01.png)
4. Entrez un **nom** pour l’extension et cliquez sur le **suivant** bouton : 

    [ ![](advanced-user-notifications-images/notify02.png "Entrez un nom pour l’extension")](advanced-user-notifications-images/notify02.png)
5. Ajuster la **nom du projet** et/ou **nom de la Solution** si nécessaire et cliquez sur le **créer** bouton : 

    [ ![](advanced-user-notifications-images/notify03.png "Ajuster le nom du projet et/ou le nom de la Solution")](advanced-user-notifications-images/notify03.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Ouvrez la solution de l’application dans Visual Studio pour Mac.
2. Avec le bouton droit sur le nom de la Solution dans le **l’Explorateur de solutions** et sélectionnez **ajouter** > **ajouter un nouveau projet**.
3. Sélectionnez **iOS** > **Extensions** > **Extensions contenu de la Notification**: 

    [ ![](advanced-user-notifications-images/notify01w.png "Sélectionnez les Extensions de contenu de Notification")](advanced-user-notifications-images/notify01w.png)
4. Entrez un **nom** pour l’extension et cliquez sur le **OK** bouton.

-----

Lorsque la Notification de l’Extension de contenu est ajoutée à la solution, trois fichiers seront créés dans le projet de l’Extension :

1. `NotificationViewController.cs` -C’est le contrôleur de la vue principale pour l’Extension de contenu de Notification.
2. `MainInterface.storyboard` -Où le développeur présente l’interface utilisateur visible pour l’Extension de contenu de Notification dans le concepteur iOS.
3. `Info.plist` : Contrôle la configuration de l’Extension de contenu de Notification.

La valeur par défaut `NotificationViewController.cs` fichier ressemble à celui-ci :

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;
using UserNotificationsUI;


namespace MonkeyChatNotifyExtension
{
    public partial class NotificationViewController : UIViewController, IUNNotificationContentExtension
    {
        #region Constructors
        protected NotificationViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any required interface initialization here.
        }
        #endregion

        #region Public Methods
        [Export ("didReceiveNotification:")]
        public void DidReceiveNotification (UNNotification notification)
        {
            label.Text = notification.Request.Content.Body;

            // Grab content
            var content = notification.Request.Content;

        }
        #endregion
    }
}
```

Le `DidReceiveNotification` méthode est appelée lorsque la Notification est développée par l’utilisateur afin que l’Extension de contenu de Notification peut remplir l’interface utilisateur personnalisée avec le contenu de la `UNNotification`. Dans l’exemple ci-dessus, une étiquette a été ajoutée à la vue, exposée au code portant le nom `label` et est utilisé pour afficher le corps de la Notification.

### <a name="setting-the-notification-content-extensions-categories"></a>Définition catégories de l’Extension contenu Notification

Le système doit être informé sur la façon de trouver l’Extension de contenu de Notification de l’application selon les catégories spécifiques, à qu'il répond. Effectuez ce qui suit :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1. Double-cliquez sur l’Extension `Info.plist` de fichiers dans le **Solution remplissage** à ouvrir pour le modifier.
2. Basculez vers le **Source** vue.
3. Développez le `NSExtension` clé.
4. Ajouter le `UNNotificationExtensionCategory` clé en tant que type **chaîne** avec la valeur de la catégorie de l’Extension appartient à (dans cet exemple ' événement-invitation) : 

    [ ![](advanced-user-notifications-images/customui02.png "Ajouter la clé UNNotificationExtensionCategory")](advanced-user-notifications-images/customui02.png)
5. Enregistrez les modifications apportées.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Double-cliquez sur l’Extension `Info.plist` de fichiers dans le **l’Explorateur de solutions** à ouvrir pour le modifier.
3. Développez le `NSExtension` clé.
4. Ajouter le `UNNotificationExtensionCategory` clé en tant que type **chaîne** avec la valeur de la catégorie de l’Extension appartient à (dans cet exemple ' événement-invitation) : 

    [ ![](advanced-user-notifications-images/customui02w.png "Ajouter la clé UNNotificationExtensionCategory")](advanced-user-notifications-images/customui02w.png)
5. Enregistrez les modifications apportées.

-----

Notification des catégories de contenu Extension (`UNNotificationExtensionCategory`) utilisent les mêmes valeurs de catégorie sont utilisées pour enregistrer les Actions de Notification. Dans le cas où l’application utilise la même interface utilisateur pour plusieurs catégories, basculez le `UNNotificationExtensionCategory` au type **tableau** et fournir toutes les catégories que nécessaires. Exemple :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[ ![](advanced-user-notifications-images/customui03.png "Catégories de l’Extension de contenu de notification")](advanced-user-notifications-images/customui03.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![](advanced-user-notifications-images/customui03w.png "Catégories de l’Extension de contenu de notification")](advanced-user-notifications-images/customui03w.png)

-----

### <a name="hiding-the-default-notification-content"></a>Masquer le contenu de la Notification par défaut

Dans le cas où l’interface utilisateur de Notification personnalisée affichera le même contenu en tant que la valeur par défaut de Notification (titre, sous-titre et corps affichée automatiquement au bas de l’interface utilisateur de Notification), ces informations par défaut peuvent être masquées en ajoutant le `UNNotificationExtensionDefaultContentHidden`clés pour le `NSExtensionAttributes` clé en tant que type **booléenne** avec la valeur `YES` dans de l’Extension `Info.plist` fichier :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[ ![](advanced-user-notifications-images/customui04.png "Recherche d’informations par défaut")](advanced-user-notifications-images/customui04.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![](advanced-user-notifications-images/customui04w.png "Recherche d’informations par défaut")](advanced-user-notifications-images/customui04w.png)

-----

### <a name="designing-the-custom-ui"></a>Conception de l’interface utilisateur personnalisée

Pour concevoir l’interface utilisateur personnalisée de la Notification l’Extension de contenu, double-cliquez sur le `MainInterface.storyboard` fichier à ouvrir pour le modifier dans le concepteur, iOS faites glisser les éléments dont vous avez besoin pour créer l’interface de votre choix (tel que `UILabels` et `UIImageViews`).

> [!NOTE]
> **Remarque :** l’interface utilisateur de Notification est _pas_ prend en charge les contrôles interactifs tels que les champs de texte ou des boutons dans une Extension de contenu de Notification. Pendant qu’ils peuvent être ajoutés au plan conceptuel, l’utilisateur ne sera pas en mesure d’interagir avec eux. Pour ajouter une interaction utilisateur à une interface utilisateur de Notification personnalisée, utilisez à la place des actions personnalisées.

Une fois que l’interface utilisateur a été mise en forme et les contrôles exposée au code c#, ouvrez le `NotificationViewController.cs` pour la modification et de modifier le `DidReceiveNotification` méthode pour remplir l’interface utilisateur lorsque l’utilisateur développe la notification. Exemple :

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;
using UserNotificationsUI;


namespace MonkeyChatNotifyExtension
{
    public partial class NotificationViewController : UIViewController, IUNNotificationContentExtension
    {
        #region Constructors
        protected NotificationViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any required interface initialization here.
        }
        #endregion

        #region Public Methods
        [Export ("didReceiveNotification:")]
        public void DidReceiveNotification (UNNotification notification)
        {
            label.Text = notification.Request.Content.Body;

            // Grab content
            var content = notification.Request.Content;

            // Display content in the UILabels
            EventTitle.Text = content.Title;
            EventDate.Text = content.Subtitle;
            EventMessage.Text = content.Body;

            // Get location and display
            var location = content.UserInfo ["location"].ToString ();
            if (location != null) {
                Event.Location.Text = location;
            }

        }
        #endregion
    }
}
```

### <a name="setting-the-content-area-size"></a>Définition de la taille de la zone de contenu

Pour ajuster la taille de la zone de contenu affichée à l’utilisateur, le code ci-dessous définit les `PreferredContentSize` propriété dans la `ViewDidLoad` méthode à la taille souhaitée. Cette taille soit également ajustée en appliquant des contraintes à la vue dans le concepteur iOS, il est laissé au développeur de choisir la méthode qui convient le mieux pour eux.

Étant donné que la Notification système est déjà en cours d’exécution avant la Notification de l’Extension de contenu est appelée, la zone de contenu est initialement complète dimensionné et être animée à la taille demandée lorsque présentées à l’utilisateur.

Pour éviter cet effet, vous devez modifier le `Info.plist` fichier pour l’Extension et le jeu le `UNNotificationExtensionInitialContentSizeRatio` clé de la `NSExtensionAttributes` clé en type **nombre** avec une valeur qui représente le rapport souhaité. Exemple :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[ ![](advanced-user-notifications-images/customui05.png "La clé UNNotificationExtensionInitialContentSizeRatio")](advanced-user-notifications-images/customui05.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![](advanced-user-notifications-images/customui05w.png "La clé UNNotificationExtensionInitialContentSizeRatio")](advanced-user-notifications-images/customui05w.png)

-----

### <a name="using-media-attachments-in-custom-ui"></a>À l’aide de médias joints dans l’interface utilisateur personnalisée

Car les pièces jointes de support (comme indiqué dans le [Ajout de pièces jointes de support](#Adding-Media-Attachments) ci-dessus) font partie de la charge utile de Notification, ils sont accessibles et affichés dans l’Extension de contenu de Notification, tout comme ils le seraient dans la valeur par défaut Interface utilisateur de notification.

Par exemple, si l’interface utilisateur personnalisée de ci-dessus inclus un `UIImageView` qui a été exposé au code c#, le code suivant peut servir à remplir à partir de la pièce jointe de média :

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;
using UserNotificationsUI;

namespace MonkeyChatNotifyExtension
{
    public partial class NotificationViewController : UIViewController, IUNNotificationContentExtension
    {
        #region Constructors
        protected NotificationViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any required interface initialization here.
        }
        #endregion

        #region Public Methods
        [Export ("didReceiveNotification:")]
        public void DidReceiveNotification (UNNotification notification)
        {
            label.Text = notification.Request.Content.Body;

            // Grab content
            var content = notification.Request.Content;

            // Display content in the UILabels
            EventTitle.Text = content.Title;
            EventDate.Text = content.Subtitle;
            EventMessage.Text = content.Body;

            // Get location and display
            var location = content.UserInfo ["location"].ToString ();
            if (location != null) {
                Event.Location.Text = location;
            }

            // Get Media Attachment
            if (content.Attachements.Length > 1) {
                var attachment = content.Attachments [0];
                if (attachment.Url.StartAccessingSecurityScopedResource ()) {
                    EventImage.Image = UIImage.FromFile (attachment.Url.Path);
                    attachment.Url.StopAccessingSecurityScopedResource ();
                }
            }
        }
        #endregion
    }
}
```

Étant donné que la pièce jointe de média est gérée par le système, il se trouve en dehors de bac à sable de l’application. L’extension doit indiquer au système qu’elle souhaite que l’accès au fichier en appelant le `StartAccessingSecurityScopedResource` (méthode). Lorsque l’extension est terminée avec le fichier, il doit appeler le `StopAccessingSecurityScopedResource` pour libérer sa connexion.

### <a name="adding-custom-actions-to-a-custom-ui"></a>Ajout d’Actions personnalisées à une interface utilisateur personnalisée

Comme indiqué ci-dessus, l’interface utilisateur de Notification ne prend pas en charge une interaction utilisateur contrôles tels que les champs de texte ou les boutons ne sont pas prises en charge dans la conception de l’interface utilisateur.

Au lieu de cela, le mécanisme d’actions personnalisées standard est utilisé pour ajouter une interactivité à une interface utilisateur de Notification personnalisée. Consultez le [l’utilisation d’Actions de Notification](~/ios/platform/user-notifications/enhanced-user-notifications.md) section de la [Notifications à l’utilisateur améliorée](~/ios/platform/user-notifications/enhanced-user-notifications.md) document pour plus d’informations sur les actions personnalisées.

Outre les actions personnalisées, l’Extension de contenu de Notification peut répondre pour les actions intégrées suivantes ainsi :

- **Action par défaut** -c’est lorsque l’utilisateur appuie sur une notification pour ouvrir l’application et afficher les détails de la notification donnée.
- **Action de rejeter** -cette action est envoyée à l’application lorsque l’utilisateur ferme une notification donnée.

Extensions de contenu de notification ont également la possibilité de mettre à jour leur interface utilisateur lorsque l’utilisateur appelle l’une des Actions personnalisées, telles qu’indiquant une date comme accepté lorsque l’utilisateur appuie sur le **accepter** bouton d’Action personnalisée. En outre, les Extensions de contenu de Notification peut indiquer le système de retarder le rejet de l’interface utilisateur de Notification pour l’utilisateur peut voir l’effet de leurs actions avant la fermeture de la Notification.

Cela est fait en implémentant une deuxième version de la `DidReceiveNotification` méthode qui inclut un gestionnaire d’achèvement. Exemple :

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;
using UserNotificationsUI;
using CoreGraphics;

namespace myApp {
    public class NotificationViewController : UIViewController, UNNotificationContentExtension {

        public override void ViewDidLoad() {
            base.ViewDidLoad();

            // Adjust the size of the content area
            var size = View.Bounds.Size
            PreferredContentSize = new CGSize(size.Width, size.Width/2);
        }

        public void DidReceiveNotification(UNNotification notification) {

            // Grab content
            var content = notification.Request.Content;

            // Display content in the UILabels
            EventTitle.Text = content.Title;
            EventDate.Text = content.Subtitle;
            EventMessage.Text = content.Body;

            // Get location and display
            var location = Content.UserInfo["location"] as string;
            if (location != null) {
                Event.Location.Text = location;
            }

            // Get Media Attachment
            if (content.Attachements.Length > 1) {
                var attachment = content.Attachments[0];
                if (attachment.Url.StartAccessingSecurityScopedResource()) {
                    EventImage.Image = UIImage.FromFile(attachment.Url.Path);
                    attachment.Url.StopAccessingSecurityScopedResource();
                }
            }
        }

        [Export ("didReceiveNotificationResponse:completionHandler:")]
        public void DidReceiveNotification (UNNotificationResponse response, Action<UNNotificationContentExtensionResponseOption> completionHandler)
        {

            // Update UI when the user interacts with the
            // Notification
            Server.PostEventResponse += (response) {
                // Take action based on the response
                switch(response.ActionIdentifier){
                case "accept":
                    EventResponse.Text = "Going!";
                    EventResponse.TextColor = UIColor.Green;
                    break;
                case "decline":
                    EventResponse.Text = "Not Going.";
                    EventResponse.TextColor = UIColor.Red;
                    break;
                }

                // Close Notification
                completionHandler (UNNotificationContentExtensionResponseOption.Dismiss);
            };
        }
    }
}
```

En ajoutant le `Server.PostEventResponse` gestionnaire à la `DidReceiveNotification` (méthode) de l’Extension de contenu de Notification, l’Extension *doit* gérer toutes les actions personnalisées. L’extension peut également transmettre les actions personnalisées à l’application conteneur en modifiant le `UNNotificationContentExtensionResponseOption`. Exemple :

```csharp
// Close Notification
completionHandler (UNNotificationContentExtensionResponseOption.DismissAndForwardAction);
```

### <a name="working-with-the-text-input-action-in-custom-ui"></a>Utilisation de l’Action d’entrée de texte dans une interface utilisateur personnalisée

Selon la conception de l’application et la Notification, il peut arriver que l’utilisateur doit entrer du texte dans la Notification (par exemple, en réponse à un message). Une Extension de contenu de Notification a accès à l’action d’entrée de texte intégré exactement comme une notification standard.

Exemple :

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;
using UserNotificationsUI;


namespace MonkeyChatNotifyExtension
{
    public partial class NotificationViewController : UIViewController, IUNNotificationContentExtension
    {
        #region Computed Properties
        // Allow to take input
        public override bool CanBecomeFirstResponder {
            get { return true; }
        }

        // Return the custom created text input view with the
        // required buttons and return here
        public override UIView InputAccessoryView {
            get { return InputView; }
        }
        #endregion

        #region Constructors
        protected NotificationViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any required interface initialization here.
        }
        #endregion

        #region Private Methods
        private UNNotificationCategory MakeExtensionCategory ()
        {

            // Create Accept Action
            ...

            // Create decline Action
            ...

            // Create Text Input Action
            var commentID = "comment";
            var commentTitle = "Comment";
            var textInputButtonTitle = "Send";
            var textInputPlaceholder = "Enter comment here...";
            var commentAction = UNTextInputNotificationAction.FromIdentifier (commentID, commentTitle, UNNotificationActionOptions.None, textInputButtonTitle, textInputPlaceholder);

            // Create category
            var categoryID = "event-invite";
            var actions = new UNNotificationAction [] { acceptAction, declineAction, commentAction };
            var intentIDs = new string [] { };
            var category = UNNotificationCategory.FromIdentifier (categoryID, actions, intentIDs, UNNotificationCategoryOptions.None);

            // Return new category
            return category;

        }
        #endregion

        #region Public Methods
        [Export ("didReceiveNotification:")]
        public void DidReceiveNotification (UNNotification notification)
        {
            label.Text = notification.Request.Content.Body;

            // Grab content
            var content = notification.Request.Content;

            // Display content in the UILabels
            EventTitle.Text = content.Title;
            EventDate.Text = content.Subtitle;
            EventMessage.Text = content.Body;

            // Get location and display
            var location = content.UserInfo ["location"].ToString ();
            if (location != null) {
                Event.Location.Text = location;
            }

            // Get Media Attachment
            if (content.Attachements.Length > 1) {
                var attachment = content.Attachments [0];
                if (attachment.Url.StartAccessingSecurityScopedResource ()) {
                    EventImage.Image = UIImage.FromFile (attachment.Url.Path);
                    attachment.Url.StopAccessingSecurityScopedResource ();
                }
            }
        }

        [Export ("didReceiveNotificationResponse:completionHandler:")]
        public void DidReceiveNotification (UNNotificationResponse response, Action<UNNotificationContentExtensionResponseOption> completionHandler)
        {

            // Is text input?
            if (response is UNTextInputNotificationResponse) {
                var textResponse = response as UNTextInputNotificationResponse;
                Server.Send (textResponse.UserText, () => {
                    // Close Notification
                    completionHandler (UNNotificationContentExtensionResponseOption.Dismiss);
                });
            }

            // Update UI when the user interacts with the
            // Notification
            Server.PostEventResponse += (response) {
                // Take action based on the response
                switch (response.ActionIdentifier) {
                case "accept":
                    EventResponse.Text = "Going!";
                    EventResponse.TextColor = UIColor.Green;
                    break;
                case "decline":
                    EventResponse.Text = "Not Going.";
                    EventResponse.TextColor = UIColor.Red;
                    break;
                }

                // Close Notification
                completionHandler (UNNotificationContentExtensionResponseOption.Dismiss);
            };
        }
        #endregion
    }
}
```

Ce code crée une nouvelle action d’entrée de texte et l’ajoute à la catégorie de l’Extension (dans le `MakeExtensionCategory`) méthode. Dans la `DidReceive` substituer la méthode, il gère l’utilisateur d’entrer du texte avec le code suivant :

```csharp
// Is text input?
if (response is UNTextInputNotificationResponse) {
    var textResponse = response as UNTextInputNotificationResponse;
    Server.Send (textResponse.UserText, () => {
        // Close Notification
        completionHandler (UNNotificationContentExtensionResponseOption.Dismiss);
    });
}
```

Si les appels de conception pour ajouter des boutons personnalisés pour le champ d’entrée de texte, ajoutez le code suivant pour les inclure :

```csharp
// Allow to take input
public override bool CanBecomeFirstResponder {
    get {return true;}
}

// Return the custom created text input view with the
// required buttons and return here
public override UIView InputAccessoryView {
    get {return InputView;}
}
```

Lorsque l’action de commentaire est déclenchée par l’utilisateur, le contrôleur d’affichage et le champ d’entrée de texte personnalisés doivent être activés :

```csharp
// Update UI when the user interacts with the
// Notification
Server.PostEventResponse += (response) {
    // Take action based on the response
    switch(response.ActionIdentifier){
    ...
    case "comment":
        BecomeFirstResponder();
        TextField.BecomeFirstResponder();
        break;
    }

    // Close Notification
    completionHandler (UNNotificationContentExtensionResponseOption.Dismiss);

};
```

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a pris une apparence avancée à l’aide de la nouvelle infrastructure de Notification de l’utilisateur dans une application Xamarin.iOS. Il couvert l’ajout de pièces jointes de support Local et distant Notification et couvert à l’aide de la nouvelle interface de Notification utilisateur pour créer des interfaces utilisateur personnalisées de Notification.


## <a name="related-links"></a>Liens associés

- [Exemples d’iOS 10](https://developer.xamarin.com/samples/ios/iOS10/)
- [Référence UserNotifications Framework](https://developer.apple.com/reference/usernotifications)
- [UserNotificationsUI](https://developer.apple.com/reference/usernotificationsui)
- [Guide de programmation Notification locaux et distants](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/Introduction.html)
