---
title: Hello, tvOS Guide de démarrage rapide
description: Ce guide explique comment créer votre première application Xamarin.tvOS et sa chaîne d’outils de développement. Elle présente également le Concepteur de Xamarin, ce qui expose des contrôles d’interface utilisateur au code et montre comment créer, exécuter et tester une application Xamarin.tvOS.
ms.prod: xamarin
ms.assetid: 6E0AFE58-A13B-492F-861E-D5D73EB1C4A3
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 02/02/2018
ms.openlocfilehash: 0adf6e326dd29db15b6bd90626f424b803dc0bc9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30785316"
---
# <a name="hello-tvos-quick-start-guide"></a>Hello, tvOS Guide de démarrage rapide

_Ce guide explique comment créer votre première application Xamarin.tvOS et sa chaîne d’outils de développement. Elle présente également le Concepteur de Xamarin, ce qui expose des contrôles d’interface utilisateur au code et montre comment créer, exécuter et tester une application Xamarin.tvOS._

Apple a publié la génération 5 de l’Apple TV, le Kit de développement Apple TV 4 Ko, qui s’exécute tvOS 11.

La plateforme Apple TV est ouverte pour les développeurs, en les autorisant à créer des applications riches et réalistes et de les libérer par le biais du magasin d’applications Apple téléviseurs intégrés.

Si vous êtes familiarisé avec le développement de Xamarin.iOS, vous devez rechercher la transition vers tvOS relativement simple. La plupart des API et des fonctionnalités est les mêmes, toutefois, de nombreuses API courantes est pas disponibles (par exemple WebKit). En outre, travailler avec les à la distance Siri pose des défis de conception qui ne sont pas présentes dans l’écran tactile en fonction des appareils iOS.

Ce guide donne une introduction à l’utilisation de tvOS dans une application Xamarin. Pour plus d’informations sur tvOS, consultez le site d’Apple [se préparer pour Apple TV 4K](https://developer.apple.com/tvos/) documentation.

## <a name="overview"></a>Vue d'ensemble

Xamarin.tvOS vous permet de développer des applications d’Apple TV entièrement natives en c# et .NET en utilisant les mêmes bibliothèques OS X et les contrôles d’interface qui sont utilisés lors du développement en *Swift* (ou *Objective-C*) et *Xcode*.

En outre, étant donné que les applications Xamarin.tvOS sont écrits en c# et .NET, commun, code principal peut être partagée avec les applications Xamarin.iOS, Xamarin.Android et Xamarin.Mac ; tout en fournir une expérience native sur chaque plateforme.

Cet article vous présente les concepts clés nécessaires pour créer une application de TV Apple à l’aide de Xamarin.tvOS et Visual Studio en vous guidant à travers le processus de création d’un base **Hello, tvOS** application qui compte le nombre de fois où un bouton a sélectionné :

[![](hello-tvos-images/run05.png "Exemple d’application exécuter")](hello-tvos-images/run05.png#lightbox)

Nous aborderons les concepts suivants :

-  **Visual Studio pour Mac** : Introduction à Visual Studio pour Mac et comment créer des applications de Xamarin.tvOS avec lui.
-  **Anatomie d’une application Xamarin.tvOS** – que Xamarin.tvOS une application comprend.
-  **Création d’une Interface utilisateur** – comment utiliser le concepteur Xamarin iOS pour créer une interface utilisateur.
-  **Déploiement et test** – comment exécuter et tester votre application dans le simulateur tvOS et sur le matériel de tvOS réel.

## <a name="starting-a-new-xamarintvos-app-in-visual-studio-for-mac"></a>Démarrage d’une nouvelle application Xamarin.tvOS dans Visual Studio pour Mac

Comme indiqué ci-dessus, nous allons créer une application Apple TV appelée `Hello-tvOS` qui ajoute un seul bouton et une étiquette à l’écran principal. Quand vous cliquez sur le bouton, l’étiquette affiche le nombre de clics effectués.

Pour commencer, nous allons effectuer les opérations suivantes :

1. Démarrez Visual Studio pour Mac :

    [![](hello-tvos-images/setup01.png "Visual Studio pour Mac")](hello-tvos-images/setup01.png#lightbox)
2. Cliquez sur le **nouvelle Solution...**  lien dans le coin supérieur gauche de l’écran pour ouvrir la **nouveau projet** boîte de dialogue.
3. Sélectionnez **tvOS** > **application** > **application vue unique** et cliquez sur le **suivant** bouton :

    [![](hello-tvos-images/setup02.png "Sélectionner l’application de la vue unique")](hello-tvos-images/setup02.png#lightbox)
4. Entrez `Hello, tvOS` pour le **nom de l’application**, entrez votre **identifiant d’organisation** et cliquez sur le **suivant** bouton :

    [![](hello-tvos-images/setup04.png "Entrez Hello, tvOS")](hello-tvos-images/setup04.png#lightbox)
5. Entrez `Hello_tvOS` pour le **nom du projet** et cliquez sur le **créer** bouton :

    [![](hello-tvos-images/setup03.png "Entrez HellotvOS")](hello-tvos-images/setup03.png#lightbox)

Visual Studio pour Mac créer la nouvelle application Xamarin.tvOS et afficher les fichiers par défaut qui sont ajoutées à votre solution d’application :

 [![](hello-tvos-images/project01.png "Permet d’afficher les fichiers par défaut")](hello-tvos-images/project01.png#lightbox)

Visual Studio pour Mac utilise **Solutions** et **projets**, de la même manière que Visual Studio. Une solution est un conteneur qui peut recevoir un ou plusieurs projets ; les projets peuvent inclure des applications, bibliothèques de prise en charge, applications de test, etc. Dans ce cas, Visual Studio pour Mac a créé une solution et un projet d’application pour vous.

Si vous le souhaitez, vous pouvez créer un ou plusieurs code des projets de bibliothèque qui contiennent le code commun partagé. Ces projets de bibliothèque peuvent être utilisés par le projet d’application ou partagés avec d’autres projets d’application Xamarin.tvOS (ou Xamarin.iOS, Xamarin.Android et Xamarin.Mac en fonction du type de code), tout comme vous le feriez si vous créez une application .NET standard.

## <a name="anatomy-of-a-xamarintvos-app"></a>Anatomie d’une application Xamarin.tvOS

Si vous êtes familiarisé avec iOS de programmation, vous remarquerez que beaucoup de similitudes ici. En fait, tvOS 9 étant un sous-ensemble d’iOS 9, un grand nombre des concepts traversera ici.

Examinons les fichiers dans le projet :

-   `Main.cs` : ce fichier contient le point d’entrée principal de l’application. Quand l’application est lancée, il contient la toute première classe et la méthode qui est exécutée.
-   `AppDelegate.cs` – Ce fichier contient la classe d’application principal est chargée pour écouter les événements du système d’exploitation.
-   `Info.plist` – Ce fichier contient les propriétés d’application telles que le nom de l’application, icônes, etc.
-   `ViewController.cs` – Il s’agit de la classe qui représente la fenêtre principale et qui contrôle le cycle de vie de celui-ci.
-   `ViewController.designer.cs` – Ce fichier contient le code de base qui vous aide à intégrer à l’interface utilisateur de l’écran principal.
-  `Main.storyboard` – L’interface utilisateur de la fenêtre principale. Ce fichier permettre être créé et géré par le Concepteur de Xamarin pour iOS.

Dans les sections suivantes, nous allons jeter un coup de œil rapide à certains de ces fichiers. Nous allons les Explorer plus en détail ultérieurement, mais il est judicieux de comprendre leurs bases maintenant.

### <a name="maincs"></a>Main.cs

Le `Main.cs` fichier contient un mappage statique `Main` méthode qui crée une instance d’application Xamarin.tvOS et transmet le nom de la classe qui gère les événements du système d’exploitation, soit dans le cas présent le `AppDelegate` classe :

```csharp
using UIKit;

namespace Hello_tvOS
{
    public class Application
    {
        // This is the main entry point of the application.
        static void Main (string[] args)
        {
            // if you want to use a different Application Delegate class from "AppDelegate"
            // you can specify it here.
            UIApplication.Main (args, null, "AppDelegate");
        }
    }
}
```

### <a name="appdelegatecs"></a>AppDelegate.cs

Le `AppDelegate.cs` fichier contient nos `AppDelegate` (classe), qui est responsable de la création de notre fenêtre et écouter des événements du système d’exploitation :

```csharp
using Foundation;
using UIKit;

namespace Hello_tvOS
{
    // The UIApplicationDelegate for the application. This class is responsible for launching the
    // User Interface of the application, as well as listening (and optionally responding) to application events from iOS.
    [Register ("AppDelegate")]
    public class AppDelegate : UIApplicationDelegate
    {
        // class-level declarations

        public override UIWindow Window {
            get;
            set;
        }

        public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
        {
            // Override point for customization after application launch.
            // If not required for your application you can safely delete this method

            return true;
        }

        public override void OnResignActivation (UIApplication application)
        {
            // Invoked when the application is about to move from active to inactive state.
            // This can occur for certain types of temporary interruptions (such as an incoming phone call or SMS message)
            // or when the user quits the application and it begins the transition to the background state.
            // Games should use this method to pause the game.
        }

        public override void DidEnterBackground (UIApplication application)
        {
            // Use this method to release shared resources, save user data, invalidate timers and store the application state.
            // If your application supports background execution this method is called instead of WillTerminate when the user quits.
        }

        public override void WillEnterForeground (UIApplication application)
        {
            // Called as part of the transition from background to active state.
            // Here you can undo many of the changes made on entering the background.
        }

        public override void OnActivated (UIApplication application)
        {
            // Restart any tasks that were paused (or not yet started) while the application was inactive.
            // If the application was previously in the background, optionally refresh the user interface.
        }

        public override void WillTerminate (UIApplication application)
        {
            // Called when the application is about to terminate. Save data, if needed. See also DidEnterBackground.
        }
    }
}
```

Ce code est probablement inconnu à moins que vous avez créé une application iOS avant, mais il est assez simple. Examinons les lignes importantes.

Tout d’abord, examinons la déclaration de variable de niveau classe :

```csharp
public override UIWindow Window {
            get;
            set;
        }

```

Le `Window` propriété fournit l’accès à la fenêtre principale. tvOS utilise ce que l'on appelle la *Model View Controller* modèle (MVC). En règle générale, pour chaque fenêtre que vous créez (et de nombreuses autres choses dans windows), il existe un contrôleur, ce qui est chargé de cycle de vie de la fenêtre, par exemple, présentant l’ajout de nouveaux affichages (contrôles) pour, etc.

Ensuite, nous avons le `FinishedLaunching` (méthode). Cette méthode s’exécute après que l’application a été instanciée, et il est chargé de réellement la création de la fenêtre d’application et commencer le processus d’affichage de la vue qu’il contient. Étant donné que notre application utilise une table de montage séquentiel pour définir son interface utilisateur, aucun code supplémentaire n’est requis ici.

Il existe de nombreuses autres méthodes qui sont fournis dans le modèle, tels que `DidEnterBackground` et `WillEnterForeground`. Il peuvent être supprimés en toute sécurité si les événements d’application ne sont pas utilisés dans votre application.

### <a name="viewcontrollercs"></a>ViewController.cs

La `ViewController` classe est contrôleur de notre de la fenêtre principale. Cela signifie qu’il est chargé pour le cycle de vie de la fenêtre principale. Nous allons examiner cela en détail ultérieurement, pour l’instant voyons un coup de œil il :

```csharp
using System;
using Foundation;
using UIKit;

namespace Hello_tvOS
{
    public partial class ViewController : UIViewController
    {
        public ViewController (IntPtr handle) : base (handle)
        {
        }

        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();
            // Perform any additional setup after loading the view, typically from a nib.
        }

        public override void DidReceiveMemoryWarning ()
        {
            base.DidReceiveMemoryWarning ();
            // Release any cached data, images, etc that aren't in use.
        }
    }
}
```

#### <a name="viewcontrollerdesignercs"></a>ViewController.Designer.cs

Le fichier du concepteur pour la classe de fenêtre principale est maintenant vide, mais il est rempli automatiquement par Visual Studio pour Mac que nous créons notre Interface utilisateur avec le concepteur iOS :

```csharp
using Foundation;

namespace HellotvOS
{
    [Register ("ViewController")]
    partial class ViewController
    {
        void ReleaseDesignerOutlets ()
        {
        }
    }
}
```

Nous ne sont pas concernés généralement avec les fichiers de concepteur, car ils sont automatiquement gérés par Visual Studio pour Mac et simplement fournir le code de base nécessaires qui autorise l’accès aux contrôles que nous ajouter à n’importe quelle fenêtre, ou afficher dans notre application.

Maintenant que nous avons créé notre application Xamarin.tvOS et nous avons une connaissance élémentaire de ses composants, examinons la création de l’interface utilisateur.

<a name="Creating-the-User-Interface" />

## <a name="creating-the-user-interface"></a>Création de l’interface utilisateur

Vous n’êtes pas obligé d’utiliser le Concepteur de Xamarin pour iOS pour créer l’Interface utilisateur pour votre application Xamarin.tvOS, l’interface utilisateur peut être créé directement à partir de code c#, mais qui est abordée dans cet article. Par souci de simplicité, nous utiliserons le concepteur iOS pour créer notre interface utilisateur dans le reste de ce didacticiel.

Pour commencer à créer votre interface utilisateur, nous allons double-cliquez sur le `Main.storyboard` de fichiers dans le **l’Explorateur de solutions** à ouvrir pour le modifier dans le concepteur iOS :

[![](hello-tvos-images/designer01.png "Fichier Main.storyboard dans l’Explorateur de solutions")](hello-tvos-images/designer01.png#lightbox)

Cela doit lancer le concepteur et l’aspect suivant :

[![](hello-tvos-images/designer02.png "Le Concepteur")](hello-tvos-images/designer02.png#lightbox)

Pour plus d’informations sur le concepteur iOS et son fonctionnement, reportez-vous à la [Introduction au Concepteur de Xamarin pour iOS](~/ios/user-interface/designer/introduction.md) guide.

Nous pouvons maintenant démarrer l’ajout de contrôles à l’aire de conception de notre application Xamarin.tvOS.

Effectuez ce qui suit :

1. Recherchez le **boîte à outils**, qui doit être situé à droite de l’aire de conception :

    [![](hello-tvos-images/designer03.png "La boîte à outils")](hello-tvos-images/designer03.png#lightbox)

    Si vous ne le trouvez pas ici, accédez à **vue > remplit > boîte à outils** pour l’afficher.
2. Faites glisser un **étiquette** à partir de la **boîte à outils** à l’aire de conception :

    [![](hello-tvos-images/designer04.png "Faites glisser une étiquette de la boîte à outils")](hello-tvos-images/designer04.png#lightbox)
3. Cliquez sur le **titre** propriété dans le **remplissage de la propriété** et modifier le titre du bouton à `Hello, tvOS` et définir le **la taille de police** à 128 :

    [![](hello-tvos-images/designer05.png "Définir le titre Hello, tvOS et définir la taille de police à 128")](hello-tvos-images/designer05.png#lightbox)
4. Redimensionner l’étiquette afin que tous les mots sont visibles et placez-le centré vers le haut de la fenêtre :

    [![](hello-tvos-images/designer06.png "Redimensionner et centrer l’étiquette")](hello-tvos-images/designer06.png#lightbox)
5. L’étiquette auront désormais besoin d’être contraints à sa position, afin qu’il apparaisse comme prévu. quelle que soit la taille de l’écran. Pour ce faire, cliquez sur l’étiquette jusqu'à ce que le *poignée T* s’affiche :

    [![](hello-tvos-images/designer07.png "La poignée T")](hello-tvos-images/designer07.png#lightbox)
6. Pour limiter l’étiquette horizontalement, sélectionnez le carré de centre et faites-le glisser vers la ligne verticale en pointillé :

    [![](hello-tvos-images/designer08.png "Sélectionnez le carré central")](hello-tvos-images/designer08zoom.png#lightbox)

     L’étiquette doit devenir orange.
7. Sélectionnez la poignée T en haut de l’étiquette et faites-le glisser vers le bord supérieur de la fenêtre :

    [![](hello-tvos-images/designer09.png "Faites glisser la poignée sur le bord supérieur de la fenêtre")](hello-tvos-images/designer09.png#lightbox)
8. Ensuite, cliquez sur la largeur et hauteur *les handle OS* comme illustré ci-dessous :

    [![](hello-tvos-images/designer10.png "La largeur et la hauteur des handles OS")](hello-tvos-images/designer10.png#lightbox)

     Lorsque chaque *les handle OS* est activé, sélectionnez la largeur et hauteur respectivement pour définir des dimensions fixes.
9. Lorsque terminée, vos contraintes doivent ressembler à celles figurant dans l’onglet Disposition de la zone de propriétés :

    [![](hello-tvos-images/designer11.png "Contraintes d’exemple")](hello-tvos-images/designer11.png#lightbox)
8. Faites glisser un **bouton** à partir de la **boîte à outils** et le placer sous l’étiquette.
9. Cliquez sur le **titre** propriété dans le **remplissage de la propriété** et modifier le titre du bouton à `Click Me`:

    [![](hello-tvos-images/designer12.png "Modifier le titre de boutons Click Me")](hello-tvos-images/designer12.png#lightbox)
10. Répétez les étapes 5 à 8 pour contraindre le bouton dans la fenêtre tvOS ci-dessus. Toutefois, au lieu de faire glisser la poignée de T vers le haut de la fenêtre (comme étape #7), faites-le glisser vers le bas de l’étiquette :

    [![](hello-tvos-images/designer14.png "Limiter le bouton")](hello-tvos-images/designer14.png#lightbox)
11. Faites glisser une autre étiquette sous le bouton, de taille pour la même largeur que le premier contrôle label et son **alignement** à **Center**:

    [![](hello-tvos-images/designer15.png "Faites glisser une autre étiquette sous le bouton, de taille pour la même largeur que le premier contrôle label et définir son alignement au centre")](hello-tvos-images/designer15.png#lightbox)
12. Comme la première étiquette et bouton, définissez cette étiquette pour épingler dans l’emplacement et la taille et de centrage :

    [![](hello-tvos-images/designer16.png "L’étiquette dans l’emplacement et la taille de code confidentiel")](hello-tvos-images/designer16.png#lightbox)
13. Enregistrez vos modifications dans l’Interface utilisateur.

Quand vous ont été redimensionnement et le déplacement des contrôles autour, vous avez devez remarqué que le concepteur vous donne des indicateurs de composant logiciel enfichable utiles qui sont basées sur [les recommandations d’Interface humaine Apple TV](https://developer.apple.com/tvos/human-interface-guidelines/). Ces recommandations vous aideront à créer des applications de haute qualité qui auront un aspect familier pour les utilisateurs d’Apple TV.

Si vous regardez dans le **structure du Document** section, notez la manière dont la disposition et la hiérarchie des éléments qui composent notre Interface utilisateur sont affichés :

[![](hello-tvos-images/designer17.png "La section de la structure du Document")](hello-tvos-images/designer17.png#lightbox)

À ce stade, vous pouvez sélectionner des éléments à modifier, ou faites glisser pour réorganiser les éléments d’interface utilisateur, si nécessaire. Par exemple, si un élément d’interface utilisateur a été réalisée par un autre élément, vous impossible le glisser vers le bas de la liste pour le rendre l’élément supérieur de la fenêtre.

Maintenant que nous avons notre Interface utilisateur créée, nous devons exposer les éléments d’interface utilisateur pour qu’un Xamarin.tvOS peut accéder et interagir avec eux dans le code c#.

### <a name="accessing-the-controls-in-the-code-behind"></a>Accéder aux contrôles dans le code-behind

Il existe deux façons pour accéder aux contrôles que vous avez ajoutés dans le concepteur iOS à partir du code :

* Création d’un gestionnaire d’événements sur un contrôle.
* En donnant le contrôle un nom, afin de nous pouvons référencer ultérieurement.

Lorsque une de ces sont ajoutées, la classe partielle dans le `ViewController.designer.cs` sera mise à jour pour refléter les modifications. Cela vous permettra à ensuite accéder aux contrôles dans le contrôleur de la vue.

### <a name="creating-an-event-handler"></a>Création d’un gestionnaire d’événements

Dans cet exemple d’application, lorsque l’utilisateur clique sur le bouton nous voulons _quelque chose_ se produire, par conséquent, un gestionnaire d’événements doit être ajouté à un événement spécifique sur le bouton. Pour ce faire, procédez comme suit :

1. Dans le concepteur du Xamarin iOS, sélectionnez le bouton sur le contrôleur de la vue.
2. Dans la zone Propriétés, sélectionnez le **événements** onglet :

    [![](hello-tvos-images/event1.png "L’onglet événements")](hello-tvos-images/event1.png#lightbox)
3. Recherchez l’événement TouchUpInside et lui donner un gestionnaire d’événements nommé `Clicked`:

    [![](hello-tvos-images/event2.png "L’événement TouchUpInside")](hello-tvos-images/event2.png#lightbox)
4. Lorsque vous appuyez sur **entrée**, le **ViewController**fichier .cs s’ouvre, et suggère des emplacements pour votre gestionnaire d’événements dans le code. Utilisez les touches de direction de votre clavier pour définir l’emplacement :

    [![](hello-tvos-images/event3.png "Définition de l’emplacement")](hello-tvos-images/event3.png#lightbox)
5. Cela crée une méthode partielle, comme indiqué ci-dessous :

    [![](hello-tvos-images/event4.png "La méthode partielle")](hello-tvos-images/event4.png#lightbox)

Nous sommes maintenant prêts à commencer à ajouter du code pour autoriser le bouton de la fonction.

### <a name="naming-a-control"></a>Un contrôle d’affectation de noms

Lorsque le bouton est activé, l’étiquette doit mettre à jour en fonction du nombre de clics. Pour ce faire, nous devons accéder à l’étiquette dans le code. Pour cela, vous devez lui donner un nom. Effectuez ce qui suit :

1. Ouvrez le plan conceptuel et sélectionnez l’étiquette au bas de la vue de contrôleur.
2. Dans la zone Propriétés, sélectionnez le **Widget** onglet :

    [![](hello-tvos-images/name1.png "Sélectionnez l’onglet du Widget")](hello-tvos-images/name1.png#lightbox)
3. Sous **identité > nom**, ajouter `ClickedLabel`:

    [![](hello-tvos-images/name2.png "Ensemble ClickedLabel")](hello-tvos-images/name2.png#lightbox)

Nous sommes maintenant prêts à commencer la mise à jour de l’étiquette !

### <a name="how-controls-are-accessed"></a>Mode d’accès aux contrôles

Si vous sélectionnez le `ViewController.designer.cs` dans les **l’Explorateur de solutions** vous serez en mesure de voir comment la la `ClickedLabel` étiquette et la `Clicked` Gestionnaire d’événements ont été mappées à une **prise** et  **Action** en c# :

[![](hello-tvos-images/accesscontrol.png "Prises et Actions")](hello-tvos-images/accesscontrol.png#lightbox)

Vous pouvez également remarquer que `ViewController.designer.cs` est une classe partielle, afin que Visual Studio pour Mac ne doit pas nécessairement modifier `ViewController.cs` qui remplace toutes les modifications que nous avons apporté à la classe.

Exposer les éléments d’interface utilisateur de cette façon, vous permet d’y accéder dans le contrôleur de la vue.

Normalement, vous n’avez jamais besoin ouvrir le `ViewController.designer.cs` vous-même, il a été présenté ici à des fins pédagogiques uniquement.

<a name="Writing-the-Code" />

## <a name="writing-the-code"></a>Écriture du code

Avec notre Interface utilisateur créée et ses éléments d’interface utilisateur exposées au code via **prises** et **Actions**, nous sommes enfin prêts à écrire le code pour permettre à la fonctionnalité du programme.

Dans notre application, chaque fois que le premier bouton est activé, nous allons mettre à jour notre étiquette pour afficher le nombre de fois où le bouton a été utilisé. Pour ce faire, nous avons besoin ouvrir la `ViewController.cs` fichier pour le modifier en double-cliquant dessus dans le **Solution remplissage**:

[![](hello-tvos-images/code01.png "Le remplissage de la Solution")](hello-tvos-images/code01.png#lightbox)

Tout d’abord, nous devons créer une variable de niveau classe dans notre `ViewController` classe pour effectuer le suivi du nombre de clics qui se sont produites. Modifiez la définition de classe pour qu’elle ressemble à ce qui suit :

```csharp
using System;
using Foundation;
using UIKit;

namespace Hello_tvOS
{
    public partial class ViewController : UIViewController
    {
        private int numberOfTimesClicked = 0;
        ...
```

Ensuite, dans la même classe (`ViewController`), nous devons remplacer le **ViewDidLoad** (méthode) et ajoutez du code pour définir le message initial pour notre étiquette :

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Set the initial value for the label
    ClickedLabel.Text = "Button has not been clicked yet.";
}
```

Nous devons utiliser `ViewDidLoad `, au lieu d’une autre méthode telle que `Initialize`, car `ViewDidLoad ` est appelé *après* le système d’exploitation a chargé et instancié l’Interface utilisateur à partir de la `.storyboard` fichier. Si nous avons tenté d’accéder au contrôle label avant le `.storyboard` fichier a été entièrement chargé et instancié, vous obtenez un `NullReferenceException` erreur étant donné que le contrôle d’étiquette n’est pas encore créé.

Ensuite, nous devons ajouter le code pour répondre à l’utilisateur en cliquant sur le bouton. Ajoutez le code suivant partielle classe que nous avons créé :

```csharp
partial void Clicked (UIButton sender)
{
    ClickedLabel.Text = string.Format("The button has been clicked {0} time{1}.", ++numberOfTimesClicked, (numberOfTimesClicked
```

Ce code sera appelé à tout moment, l’utilisateur clique sur notre bouton.

Tous les éléments en place, nous sommes maintenant prêts générer et tester l’application Xamarin.tvOS.

## <a name="testing-the-application"></a>Test de l'application

Il est temps pour générer et exécuter votre application pour vous assurer qu’il s’exécute comme prévu. Nous pouvons créer et exécuter tout en une seule étape, ou nous pouvons générer sans l’exécuter.

Chaque fois que nous générer une application, nous pouvons choisir le type de build que nous voulons :

-   **Déboguer** – une version debug est compilée dans un « fichier (application) avec des métadonnées supplémentaires qui nous permet de déboguer ce qui se passe lors de l’application est en cours d’exécution.
-   **Version** – une version Release crée également un « fichier, mais il n’inclut pas les informations de débogage, il est plus petite et s’exécute plus vite.  

Vous pouvez sélectionner le type de build de la **Configuration sélecteur** dans l’angle supérieur gauche de Visual Studio pour l’écran de Mac :

[![](hello-tvos-images/run01.png "Sélectionnez le type de build")](hello-tvos-images/run01.png#lightbox)

### <a name="building-the-application"></a>Génération de l'application

Dans notre cas, nous voulons simplement une version debug, nous allons donc vous assurer que **déboguer** est sélectionnée. Commençons par créer notre application tout d’abord en appuyant sur la combinaison **⌘B**, ou à partir de la **générer** menu, choisissez **générer tout**.

S’il n’a pas été toutes les erreurs, vous verrez un **génération a réussi** message dans Visual Studio pour la barre d’état du Mac. Si les erreurs se sont produites, passez en revue votre projet et assurez-vous que vous avez suivi les étapes correctement. Démarrez en confirmant que votre code (à la fois dans Xcode et dans Visual Studio pour Mac) correspond au code dans le didacticiel.

### <a name="running-the-application"></a>Exécution de l'application

Pour exécuter l’application, nous avons trois options :

-  Appuyez sur **⌘+Entrée**.
-  Dans le menu **Exécuter**, choisissez **Déboguer**.
-  Cliquez sur le bouton **Play** (Lire) dans la barre d’outils Visual Studio pour Mac (juste au-dessus de l’**Explorateur de solutions**).

L’application générera (si elle n’a pas été construit déjà), démarrage en mode débogage, le simulateur tvOS démarre et pour lancer l’application et afficher sa fenêtre de l’interface principale :

[![L’écran d’accueil application exemple](hello-tvos-images/run03.png)](hello-tvos-images/run03.png#lightbox)

À partir de la **matériel** menu Sélectionnez **afficher Apple TV distant** afin de contrôler le simulateur.

[![](hello-tvos-images/run04.png "Sélectionnez Afficher Apple TV distant")](hello-tvos-images/run04.png#lightbox)

À l’aide à distance du simulateur, si vous cliquez sur le bouton plusieurs fois, l’étiquette doit être mis à jour avec le nombre de :

[![](hello-tvos-images/run05.png "L’étiquette dont le nombre de mises à jour")](hello-tvos-images/run05.png#lightbox)

Félicitations ! Nous avons abordé de nombreux points ici, mais si vous avez suivi ce didacticiel à partir du début à la fin, vous devez maintenant avoir une connaissance approfondie des composants d’une application Xamarin.tvOS, ainsi que les outils utilisés pour les créer.

## <a name="where-to-next"></a>Où suivant ?

Développement présente d’applications Apple TV quelques défis en raison de la déconnexion entre l’utilisateur et l’interface (il s’agit dans la salle pas en main de l’utilisateur) et les limitations tvOS place sur le stockage et la taille de l’application.

Par conséquent, nous suggérons hautement que votre lecture suivantes documents avant de passer à la conception d’une application Xamarin.tvOS :

- [Introduction à tvOS 9](~/ios/tvos/platform/tvos9.md) : cet article présente tous les nouveaux et modifiés API et fonctionnalités disponibles dans tvOS 9 pour les développeurs de Xamarin.tvOS.
- [Utilisation de Navigation et le Focus](~/ios/tvos/app-fundamentals/navigation-focus.md) – les utilisateurs de votre application Xamarin.tvOS pas interagir avec son interface directement en tant qu’iOS où ils appuyer sur les images sur l’écran du périphérique, mais indirectement à travers l’espace à l’aide de l’élément distant Siri. Cet article décrit le concept de Focus et comment il est utilisé pour gérer la Navigation dans l’Interface utilisateur d’une application Xamarin.tvOS.
- [Siri distants et les contrôleurs Bluetooth](~/ios/tvos/platform/remote-bluetooth.md) : la méthode principale que les utilisateurs d’interagir avec l’Apple TV et votre application Xamarin.tvOS, s’effectue via l’élément distant Siri inclus. Si votre application est un jeu, vous pouvez éventuellement générer prise en charge pour le tiers 3e, effectuées pour les contrôleurs de jeu de Bluetooth iOS (IFM) dans votre application ainsi. Cet article décrit la prise en charge les contrôleurs de jeu Siri distant et Bluetooth nouvelle dans vos applications Xamarin.tvOS.
- [Stockage des données et des ressources](~/ios/tvos/app-fundamentals/resources-data-storage.md) – Contrairement aux appareils iOS, le nouveau Apple TV ne fournit pas un stockage local pour les applications de tvOS. Par conséquent, si votre application Xamarin.tvOS doit conserver des informations (telles que les préférences de l’utilisateur) il doit stocker et récupérer ces données à partir d’iCloud. Cet article décrit l’utilisation des ressources et du stockage des données persistantes dans une application Xamarin.tvOS.
- [Utilisez des icônes et des Images](~/ios/tvos/app-fundamentals/icons-images.md) – création captivantes des icônes et des images sont un élément essentiel du développement d’une expérience utilisateur réalistes pour vos applications Apple TV. Ce guide couvre les étapes requises pour créer et inclure les éléments de graphique nécessaires pour vos applications Xamarin.tvOS.
- [Interface utilisateur](~/ios/tvos/user-interface/index.md) – couverture général utilisateur expérience (UX) y compris les contrôles d’Interface utilisateur (IU), utilisez Interface Générateur de Xcode et les principes de conception d’expérience utilisateur lorsque vous travaillez avec Xamarin.tvOS.
- [Déploiement et test](~/ios/tvos/deploy-test/index.md) – cette section traite des rubriques utilisés pour tester une application et la manière de le distribuer. Rubriques incluent des éléments tels que les outils utilisés pour le débogage, le déploiement pour les testeurs et comment publier une application sur l’App Store d’Apple TV.

Si vous rencontrez des problèmes avec Xamarin.tvOS, veuillez consulter notre [dépannage](~/ios/tvos/troubleshooting.md) documentation pour obtenir la liste de problèmes connus et solutions.

## <a name="summary"></a>Récapitulatif

Cet article a fourni un démarrage rapide pour le développement d’applications de tvOS avec Visual Studio pour Mac en créant un simple Hello, tvOS application. Il couvert les principes fondamentaux de l’appareil tvOS configuration, la création d’interfaces, de codage de tvOS et de test sur le simulateur de tvOS.


## <a name="related-links"></a>Liens associés

- [Exemples tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS les Guides Interface humaine](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Guide de programmation d’application de tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
- [Création d’applications tvOS avec Xamarin (vidéo)](https://university.xamarin.com/lightninglectures/tvos-with-xamarin)
