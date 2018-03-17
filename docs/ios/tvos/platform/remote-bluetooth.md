---
title: "Siri distants et les contrôleurs Bluetooth"
description: "Cet article décrit la prise en charge les contrôleurs de jeu Siri distant et Bluetooth nouvelle dans vos applications Xamarin.tvOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: BDB9894A-236B-424B-9032-ACD12A6C5720
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: ca3dd71c3da316e467d8c388efbbded3d9778bf0
ms.sourcegitcommit: 5fc1c4d17cd9c755604092cf7ff038a6358f8646
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/17/2018
---
# <a name="siri-remote-and-bluetooth-controllers"></a>Siri distants et les contrôleurs Bluetooth

_Cet article décrit la prise en charge les contrôleurs de jeu Siri distant et Bluetooth nouvelle dans vos applications Xamarin.tvOS._


Les utilisateurs de votre application Xamarin.tvOS pas interagir avec son interface directement en tant qu’iOS où ils appuyer sur les images sur l’écran du périphérique, mais indirectement à partir de sur l’espace à l’aide du [Siri distant](#The-Siri-Remote).

Si votre application est un jeu, vous pouvez éventuellement générer prise en charge de 3e partie effectuées pour iOS (IFM) [contrôleurs de jeu Bluetooth](#Bluetooth-Game-Controllers) dans votre application également.

[![](remote-bluetooth-images/intro01.png "Le Bluetooth distant et le contrôleur de jeu")](remote-bluetooth-images/intro01.png#lightbox)

Cet article décrit le [Siri distant](#The-Siri-Remote), [se touchent les mouvements de Surface](#Touch-Surface-Gestures) et [Siri des boutons à distance](#Siri-Remote-Buttons) et montre comment les utiliser via [mouvements et Storyboards](#Gestures-and-Storyboards), [de mouvements et Code](#Gestures-and-Code) et [la gestion des événements de bas niveau](#Low-Level-Event-Handling). Enfin, il décrit [utilisation de contrôleurs de jeu](#Working-with-Game-Controllers) dans une application Xamarin.tvOS.

<a name="The-Siri-Remote" />

## <a name="the-siri-remote"></a>Siri à distance

Est le moyen principal que les utilisateurs d’interagir avec l’Apple TV et de votre application Xamarin.tvOS, distant Siri inclus. Apple conçu distant pour combler la distance entre l’utilisateur sur le canapé et l’interface utilisateur de Apple téléviseurs affichées à la place sur l’écran.

Votre défi en tant qu’un développeur d’application tvOS est la créer une interface utilisateur rapide, facile à utiliser et visuellement agréables qui tire parti de la surface de tactile de la Siri Remote, accéléromètre, gyroscope et les boutons.

[![](remote-bluetooth-images/remote01.png "Siri à distance")](remote-bluetooth-images/remote01.png#lightbox)

Siri distant a des utilisations attendues au sein de votre application tvOS et les fonctionnalités suivantes :

|Fonctionnalité|Utilisation des applications général|L’utilisation des applications de jeu|
|---|---|---|
|**Touchez Surface**<br />Faites défiler pour accéder, appuyez sur pour sélectionner et à maintenir pour les menus contextuels.|**Drainage/balayez**<br />L’interface utilisateur de la Navigation entre les éléments peut être actif.<br /><br />**Click**<br />Active (actif) sélectionnée.|**Drainage/balayez**<br />Dépend de jeu et peut servir d’un boîtier en appuyant sur les bords.<br /><br />**Click**<br />Effectuer la fonction du bouton principal.|
|**Menu**<br />Appuyez sur pour revenir à l’écran précédent ou du menu.|Retourne à l’écran précédent et se termine à l’écran Apple TV accueil à partir de l’écran principal de l’application.|Suspendre et reprendre le jeu, retourne à l’écran précédent et se termine à l’écran Apple TV accueil à partir de l’écran principal de l’application.|
|**Siri/Search**<br />Dans les pays Siri, appuyez et maintenez pour le contrôle vocal, dans tous les autres pays, affiche d’écran de recherche.|N/A|N/A|
|**Play/Pause**<br />Lecture et Pause du média ou fournit une fonction secondaire dans les applications.|Démarre la lecture du média et la lecture de pause/reprise.|Exécute la fonction du bouton secondaire ou ignore la vidéo de présentation (s’il existe).|
|**Accueil**<br />Appuyez sur pour revenir à l’écran d’accueil, double-cliquez dessus pour afficher les applications en cours d’exécution, maintenez mise en veille du périphérique.|N/A|N/A|
|**Volume**<br />Les contrôles attachés volume d’équipement audio/vidéo.|N/A|N/A|

<a name="Touch-Surface-Gestures" />

## <a name="touch-surface-gestures"></a>Aire de conception tactiles

Surface de toucher de le Siri Remote est en mesure de détecter les mouvements seul doigt auquel vous pouvez répondre dans votre application Xamarin.tvOS diverses :

|Effectuez un balayage|Clic|Appuyez sur|
|---|---|---|
|![](remote-bluetooth-images/Gesture01.png)|![](remote-bluetooth-images/Gesture02.png)|![](remote-bluetooth-images/Gesture03.png)|
|Déplace la sélection (focus) entre les éléments d’interface utilisateur à écran (haut, bas à gauche, droite). Le glissement peut être utilisé pour faire défiler de longues listes de contenu rapidement à l’aide de l’inertie.|Active l’élément sélectionné (actif) ou agit comme le bouton principal dans un jeu. Cliquez et maintenez peuvent activer des menus contextuels ou les fonctions secondaires.|Légèrement en appuyant sur la Surface tactile sur les bords agit comme des boutons de direction sur un boîtier, déplacer le focus, haut, bas, gauche ou droite en fonction de la zone tapées. En fonction de l’application, peut être utilisé pour faire apparaître les contrôles masqués.|

Apple offre des suggestions suivantes pour travailler avec les mouvements tactiles la Surface :

* **Différence entre les clics et les clics** -en cliquant sur une action intentionnelle par l’utilisateur et est parfaitement adaptée pour la sélection, l’activation et le bouton principal d’un jeu. En appuyant sur est plus subtil et doit être utilisé avec prudence, car l’utilisateur distant Siri souvent maintient en main et accidentellement facilement activer un événement de clic.
* **Ne pas redéfinir les mouvements Standard** -l’utilisateur a un résultat attendu que les mouvements spécifiques effectue des actions spécifiques, vous ne devez pas redéfinir la signification ou la fonction de ces mouvements dans votre application. La seule exception est une application de jeu pendant jeu actif.
* **Définir de nouveaux mouvements avec parcimonie** -là encore, l’utilisateur a un résultat attendu que les mouvements spécifiques effectue des actions spécifiques. Vous devez éviter de définition des mouvements personnalisés pour effectuer les actions standard. Et là encore, jeux sont l’exception plus habituelle où des mouvements personnalisés peuvent ajouter play agréable et réalistes au jeu.
* **Le cas échéant, répondre à boîtier appuie** - touchant légèrement dans l’angle de bords de la Surface tactile seront réagir comme un boîtier sur un déplacer le focus de contrôleur ou de la direction vers le haut, bas, gauche ou vers la droite. Le cas échéant, vous devez répondre à ces mouvements dans votre application ou d’un jeu.

<a name="Siri-Remote-Buttons" />

## <a name="siri-remote-buttons"></a>Boutons de Siri distant

En plus des gestes sur la Surface tactile, votre application peut répondre à l’utilisateur en cliquant sur la Surface tactile ou en appuyant sur le bouton de lecture/Pause. Si vous accédez à l’élément distant Siri à l’aide de l’infrastructure de contrôleur de jeu, vous pouvez également détecter le comportement du bouton de Menu. 

En outre, les pressions de bouton de menu peuvent être détectées à l’aide d’un module de reconnaissance de mouvement avec standard `UIKit` éléments. Si vous intercepter le comportement du bouton de Menu, vous être responsable de la fermeture de la vue et le contrôleur d’affichage actuel et revenir à la précédente.

> [!IMPORTANT]
> Vous devez **toujours** affecter une fonction sur le bouton de lecture/Pause sur l’instance distante. Avoir un bouton non fonctionnelles rendez votre application altérée à l’utilisateur final. Si vous n’avez pas une fonction valide pour ce bouton, attribuer la même fonction que le bouton principal (toucher la cliquez sur Surface).




<a name="Gestures-and-Storyboards" />

## <a name="gestures-and-storyboards"></a>Mouvements et animations

Pour travailler avec distant Siri dans votre application Xamarin.tvOS le plus simple consiste à ajouter des modules de reconnaissance de mouvement à vos vues dans le Concepteur de l’Interface.

Pour ajouter un module de reconnaissance de mouvement, procédez comme suit :

1. Dans le **l’Explorateur de solutions**, double-cliquez sur le `Main.storyboard` et ouvrez-le pour le Concepteur de l’Interface de modification.
2. Faites glisser un **appuyez sur la reconnaissance de mouvement** à partir de la **bibliothèque** et déposez-la sur la vue : 

    [![](remote-bluetooth-images/storyboard01.png "Un module de reconnaissance de mouvement Tap")](remote-bluetooth-images/storyboard01.png#lightbox)
3. Vérifiez **sélectionnez** dans les **bouton** section de la **inspecteur de l’attribut**: 

    [![](remote-bluetooth-images/storyboard02.png "Activez")](remote-bluetooth-images/storyboard02.png#lightbox)
4. **Sélectionnez** signifie que le mouvement répondra à l’utilisateur en cliquant sur le **Touch une Surface** sur Siri à distance. Vous avez également la possibilité de répondre à la **Menu**, **Lecture/Pause**, **des**, **vers le bas**, **gauche** et **Droite** boutons.
5. Ensuite, rattachez une **Action** à partir de la **appuyez sur la reconnaissance de mouvement** et appelez-le `TouchSurfaceClicked`: 

    [![](remote-bluetooth-images/storyboard03.png "Une Action à partir de la reconnaissance de mouvement Tap")](remote-bluetooth-images/storyboard03.png#lightbox)
6. Enregistrer vos modifications et revenir à Visual Studio pour Mac.

Modifier votre vue contrôleur (exemple `FirstViewController.cs`) et ajoutez le code suivant pour gérer le mouvement de déclenchement :

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

Pour plus d’informations sur l’utilisation des plans conceptuels, veuillez consulter notre [Hello, Guide de démarrage rapide de tvOS](~/ios/tvos/get-started/hello-tvos.md). En particulier la [création de l’Interface utilisateur](~/ios/tvos/get-started/hello-tvos.md#Creating-the-User-Interface) et [l’écriture du Code avec les sorties et les actions](~/ios/tvos/get-started/hello-tvos.md#Writing-the-Code) sections.

<a name="Gestures-and-Code" />

## <a name="gestures-and-code"></a>Mouvements et Code

Si vous le souhaitez, vous pouvez créer des mouvements directement dans le code c# et les ajouter à des vues dans votre Interface utilisateur. Par exemple, pour ajouter une série de modules de reconnaissance de mouvement balayez, modifier votre contrôleur de vue et ajoutez le code suivant :

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

Si vous créez un type personnalisé en fonction de `UIKit` dans votre application Xamarin.tvOS (par exemple `UIView`), vous avez également la possibilité de fournir une gestion plus bas niveau d’appui sur le bouton via `UIPress` événements. 

A `UIPress` est tvOS qu’un `UITouch` événement consiste à iOS, à l’exception `UIPress` retourne plus d’informations sur le bouton appuie sur l’instance distante de Siri ou autres équipements Bluetooth (par exemple, un contrôleur de jeu). `UIPress` événements décrivent le comportement du bouton et son état (Began, annulé, modifié ou terminé). 

Pour les boutons analogique sur des périphériques tels que les contrôleurs de jeu Bluetooth, `UIPress` retourne également la quantité de force appliquée au bouton. Le `Type` propriété de la `UIPress` événement définit quel bouton physique a un état modifié, pendant que le reste des propriétés décrivent les modifications apportées.

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

Comme avec `UITouch` si vous avez besoin implémenter des événements le `UIPress` remplacements d’événements, vous devez implémenter les quatre.

<a name="Bluetooth-Game-Controllers" />

## <a name="bluetooth-game-controllers"></a>Contrôleurs de jeu de Bluetooth

En plus de distant Siri standard fourni Apple TV, 3e partie, e/s effectuées pour les contrôleurs de jeu Bluetooth (IFM) peut être associés à l’Apple TV et permettent de contrôler votre application Xamarin.tvOS.

[![](remote-bluetooth-images/game01.png "Contrôleurs de jeu de Bluetooth")](remote-bluetooth-images/game01.png#lightbox)

Contrôleurs de jeu peuvent être utilisés pour améliorer le jeu et de fournir une idée d’immersion dans un jeu. Ils peuvent également servir à contrôler l’interface Apple TV standard afin de l’utilisation ne doit pas basculer entre le contrôleur et distant.

> [!IMPORTANT]
> Les contrôleurs de jeu Bluetooth sont un bon facultatif qui risque de rendre les utilisateurs finaux, votre application ne peut pas forcer l’utilisateur à acheter un. Si votre application prend en charge les contrôleurs de jeu il doit également prendre en charge Siri distant afin que le jeu est utilisable par tous les utilisateurs d’Apple TV.

Un contrôleur de jeu a des utilisations attendues au sein de votre application tvOS et les fonctionnalités suivantes :

|Fonctionnalité|Utilisation des applications général|L’utilisation des applications de jeu|
|---|---|---|
|**D-Pad**|Parcourt les éléments d’interface utilisateur (fait passer le focus).|Dépend de jeu.|
|**A**|Active l’élément sélectionné (actif).|Interroge le bouton principal et confirme les actions de la boîte de dialogue.|
|**B**|Retourne à l’écran précédent ou s’arrête à l’écran d’accueil si vous utilisez l’écran de l’application principale.|Exécute la fonction de bouton secondaire ou renvoie à l’écran précédent.|
|**X**|Démarre la lecture du média ou pause/reprend la lecture.|Dépend de jeu.|
|**Y**|N/A|Dépend de jeu.|
|**Menu**|Retourne à l’écran précédent ou s’arrête à l’écran d’accueil si vous utilisez l’écran de l’application principale.|Pause/reprise jeu, retourne à l’écran précédent ou s’arrête à l’écran d’accueil si vous utilisez l’écran de l’application principale.|
|**Bouton d’épaule gauche**|Navigue de gauche.|Dépend de jeu.|
|**Déclencheur de gauche**|Navigue de gauche.|Dépend de jeu.|
|**Bouton de privilège de droite**|Accède à droite.|Dépend de jeu.|
|**Déclencheur de droite**|Accède à droite|Dépend de jeu.|
|**Stick analogique gauche**|Parcourt les éléments d’interface utilisateur (fait passer le focus).|Dépend de jeu.|
|**Stick analogique droit**|N/A|Dépend de jeu.|

Apple offre des suggestions suivantes pour travailler avec les contrôleurs de jeu :

- **Vérifiez les connexions du contrôleur de jeu** -votre application tvOS permettre être démarrée et arrêtée à tout moment par l’utilisateur final. Vous devez toujours vérifier la présence d’un contrôleur de jeu au démarrage de l’application ou une fois mis en éveil et prendre des mesures en fonction des besoins.
- **Vous assurer que votre application fonctionne, sur les contrôleurs de jeu et Siri distant** -ne nécessitent pas aux utilisateurs de basculer entre un contrôleur de jeu et distant Siri à utiliser votre application. Tester votre application souvent avec les deux types de contrôleurs de s’assurer que tout est facile à parcourir et qu’il fonctionne comme prévu.
- **Fournir un moyen de revenir** -en appuyant sur le bouton de Menu doit toujours renvoyer à l’écran précédent. Si l’utilisateur est à l’écran principal de l’application, le bouton de Menu doit les retourner à l’écran d’accueil TV Apple. Pendant le jeu, le bouton de Menu doit afficher une alerte en donnant à l’utilisateur la possibilité de jeu d’interruption ou de reprise ou de revenir au menu principal.

<a name="Working-with-Game-Controllers" />

## <a name="working-with-game-controllers"></a>Utilisation de contrôleurs de jeu

Comme indiqué ci-dessus, en plus de la norme Siri à distance fourni avec Apple TV, l’utilisateur peut éventuellement attacher un 3e partie, effectuées pour les contrôleurs de jeu Bluetooth iOS (IFM) et l’utiliser pour contrôler votre application Xamarin.tvOS.

Si votre application requise entrée d’un contrôleur de bas niveau, vous pouvez utilise d’Apple [jeu contrôleur Framework](https://developer.apple.com/library/prerelease/tvos/documentation/ServicesDiscovery/Conceptual/GameControllerPG/Introduction/Introduction.html#//apple_ref/doc/uid/TP40013276) qui a les modifications suivantes de tvOS :

- Le profil du contrôleur de jeu Micro (`GCMicroGamepad`) a été ajouté à la cible distant Siri.
- La nouvelle `GCEventViewController` classe peut être utilisé pour acheminer les événements de contrôleur via votre application. Consultez le [déterminer une entrée de contrôleur de jeu](#Determining-Game-Controller-Input) section ci-dessous pour plus de détails.

<a name="Game-Controller-Support-Requirements" />

### <a name="game-controller-support-requirements"></a>Configuration requise prise en charge du contrôleur de jeu

Apple présente plusieurs conditions spécifiques qui doivent être satisfaites si votre application Xamarin.tvOS prend en charge les contrôleurs de jeu :

- **Prise en charge à distance Siri** -vous devez toujours prendre en charge à distance Siri. Votre jeu ne peuvent pas requérir un tiers 3e contrôleur pour être lu.
- **Vous devez prendre en charge la disposition des contrôles étendus** -tvOS tous les contrôleurs de jeu sont non formfitting étendus contrôleurs.
- **Jeux doivent être Playable avec des contrôleurs d’autonome** -si votre application prend en charge un contrôleur de jeu étendu, il doit être lisible uniquement avec ce contrôleur de jeu.
- **Vous devez prendre en charge le bouton de lecture/Pause** -pendant le jeu, si l’utilisateur appuie sur le bouton de lecture/Pause, vous devez afficher une alerte en donnant à l’utilisateur la possibilité de jeu de pause/reprise ou revenir au menu principal.

<a name="Enabling-Game-Controller-Support" />

### <a name="enabling-game-controller-support"></a>Prise en charge de contrôleur de jeu

Pour activer la prise en charge de contrôleur de jeu dans votre application Xamarin.tvOS, double-cliquez sur le `Info.plist` de fichiers dans le **l’Explorateur de solutions** à ouvrir pour le modifier :

[![](remote-bluetooth-images/game02.png "L’éditeur de fichier Info.plist.")](remote-bluetooth-images/game02.png#lightbox)

Sous le **contrôleur de jeu** section, cochez la case en **activer les contrôleurs de jeu**, puis vérifier tous les types de contrôleurs de jeu qui seront pris en charge par l’application.

<a name="Using-the-Siri-Remote-as-a-Game-Controller" />

### <a name="using-the-siri-remote-as-a-game-controller"></a>À l’aide de l’élément distant Siri comme un contrôleur de jeu

Siri distant qui accompagnent l’Apple TV peut servir en tant qu’un contrôleur de jeu limité. Comme les autres contrôleurs de jeu, il s’affiche dans le cadre du contrôleur de jeu en tant qu’un `GCController` objet et prend en charge la `GCMotion` et le `GCMicroGamepad` profils.

Siri distant présente les caractéristiques suivantes quand utilisé comme un contrôleur de jeu :

- La Surface tactile peut servir d’un boîtier qui fournit des données d’entrée analogiques.
- Distant peut être utilisé dans un mode portrait ou le paysage et décide de votre application si l’objet de profil doit retourner automatiquement les données d’entrée.
- En cliquant sur la Surface tactile joue le rôle en appuyant sur le bouton **A** sur un contrôleur de jeu.
- Le bouton de lecture/Pause se comporte comme bouton **X** sur un contrôleur de jeu.
- Le bouton de Menu doit afficher une alerte en donnant à l’utilisateur la possibilité de jeu d’interruption ou de reprise ou de revenir au menu principal.

<a name="Summary" />

### <a name="determining-game-controller-input"></a>Déterminer l’entrée de contrôleur de jeu

Contrairement à iOS où les événements de contrôleur de jeu peuvent être reçus en parallèle avec les événements tactiles, tvOS traite tous les événements de bas niveau pour offrir un haut niveau `UIKit` événements. Par conséquent, si vous avez besoin d’accès pour les événements de contrôleur de jeu de bas niveau, vous devrez désactiver `UIKit`du comportement par défaut.

Sur tvOS, lorsque vous souhaitez traiter l’entrée de contrôleur de jeu directement vous devez utiliser un `GCEventViewController` (ou une sous-classe) pour afficher le jeu de l’Interface utilisateur. Chaque fois qu’un `GCEventViewController` est la *premier répondeur*, entrée du contrôleur de jeu est capturée et remise à votre application par le biais du contrôleur de jeu.

Vous pouvez utiliser la `UserInteractionEnabled` propriété de la `GCEventViewController` classe pour activer/désactiver la façon dont les événements sont traités et gérés.

Pour plus d’informations sur l’implémentation de prise en charge du contrôleur de jeu, consultez le site d’Apple [utilisation de contrôleurs de jeu](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/WorkingwithGameControllers.html) section dans le [Guide de programmation d’application de tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/index.html) et [de contrôleur de jeu Guide de programmation](https://developer.apple.com/library/prerelease/tvos/documentation/ServicesDiscovery/Conceptual/GameControllerPG/Introduction/Introduction.html).

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a couvert nouvelle Siri distant qui est fourni avec les boutons Siri distant, les mouvements tactiles la Surface et Apple TV. Ensuite, il couvert utilisation des mouvements et Storyboards, mouvements de code et des événements de niveau inférieur. Enfin, si décrit l’utilisation de contrôleurs de jeu.



## <a name="related-links"></a>Liens associés

- [Exemples tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS les Guides Interface humaine](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Guide de programmation d’application de tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
