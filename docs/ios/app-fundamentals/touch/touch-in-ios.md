---
title: Événements tactiles et les mouvements dans Xamarin.iOS
description: Ce document décrit comment utiliser des événements tactiles multipoint, mouvements, plusieurs mouvements et des mouvements personnalisés dans les applications Xamarin.iOS.
ms.prod: xamarin
ms.assetid: DA666DC9-446E-4CD1-B5A0-C6FFBC7E53AD
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: f7160c48e1b1ac85f4aa0173c0eb9f42b8fefca2
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61218616"
---
# <a name="touch-events-and-gestures-in-xamarinios"></a>Événements tactiles et les mouvements dans Xamarin.iOS

Il est important de comprendre les événements tactiles et de toucher l’API dans une application iOS, tels qu’ils sont central pour toutes les interactions avec l’appareil physiques. Toutes les interactions tactiles impliquent un `UITouch` objet. Dans cet article, nous allez apprendre à utiliser le `UITouch` classe et ses API pour prendre en charge tactile. Plus tard, nous étendrons sur nos connaissances pour savoir comment prendre en charge des gestes.

## <a name="enabling-touch"></a>L’activation tactile

Contrôles dans `UIKit` – ces sous-classé à partir du contrôle – dépendent c’est le cas intervention de l’utilisateur qui sont intégrées à UIKit de mouvements et par conséquent il n’est pas nécessaire d’activer Touch. Il est déjà activé.

Toutefois, un grand nombre des vues dans `UIKit` n’ont pas tactile est activé par défaut. Il existe deux façons d’activer touch sur un contrôle. La première consiste à vérifier la case à cocher activée d’Interaction utilisateur dans le panneau de la propriété du concepteur, iOS comme indiqué dans la capture d’écran suivante :

 [![](touch-in-ios-images/image1.png "Case à cocher activée d’Interaction utilisateur dans le panneau de la propriété du concepteur iOS")](touch-in-ios-images/image1.png#lightbox)

Nous pouvons également utiliser un contrôleur pour définir le `UserInteractionEnabled` propriété sur true sur un `UIView` classe. Cela est nécessaire si l’interface utilisateur est créé dans le code.

La ligne de code suivante est un exemple :

```csharp
imgTouchMe.UserInteractionEnabled = true;
```

## <a name="touch-events"></a>Événements tactiles

Il existe trois phases de l’entrée tactile qui se produisent lorsque l’utilisateur touche l’écran, déplace son doigt ou supprime son doigt. Ces méthodes sont définies dans `UIResponder`, qui est la classe de base pour UIView. iOS remplacent les méthodes associées sur le `UIView` et `UIViewController` pour gérer tactile :

-  `TouchesBegan` : Il est appelé lorsque l’écran est touché tout d’abord.
-  `TouchesMoved` : Il est appelé lorsque l’emplacement des modifications tactile en tant que l’utilisateur est décalée à leurs doigts dans l’écran.
-  `TouchesEnded` ou `TouchesCancelled` – `TouchesEnded` est appelée lorsque les doigts de l’utilisateur sont levées en dehors de l’écran.  `TouchesCancelled` est appelée si iOS annule la pression tactile – par exemple, si un utilisateur saisit son doigt en dehors d’un bouton pour annuler l’appui sur des diapositives.


Touch événements voyage de manière récursive vers le bas dans la pile de UIViews, pour vérifier si l’événement tactile est dans les limites d’un objet de vue. On parle souvent _le test de positionnement_. Ils seront tout d’abord être appelées sur le premier `UIView` ou `UIViewController` et est ensuite appelée sur le `UIView` et `UIViewControllers` ci-dessous dans la hiérarchie d’affichage.

Un `UITouch` objet sera créé chaque fois que l’utilisateur touche l’écran. Le `UITouch` objet inclut des données sur les fonctions tactiles, tels que lorsque la pression tactile se sont produites, où il s’est produite, si la pression tactile a été un balayage etc. Les événements tactiles transmis une propriété finales – un `NSSet` contenant un ou plusieurs finales. Nous pouvons utiliser cette propriété pour obtenir une référence à une pression tactile et déterminer la réponse de l’application.

Les classes qui remplacent un des événements tactiles doivent d’abord appeler l’implémentation de base, puis obtenez le `UITouch` objet associé à l’événement. Pour obtenir une référence à la première fonction tactile, appelez le `AnyObject` propriété et effectuer un cast en tant qu’un `UITouch` comme indiqué dans l’exemple suivant :

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

iOS reconnaît automatiquement successive rapide aborde l’écran et collectera tout comme un simple clic dans une seule `UITouch` objet. Cela rend inutile la vérification pour un double appui aussi simple que la vérification du `TapCount` propriété, comme illustré dans le code suivant :

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

## <a name="multi-touch"></a>L’interaction tactile multipoint

L’interaction tactile multipoint n’est pas activée par défaut sur les contrôles. L’interaction tactile multipoint peut être activée dans le concepteur, iOS, comme illustré par la capture d’écran suivante :

 [![](touch-in-ios-images/image2.png "L’interaction tactile multipoint est activée dans le concepteur iOS")](touch-in-ios-images/image2.png#lightbox)

Il est également possible de définir l’interaction tactile multipoint par programme en définissant le `MultipleTouchEnabled` comme indiqué dans la ligne de code suivante :

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

## <a name="determining-touch-location"></a>Détermination de l’emplacement de tactile

La méthode `UITouch.LocationInView` retourne un objet CGPoint qui contient les coordonnées de la pression tactile au sein d’une vue donnée. En outre, nous pouvons tester si cet emplacement est dans un contrôle en appelant la méthode `Frame.Contains`. L’extrait de code suivant montre un exemple de ceci :

```csharp
if (this.imgTouchMe.Frame.Contains (touch.LocationInView (this.View)))
{
    // the touch event happened inside the UIView imgTouchMe.
}
```

Maintenant que nous avons une compréhension des événements tactiles dans iOS, nous allons en savoir plus sur les modules de reconnaissance de mouvement.

## <a name="gesture-recognizers"></a>Modules de reconnaissance de mouvement

Modules de reconnaissance de mouvement peuvent considérablement simplifier et réduire l’effort de programmation pour prendre en charge touch dans une application. modules de reconnaissance de mouvement iOS regrouper une série d’événements tactiles dans un événement tactile unique.

Xamarin.iOS fournit la classe `UIGestureRecognizer` comme classe de base pour les modules de reconnaissance de mouvement intégrés suivants :

-  *UITapGestureRecognizer* – il s’agit pour un ou plusieurs clics.
-  *UIPinchGestureRecognizer* – Pinching et la propagation des uns des autres doigts.
-  *UIPanGestureRecognizer* : panoramique ou glissement.
-  *UISwipeGestureRecognizer* – balayant dans n’importe quelle direction.
-  *UIRotationGestureRecognizer* – rotation des deux doigts en un mouvement dans le sens horaire ou antihoraire.
-  *UILongPressGestureRecognizer* – appuyer et maintenir, parfois dénommé un long, appuyez sur ou cliquez sur du long.


Le modèle de base à l’aide d’un module de reconnaissance de mouvement est comme suit :

1.  **Instancier la reconnaissance de mouvement** – tout d’abord, instanciez un `UIGestureRecognizer` sous-classe. L’objet est instancié est associée par une vue et est garbage collecté lorsque la vue est supprimée. Il n’est pas nécessaire de créer cette vue comme une variable de niveau classe.
1.  **Configurer des paramètres de mouvement** – l’étape suivante consiste à configurer la reconnaissance de mouvement. La documentation de Xamarin sur `UIGestureRecognizer` et ses sous-classes pour obtenir la liste des propriétés qui peuvent être définies pour contrôler le comportement d’un `UIGestureRecognizer` instance.
1.  **Configurer la cible** – en raison de son héritage Objective-C, Xamarin.iOS ne déclenchent des événements lorsqu’un module de reconnaissance de mouvement correspond à un mouvement.  `UIGestureRecognizer` possède une méthode – `AddTarget` – qui peut accepter un délégué anonyme ou un sélecteur d’Objective-C avec le code à exécuter lors de la reconnaissance de mouvement est en correspondance.
1.  **Activer la reconnaissance de mouvement** : tout comme avec les événements tactiles, les mouvements sont reconnus uniquement si les interactions tactiles sont activées.
1.  **Ajouter la reconnaissance de mouvement à la vue** – la dernière étape consiste à ajouter le mouvement à une vue en appelant `View.AddGestureRecognizer` et en lui passant un objet de module de reconnaissance de mouvement.

Reportez-vous à la [exemples de module de reconnaissance du mouvement](~/ios/app-fundamentals/touch/ios-touch-walkthrough.md#Gesture_Recognizer_Samples) pour plus d’informations sur la façon de les implémenter dans le code.

Lorsque les cibles du mouvement sont appelée, il sera passé à une référence pour le mouvement s’est produite. Ainsi, la cible de mouvement obtenir des informations sur le mouvement s’est produite. L’étendue des informations disponibles varie selon le type de module de reconnaissance de mouvement qui a été utilisé. Consultez la documentation Xamarin pour plus d’informations sur les données disponibles pour chaque `UIGestureRecognizer` sous-classe.

Il est important de se rappeler qu’une fois qu’un module de reconnaissance de mouvement a été ajouté à une vue, la vue (et toutes les vues en dessous) ne recevront pas les événements tactiles. Pour permettre aux événements tactiles simultanément les gestes, les `CancelsTouchesInView` propriété doit être définie sur false, comme l’illustre le code suivant :

```csharp
_tapGesture.Recognizer.CancelsTouchesInView = false;
```

Chaque `UIGestureRecognizer` possède une propriété d’état qui fournit des informations importantes sur l’état de la reconnaissance de mouvement. Chaque fois que la valeur de cette propriété change, iOS appellera la méthode abonnée en lui attribuant une mise à jour. Si un module de reconnaissance de mouvement personnalisée met jamais à jour la propriété d’état, l’abonné n’est jamais appelé, la reconnaissance de mouvement de rendu inutilisable.

Mouvements peuvent être résumées comme l’un des deux types :

1.  *Discrètes* – ces mouvements seule incendie la première fois où ils sont reconnus.
1.  *Continue* – ces mouvements continuent à déclencher tant qu’elles ne sont reconnues.


Modules de reconnaissance de mouvement existe dans un des états suivants :

-  *Possible* – c’est l’état initial de tous les modules de reconnaissance de mouvement. Il s’agit de la valeur par défaut la propriété State.
-  *Began* – lorsqu’un mouvement continu est reconnu tout d’abord, l’état est défini sur Began. Cela permet de s’abonne pour faire la distinction entre l’heure de début de la reconnaissance de mouvement et lorsqu’il est modifié.
-  *Changed* – lorsqu’un mouvement continu a commencé, mais n’est pas terminée, l’état sera défini sur Changed chaque fois qu’une pression tactile se déplace ou change, tant qu’elle se trouve toujours dans les paramètres prévus du geste.
-  *Annulé* : cet état est défini si le module de reconnaissance est passé de Began Changed, puis la touche modifiée de manière à ne plus ajuster le modèle du geste.
-  *Reconnu* – l’état est défini lors de la reconnaissance de mouvement correspond à un ensemble de fonctions tactiles et informe l’abonné que le geste est terminé.
-  *Fin* – il s’agit d’un alias pour l’état reconnues.
-  *Échec de* : lors de la reconnaissance de mouvement peut ne correspondent pas à la touche est à l’écoute pour l’état sera remplacé par échec.


Xamarin.iOS représente ces valeurs dans le `UIGestureRecognizerState` énumération.

## <a name="working-with-multiple-gestures"></a>Utilisation de gestes plusieurs

Par défaut, iOS n’autorise pas les gestes par défaut pour s’exécuter simultanément. Au lieu de cela, chaque module de reconnaissance de mouvement recevoir des événements tactiles dans un ordre non déterministe. L’extrait de code suivant illustre comment rendre un module de reconnaissance de mouvement à s’exécuter simultanément :

```csharp
gesture.ShouldRecognizeSimultaneously += (UIGestureRecognizer r) => { return true; };
```

Il est également possible de désactiver un mouvement dans iOS. Il existe deux propriétés de délégué qui autorise un module de reconnaissance de mouvement examiner l’état d’une application et les événements tactiles actuelle, pour rendre des décisions sur la façon et si un mouvement doit être reconnu. Les deux événements sont :

1.  *ShouldReceiveTouch* : ce délégué est appelé juste avant que la reconnaissance de mouvement est passée à un événement tactile et offre la possibilité d’examiner la touche et de décider quels finales seront gérées par le module de reconnaissance de mouvement.
1.  *ShouldBegin* – il s’agit quand un module de reconnaissance tente de modifier l’état de Possible à un autre état. Retourne la valeur false forcera l’état de la reconnaissance de mouvement pour être changé en échec.


Vous pouvez substituer ces méthodes avec fortement typé `UIGestureRecognizerDelegate`, un délégué faible ou la liaison via la syntaxe de gestionnaire d’événements, comme l’illustre l’extrait de code suivant :

```csharp
gesture.ShouldReceiveTouch += (UIGestureRecognizer r, UITouch t) => { return true; };
```

Enfin, il est possible en file d’attente un module de reconnaissance de mouvement afin qu’elle réussira uniquement si un autre module de reconnaissance de mouvement échoue. Par exemple, un module de reconnaissance de mouvement de simple clic doit réussir uniquement en cas d’échec d’un module de reconnaissance de mouvement appuyer deux fois. L’extrait de code suivant fournit un exemple de ceci :

```csharp
singleTapGesture.RequireGestureRecognizerToFail(doubleTapGesture);
```

## <a name="creating-a-custom-gesture"></a>Création d’un mouvement personnalisé

Bien qu’iOS fournit certains par défaut des modules de reconnaissance de mouvement, il peut être nécessaire de créer des modules de reconnaissance de mouvement personnalisées dans certains cas. Création d’un module de reconnaissance de mouvement personnalisée implique les étapes suivantes :

1.  Sous-classe `UIGestureRecognizer` .
1.  Substituez les méthodes d’événement tactile approprié.
1.  Statut de reconnaissance via la propriété d’état de la classe de base se propager.


Un exemple pratique de ce sera abordée dans le [à l’aide de Touch dans iOS](ios-touch-walkthrough.md) procédure pas à pas.
