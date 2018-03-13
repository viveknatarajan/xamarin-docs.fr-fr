---
title: "Multitâche pour iPad"
description: "iOS 9 prend en charge les deux applications qui s’exécutent en même temps, à l’aide de la diapositive sur ou mode fractionné. Il prend également en charge la lecture d’image vidéo."
ms.topic: article
ms.prod: xamarin
ms.assetid: 0F2266D7-21FF-404D-A148-0CFDE76B12AA
ms.technology: xamarin-ios
ms.custom: xamu-video
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 39c699b10280218223b6f6022d419f77aba875dc
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/12/2018
---
# <a name="multitasking-for-ipad"></a>Multitâche pour iPad

_iOS 9 prend en charge les deux applications qui s’exécutent en même temps, à l’aide de la diapositive sur ou mode fractionné. Il prend également en charge la lecture d’image vidéo._

![](multitasking-images/about02-sml.png "Exemple d’écran de fractionner") ![ ] (multitasking-images/about03-sml.png "d’image exemple")

iOS 9 prend en charge multitâche exécutant deux applications en même temps sur un iPad spécifique matériel. Multitâche pour iPad est pris en charge via les fonctionnalités suivantes :

- [**Sur la diapositive** ](#Slide-Over) -permet à l’utilisateur afin d’exécuter provisoirement une application iOS de seconde dans une diapositive panneau (soit sur le côté droit ou gauche de l’écran en fonction de la direction de langage) qui couvre environ 25 % de l’application principale en cours d’exécution. Faites glisser sur est uniquement disponible sur un iPad Pro, iPad Air, iPad Air 2, iPad Mini 2, iPad Mini 3 ou iPad Mini 4.
- [**Mode fractionné** ](#Split-View) -sur le matériel pris en charge iPad (iPad Air 2, iPad Mini 4 et iPad Pro uniquement), l’utilisateur peut choisir une deuxième application et s’exécuter côte à côte avec l’application en cours d’exécution dans un mode d’écran de fractionnement. L’utilisateur peut contrôler le pourcentage de l’écran principal qui occupe de chaque application.
- [**Image dans l’image** ](#Picture-in-Picture) - pour les applications de lire du contenu vidéo, la vidéo peut maintenant être lu dans une fenêtre déplaçable et redimensionnable qui flotte au-dessus des autres applications en cours d’exécution sur l’appareil iOS. L’utilisateur a un contrôle total sur la taille et la position de cette fenêtre. Image dans l’image est uniquement disponible sur un iPad Pro, iPad Air, iPad Air 2, iPad Mini 2, iPad Mini 3 ou iPad Mini 4.

Il existe un nombre d’éléments à prendre en compte lorsque [prenant en charge les travaux multitâches dans votre application](#Supporting-Multitasking-in-your-App), y compris :

- [Taille de l’écran et l’Orientation](#Screen-Size-Considerations)
- [Raccourcis clavier de matériel personnalisé](#Custom-Hardware-Keyboard-Shortcuts)
- [Gestion des ressources](#Resource-Management-Considerations)

En tant que développeur d’application que vous pouvez également [annulations de traitement multitâche](#Opting-Out-of-Multitasking), y compris [la désactivation de la lecture vidéo PIP](#Disabling-PIP-Video-Playback).

Cet article décrit les étapes requises pour vous assurer que votre application Xamarin.iOS s’exécute correctement dans un environnement multitâche, ou comment refuser multitâche s’il n’est pas une bonne adapté à votre application.

> [!VIDEO https://youtube.com/embed/GctYAozoLr8]

**Multitâche pour iPad, par [Xamarin University](https://university.xamarin.com)**


<a name="Multitasking-QuickStart" />

## <a name="multitasking-quickstart"></a>Démarrage rapide de multitâche

Pour prendre en charge **diapositive sur** ou **mode fractionné** votre application doit effectuer les opérations suivantes :

 - Généré à partir d’iOS 9 (ou version ultérieure).
 - Utiliser une table de montage séquentiel pour son écran de lancement (et pas l’image actifs).
 - Utilisez un plan conceptuel avec mise en forme automatique et les Classes de taille pour son interface utilisateur.
 - Prend en charge toutes les 4 iOS appareil orientations (Portrait, envers Portrait, paysage gauche et droit du paysage).

<a name="Multitasking" />

## <a name="about-multitasking-for-ipad"></a>Sur les travaux multitâches pour iPad

iOS 9 offre des fonctionnalités multitâche sur iPad avec l’introduction de _diapositive sur_, _mode fractionné_ (iPad Air 2, iPad Mini 4 et iPad Pro) et _image image_. Nous allons étudier ces fonctionnalités dans les sections suivantes.

<a name="Slide-Over" />

### <a name="slide-over"></a>Diapositive sur

La fonctionnalité de diapositive sur permet à l’utilisateur à choisir une deuxième application et les afficher dans un panneau glissant petite pour fournir une interaction rapide. Le panneau diapositive sur est temporaire et se ferme lorsque l’utilisateur revient à l’utilisation de l’application principale à nouveau.

[![](multitasking-images/about01.png "Le panneau diapositive sur")](multitasking-images/about01.png#lightbox)

L’essentiel à retenir est que l’utilisateur décide des deux applications qui s’exécuteront côte à côte et que le développeur n’a aucun contrôle sur ce processus. Par conséquent, il existe des choses que vous devez faire pour vous assurer de que votre application Xamarin.iOS s’exécute correctement dans le panneau diapositive sur :

- **Utilisez les Classes de taille et de mise en forme automatique** —, car il est désormais possible d’exécuter votre application Xamarin.iOS dans le Panneau coulissant côté, vous ne pouvez plus compter sur le périphérique, sa taille de l’écran ou son orientation dans la disposition de votre interface utilisateur. Pour vous assurer que votre application évolue correctement son interface, vous devez utiliser les Classes de taille et de mise en forme automatique. Pour plus d’informations, consultez notre [présentation unifiée des animations](~/ios/user-interface/storyboards/unified-storyboards.md) documentation.
- **Utiliser efficacement les ressources** : étant donné que votre application peut maintenant être partage le système avec une autre application en cours d’exécution, il est essentiel que votre application utilise efficacement les ressources système. Lorsque la mémoire devient éparse, le système arrête automatiquement l’application qui consomme le plus de mémoire. Consultez d’Apple [Guide de l’efficacité énergétique pour des applications iOS](https://developer.apple.com/library/prerelease/ios/documentation/Performance/Conceptual/EnergyGuide-iOS/index.html#//apple_ref/doc/uid/TP40015243) pour plus d’informations.

Faites glisser sur est uniquement disponible sur un iPad Pro, iPad Air, iPad Air 2, iPad Mini 2, iPad Mini 3 ou iPad Mini 4. Pour plus d’informations sur la préparation de votre application pour la diapositive sur, consultez le site d’Apple [adoption des améliorations multitâche sur iPad](https://developer.apple.com/library/prerelease/ios/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/index.html#//apple_ref/doc/uid/TP40015145) documentation.

<a name="Split-View" />

### <a name="split-view"></a>Mode Fractionné

Sur le matériel pris en charge iPad (iPad Air 2, iPad Mini 4 et iPad Pro uniquement), l’utilisateur peut choisir une deuxième application et s’exécuter côte à côte avec l’application en cours d’exécution dans un mode d’écran de fractionnement. L’utilisateur peut contrôler le pourcentage de l’écran principal qui occupe de chaque application en faisant glisser un écran séparateur.

[![](multitasking-images/about02.png "Le mode fractionné")](multitasking-images/about02.png#lightbox)

Comme les faites glisser sur l’utilisateur décide que les deux applications seront exécuter côte à côte et de nouveau, le développeur n’a aucun contrôle sur ce processus. Par conséquent, mode fractionné place des configurations similaires sur une application Xamarin.iOS :

- **Utilisez les Classes de taille et de mise en forme automatique** —, car il est désormais possible d’exécuter votre application Xamarin.iOS dans un mode d’écran de fractionnement à la taille spécifiée de l’utilisateur, vous ne pouvez plus compter sur le périphérique, sa taille de l’écran ou son orientation dans la disposition de votre interface utilisateur. Pour vous assurer que votre application évolue correctement son interface, vous devez utiliser les Classes de taille et de mise en forme automatique. Pour plus d’informations, consultez notre [présentation unifiée des animations](~/ios/user-interface/storyboards/unified-storyboards.md) documentation.
- **Utiliser efficacement les ressources** : étant donné que votre application peut maintenant être partage le système avec une autre application en cours d’exécution, il est essentiel que votre application utilise efficacement les ressources système. Lorsque la mémoire devient éparse, le système arrête automatiquement l’application qui consomme le plus de mémoire. Consultez d’Apple [Guide de l’efficacité énergétique pour des applications iOS](https://developer.apple.com/library/prerelease/ios/documentation/Performance/Conceptual/EnergyGuide-iOS/index.html#//apple_ref/doc/uid/TP40015243) pour plus d’informations.

Pour plus d’informations sur la préparation de votre application pour le mode fractionné, consultez le site d’Apple [adoption des améliorations multitâche sur iPad](https://developer.apple.com/library/prerelease/ios/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/index.html#//apple_ref/doc/uid/TP40015145) documentation.

<a name="Picture-in-Picture" />

### <a name="picture-in-picture"></a>Image dans l’image

La nouvelle image dans l’image (également appelé _PIP_) permet à l’utilisateur regarder une vidéo dans une petite fenêtre flottante que l’utilisateur peut placer n’importe où sur l’écran au-dessus des autres applications en cours d’exécution.

[![](multitasking-images/about03.png "Un exemple de l’image dans la fenêtre flottante d’image")](multitasking-images/about03.png#lightbox)

Comme avec les diapositives sur et mode fractionné, l’utilisateur a un contrôle total sur regarder une vidéo de l’image en mode d’image. Si la fonction principale de votre application est à regarder la vidéo, il doit apporter certaines modifications pour fonctionner correctement en mode PIP. Dans le cas contraire, aucun changement est nécessaire pour prendre en charge des PIP.

À afficher la vidéo PIP à la demande d’utilisateur de votre application, vous devez utiliser soit _AVKit_ ou _AV Foundation API_. Media Player framework a été supprimée dans iOS 9 et ne prend pas en charge le PIP.

Image dans l’image est uniquement disponible sur un iPad Pro, iPad Air, iPad Air 2, iPad Mini 2, iPad Mini 3 ou iPad Mini 4. Pour plus d’informations, consultez notre [PictureInPicture, exemple d’application](https://developer.xamarin.com/samples/ios/iOS9/) et d’Apple [image dans l’image de démarrage rapide](https://developer.apple.com/library/prerelease/ios/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/QuickStartForPictureInPicture.html#//apple_ref/doc/uid/TP40015145-CH14) documentation.

<a name="Supporting-Multitasking-in-your-App" />

## <a name="supporting-multitasking-in-your-app"></a>Prise en charge les travaux multitâches dans votre application

Pour n’importe quelle application Xamarin.iOS existante, prenant en charge les travaux multitâches est une tâche transparente tant que votre application suit déjà des guides de conception d’Apple et les meilleures pratiques pour iOS 8. Cela signifie que l’application doit être à l’aide de plans conceptuels avec mise en forme automatique et les Classes de taille pour les dispositions de son Interface utilisateur (voir notre [présentation unifiée des animations](~/ios/user-interface/storyboards/unified-storyboards.md) pour plus d’informations).

Pour ces applications, peu ou pas de modifications sont nécessaires pour prendre en charge les travaux multitâches et se comporte bien qu’il contient. Si l’interface utilisateur de votre application, a été créé à l’aide d’autres méthodes telles que directement de positionnement et le redimensionnement des éléments d’interface utilisateur dans le code c# ou si elle s’appuie sur les tailles d’écran de périphérique spécifique ou les orientations, il sera nécessiter une modification significative pour prendre en charge d’iOS 9 multitâche correctement.

Pour prendre en charge iOS 9 multitâche sur n’importe quelle application Xamarin.iOS nouveau, utilisez des animations avec mise en forme automatique et les Classes de taille pour toutes les dispositions d’Interface utilisateur de l’application et implémenter les instructions fournies dans les sections suivantes.

<a name="Screen-Size-Considerations" />

### <a name="screen-size-and-orientation-considerations"></a>Taille de l’écran et les considérations relatives à l’Orientation

Avant d’iOS 9, vous pouvez concevoir votre application sur les tailles d’écran de périphérique spécifique et orientations. Car il est désormais possible d’exécuter une application dans le panneau diapositive Out ou en mode de fractionnement, il peut trouver lui-même en cours d’exécution, que ce soit une classe de la taille horizontale de compact ou régulière sur iPad, quelle que soit la taille physique de l’orientation ou de l’écran du périphérique.

[![](multitasking-images/sizeclasses01.png "Taille de l’écran et les considérations relatives à l’Orientation")](multitasking-images/sizeclasses01.png#lightbox)

Sur un iPad, une application en plein écran comporte des Classes de taille horizontale et verticale régulière. Tous les iPhone, mais l’iPhone 6 Plus et iPhone 6 s Plus avoir de classes de taille réduite dans les deux sens dans n’importe quelle orientation. IPhone 6 Plus et iPhone 6 s Plus en mode paysage ont une classe de taille horizontale normale et une classe de taille verticale Compact (un peu comme un iPad Mini).

Sur un iPad qui prennent en charge de la diapositive sur et mode fractionné, vous pouvez vous retrouver avec les combinaisons suivantes :

| **Orientation** | **Application principale** | **Application secondaire** |
|--- |--- |--- |
| **Portrait** |75 % de l’écran<br />Horizontal Compact<br />Vertical régulière|25 % de l’écran<br />Horizontal Compact<br />Vertical régulière|
| **Paysage** |75 % de l’écran<br />Horizontal régulière<br />Vertical régulière|25 % de l’écran<br />Horizontal Compact<br />Vertical régulière|
| **Paysage** |50 % de l’écran<br />Horizontal Compact<br />Vertical régulière|50 % de l’écran<br />Horizontal Compact<br />Vertical régulière|

Dans l’exemple [MuliTask](https://developer.xamarin.com/samples/monotouch/ios9/MultiTask/) application, si elle est exécutée plein écran sur un iPad en mode paysage, il présente la liste et la vue de détail en même temps :

[![](multitasking-images/sizeclasses03.png "La liste et l’affichage des détails présentés en même temps")](multitasking-images/sizeclasses03.png#lightbox)

Si la même application est exécutée dans un panneau diapositive sur, elle est mise en forme comme une classe de taille horizontale Compact et affiche uniquement la liste :

[![](multitasking-images/sizeclasses04.png "Uniquement dans la liste affichée quand l’appareil est horizontal")](multitasking-images/sizeclasses04.png#lightbox)

Pour vous assurer que votre application se comporte correctement dans ces situations, vous devez adopter la caractéristique des Collections, ainsi que les Classes de taille et est conforme à la `IUIContentContainer` et `IUITraitEnvironment` interfaces. Consultez d’Apple [référence de classe UITraitCollection](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UITraitCollection_ClassReference/index.html#//apple_ref/doc/uid/TP40014202) et notre [présentation unifiée des animations](~/ios/user-interface/storyboards/unified-storyboards.md) guide pour plus d’informations.

En outre, vous ne pouvez plus compter sur les limites d’écran des appareils pour définir la zone visible de l’application, vous devez utiliser à la place des limites de la fenêtre de votre application. Étant donné que les limites sont entièrement sous le contrôle de l’utilisateur, vous ne peut pas par programme les ajuster ou empêcher l’utilisateur de modifier ces limites.

Enfin, votre application doit utiliser un fichier d’animation pour présenter son écran de lancement, au lieu d’utiliser un ensemble de **.png** fichiers image et prend en charge toutes les orientations de l’interface quatre (Portrait, envers Portrait, paysage gauche et droit du paysage) à prendre en considération pour l’exécution dans le panneau diapositive sur ou en mode d’affichage fractionné.

<a name="Custom-Hardware-Keyboard-Shortcuts" />

### <a name="custom-hardware-keyboard-shortcuts"></a>Raccourcis clavier de matériel personnalisé

Dans iOS 9 s’exécutant sur un iPad, Apple a une prise en charge étendue pour les claviers de matériel. iPad incluses toujours prise en charge de base clavier externe via certains claviers fabricants créés clavier incluant des clés spécifiques à iOS câblés et Bluetooth.

Maintenant, avec iOS 9, les applications peuvent créer leurs propres raccourcis clavier personnalisés. En outre, certains raccourcis clavier de base sont disponibles, comme **commande-c** (copie), **X de la commande** (Couper), **commande-v** (collez) et **commande-MAJ-H**  (accueil), sans une application qui est spécifiquement écrit répondre leur.

**Commande-onglet** un sélecteur d’application qui permet à l’utilisateur basculer rapidement entre des applications à l’aide du clavier, comme le système d’exploitation Mac s’affiche :

[![](multitasking-images/keyboard01.png "Le sélecteur d’application")](multitasking-images/keyboard01.png#lightbox)

Si une application iOS 9 inclut des raccourcis clavier, l’utilisateur peut maintenir enfoncée le **commande**, **Option** ou **contrôle** clés pour les afficher dans une fenêtre contextuelle :

[![](multitasking-images/keyboard02.png "Menu contextuel des raccourcis clavier")](multitasking-images/keyboard02.png#lightbox)

#### <a name="defining-custom-keyboard-shortcuts"></a>Définition des raccourcis clavier personnalisés

Si nous ajoutons le code suivant à une vue ou une vue contrôleur dans notre application, lorsque cette vue ou un contrôleur est visible, un raccourci clavier personnalisés seront disponible :

```csharp
#region Custom Keyboard Shortcut
public override bool CanBecomeFirstResponder {
    get { return true; }
}

public override UIKeyCommand[] KeyCommands {
    get {

        var keyCommand = UIKeyCommand.Create (new NSString("n"), UIKeyModifierFlags.Command, new Selector ("NewEntry"), new NSString("New Entry"));
        return new UIKeyCommand[]{ keyCommand };
    }
}

[Export("NewEntry")]
public void NewEntry() {

    // Add a new entry
    ...

}
#endregion
```

Tout d’abord, nous remplaçons le `CanBecomeFirstResponder` propriété et retour `true` afin que la vue ou la vue contrôleur peut recevoir d’entrée au clavier. 

Ensuite, nous remplaçons le `KeyCommands` propriété et créer un nouveau `UIKeyCommand` pour le **commande-N** séquence de touches. Lorsque la séquence de touches est activé, nous appelons le `NewEntry` (méthode) (qui nous exposons d’iOS 9 à l’aide du `Export` commande) pour effectuer l’action demandée.

Si cette application est exécutée sur un iPad avec un clavier matériel connecté et l’utilisateur tape **commande-N**, une nouvelle entrée sera être ajoutée à la liste. Si l’utilisateur appuie le **commande** clé, la liste des raccourcis s’affichera :

[![](multitasking-images/keyboard03.png "Menu contextuel des raccourcis clavier")](multitasking-images/keyboard03.png#lightbox)

Consultez l’exemple [multi-tâches application](http://developer.xamarin.com/samples/monotouch/ios9/MultiTask/) pour un exemple d’implémentation.

<a name="Resource-Management-Considerations" />

### <a name="resource-management-considerations"></a>Considérations relatives à la gestion des ressources

Même pour les applications qui utilisent déjà les guides de conception et les meilleures pratiques iOS 8, gestion efficace des ressources est peut-être toujours un problème. Dans iOS 9, les applications n’ont plus une utilisation exclusive de la mémoire, processeur ou autres ressources système.

Par conséquent, vous devez affiner votre application Xamarin.iOS pour utiliser efficacement les ressources système, ou il fait face à l’arrêt dans les situations de mémoire insuffisante. Cela est également vrai pour les applications qui s’en désengager de multitâche, depuis une seconde application peut toujours être exécutée dans un panneau diapositive sur ou une image dans la fenêtre de l’image qui requièrent des ressources supplémentaires ou à l’origine de la fréquence d’actualisation se situent en dessous de 60 frames par seconde.

Examinez les actions utilisateur suivantes et leurs implications :

- **Saisie de texte dans un panneau diapositive sur** -même si votre application ne comporte aucune entrée de texte, le clavier du système permettre désormais être affiché via son interface utilisateur. Par conséquent, l’application devrez répondre pour afficher les notifications de clavier (par exemple, l’affichage et le masquage du clavier).
- **Une deuxième application en cours d’exécution dans le panneau diapositive sur** -la nouvelle application est maintenant en cours d’exécution au premier plan et en concurrence avec l’application existante pour les ressources système telles que la mémoire et les cycles de processeur.
- **Lecture d’une vidéo dans une fenêtre PIP** - non seulement cette fenêtre peut couvrir une partie de l’interface de votre application, mais l’application qui a lancé la vidéo est toujours en cours d’exécution en arrière-plan et la consommation des ressources processeur et mémoire.

Pour vous assurer que votre application utilise des ressources efficacement, vous devez procédez comme suit :

- **Profiler l’application avec les Instruments** -rechercher les fuites de mémoire, les manifestes de l’UC et de zones où l’application peut bloquer le thread principal.
- **Répondre aux méthodes de Transitions d’état** - dans votre **AppDelegate.cs** substitution de fichiers et de réponse à l’état de modifier les méthodes telles que l’application en entrant ou le renvoi de l’arrière-plan. Libérer des actifs superflus, tels que des images, des données ou des vues et contrôleur de la vue.
- **Test côte à côte avec les applications utilisant beaucoup de mémoire** : exécuter votre application à l’aide de la diapositive Out et mode fractionné sur du matériel d’e/s physiques avec une application intensive mémoire telles que des cartes (en mode d’affichage Satellite) et tester les deux applications de restent réactives et de se bloque pas.

Consultez d’Apple [Guide de l’efficacité énergétique pour des applications iOS](https://developer.apple.com/library/prerelease/ios/documentation/Performance/Conceptual/EnergyGuide-iOS/index.html#//apple_ref/doc/uid/TP40015243) pour plus d’informations sur la gestion des ressources.

<a name="Opting-Out-of-Multitasking" />

## <a name="opting-out-of-multitasking"></a>Si vous activez à la sortie de traitement multitâche

Lors de l’Apple indique que toutes les applications iOS 9 prennent en charge les travaux multitâches, il peut y avoir des raisons très spécifiques pour une application pas trop, tels que des jeux ou des applications d’appareil photo qui requièrent le mode plein écran pour fonctionner correctement.

Pour votre application Xamarin.iOS pour s’en désengager d’en cours d’exécution dans soit une diapositive à panneau de configuration ou en mode d’affichage fractionné, modification du projet **Info.plist** de fichier et vérifiez **nécessite un plein écran**:

[![](multitasking-images/fullscreen01.png "Si vous activez à la sortie de traitement multitâche")](multitasking-images/fullscreen01.png#lightbox)

> [!IMPORTANT]
> **Remarque :** tandis que hors-Opting multitâche empêche votre application dans la diapositive Out ou en mode fractionné, il effectue **pas** empêcher l’exécution dans la diapositive Out ou une image dans une vidéo de l’image à partir de l’affichage avec une autre application votre application.




<a name="Disabling-PIP-Video-Playback" />

### <a name="disabling-pip-video-playback"></a>La désactivation de la lecture de la vidéo PIP

Dans la plupart des cas, votre application doit autoriser l’utilisateur à jouer tout contenu vidéo, qu'il s’affiche dans une image dans la fenêtre flottante d’image. Toutefois, il peut arriver là où cela ne peut pas être souhaitée, tels que des vidéos de scène coupé jeu.

Pour s’en désengager de lecture vidéo du PIP, procédez comme suit dans votre application :

 - Si vous utilisez un `AVPlayerViewController` pour afficher la vidéo, définissez la `AllowsPictureInPicturePlayback` propriété `false`.
 - Si vous utilisez la `AVPlayerLayer` pour afficher la vidéo, ne pas instancier un `AVPictureInPictureController`.
 - Si vous utilisez un `WKWebView` pour afficher la vidéo, définissez la `AllowsPictureInPictureMediaPlayback` propriété `false`.

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a couvert les étapes requises pour vous assurer qu’une application Xamarin.iOS s’exécutera et se comporter correctement dans la nouvelle capacité de multitâche iOS 9 pour iPad. En outre, il couvert vous inscrire à la sortie de traitement multitâche pour les applications où il n’est pas adapté.



## <a name="related-links"></a>Liens associés

- [Exemples d’iOS 9](https://developer.xamarin.com/samples/ios/iOS9/)
- [Multitâches (exemple)](http://developer.xamarin.comhttps://developer.xamarin.com/samples/monotouch/ios9/MultiTask/)
- [Introduction à des plans conceptuels unifiées](~/ios/user-interface/storyboards/unified-storyboards.md)
- [iOS 9 pour les développeurs](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Adoption multitâche améliorations sur iPad](https://developer.apple.com/library/prerelease/ios/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/index.html#//apple_ref/doc/uid/TP40015145)
- [billet de blog](https://blog.xamarin.com/using-auto-layouts-for-ios-9-splitview/)
