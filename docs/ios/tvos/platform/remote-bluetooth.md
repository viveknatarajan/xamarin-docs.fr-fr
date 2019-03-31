---
title: Siri Remote et Bluetooth des contrôleurs de tvOS dans Xamarin
description: Cet article décrit comment travailler avec les Siri Remote et les contrôleurs de jeu Bluetooth dans les applications tvOS écrites avec Xamarin.
ms.prod: xamarin
ms.assetid: BDB9894A-236B-424B-9032-ACD12A6C5720
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 79022f7a454ea423fa3112a4c4ade2bcd471fbb8
ms.sourcegitcommit: 946ce514fd6575aa6b93ff24181e02a60b24b106
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/30/2019
ms.locfileid: "58677948"
---
# <a name="siri-remote-and-bluetooth-controllers-for-tvos-in-xamarin"></a>Siri Remote et Bluetooth des contrôleurs de tvOS dans Xamarin

Les utilisateurs de votre application Xamarin.tvOS pas d’interagir avec son interface directement en tant qu’avec iOS où ils appuyer sur les images sur l’écran du périphérique, mais indirectement à partir sur la place à l’aide du [Siri Remote](#The-Siri-Remote).

Si votre application est un jeu, vous pouvez éventuellement générer dans prise en charge pour le tiers 3e, effectuées pour iOS (IFM) [contrôleurs de jeu Bluetooth](#Bluetooth-Game-Controllers) dans votre application.

[![](remote-bluetooth-images/intro01.png "Le Bluetooth distant et le contrôleur de jeu")](remote-bluetooth-images/intro01.png#lightbox)

Cet article décrit le [Siri Remote](#The-Siri-Remote), [Touch les mouvements de Surface](#Touch-Surface-Gestures) et [boutons distant Siri](#Siri-Remote-Buttons) et montre comment travailler avec eux via [mouvements et Storyboards](#Gestures-and-Storyboards), [mouvements et Code](#Gestures-and-Code) et [gestion des événements de bas niveau](#Low-Level-Event-Handling). Enfin, il aborde [utilisation de contrôleurs de jeu](#Working-with-Game-Controllers) dans une application Xamarin.tvOS.

<a name="The-Siri-Remote" />

## <a name="the-siri-remote"></a>L’instance distante de Siri

Est le moyen principal que les utilisateurs d’interagir avec l’Apple TV et votre application Xamarin.tvOS, distant Siri inclus. Apple conçu la télécommande pour combler la distance entre l’utilisateur assis sur le canapé et l’interface utilisateur des téléviseurs Apple affiché à travers la pièce sur l’écran.

Votre défi en tant qu’un développeur d’application tvOS est la créer une interface utilisateur visuellement attrayante et rapide, facile à utiliser qui s’appuie sur la surface de tactile de le Siri Remote, accéléromètre, gyroscope et les boutons.

[![](remote-bluetooth-images/remote01.png "L’instance distante de Siri")](remote-bluetooth-images/remote01.png#lightbox)

Siri distant a des utilisations attendues au sein de votre application tvOS et les fonctionnalités suivantes :

|Fonctionnalité|Utilisation des applications générales|L’utilisation des applications de jeux|
|---|---|---|
|**Touchez Surface**<br />Balayez d’accéder, appuyez sur pour sélectionner et de conserver les menus contextuels.|**Appuyez sur/balayage**<br />Navigation de l’interface utilisateur entre les éléments pouvant être actif.<br /><br />**Cliquez sur**<br />Active (actif) sélectionnée.|**Appuyez sur/balayage**<br />Varie selon la conception de jeux et peut être utilisé comme un pavé directionnel en appuyant sur les bords.<br /><br />**Cliquez sur**<br />Effectuer la fonction du bouton principal.|
|**Menu**<br />Appuyez sur pour revenir à l’écran précédent ou de menu.|Retourne à l’écran précédent et se termine à l’écran d’accueil TV Apple à partir de l’écran de l’application principale.|Suspendre et reprendre le jeu, retourne à l’écran précédent et se termine à l’écran d’accueil TV Apple à partir de l’écran de l’application principale.|
|**Siri/recherche**<br />Dans les pays Siri, appuyez et maintenez pour le contrôle vocal, dans tous les autres pays, affiche d’écran de recherche.|N/A|N/A|
|**Lecture/Pause**<br />Lecture et Pause du média ou fournit une fonction secondaire dans les applications.|Démarre la lecture du média et suspendre/reprendre la lecture.|Effectue la fonction de bouton secondaire ou ignore la vidéo de présentation (s’il existe).|
|**Accueil**<br />Appuyez sur pour revenir à l’écran d’accueil, double-cliquez dessus pour afficher les applications en cours d’exécution, maintenez en veille d’appareil.|N/A|N/A|
|**Volume**<br />Contrôles attaché volume d’équipements audio/vidéo.|N/A|N/A|

<a name="Touch-Surface-Gestures" />

## <a name="touch-surface-gestures"></a>Aire de conception gestes tactiles

Touch Surface de la Siri Remote est en mesure de détecter une variété de mouvements d’un seul doigt auquel vous pouvez répondre dans votre application Xamarin.tvOS :

|Effectuez un balayage|Clic|Appuyez sur|
|---|---|---|
|![](remote-bluetooth-images/Gesture01.png)|![](remote-bluetooth-images/Gesture02.png)|![](remote-bluetooth-images/Gesture03.png)|
|Déplace la sélection (focus) entre les éléments d’interface utilisateur sur l’écran (haut, bas à gauche, avec le bouton droit). Balayant peut être utilisé pour faire défiler les grandes listes de contenu rapidement à l’aide de l’inertie.|Active l’élément sélectionné (actif) ou joue le rôle du bouton principal dans un jeu. Cliquez et maintenez peuvent activer des menus contextuels ou les fonctions secondaires.|À la légère en appuyant sur la Surface tactile sur les bords agit comme des boutons directionnels sur un boîtier, déplacer le focus, haut, bas, gauche ou droite en fonction de la zone de l’appui sur. Selon l’application, peut être utilisé pour afficher les contrôles masqués.|

Apple propose les suggestions suivantes pour l’utilisation de gestes de Surface Touch :

* **Faire la distinction entre les clics et appuie sur** -en cliquant sur une action intentionnelle par l’utilisateur et est bien adapté à la sélection, l’activation et le bouton principal d’un jeu. En appuyant sur est plus subtile et doit être utilisé avec parcimonie, car l’utilisateur maintient souvent Siri distant dans la main et peut activer accidentellement un événement Tap facilement.
* **Ne pas redéfinir les mouvements Standard** -l’utilisateur a une attente que les gestes spécifiques effectuera les actions spécifiques, vous ne devez pas redéfinir la signification ou la fonction de ces mouvements dans votre application. La seule exception concerne une application de jeu pendant jeu actif.
* **Définir de nouveaux mouvements avec parcimonie** -là encore, l’utilisateur a une attente que les gestes spécifiques effectuera des actions spécifiques. Vous devez éviter de définir des mouvements personnalisés pour effectuer des actions standards. Et là encore, jeux sont l’exception la plus courante où des mouvements personnalisés peuvent ajouter play agréable et immersive au jeu.
* **Le cas échéant, répondre aux appuie sur le pavé directionnel** - légèrement en appuyant sur l’angle de bords de la Surface Touch seront réagir comme un pavé directionnel sur un déplacer le focus de contrôleur de jeu ou la direction vers le haut, bas, gauche ou vers la droite. Le cas échéant, vous devez répondre à ces mouvements dans votre application ou votre jeu.

<a name="Siri-Remote-Buttons" />

## <a name="siri-remote-buttons"></a>Boutons de Siri Remote

En plus des gestes sur la Surface tactile, votre application peut répondre à l’utilisateur en cliquant sur l’aire de Touch ou en appuyant sur le bouton de lecture/Pause. Si vous accédez à la télécommande Siri à l’aide de l’infrastructure de contrôleur de jeu, vous pouvez également détecter le comportement du bouton de Menu. 

En outre, les activations de bouton de menu peuvent être détectées à l’aide d’un module de reconnaissance de mouvement avec standard `UIKit` éléments. Si vous intercepter le comportement du bouton de Menu, responsable de la fermeture de la vue et le contrôleur d’affichage actuel et vous renvoyer à la précédente.

> [!IMPORTANT]
> Vous devez **toujours** affecter une fonction au bouton Lecture/Pause sur l’instance distante. Avoir un bouton non fonctionnels peut rendre votre application altérée à l’utilisateur final. Si vous n’avez pas une fonction valide pour ce bouton, affectez la même fonction que le bouton principal (toucher la cliquez sur Surface).

<a name="Gestures-and-Storyboards" />

## <a name="gestures-and-storyboards"></a>Mouvements et Storyboards

Le moyen le plus simple pour travailler avec le Siri Remote dans votre application Xamarin.tvOS consiste à ajouter des modules de reconnaissance de mouvement à vos vues dans le Concepteur d’Interface.

Pour ajouter un module de reconnaissance de gestes, procédez comme suit :

1. Dans le **l’Explorateur de solutions**, double-cliquez sur le `Main.storyboard` et ouvrez-le pour le Concepteur d’Interface de modification.
2. Faites glisser un **appuyez sur la reconnaissance de mouvement** à partir de la **bibliothèque** et déposez-la sur la vue : 

    [![](remote-bluetooth-images/storyboard01.png "Un module de reconnaissance de mouvement Tap")](remote-bluetooth-images/storyboard01.png#lightbox)
3. Vérifiez **sélectionnez** dans le **bouton** section de la **inspecteur d’attributs**: 

    [![](remote-bluetooth-images/storyboard02.png "Vérifiez la sélection")](remote-bluetooth-images/storyboard02.png#lightbox)
4. **Sélectionnez** signifie que le mouvement répondra à l’utilisateur clique sur le **Surface Touch** Siri à un emplacement distant. Vous avez également la possibilité de répondre à la **Menu**, **Lecture/Pause**, **des**, **vers le bas**, **gauche** et **Droite** boutons.
5. Ensuite, associer un **Action** à partir de la **appuyez sur la reconnaissance de mouvement** et appelez-le `TouchSurfaceClicked`: 

    [![](remote-bluetooth-images/storyboard03.png "Une Action à partir de la reconnaissance de mouvement Tap")](remote-bluetooth-images/storyboard03.png#lightbox)
6. Enregistrez vos modifications et revenir à Visual Studio pour Mac.

Modifier votre contrôleur d’affichage (exemple `FirstViewController.cs`) et ajoutez le code suivant pour gérer le mouvement de déclenchement :

```csharp
using System;
using UIKit;

namespace tvRemote
{
    public partial class FirstViewController : UIViewController
    {
        ...

        #region Custom Actions
        partial void TouchSurfaceClicked (Foundation.NSObject sender) {
            // Handle click here
            ...
        }
        #endregion
    }
}
```

Pour plus d’informations sur l’utilisation des tables de montage séquentiel, consultez notre [Hello, tvOS Guide de démarrage rapide](~/ios/tvos/get-started/hello-tvos.md). Plus précisément le [création de l’Interface utilisateur](~/ios/tvos/get-started/hello-tvos.md#Creating-the-User-Interface) et [écriture du Code avec les outlets et actions](~/ios/tvos/get-started/hello-tvos.md#Writing-the-Code) sections.

<a name="Gestures-and-Code" />

## <a name="gestures-and-code"></a>Mouvements et Code

Si vous le souhaitez, vous pouvez créer des mouvements directement dans C# de code et de les ajouter à des vues dans votre Interface utilisateur. Par exemple, pour ajouter une série de modules de reconnaissance de mouvement de balayage, modifier votre contrôleur d’affichage et ajoutez le code suivant :

```csharp
using System;
using UIKit;

namespace tvRemote
{
    public partial class SecondViewController : UIViewController
    {
        #region Constructors
        public SecondViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();    

            // Wire-up gestures
            var upGesture = new UISwipeGestureRecognizer (() => {
                RemoteView.ArrowPressed = "Up";
                ButtonLabel.Text = "Swiped Up";
            }) {
                Direction = UISwipeGestureRecognizerDirection.Up
            };
            this.View.AddGestureRecognizer (upGesture);

            var downGesture = new UISwipeGestureRecognizer (() => {
                RemoteView.ArrowPressed = "Down";
                ButtonLabel.Text = "Swiped Down";
            }) {
                Direction = UISwipeGestureRecognizerDirection.Down
            };
            this.View.AddGestureRecognizer (downGesture);

            var leftGesture = new UISwipeGestureRecognizer (() => {
                RemoteView.ArrowPressed = "Left";
                ButtonLabel.Text = "Swiped Left";
            }) {
                Direction = UISwipeGestureRecognizerDirection.Left
            };
            this.View.AddGestureRecognizer (leftGesture);

            var rightGesture = new UISwipeGestureRecognizer (() => {
                RemoteView.ArrowPressed = "Right";
                ButtonLabel.Text = "Swiped Right";
            }) {
                Direction = UISwipeGestureRecognizerDirection.Right
            };
            this.View.AddGestureRecognizer (rightGesture);
        }
        #endregion
    }
}
```

<a name="Low-Level-Event-Handling" />

## <a name="low-level-event-handling"></a>Gestion des événements de bas niveau

Si vous créez un type personnalisé en fonction de `UIKit` dans votre application Xamarin.tvOS (par exemple `UIView`), vous avez également la possibilité de fournir une gestion de bas niveau de sur le bouton via `UIPress` événements. 

Un `UIPress` événement est à tvOS qu’un `UITouch` événement consiste à iOS, à l’exception `UIPress` retourne plus d’informations sur le bouton appuie sur l’instance distante de Siri ou autre attaché périphériques Bluetooth (par exemple, un contrôleur de jeu). `UIPress` événements décrivent le comportement du bouton et son état (Began, Canceled, a été modifié ou terminé). 

Pour les boutons analogique sur des appareils tels que les contrôleurs de jeu de Bluetooth, `UIPress` retourne également la quantité de force appliqué au bouton. Le `Type` propriété de la `UIPress` événement définit quel bouton physique a un état modifié, pendant que le reste des propriétés décrivent la modification s’est produite.

Le code suivant montre un exemple de gestion de bas niveau `UIPress` événements pour un `UIView`:

```csharp
using System;
using Foundation;
using UIKit;

namespace tvRemote
{
    public partial class EventView : UIView
    {
        #region Computed Properties
        public override bool CanBecomeFocused {
            get {
                return true;
            }
        }
        #endregion

        #region 
        public EventView (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void PressesBegan (NSSet<UIPress> presses, UIPressesEvent evt)
        {
            base.PressesBegan (presses, evt);

            foreach (UIPress press in presses) {
                // Was the Touch Surface clicked?
                if (press.Type == UIPressType.Select) {
                    BackgroundColor = UIColor.Red;
                }
            }
        }

        public override void PressesCancelled (NSSet<UIPress> presses, UIPressesEvent evt)
        {
            base.PressesCancelled (presses, evt);

            foreach (UIPress press in presses) {
                // Was the Touch Surface clicked?
                if (press.Type == UIPressType.Select) {
                    BackgroundColor = UIColor.Clear;
                }
            }
        }

        public override void PressesChanged (NSSet<UIPress> presses, UIPressesEvent evt)
        {
            base.PressesChanged (presses, evt);
        }

        public override void PressesEnded (NSSet<UIPress> presses, UIPressesEvent evt)
        {
            base.PressesEnded (presses, evt);

            foreach (UIPress press in presses) {
                // Was the Touch Surface clicked?
                if (press.Type == UIPressType.Select) {
                    BackgroundColor = UIColor.Clear;
                }
            }
        }
        #endregion
    }
}
```

Comme avec `UITouch` événements, si vous avez besoin de mettre en œuvre toutes les `UIPress` remplacements d’événements, vous devez implémenter les quatre.

<a name="Bluetooth-Game-Controllers" />

## <a name="bluetooth-game-controllers"></a>Contrôleurs de jeu de Bluetooth

En plus de l’instance distante Siri standard qui est fourni avec l’Apple TV, 3ème partie, effectuées pour iOS, contrôleurs de jeu de Bluetooth (IFM) peut être associés à l’Apple TV et utilisés pour contrôler votre application Xamarin.tvOS.

[![](remote-bluetooth-images/game01.png "Contrôleurs de jeu de Bluetooth")](remote-bluetooth-images/game01.png#lightbox)

Contrôleurs de jeu peuvent être utilisés pour améliorer le jeu et le sentiment d’immersion dans un jeu. Ils peuvent également servir à contrôler l’interface de Apple TV standard pour l’utilisation n’avoir à basculer entre l’instance distante et le contrôleur.

> [!IMPORTANT]
> Les contrôleurs de jeu Bluetooth sont un bon facultatif risquant de rendre les utilisateurs finaux, votre application ne peut pas forcer l’utilisateur à acheter un. Si votre application prend en charge les contrôleurs de jeu il doit également prendre en charge distant Siri afin que le jeu soit utilisable par tous les utilisateurs d’Apple TV.

Un contrôleur de jeu a des utilisations attendues au sein de votre application tvOS et les fonctionnalités suivantes :

|Fonctionnalité|Utilisation des applications générales|L’utilisation des applications de jeux|
|---|---|---|
|**D-Pad**|Parcourt les éléments d’interface utilisateur (fait basculer le focus).|Dépend de jeu.|
|**A**|Active l’élément sélectionné (actif).|Exécute la fonction du bouton principal et confirme les actions de la boîte de dialogue.|
|**B**|Retourne à l’écran précédent ou s’arrête à l’écran d’accueil si vous utilisez l’écran de l’application principale.|Exécute la fonction de bouton secondaire ou retourne à l’écran précédent.|
|**X**|Démarre la lecture du média ou pause/reprend la lecture.|Dépend de jeu.|
|**Y**|N/A|Dépend de jeu.|
|**Menu**|Retourne à l’écran précédent ou s’arrête à l’écran d’accueil si vous utilisez l’écran de l’application principale.|Suspendre/reprendre jeu, retourne à l’écran précédent ou s’arrête à l’écran d’accueil si vous utilisez l’écran de l’application principale.|
|**Bouton de l’épaule gauche**|Navigue de gauche.|Dépend de jeu.|
|**Déclencheur de gauche**|Navigue de gauche.|Dépend de jeu.|
|**Bouton de droite épaule**|Navigue de droite.|Dépend de jeu.|
|**Déclencheur de droite**|Navigue de droite|Dépend de jeu.|
|**Stick analogique gauche**|Parcourt les éléments d’interface utilisateur (fait basculer le focus).|Dépend de jeu.|
|**Stick analogique droit**|N/A|Dépend de jeu.|

Apple propose les suggestions suivantes pour l’utilisation des contrôleurs de jeu :

- **Vérifiez les connexions de contrôleur de jeu** -votre application tvOS pouvant être démarrée et arrêtée à tout moment par l’utilisateur final. Vous devez toujours vérifier la présence d’un contrôleur de jeu au démarrage de l’application ou d’heures éveillés et prendre des mesures en fonction des besoins.
- **Vous assurer que votre application fonctionne sur Siri Remote et contrôleurs de jeu** -ne nécessitent pas aux utilisateurs de basculer entre les Siri Remote et un contrôleur de jeu à utiliser votre application. Testez votre application souvent avec les deux types de contrôleurs de s’assurer que tout est facile à parcourir et qu’il fonctionne comme prévu.
- **Fournir un moyen de revenir** -en appuyant sur le bouton de Menu doit toujours renvoyer à l’écran précédent. Si l’utilisateur est à l’écran de l’application principale, le bouton de Menu doit les retourner à l’écran d’accueil de Apple TV. Cours de jeu, le bouton de Menu doit afficher une alerte de donner à l’utilisateur la possibilité de mettre en pause/relancer le jeu ou revenir au menu principal.

<a name="Working-with-Game-Controllers" />

## <a name="working-with-game-controllers"></a>Utilisation de contrôleurs de jeu

Comme indiqué ci-dessus, en plus de la norme Siri à distance fourni avec l’Apple TV, l’utilisateur peut éventuellement attacher un 3e tiers, effectuées pour les contrôleurs de jeu iOS (IFM) Bluetooth et l’utiliser pour contrôler votre application Xamarin.tvOS.

Si votre application nécessaire l’entrée d’un contrôleur de bas niveau, vous pouvez utilise d’Apple [Framework de contrôleur de jeu](https://developer.apple.com/library/prerelease/tvos/documentation/ServicesDiscovery/Conceptual/GameControllerPG/Introduction/Introduction.html#//apple_ref/doc/uid/TP40013276) qui a les modifications suivantes à tvOS :

- Le profil du contrôleur de jeu Micro (`GCMicroGamepad`) a été ajoutée pour cibler le Siri Remote.
- La nouvelle `GCEventViewController` classe peut être utilisée pour acheminer des événements de contrôleur de jeu via votre application. Consultez le [déterminer une entrée de contrôleur de jeu](#determining-game-controller-input) section ci-dessous pour plus d’informations.

<a name="Game-Controller-Support-Requirements" />

### <a name="game-controller-support-requirements"></a>Exigences de prise en charge de contrôleur de jeu

Apple a plusieurs conditions spécifiques qui doivent être satisfaites si votre application Xamarin.tvOS prend en charge les contrôleurs de jeu :

- **Vous devez prendre en charge la Siri Remote** -vous devez toujours prendre en charge l’instance distante de Siri. Votre jeu ne pouvez pas exiger une 3ème partie contrôleur de jeu à être lu.
- **Vous devez prendre en charge la disposition des contrôles étendus** -tvOS tous les contrôleurs de jeu sont non formfitting étendu contrôleurs.
- **Jeux doivent être Playable avec des contrôleurs de Stand-Alone** -si votre application prend en charge un contrôleur de jeu étendu, il doit être être utilisée uniquement avec ce contrôleur de jeu.
- **Vous devez prendre en charge le bouton de lecture/Pause** -cours de jeu, si l’utilisateur appuie sur le bouton de lecture/Pause, vous devez afficher une alerte de donner à l’utilisateur la possibilité de mettre en pause/relancer le jeu ou revenir au menu principal.

<a name="Enabling-Game-Controller-Support" />

### <a name="enabling-game-controller-support"></a>Prise en charge de contrôleur de jeu

Pour activer la prise en charge de contrôleur de jeu dans votre application Xamarin.tvOS, double-cliquez sur le `Info.plist` de fichiers dans le **l’Explorateur de solutions** à ouvrir pour modification :

[![](remote-bluetooth-images/game02.png "L’éditeur Info.plist")](remote-bluetooth-images/game02.png#lightbox)

Sous le **de contrôleur de jeu** section, cochez la case par **activer les contrôleurs de jeu**, puis vérifier tous les types de contrôleur de jeu qui seront pris en charge par l’application.

<a name="Using-the-Siri-Remote-as-a-Game-Controller" />

### <a name="using-the-siri-remote-as-a-game-controller"></a>À l’aide de l’instance distante de Siri comme un contrôleur de jeu

L’instance distante de Siri qui accompagnent l’Apple TV peut servir d’un contrôleur de jeu limité. Comme les autres contrôleurs de jeu, il s’affiche dans le Framework de contrôleur de jeu comme un `GCController` objet et prend en charge la `GCMotion` et le `GCMicroGamepad` profils.

Siri distant présente les caractéristiques suivantes quand utilisé comme un contrôleur de jeu :

- La Surface Touch peut servir d’un boîtier qui fournit des données d’entrée analogiques.
- L’instance distante peut être utilisée dans un mode portrait ou le paysage et décide de votre application si l’objet de profil doit retourner des données d’entrée automatiquement.
- Cliquant sur l’aire Touch joue le rôle en appuyant sur le bouton **A** sur un contrôleur de jeu.
- Le bouton de lecture/Pause agit comme bouton **X** sur un contrôleur de jeu.
- Le bouton de Menu doit s’afficher une alerte de donner à l’utilisateur la possibilité de mettre en pause/relancer le jeu ou revenir au menu principal.

<a name="Summary" />

### <a name="determining-game-controller-input"></a>Déterminer l’entrée de contrôleur de jeu

Contrairement à iOS où les événements de contrôleur de jeu peuvent être reçus en parallèle avec les événements tactiles, tvOS traite tous les événements de bas niveau pour fournir un haut niveau `UIKit` événements. Par conséquent, si vous devez accéder aux événements de contrôleur de jeu de bas niveau, vous devrez désactiver `UIKit`du comportement par défaut.

Sur tvOS, lorsque vous souhaitez traiter l’entrée de contrôleur de jeu directement vous devez utiliser un `GCEventViewController` (ou une sous-classe) pour afficher le jeu de l’Interface utilisateur. Chaque fois qu’un `GCEventViewController` est la *premier répondeur*, entrée de contrôleur de jeu est capturée et remise à votre application via l’infrastructure de contrôleur de jeu.

Vous pouvez utiliser la `UserInteractionEnabled` propriété de la `GCEventViewController` classe pour activer/désactiver la façon dont les événements sont traités et gérés.

Pour plus d’informations sur l’implémentation de prise en charge du contrôleur de jeu, consultez le site d’Apple [utilisation de contrôleurs de jeu](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/WorkingwithGameControllers.html) section dans le [Guide de programmation d’application pour tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/index.html) et [de contrôleur de jeu Guide de programmation](https://developer.apple.com/library/prerelease/tvos/documentation/ServicesDiscovery/Conceptual/GameControllerPG/Introduction/Introduction.html).

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a présenté la nouvelle référence distante Siri qui accompagne l’Apple TV, les mouvements de toucher la Surface et les boutons de Siri Remote. Ensuite, elle couvrait utilisation mouvements et des plans conceptuels, mouvements et code et événements de bas niveau. Enfin, si indiqué fonctionne avec les contrôleurs de jeu.



## <a name="related-links"></a>Liens associés

- [Exemples tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS Human Interface Guides](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Guide de programmation d’application pour tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
