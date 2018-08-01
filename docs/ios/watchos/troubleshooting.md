---
title: watchOS résolution des problèmes
description: Ce document traite des problèmes connus et solutions de contournement pour le développement watchOS avec Xamarin. Elle décrit les images avec des problèmes, ajoutez manuellement les fichiers de contrôleur d’interface, lancer une application de surveillance à partir de la ligne de commande et bien plus encore.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 27C31DB8-451E-4888-BBC1-CE0DFC2F9DEC
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 4e84028336669738c40da9e37cd22f32ba11dfc1
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34791763"
---
# <a name="watchos-troubleshooting"></a>watchOS résolution des problèmes

Cette page contient des informations supplémentaires et des solutions pour les fonctions en cours de développement. Certaines de ces solutions de contournement s’appliquent uniquement aux versions préliminaires de notre.

- [Problèmes connus](#knownissues)

- [Suppression du canal Alpha à partir d’Images d’icône](#noalpha)

- [Ajoutez manuellement les fichiers de contrôleur d’Interface](#add) pour Xcode Interface Builder.

- [Lancer le WatchApp à partir de la ligne de commande](#command_line).

<a name="knownissues" />

## <a name="known-issues"></a>Problèmes connus

### <a name="general"></a>Général

<a name="deploy" />

- Les versions antérieures de Visual Studio pour Mac incorrectement affichent l’une du le **AppleCompanionSettings** icônes comme étant pixels 88 x 88 ; ce qui entraîne un **erreur d’icône manquant** si vous essayez d’envoyer à l’application Magasin.
    Cette icône doit être 87 x 87 pixels (29 unités pour **@3x** les écrans rétine). Vous ne pouvez pas résoudre ce problème dans Visual Studio pour Mac - modifier le composant de l’image dans Xcode ou modifier manuellement le **Contents.json** fichier (pour correspondre à [cet exemple](https://github.com/xamarin/monotouch-samples/blob/master/WatchKit/WatchKitCatalog/WatchApp/Resources/Images.xcassets/AppIcons.appiconset/Contents.json#L126-L132)).

- Si du projet Extension espion **Info.plist > ID d’offre groupée WKApp** n’est pas [définir correctement](~/ios/watchos/get-started/project-references.md) pour correspondre à l’application de surveillance **ID d’offre groupée**, le débogueur ne pourront pas se connecter et Visual Studio pour Mac devront attendre avec le message *« En attente pour le débogueur pour se connecter »*.

- Le débogage est pris en charge dans **Notifications** mode mais peut ne pas être fiables. Une nouvelle tentative parfois fonctionne. Vérifiez que l’application de surveillance **Info.plist** `WKCompanionAppBundleIdentifier` est défini pour correspondre à l’identificateur de lot de l’application conteneur/parent (ie. celui qui s’exécute sur l’iPhone).

- iOS concepteur n’affiche pas les flèches vers le point d’entrée pour les contrôleurs d’interface aperçu ou de Notification.

- Vous ne pouvez pas ajouter deux `WKNotificationControllers` à un plan conceptuel.
    Solution de contournement : Le `notificationCategory` dans la table de montage séquentiel XML est toujours inséré avec la même `id`. Pour contourner ce problème, vous pouvez ajouter des contrôleurs de Notification de deux (ou plus), ouvrez le fichier d’animation dans un éditeur de texte et modifiez manuellement le `id` élément unique.

    [![](troubleshooting-images/duplicate-id-sml.png "Ouverture de la table de montage séquentiel de fichiers dans un éditeur de texte et modifiez manuellement l’élément id pour être unique")](troubleshooting-images/duplicate-id.png#lightbox)

- Vous pouvez voir une erreur « l’application n’a pas été construite » lorsque vous tentez de lancer l’application. Cela se produit après un **Clean** lorsque le projet de démarrage est défini pour le projet d’extension de surveillance.
    Le correctif consiste à sélectionner **Générer > Régénérer tout** , puis relancez l’application.

### <a name="visual-studio"></a>Visual Studio

Le concepteur iOS prennent en charge pour le Kit d’espion *requiert* la solution doit être configuré correctement. Si les références de projet ne sont pas définies (voir [comment définir les références](~/ios/watchos/get-started/project-references.md)) l’aire de conception ne fonctionnera pas correctement.

<a name="noalpha" />

## <a name="removing-the-alpha-channel-from-icon-images"></a>Suppression du canal Alpha à partir d’Images d’icône

Icônes ne doivent pas contenir un canal alpha (le canal alpha définit les zones transparentes d’une image), sinon l’application est rejetée pendant l’envoi du magasin d’applications avec une erreur similaire à ceci :

```csharp
Invalid Icon - The watch application '...watchkitextension.appex/WatchApp.app'
contains an icon file '...watchkitextension.appex/WatchApp.app/Icon-27.5@2x.png'
with an alpha channel. Icons should not have an alpha channel.
```

Il est facile de supprimer le canal alpha sur Mac OS X à l’aide de la **aperçu** application :

1. Ouvrir l’image d’icône dans **aperçu** , puis **fichier > Exporter**.

2. La boîte de dialogue qui apparaît inclut un **Alpha** case à cocher si un canal alpha est présent.

    ![](troubleshooting-images/remove-alpha-sml.png "La boîte de dialogue qui apparaît inclut une case à cocher Alpha s’il existe un canal alpha")

3. *Untick* le **Alpha** case à cocher et **enregistrer** le fichier à l’emplacement correct.

4. L’image d’icône doit passer maintenant les contrôles de validation d’Apple.


<a name="add" />

## <a name="manually-adding-interface-controller-files"></a>Ajoutez manuellement les fichiers de contrôleur d’Interface

> [!IMPORTANT]
> Prise en charge de Xamarin WatchKit inclut la conception d’animations espion dans le concepteur iOS (dans Visual Studio pour Mac et Visual Studio), qui ne nécessite pas les étapes décrites ci-dessous. Simplement permettre à un contrôleur d’interface un nom de classe dans Visual Studio pour Mac propriétés remplissage et les fichiers de code c# seront créés automatiquement.


*Si* à l’aide de Xcode Interface Builder, procédez comme suit pour créer de nouveaux contrôleurs d’interface pour votre application de surveillance et activer la synchronisation avec Xcode afin que les sorties et les actions sont disponibles en c# :

1. Ouvrez l’application de surveillance **Interface.storyboard** dans **Xcode Interface Builder**.
    
    ![](troubleshooting-images/add-6.png "Ouvrir le plan conceptuel dans Xcode Interface Builder")

2. Faites glisser une nouvelle `InterfaceController` sur le plan conceptuel :

    ![](troubleshooting-images/add-1.png "Un InterfaceController")

3. Vous pouvez maintenant faire glisser des contrôles sur le contrôleur d’interface (par exemple). les étiquettes et les boutons) mais vous ne peut pas créer prises ou actions encore, étant donné qu’aucun **.h** fichier d’en-tête. Les étapes suivantes provoquera requis **.h** fichier d’en-tête doit être créé.

    ![](troubleshooting-images/add-2.png "Un bouton dans la disposition")

4. Fermez le plan conceptuel et revenez à Visual Studio pour Mac. Créer un nouveau fichier c# **MyInterfaceController.cs** (ou le nom que vous le souhaitez) dans le **regarder l’extension d’application** projet (pas l’application espion où le plan conceptuel est). Ajoutez le code suivant (mise à jour de l’espace de noms, nom de classe et le nom du constructeur) :

        using System;
        using WatchKit;
        using Foundation;
        
        namespace WatchAppExtension  // remember to update this
        {
            public partial class MyInterfaceController // remember to update this
            : WKInterfaceController
            {
                public MyInterfaceController // remember to update this
                (IntPtr handle) : base (handle)
                {
                }
                public override void Awake (NSObject context)
                {
                    base.Awake (context);
                    // Configure interface objects here.
                    Console.WriteLine ("{0} awake with context", this);
                }
                public override void WillActivate ()
                {
                    // This method is called when the watch view controller is about to be visible to the user.
                    Console.WriteLine ("{0} will activate", this);
                }
                public override void DidDeactivate ()
                {
                    // This method is called when the watch view controller is no longer visible to the user.
                    Console.WriteLine ("{0} did deactivate", this);
                }
            }
        }

5. Créer un nouveau fichier c# **MyInterfaceController.designer.cs** dans les **regarder l’extension d’application** de projet et ajoutez le code ci-dessous. Veillez à mettre à jour de l’espace de noms, le nom de classe et le `Register` attribut :

    ```csharp
    using Foundation;
    using System.CodeDom.Compiler;
    
    namespace HelloWatchExtension  // remember to update this
    {
        [Register ("MyInterfaceController")] // remember to update this
        partial class MyInterfaceController  // remember to update this
        {
            void ReleaseDesignerOutlets ()
            {
            }
        }
    }
    ```
    
    Conseil : Vous pouvez (facultatif) faire de ce fichier un nœud enfant du premier fichier en le faisant glisser sur l’autre fichier c# dans Visual Studio pour Mac Solution remplissage. Il apparaît comme suit :
    
    ![](troubleshooting-images/add-5.png "Le remplissage de la Solution")

6. Sélectionnez **Générer > générer tout** afin que la synchronisation de Xcode reconnaîtra la nouvelle classe (via la `Register` attribut) que nous avons utilisés.

7. Ouvrez à nouveau la table de montage séquentiel en cliquant sur le fichier d’animation application espion et en sélectionnant **ouvrir avec > Xcode Interface Builder**:

    ![](troubleshooting-images/add-6.png "Ouverture de la table de montage séquentiel dans le constructeur d’Interface")

8. Sélectionnez votre nouveau contrôleur de l’interface et la classe définie ci-dessus, par exemple. `MyInterfaceController`.
Si tout a fonctionné correctement, il doit apparaître automatiquement dans le **classe :** zone de liste déroulante et vous pouvez le sélectionner à partir de là.

    ![](troubleshooting-images/add-4.png "Définition d’une classe personnalisée")

9. Choisissez le **Assistant éditeur** afficher dans Xcode (l’icône avec deux cercles se chevauchant) afin que vous puissiez voir le plan conceptuel et le code côte à côte :

    ![](troubleshooting-images/add-7.png "L’élément de barre d’outils Éditeur de l’Assistant")

    Lorsque le focus est dans le volet du code, assurez-vous que vous êtes examiner le **.h** fichier d’en-tête et pas avec le bouton droit dans la barre de navigation et sélectionnez le fichier approprié (**MyInterfaceController.h**)

    ![](troubleshooting-images/add-8.png "Sélectionnez MyInterfaceController")

10. Vous pouvez désormais créer des points de vente et les actions par **Ctrl + glisser** à partir de la table de montage séquentiel dans le **.h** fichier d’en-tête.

    ![](troubleshooting-images/add-9.png "Création de points de vente et les actions")

    Lorsque vous relâchez l’opération glisser, vous êtes invité à sélectionner si vous souhaitez créer une prise de courant ou une action, choisissez son nom :

    ![](troubleshooting-images/add-a.png "La sortie et une boîte de dialogue d’action")

11. Une fois les modifications de la table de montage séquentiel sont enregistrées et Xcode est fermé, retournez dans Visual Studio pour Mac. Détecte les modifications de fichier d’en-tête et ajouter automatiquement le code pour le **. designer.cs** fichier :


        [Register ("MyInterfaceController")]
        partial class MyInterfaceController
        {
            [Outlet]
            WatchKit.WKInterfaceButton myButton { get; set; }
        
            void ReleaseDesignerOutlets ()
            {
                if (myButton != null) {
                    myButton.Dispose ();
                    myButton = null;
                }
            }
        }


Vous pouvez maintenant référencer le contrôle (ou l’implémentation de l’action) en c# !


<a name="command_line" />

## <a name="launching-the-watch-app-from-the-command-line"></a>Lancement de l’application de surveillance à partir de la ligne de commande

> [!IMPORTANT]
> Vous pouvez démarrer l’application de surveillance en mode d’application normal par défaut, ainsi que dans **aperçu** ou **Notification** à l’aide des modes [les paramètres d’exécution personnalisés](~/ios/watchos/get-started/installation.md#custommodes) dans Visual Studio pour Mac et Visual Studio.


Vous pouvez également utiliser la ligne de commande pour contrôler le simulateur iOS. L’outil de ligne de commande utilisée pour lancer des applications de surveillance est **mtouch**.

Voici un exemple complet (exécuté en tant qu’une seule ligne dans le terminal) :

```bash
/Library/Frameworks/Xamarin.iOS.framework/Versions/Current/bin/mtouch --sdkroot=/Applications/Xcode.app/Contents/Developer/ --device=:v2:runtime=com.apple.CoreSimulator.SimRuntime.iOS-8-2,devicetype=com.apple.CoreSimulator.SimDeviceType.iPhone-6
--launchsimwatch=/path/to/watchkitproject/watchsample/bin/iPhoneSimulator/Debug/watchsample.app
```

Le paramètre que vous devez mettre à jour pour refléter votre application est `launchsimwatch`:

### <a name="--launchsimwatch"></a>--launchsimwatch

Le chemin d’accès complet à l’offre groupée d’application principal *de l’application iOS qui contient l’application de surveillance et l’extension*.

> [!NOTE]
> Le chemin d’accès que vous deviez fournir concerne le *fichier .app de l’application iPhone*, c'est-à-dire celui qui sera déployé sur le simulateur iOS et qui contient l’extension de surveillance et l’application watch.

Exemple :

```bash
--launchsimwatch=/path/to/watchkitproject/watchsample/bin/iPhoneSimulator/Debug/watchsample.app
```


## <a name="notification-mode"></a>Mode de notification

Pour tester l’application [ **Notification** mode](~/ios/watchos/platform/notifications.md), définissez le `watchlaunchmode` paramètre `Notification` et fournir un chemin d’accès à un fichier JSON qui contient une notification de test de charge utile.

Le paramètre de la charge utile est *requis* pour le mode de Notification.

Par exemple, ajoutez ces arguments à la commande mtouch :

```bash
--watchlaunchmode=Notification --watchnotificationpayload=/path/to/file.json
```


## <a name="other-arguments"></a>Autres Arguments

Les arguments restants sont expliquées ci-dessous :

### <a name="--sdkroot"></a>--sdkroot

Obligatoire. Spécifie le chemin d’accès à Xcode (6.2 ou version ultérieure).

Exemple :

```bash
 --sdkroot /Applications/Xcode.app/Contents/Developer/
```

### <a name="--device"></a>--APPAREIL

L’appareil de simulateur à exécuter. Cela peut être spécifié de deux manières, à l’aide de l’udid d’un périphérique spécifique, ou à l’aide d’une combinaison du type de runtime et d’appareils.

Les valeurs exactes varie entre les ordinateurs et peuvent être interrogées à l’aide d’Apple **simctl** outil :

```bash
/Applications/Xcode.app/Contents/Developer/usr/bin/simctl list
```

**UDID**

Exemple :

```bash
--device=:v2:udid=AAAAAAA-BBBB-CCCC-DDDD-EEEEEEEEEEEE
```

**Type de runtime et de périphérique**

Exemple :

```bash
--device=:v2:runtime=com.apple.CoreSimulator.SimRuntime.iOS-8-2,devicetype=com.apple.CoreSimulator.SimDeviceType.iPhone-6
```



## <a name="related-links"></a>Liens associés

- [WatchKitCatalog (exemple)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [WatchTables (exemple)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchTables/)
