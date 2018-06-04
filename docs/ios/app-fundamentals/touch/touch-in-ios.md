---
title: Touch dans iOS
ms.prod: xamarin
ms.assetid: DA666DC9-446E-4CD1-B5A0-C6FFBC7E53AD
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 9ed90a9c4ddcd398d834cb8c91553a57e7bd5ad8
ms.sourcegitcommit: a4c2a63ba76b839cda99e4474e7ab46fe307cd39
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34689539"
---
# <a name="touch-in-ios"></a>Touch dans iOS

Il est important de comprendre les événements tactiles et de toucher les API dans une application iOS, car ils sont au cœur de toutes les interactions physiques avec le périphérique. Toutes les interactions tactiles impliquent un `UITouch` objet. Dans cet article, vous allez apprendre comment utiliser le `UITouch` classe et ses API pour prendre en charge les fonctions tactiles. Une version ultérieure, nous étendrons sur nos connaissances pour savoir comment prendre en charge les mouvements.

## <a name="enabling-touch"></a>L’activation tactile

Contrôles de `UIKit` – ces sous-classé à partir du contrôle – dépendent de ce intervention de l’utilisateur qu’ils ont des mouvements intégrées à UIKit et par conséquent, il n’est pas nécessaire activer Touch. Il est déjà activé.

Toutefois, un grand nombre des vues dans `UIKit` n’ont pas de touche activée par défaut. Il existe deux façons d’activer des fonctions tactiles sur un contrôle. La première consiste à vérifier la case à cocher activée d’Interaction utilisateur dans la zone de propriété du concepteur, iOS comme indiqué dans la capture d’écran suivante :

 [![](touch-in-ios-images/image1.png "Case à cocher activée d’Interaction utilisateur dans la zone de propriété du concepteur iOS")](touch-in-ios-images/image1.png#lightbox)

Nous pouvons également utiliser un contrôleur pour définir le `UserInteractionEnabled` propriété la valeur true sur un `UIView` classe. Cela est nécessaire si l’interface utilisateur est créé dans le code.

La ligne de code suivante est un exemple :

```csharp
imgTouchMe.UserInteractionEnabled = true;
```

## <a name="touch-events"></a>Événements tactiles

Il existe trois phases de tactile qui se produisent lorsque l’utilisateur touche l’écran, déplace leur doigt ou supprime leur doigt. Ces méthodes sont définies dans `UIResponder`, qui est la classe de base pour UIView. iOS remplacent les méthodes associées sur le `UIView` et `UIViewController` pour gérer l’interaction tactile :

-  `TouchesBegan` -Il est appelé lorsque l’écran est tout d’abord couvertes.
-  `TouchesMoved` -Il est appelé lorsque l’emplacement des modifications tactile en tant que l’utilisateur est décalée leurs doigts sur l’écran.
-  `TouchesEnded` ou `TouchesCancelled` – `TouchesEnded` est appelée lors de la levée des doigts de l’utilisateur désactivé l’écran.  `TouchesCancelled` est appelée à si iOS annule les fonctions tactiles – par exemple, si un utilisateur son doigt en dehors d’un bouton pour annuler une pression sur les diapositives.


Touch événements voyage de manière récursive vers le bas dans la pile de UIViews, pour vérifier si l’événement tactile est dans les limites d’un objet de vue. Cela est souvent appelé _le test de positionnement_. Ils seront appelées tout d’abord sur le plus haut `UIView` ou `UIViewController` et est ensuite appelée sur le `UIView` et `UIViewControllers` ci-dessous dans la hiérarchie.

A `UITouch` objet est créé chaque fois que l’utilisateur touche l’écran. Le `UITouch` objet inclut des données sur les fonctions tactiles, tels que lorsque la fonction tactile s’est produite, où elle s’est produite, si la touche a été un balayage etc. Les événements tactiles transmis une propriété touche – un `NSSet` contenant un ou plusieurs des touches finales. Nous pouvons utiliser cette propriété pour obtenir une référence à une fonction tactile et déterminer la réponse de l’application.

Les classes qui remplacent un des événements tactiles doivent d’abord appeler l’implémentation de base, puis obtenez le `UITouch` objet associé à l’événement. Pour obtenir une référence à la première fonction tactile, appelez le `AnyObject` propriété et effectuez un cast en tant qu’un `UITouch` comme illustré dans l’exemple suivant :

```csharp
public override void TouchesBegan (NSSet touches, UIEvent evt)
{
    base.TouchesBegan (touches, evt);
    UITouch touch = touches.AnyObject as UITouch;
    if (touch != null)
    {
        //code here to handle touch
    }
}
```

iOS reconnaît automatiquement rapide successive affecte à l’écran et collectera sous la forme d’un seul clic dans une seule `UITouch` objet. Cela rend la vérification pour un double appui aussi simple que la vérification de la `TapCount` propriété, comme illustré dans le code suivant :

```csharp
public override void TouchesBegan (NSSet touches, UIEvent evt)
{
    base.TouchesBegan (touches, evt);
    UITouch touch = touches.AnyObject as UITouch;
    if (touch != null)
    {
        if (touch.TapCount == 2)
        {
            // do something with the double touch.
        }
    }
}
```

## <a name="multi-touch"></a>Tactiles

Tactiles n’est pas activé par défaut sur les contrôles. Tactiles peut être activée dans le concepteur, iOS, comme illustré par la capture d’écran suivante :

 [![](touch-in-ios-images/image2.png "Tactiles activée dans le concepteur iOS")](touch-in-ios-images/image2.png#lightbox)

Il est également possible de définir tactiles par programme en définissant le `MultipleTouchEnabled` propriété comme indiqué dans la ligne de code suivante :

```csharp
imgTouchMe.MultipleTouchEnabled = true;
```

Pour déterminer combien de doigts toucher l’écran, utilisez le `Count` propriété sur le `UITouch` propriété :

```csharp
public override void TouchesBegan (NSSet touches, UIEvent evt)
{
    base.TouchesBegan (touches, evt);
    lblNumberOfFingers.Text = "Number of fingers: " + touches.Count.ToString();
}
```

## <a name="determining-touch-location"></a>Détermination de l’emplacement de l’interaction tactile

La méthode `UITouch.LocationInView` retourne un objet CGPoint qui contient les coordonnées de la fonction tactile dans une vue donnée. En outre, nous pouvons tester pour voir si cet emplacement est dans un contrôle en appelant la méthode `Frame.Contains`. L’extrait de code suivant montre un exemple :

```csharp
if (this.imgTouchMe.Frame.Contains (touch.LocationInView (this.View)))
{
    // the touch event happened inside the UIView imgTouchMe.
}
```

Maintenant que nous avons une compréhension des événements tactiles dans iOS, nous allons en savoir plus sur les modules de reconnaissance de mouvement.

## <a name="gesture-recognizers"></a>Modules de reconnaissance de mouvement

Modules de reconnaissance de mouvement peuvent considérablement simplifier et réduire l’effort de programmation pour prendre en charge les fonctions tactiles dans une application. modules de reconnaissance de mouvement iOS agrègent une série d’événements tactiles dans un événement de clic.

Xamarin.iOS fournit la classe `UIGestureRecognizer` en tant que classe de base pour les modules de reconnaissance de mouvement intégrés suivants :

-  *UITapGestureRecognizer* – il s’agit d’un ou plusieurs clics.
-  *UIPinchGestureRecognizer* – Pinching et la propagation des doigts éloignés.
-  *UIPanGestureRecognizer* : panoramique ou glisser-déplacer.
-  *UISwipeGestureRecognizer* – passant dans n’importe quelle direction.
-  *UIRotationGestureRecognizer* – rotation des deux doigts dans un mouvement vers la droite ou vers la gauche.
-  *UILongPressGestureRecognizer* – appuyer et maintenir, parfois en tant que long, appuyez sur ou cliquez de longue durée.


Le modèle de base à l’aide d’un module de reconnaissance de mouvement est comme suit :

1.  **Le module de reconnaissance de mouvement d’instancier** – tout d’abord, instanciez un `UIGestureRecognizer` sous-classe. L’objet est instancié est associée par une vue et sera le garbage collector lorsque la vue est supprimée. Il n’est pas nécessaire de créer cette vue comme une variable de niveau classe.
1.  **Configurer des paramètres de mouvement** – l’étape suivante consiste à configurer le module de reconnaissance de mouvement. Consultez la documentation de Xamarin sur `UIGestureRecognizer` et de ses sous-classes pour obtenir la liste des propriétés qui peuvent être définies pour contrôler le comportement d’un `UIGestureRecognizer` instance.
1.  **Configurer la cible** – en raison de son héritage Objective-C, Xamarin.iOS ne déclenche des événements lorsqu’un module de reconnaissance de mouvement correspond à un mouvement.  `UIGestureRecognizer` a une méthode – `AddTarget` – qui peut accepter un délégué anonyme ou un sélecteur d’Objective-C avec le code à exécuter lorsque le module de reconnaissance de mouvement établit une correspondance.
1.  **Activer la reconnaissance de mouvement** – tout comme avec les événements tactiles, les mouvements sont reconnus uniquement si des interactions tactiles sont activées.
1.  **Ajouter le module de reconnaissance de mouvement à la vue** – l’étape finale consiste à ajouter du mouvement à une vue en appelant `View.AddGestureRecognizer` et en lui passant un objet de module de reconnaissance de mouvement.

Reportez-vous à la [exemples de module de reconnaissance de mouvement](~/ios/app-fundamentals/touch/ios-touch-walkthrough.md#Gesture_Recognizer_Samples) pour plus d’informations sur la façon de les implémenter dans le code.

Lorsque les cibles de mouvement sont appelée, elle est passée à une référence au mouvement qui s’est produite. Ainsi, la cible de mouvement obtenir des informations sur le mouvement qui s’est produite. L’étendue des informations disponibles varie selon le type de module de reconnaissance de mouvement qui a été utilisée. Consultez la documentation de Xamarin pour plus d’informations sur les données disponibles pour chaque `UIGestureRecognizer` sous-classe.

Il est important de se rappeler qu’une fois qu’un module de reconnaissance de mouvement a été ajouté à une vue, la vue (et toutes les vues en dessous) ne recevront pas les événements tactiles. Pour autoriser les événements tactiles simultanément des gestes, le `CancelsTouchesInView` propriété doit être définie sur false, comme illustré par le code suivant :

```csharp
_tapGesture.Recognizer.CancelsTouchesInView = false;
```

Chaque `UIGestureRecognizer` possède une propriété d’état qui fournit des informations importantes sur l’état de la reconnaissance de mouvement. Chaque fois que la valeur de cette propriété change, iOS appellera la méthode d’abonnement en lui attribuant une mise à jour. Si un module de reconnaissance de mouvement personnalisée jamais des mises à jour la propriété d’état, l’abonné n’est jamais appelé, le module de reconnaissance de mouvement de rendu inutile.

Mouvements peuvent se résumer comme l’un des deux types :

1.  *Discrètes* – ces mouvements seule déclencher la première fois qu’ils sont reconnus.
1.  *Continu* – ces mouvements continuent à se déclencher tant qu’ils sont reconnus.


Modules de reconnaissance de mouvement existe dans un des états suivants :

-  *Possible* – c’est l’état initial de tous les modules de reconnaissance de mouvement. Il s’agit de la valeur par défaut la propriété d’état.
-  *Began* – lorsqu’un mouvement continu est reconnu, l’état est défini sur Began. Cela permet de s’abonne pour faire la différence entre le démarrage de la reconnaissance de mouvement et lorsqu’elle est modifiée.
-  *A été modifié* – après un mouvement continu a commencé, mais n’est pas terminée, l’état est fixé à modifié chaque fois qu’une touche déplace ou change, tant qu’il est toujours dans les paramètres attendus du mouvement.
-  *Annulé* – cet état sera défini si le module de reconnaissance est passé de Began Changed, puis la touche modifié de manière à ne plus ajuster le modèle du mouvement.
-  *Reconnu* – l’état sera défini lors de la reconnaissance de mouvement correspond à un ensemble de fonctions tactiles et informe l’abonné que le mouvement est terminé.
-  *Fin* – il s’agit d’un alias pour l’état reconnues.
-  *Échec de* : lorsque le module de reconnaissance de mouvement peut ne correspondent plus les fonctions tactiles est à l’écoute de l’état est passé à Échec.


Xamarin.iOS représente ces valeurs dans le `UIGestureRecognizerState` énumération.

## <a name="working-with-multiple-gestures"></a>Utilisation de plusieurs mouvements

Par défaut, iOS n’autorise pas les mouvements par défaut à exécuter simultanément. Au lieu de cela, chaque module de reconnaissance de mouvement recevoir des événements tactiles dans un ordre non déterministe. L’extrait de code suivant illustre comment créer un module de reconnaissance de mouvement à s’exécuter simultanément :

```csharp
gesture.ShouldRecognizeSimultaneously += (UIGestureRecognizer r) => { return true; };
```

Il est également possible de désactiver un mouvement dans iOS. Il existe deux propriétés de délégué qui permettent un module de reconnaissance de mouvement examiner l’état d’une application et les événements tactiles actuelle, à prendre des décisions sur la façon d’et si un mouvement doit être reconnu. Les deux événements sont :

1.  *ShouldReceiveTouch* : ce délégué est appelé juste avant que la reconnaissance de mouvement est passée à un événement tactile et offre la possibilité d’examiner les fonctions tactiles et de décider quelle touche sera gérée par le module de reconnaissance de mouvement.
1.  *ShouldBegin* – appelé lorsqu’un module de reconnaissance tente de modifier l’état de Possible à un autre état. Retourne la valeur false force l’état de la reconnaissance de mouvement à être passé à Échec.


Vous pouvez substituer ces méthodes avec fortement typé `UIGestureRecognizerDelegate`, un délégué faible ou la liaison via la syntaxe de gestionnaire d’événements, comme illustré par l’extrait de code suivant :

```csharp
gesture.ShouldReceiveTouch += (UIGestureRecognizer r, UITouch t) => { return true; };
```

Enfin, il est possible en file d’attente d’un module de reconnaissance de mouvement afin qu’elle fonctionne uniquement si un autre module de reconnaissance de mouvement échoue. Par exemple, un module de reconnaissance de mouvement doigt aboutira uniquement en cas d’échec d’un module de reconnaissance de mouvement de double appui. L’extrait de code suivant fournit un exemple :

```csharp
singleTapGesture.RequireGestureRecognizerToFail(doubleTapGesture);
```

## <a name="creating-a-custom-gesture"></a>Création d’un mouvement personnalisé

Bien qu’iOS fournit certains par défaut des modules de reconnaissance de mouvement, il peut être nécessaire de créer des modules de reconnaissance de mouvement personnalisé dans certains cas. Création d’un module de reconnaissance de mouvement personnalisée implique les étapes suivantes :

1.  Sous-classe `UIGestureRecognizer` .
1.  Substituer les méthodes d’événement de touche approprié.
1.  État de la reconnaissance via la propriété d’état de la classe de base se propager.


Un exemple pratique est traité dans le [à l’aide de tactile dans iOS](ios-touch-walkthrough.md) procédure pas à pas.
