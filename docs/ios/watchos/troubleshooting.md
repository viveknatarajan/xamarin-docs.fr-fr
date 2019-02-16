---
title: watchOS résolution des problèmes
description: Ce document aborde les problèmes connus et solutions de contournement pour le développement de watchOS avec Xamarin. Il décrit les images avec des problèmes, l’ajout manuel de fichiers de contrôleur d’interface, lancement d’une application watch à partir de la ligne de commande et bien plus encore.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 27C31DB8-451E-4888-BBC1-CE0DFC2F9DEC
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 70ef341c066c77e214761d75c173faef00266e4c
ms.sourcegitcommit: 2713f2c1d74e3582704c3d0ca65b6651119ed489
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/15/2019
ms.locfileid: "56321153"
---
# <a name="watchos-troubleshooting"></a>watchOS résolution des problèmes

Cette page contient des informations supplémentaires et des solutions de contournement pour les problèmes que vous pouvez rencontrer.

- [Problèmes connus](#knownissues)

- [Suppression du canal Alpha à partir d’Images d’icône](#noalpha)

- [Ajout manuel de fichiers de contrôleur d’Interface](#add) pour Xcode Interface Builder.

- [Lancement de la WatchApp à partir de la ligne de commande](#command_line).

<a name="knownissues" />

## <a name="known-issues"></a>Problèmes connus

### <a name="general"></a>Général

<a name="deploy" />

- Les versions antérieures de Visual Studio pour Mac incorrectement affichent l’une de la **AppleCompanionSettings** icônes comme étant 88 x 88 pixels ; ce qui entraîne un **erreur d’icône manquant** si vous essayez d’envoyer à l’App Store.
    Cette icône doit être 87 x 87 pixels (29 unités pour **@3x** les écrans rétine). Vous ne pouvez pas résoudre ce problème dans Visual Studio pour Mac - modifier la ressource image dans Xcode ou modifier manuellement le **Contents.json** fichier (pour correspondre à [cet exemple](https://github.com/xamarin/monotouch-samples/blob/master/WatchKit/WatchKitCatalog/WatchApp/Resources/Images.xcassets/AppIcons.appiconset/Contents.json#L126-L132)).

- Si le projet d’espion Extension **Info.plist > ID d’ensemble WKApp** n’est pas [correctement définie](~/ios/watchos/get-started/project-references.md) pour correspondre à l’application Watch **ID d’offre groupée**, le débogueur ne parviendra pas à se connecter et Visual Studio pour Mac devront attendre avec le message *« En attente pour la connexion du débogueur »*.

- Le débogage est pris en charge dans **Notifications** mode mais peuvent être incertains. Une nouvelle tentative fonctionne parfois. Vérifiez que l’application Watch **Info.plist** `WKCompanionAppBundleIdentifier` est redéfini pour correspondre à l’identificateur de bundle de l’application de conteneur/parent iOS (ie. celui qui s’exécute sur l’iPhone).

- Concepteur iOS n’affiche pas les flèches de point d’entrée pour les contrôleurs d’interface coup de œil ou de Notification.

- Vous ne pouvez pas ajouter deux `WKNotificationControllers` de montage.
    Solution de contournement : Le `notificationCategory` dans le storyboard XML est toujours inséré avec la même `id`. Pour contourner ce problème, vous pouvez ajouter deux (ou plus) contrôleurs de Notification, ouvrez le fichier d’animation dans un éditeur de texte et modifiez manuellement le `id` élément unique.

    [![](troubleshooting-images/duplicate-id-sml.png "Ouvrir le fichier de storyboard dans un éditeur de texte et modifiez manuellement l’élément id pour être unique")](troubleshooting-images/duplicate-id.png#lightbox)

- Vous pouvez rencontrer une erreur « l’application n’a pas été générée » lorsque vous tentez de lancer l’application. Cela se produit après un **Clean** quand le projet de démarrage est défini sur le projet d’extension watch.
    La solution consiste à sélectionner **Générer > tout reconstruire** , puis relancez l’application.

### <a name="visual-studio"></a>Visual Studio

Le concepteur iOS prend en charge pour le Kit espion *requiert* la solution configurée correctement. Si les références de projet ne sont pas définies (consultez [comment définir les références](~/ios/watchos/get-started/project-references.md)) l’aire de conception ne fonctionnera pas correctement.

<a name="noalpha" />

## <a name="removing-the-alpha-channel-from-icon-images"></a>Suppression du canal Alpha à partir d’Images d’icône

Icônes ne doivent pas contenir un canal alpha (le canal alpha définit des zones transparentes d’une image), sinon l’application est rejetée pendant la soumission App Store avec une erreur similaire à ceci :

```csharp
Invalid Icon - The watch application '...watchkitextension.appex/WatchApp.app'
contains an icon file '...watchkitextension.appex/WatchApp.app/Icon-27.5@2x.png'
with an alpha channel. Icons should not have an alpha channel.
```

Il est facile de supprimer le canal alpha sur Mac OS X à l’aide de la **aperçu** application :

1. Ouvrir l’image d’icône dans **aperçu** , puis **fichier > Exporter**.

2. La boîte de dialogue qui apparaît inclut un **Alpha** case à cocher si un canal alpha est présent.

    ![](troubleshooting-images/remove-alpha-sml.png "La boîte de dialogue qui apparaît inclut une case à cocher Alpha s’il existe un canal alpha")

3. *Untick* le **Alpha** case à cocher et **enregistrer** le fichier vers l’emplacement correct.

4. L’image d’icône devrait maintenant fonctionner des contrôles de validation d’Apple.


<a name="add" />

## <a name="manually-adding-interface-controller-files"></a>Ajout manuel de fichiers de contrôleur d’Interface

> [!IMPORTANT]
> Prise en charge de la WatchKit de Xamarin inclut la conception des storyboards espion dans le concepteur iOS (dans Visual Studio pour Mac et Visual Studio), qui ne nécessite pas les étapes décrites ci-dessous. Simplement donner à un contrôleur d’interface un nom de classe dans Visual Studio pour Mac propriétés de remplissage et la C# fichiers de code sont créés automatiquement.


*Si* que vous utilisez Xcode Interface Builder, suivez ces étapes pour créer de nouveaux contrôleurs d’interface pour votre application watch et activer la synchronisation avec Xcode afin que les prises de courant et les actions sont disponibles dans C#:

1. Ouvrez l’application watch **Interface.storyboard** dans **Xcode Interface Builder**.
    
    ![](troubleshooting-images/add-6.png "Ouvrir le fichier de storyboard dans Xcode Interface Builder")

2. Faites glisser une nouvelle `InterfaceController` sur le plan conceptuel :

    ![](troubleshooting-images/add-1.png "A InterfaceController")

3. Vous pouvez maintenant faire glisser des contrôles sur le contrôleur d’interface (par exemple). étiquettes et les boutons), mais Impossible de créer des prises de courant ou actions encore, car il n’est pas **.h** fichier d’en-tête. Les étapes suivantes entraîne requis **.h** fichier d’en-tête doit être créé.

    ![](troubleshooting-images/add-2.png "Un bouton dans la disposition")

4. Fermez le plan conceptuel et retourner à Visual Studio pour Mac. Créer un nouveau C# fichier **MyInterfaceController.cs** (ou le nom que vous le souhaitez) dans le **regarder l’extension d’application** projet (pas l’application watch elle-même où la table de montage séquentiel est). Ajoutez le code suivant (mise à jour de l’espace de noms, nom de classe et le nom du constructeur) :

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

5. Créez une autre nouvelle C# fichier **MyInterfaceController.designer.cs** dans le **regarder l’extension d’application** de projet et ajoutez le code ci-dessous. Veillez à mettre à jour de l’espace de noms, le nom de classe et le `Register` attribut :

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
    
    Conseil : Vous pouvez (facultativement) faire de ce fichier un nœud enfant du premier fichier en le faisant glisser sur l’autre C# fichier dans Visual Studio pour Mac Solution Pad. Il s’affiche alors comme suit :
    
    ![](troubleshooting-images/add-5.png "Le panneau Solution")

6. Sélectionnez **Générer > générer tout** afin que la synchronisation de Xcode reconnaîtra la nouvelle classe (via le `Register` attribut) que nous avons utilisé.

7. Ouvrez à nouveau la table de montage séquentiel en effectuant un clic droit sur le fichier de storyboard application watch et en sélectionnant **ouvrir avec > Xcode Interface Builder**:

    ![](troubleshooting-images/add-6.png "Ouverture de la table de montage séquentiel dans le Générateur d’Interface")

8. Sélectionnez votre nouveau contrôleur d’interface et lui donner le nom de classe défini ci-dessus, par ex. `MyInterfaceController`.
Si tout a fonctionné correctement, il doit apparaître automatiquement dans le **classe :** liste déroulante et vous pouvez le sélectionner à partir de là.

    ![](troubleshooting-images/add-4.png "Définition d’une classe personnalisée")

9. Choisissez le **éditeur de l’Assistant** afficher dans Xcode (l’icône avec deux cercles se chevauchant) afin que vous puissiez voir le plan conceptuel et le code côte à côte :

    ![](troubleshooting-images/add-7.png "L’élément de barre d’outils Éditeur de l’Assistant")

    Lorsque le focus est dans le volet du code, assurez-vous que vous êtes examiner le **.h** fichier d’en-tête et si pas avec le bouton droit dans la barre de navigation et sélectionner le fichier approprié (**MyInterfaceController.h**)

    ![](troubleshooting-images/add-8.png "Sélectionnez MyInterfaceController")

10. Vous pouvez désormais créer des outlets et actions par **Ctrl + glisser** à partir de la table de montage séquentiel dans le **.h** fichier d’en-tête.

    ![](troubleshooting-images/add-9.png "Création des outlets et actions")

    Lorsque vous relâchez le glisser-déplacer, vous serez invité à sélectionner si vous souhaitez créer un outlet ou une action, choisissez son nom :

    ![](troubleshooting-images/add-a.png "La sortie et une boîte de dialogue action")

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


Vous pouvez maintenant référencer le contrôle (ou implémenter l’action) dans C#!


<a name="command_line" />

## <a name="launching-the-watch-app-from-the-command-line"></a>Lancement de l’application Apple Watch à partir de la ligne de commande

> [!IMPORTANT]
> Vous pouvez démarrer l’application de surveillance en mode d’application normal par défaut, ainsi que dans **aperçu** ou **Notification** à l’aide des modes [les paramètres d’exécution personnalisée](~/ios/watchos/get-started/installation.md#custommodes) dans Visual Studio pour Mac et Visual Studio.


Vous pouvez également utiliser la ligne de commande pour contrôler le simulateur iOS. L’outil de ligne de commande utilisée pour lancer des applications cadran est **mtouch**.

Voici un exemple complet (exécuté en tant qu’une seule ligne dans le terminal) :

```bash
/Library/Frameworks/Xamarin.iOS.framework/Versions/Current/bin/mtouch --sdkroot=/Applications/Xcode.app/Contents/Developer/ --device=:v2:runtime=com.apple.CoreSimulator.SimRuntime.iOS-8-2,devicetype=com.apple.CoreSimulator.SimDeviceType.iPhone-6
--launchsimwatch=/path/to/watchkitproject/watchsample/bin/iPhoneSimulator/Debug/watchsample.app
```

Le paramètre que vous devez mettre à jour pour refléter votre application est `launchsimwatch`:

### <a name="--launchsimwatch"></a>--launchsimwatch

Le chemin complet vers le bundle d’applications principal *pour l’application iOS qui contient l’application watch et une extension*.

> [!NOTE]
> Le chemin d’accès, vous devez fournir concerne le *fichier .app de l’application iPhone*, c'est-à-dire, celle qui sera déployée sur le simulateur iOS et qui contient l’application de surveillance et l’extension watch.

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

### <a name="--device"></a>--device

L’appareil de simulateur à exécuter. Cela peut être spécifié de deux manières, à l’aide de l’udid d’un appareil spécifique, ou à l’aide d’une combinaison de type de runtime et d’appareils.

Les valeurs exactes varie entre les machines et peuvent être interrogées à l’aide d’Apple **simctl** outil :

```bash
/Applications/Xcode.app/Contents/Developer/usr/bin/simctl list
```

**UDID**

Exemple :

```bash
--device=:v2:udid=AAAAAAA-BBBB-CCCC-DDDD-EEEEEEEEEEEE
```

**Type de runtime et appareil**

Exemple :

```bash
--device=:v2:runtime=com.apple.CoreSimulator.SimRuntime.iOS-8-2,devicetype=com.apple.CoreSimulator.SimDeviceType.iPhone-6
```



## <a name="related-links"></a>Liens associés

- [WatchKitCatalog (exemple)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [WatchTables (exemple)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchTables/)
