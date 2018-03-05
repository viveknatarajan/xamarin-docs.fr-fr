---
title: "Onglet Barres et les contrôleurs de la barre d’onglet"
description: "les applications iOS à l’aide d’une interface utilisateur de navigation de l’onglet sont générées à l’aide de la classe UITabBarController. Dans cet article, nous examinerons comment configurer une application à onglets qui contient plusieurs contrôleurs et vues. Nous examinerons ensuite comment charger un UITabBarController quand il n’est pas le contrôleur de la racine, par exemple après un écran de connexion."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7C772899-2900-F139-D642-F3C4F3F14DDC
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 2a10c161c49e7cd0d45d29522a98c0dc78f7adb7
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="tab-bars-and-tab-bar-controllers"></a>Onglet Barres et les contrôleurs de la barre d’onglet

_les applications iOS à l’aide d’une interface utilisateur de navigation de l’onglet sont générées à l’aide de la classe UITabBarController. Dans cet article, nous examinerons comment configurer une application à onglets qui contient plusieurs contrôleurs et vues. Nous examinerons ensuite comment charger un UITabBarController quand il n’est pas le contrôleur de la racine, par exemple après un écran de connexion._

Applications avec onglets sont utilisées dans iOS pour prendre en charge des interfaces utilisateur où plusieurs écrans peuvent être consultées dans aucun ordre particulier. Via le `UITabBarController` (classe), les applications peuvent inclure facilement prise en charge de tels scénarios écrans multiples. `UITabBarController` prend en charge de la gestion des écran multiples, ce qui permet au développeur de se concentrer sur les détails de chaque écran.

En règle générale, les onglets des applications sont générées avec le `UITabBarController` étant le `RootViewController` de la fenêtre principale. Toutefois, avec un peu de code supplémentaire, des applications à onglets permet également à la suite à certaines autres écran initial, telles que le scénario où une application présente tout d’abord un écran de connexion, suivi de l’interface à onglets.

Nous allons examiner à l’aide des onglets ici en passant par une procédure pas à pas d’une application simple. Ensuite, nous allons examiner comment travailler avec des onglets de non - `RootViewController` scénario.

## <a name="introducing-uitabbarcontroller"></a>Présentation de UITabBarController

Le `UITabBarController` prend en charge avec onglet développement d’applications par le texte suivant :

-  Autoriser plusieurs contrôleurs à être ajouté à celle-ci.
-  En fournissant une interface utilisateur à onglets, via le `UITabBar` (classe), pour autoriser un utilisateur basculer entre les contrôleurs et les vues. 


Contrôleurs sont ajoutés à la `UITabBarController` via son `ViewControllers` propriété, qui est un `UIViewController` tableau. Le `UITabBarController` elle-même gère le chargement du contrôleur approprié et en le présentant sa vue en fonction de l’onglet sélectionné.

Les onglets sont des instances de la `UITabBarItem` (classe), qui sont contenus dans un `UITabBar` instance. Chaque `UITabBar` instance est accessible via la `TabBarItem` propriété du contrôleur dans chaque onglet.

Pour comprendre le fonctionnement de l’utilisation avec le `UITabBarController`, nous allons étudier la création d’une application simple qui utilise une.

## <a name="tabbed-application-walkthrough"></a>Procédure pas à pas une Application avec onglets

Pour cette procédure pas à pas, nous allons créer l’application suivante :

[ ![](creating-tabbed-applications-images/00-app.png "Exemple d’application à onglets")](creating-tabbed-applications-images/00-app.png)

Bien qu’il existe déjà un modèle d’application à onglets disponibles dans Visual Studio pour Mac, pour cet exemple, nous allons travailler à partir d’un projet vide pour mieux comprendre la façon dont l’application est construite.

 <a name="Creating_the_Application" />


### <a name="creating-the-application"></a>Création de l’application

Commençons par créer une nouvelle application.

Sélectionnez le **fichier > Nouveau > Solution** élément de menu dans Visual Studio pour Mac, puis sélectionnez un **iOS > application > projet vide** modèle, nommez le projet `TabbedApplication`, comme indiqué ci-dessous :

[ ![](creating-tabbed-applications-images/newsolution1.png "Sélectionnez le modèle de projet vide")](creating-tabbed-applications-images/newsolution1.png)

[ ![](creating-tabbed-applications-images/newsolution2.png "Nommez le projet TabbedApplication")](creating-tabbed-applications-images/newsolution2.png)



### <a name="adding-the-uitabbarcontroller"></a>Ajout de la UITabBarController

Ensuite, ajoutez une classe vide en sélectionnant **fichier > nouveau fichier** et en choisissant le **général : classe vide** modèle. Nommez le fichier `TabController` comme indiqué ci-dessous :

[ ![](creating-tabbed-applications-images/02-newclass.png "Ajoutez la classe TabController")](creating-tabbed-applications-images/02-newclass.png)

Le `TabController` classe contiendra l’implémentation de la `UITabBarController` qui permettent de gérer un tableau de `UIViewControllers`. Lorsque l’utilisateur sélectionne un onglet, le `UITabBarController` s’occupe de présenter la vue pour le contrôleur de la vue appropriée.

Pour implémenter le `UITabBarController` nous devons effectuer les opérations suivantes :

1.  Définir la classe de base de `TabController` à `UITabBarController` . 
1.  Créer `UIViewController` instances à ajouter à la `TabController` . 
1.  Ajouter le `UIViewController` instances dans un tableau assigné à la `ViewControllers` propriété de la `TabController` . 


Ajoutez le code suivant à la `TabController` classe pour effectuer ces étapes :

```csharp
using System;
using UIKit;

namespace TabbedApplication {
        public class TabController : UITabBarController {

                UIViewController tab1, tab2, tab3;

                public TabController ()
                {
                        tab1 = new UIViewController();
                        tab1.Title = "Green";
                        tab1.View.BackgroundColor = UIColor.Green;

                        tab2 = new UIViewController();
                        tab2.Title = "Orange";
                        tab2.View.BackgroundColor = UIColor.Orange;

                        tab3 = new UIViewController();
                        tab3.Title = "Red";
                        tab3.View.BackgroundColor = UIColor.Red;

                        var tabs = new UIViewController[] {
                                tab1, tab2, tab3
                        };

                        ViewControllers = tabs;
                }
        }
}
```

Notez que pour chaque `UIViewController` instance, nous avons défini le `Title` propriété de la `UIViewController`. Lorsque les contrôleurs sont ajoutés à la `UITabBarController`, le `UITabBarController` lira le `Title` pour chaque contrôleur et affichez-le sur l’étiquette de l’onglet associé, comme indiqué ci-dessous :

[ ![](creating-tabbed-applications-images/00-app.png "L’exemple d’application de s’exécuter")](creating-tabbed-applications-images/00-app.png)

#### <a name="setting-the-tabcontroller-as-the-rootviewcontroller"></a>Définition de la TabController comme le RootViewController

L’ordre que les contrôleurs sont placées dans les onglets correspond à l’ordre qu’ils sont ajoutés à la `ViewControllers` tableau.

Pour obtenir le `UITabController` pour charger en tant que le premier écran, nous devons faire de la fenêtre `RootViewController`, comme illustré dans le code suivant pour le `AppDelegate`:

```csharp
[Register ("AppDelegate")]
        public partial class AppDelegate : UIApplicationDelegate
        {
                UIWindow window;
                TabController tabController;

                public override bool FinishedLaunching (UIApplication app, NSDictionary options)
                {
                        window = new UIWindow (UIScreen.MainScreen.Bounds);

                        var tabController = new TabController ();
                        window.RootViewController = tabController;

                        window.MakeKeyAndVisible ();
            
                        return true;
                }
        }
```

Si nous exécutons l’application maintenant, le `UITabBarController` sera chargé avec le premier onglet sélectionné par défaut. Affichage des résultats dans le contrôleur associé d’en sélectionnant une des autres onglets présentées par le `UITabBarController,` comme indiqué ci-dessous dans laquelle l’utilisateur a sélectionné le deuxième onglet :

[ ![](creating-tabbed-applications-images/03-secondtab.png "Le deuxième onglet indiqué")](creating-tabbed-applications-images/03-secondtab.png)

 <a name="Modifying_TabBarItems" />


### <a name="modifying-tabbaritems"></a>Modification TabBarItems

Maintenant que nous avons une exécution onglet application, nous allons modifier le `TabBarItem` pour modifier l’image et le texte qui s’affiche, ainsi que pour ajouter un badge à un des onglets.

 <a name="Setting_a_System_Item" />


#### <a name="setting-a-system-item"></a>Définition d’un élément du système

Tout d’abord, nous allons définir le premier onglet à utiliser un élément du système. Dans le constructeur de la `TabController`, supprimez la ligne qui définit le nom du contrôleur `Title` pour le `tab1` de l’instance et le remplacer par le code suivant pour définir le nom du contrôleur `TabBarItem` propriété :

```csharp
tab1.TabBarItem = new UITabBarItem (UITabBarSystemItem.Favorites, 0);
```

Lorsque vous créez le `UITabBarItem` à l’aide un `UITabBarSystemItem`, le titre et l’image sont fournies automatiquement par iOS, comme indiqué dans la capture d’écran ci-dessous montrant le **favoris** icône et le titre sur le premier onglet :

 ![](creating-tabbed-applications-images/04a-tabimage.png "Le premier onglet avec une icône représentant une étoile")

 <a name="Setting_the_Title_and_Image" />


#### <a name="setting-the-title-and-image"></a>Définir le titre et l’Image

Outre l’utilisation d’un élément du système, le titre et l’image d’un `UITabBarItem` peut être défini sur des valeurs personnalisées. Par exemple, modifiez le code qui définit les `TabBarItem` propriété du contrôleur nommé `tab2` comme suit :

```csharp
tab2 = new UIViewController ();
tab2.TabBarItem = new UITabBarItem ();
tab2.TabBarItem.Image = UIImage.FromFile ("second.png");
tab2.TabBarItem.Title = "Second";
tab2.View.BackgroundColor = UIColor.Orange;
```

Le code ci-dessus suppose une image nommée `second.png` a été ajouté à la racine du projet dans Visual Studio pour Mac. Nous avons en fait ajouté trois images à notre projet, pour couvrir toutes les résolutions de périphérique, comme indiqué ci-dessous :

 [ ![](creating-tabbed-applications-images/tabbedimages7new.png "Images ajoutées au projet")](creating-tabbed-applications-images/tabbedimages7new.png)

L’image de l’onglet doit être une image png 30 x 30 avec une transparence de résolution normales, 60 x 60 pour haute résolution et 90 x 90 pour iPhone 6 Plus la résolution. Dans notre code, nous avons besoin uniquement de charger le fichier nommé `second.png` et iOS se chargera automatiquement la haute résolution sur les appareils avec un écran retina. Vous pouvez en savoir plus à ce sujet dans le [utilisation des Images](~/ios/app-fundamentals/images-icons/index.md) guides. Par défaut, éléments de barre d’onglet sont gris, avec une teinte bleue lorsque sélectionné.

**Remarque**

Les images ci-dessus peut également être ajoutés à la **ressources** directory, qui est un répertoire spécifique dont le contenu est automatiquement copié vers la racine de l’offre groupée d’application :

[ ![](creating-tabbed-applications-images/tabbedapplication8.png "Les images en tant que ressources")](creating-tabbed-applications-images/tabbedapplication8.png)

En outre, lorsque nous avons défini le `Title` propriété directement suite le `TabBarItem`, elle remplace toute valeur définie pour `Title` sur le contrôleur lui-même.

Lorsque nous exécutons l’application maintenant, le deuxième onglet montre nos titre personnalisé et une image comme indiqué ci-dessous :

[ ![](creating-tabbed-applications-images/05-customtab.png "Le deuxième onglet avec une icône carrée")](creating-tabbed-applications-images/05-customtab.png)

 <a name="Setting_the_Badge_Value" />


#### <a name="setting-the-badge-value"></a>Définition de la valeur de Badge

Un onglet peut également afficher un badge. Par exemple, ajoutez la ligne suivante de code pour définir un badge sur le troisième onglet :

```csharp
tab3.TabBarItem.BadgeValue = "Hi";
```

L’exécution de ce entraîne une étiquette rouge avec la chaîne « Hi » dans le coin supérieur gauche de l’onglet comme indiqué ci-dessous :

[ ![](creating-tabbed-applications-images/06-badge.png "Le deuxième onglet avec un badge Hi")](creating-tabbed-applications-images/06-badge.png)

Le badge est souvent utilisé pour afficher une indication numérique non lue, nouveaux éléments. Pour supprimer le badge, définissez la `BadgeValue` NULL comme indiqué ci-dessous :

```csharp
tab3.TabBarItem.BadgeValue = null;
```

 <a name="Tabs_in_Non-RootViewController_Scenarios" />


## <a name="tabs-in-non-rootviewcontroller-scenarios"></a>Onglets dans les scénarios Non-RootViewController

Dans l’exemple ci-dessus, nous vous avons montré comment travailler avec un `UITabBarController` lorsqu’il est le `RootViewController` de la fenêtre. Dans cet exemple, nous allons examiner comment utiliser un `UITabBarController` lorsqu’il n’est pas le `RootViewController` et afficher cette création d’utiliser des plans conceptuels.

 <a name="Initial_Screen_Example" />


### <a name="initial-screen-example"></a>Exemple de l’écran initial

Pour ce scénario, l’écran initial est chargé à partir d’un contrôleur qui n’est pas un `UITabBarController`. Quand l’utilisateur interagit avec l’écran en cliquant sur un bouton, le même contrôleur de la vue seront chargé dans un `UITabBarController`, qui est ensuite présenté à l’utilisateur. La capture d’écran suivante montre le flux d’application :

[ ![](creating-tabbed-applications-images/inital-screen-application.png "Cette capture d’écran montre le flux d’application")](creating-tabbed-applications-images/inital-screen-application.png)

Nous pouvons commencer une nouvelle application pour cet exemple. Là encore, nous allons utiliser la **iPhone > applications > projet vide (c#)** modèle, cette fois le projet d’affectation de noms `InitialScreenDemo`.


Dans cet exemple, nous aurons besoin une table de montage séquentiel pour contenir les contrôleurs de la vue. Pour ajouter un plan conceptuel :

- Avec le bouton droit sur le nom du projet, puis sélectionnez **Ajouter > nouveau fichier**.

- Lorsque la boîte de dialogue Nouveau fichier s’affiche, accédez à **iOS > vide iPhone Storyboard**.

Appelons cette table de montage séquentiel **MainStoryboard** , comme illustré ci-dessous : 

[ ![](creating-tabbed-applications-images/new-file-dialog.png "Ajouter un fichier MainStoryboard au projet")](creating-tabbed-applications-images/new-file-dialog.png)

Il existe quelques étapes importantes à noter lors de l’ajout d’un plan conceptuel vers un fichier précédemment non-plan conceptuel, qui sont traitées dans le [Introduction à des plans conceptuels](~/ios/user-interface/storyboards/index.md) guide. Ces équivalents sont :

 
1. Ajoutez votre nom de la table de montage séquentiel à la **Interface principale** section de la `Info.plist`:

    [![](creating-tabbed-applications-images/project-options.png "La valeur de l’Interface principale MainStoryboard")](creating-tabbed-applications-images/project-options.png)
1. Dans votre `App Delegate`, substituez la méthode de la fenêtre, avec le code suivant :

    ```csharp
    public override UIWindow Window {
      get;
      set;
    }
    ```

Nous allons d’avoir besoin de trois contrôleurs d’affichage pour cet exemple. Un, nommé `ViewController1`, sera utilisé en tant que notre contrôleur d’affichage Initial et le premier onglet. Les deux autres, nommés `ViewController2` et `ViewController3`, qui sera utilisée dans les onglets de la deuxième et troisième respectivement.

Ouvrez le concepteur en double-cliquant sur le fichier MainStoryboard.storyboard et faites glisser trois contrôleurs d’affichage dans l’aire de conception. Nous voulons que chacun de ces contrôleurs de la vue d’avoir leur propre classe correspondant au nom ci-dessus, c’est le cas, sous **identité > classe**, tapez son nom, comme illustré dans la capture d’écran ci-dessous :

[ ![](creating-tabbed-applications-images/class-name.png "La valeur de la classe ViewController1")](creating-tabbed-applications-images/class-name.png)

Visual Studio pour Mac génère automatiquement les classes et les fichiers de concepteur nécessaires, ceci peut être observé dans la zone de la Solution, comme illustré ci-dessous :

[ ![](creating-tabbed-applications-images/solution-pad2.png "Généré automatiquement les fichiers dans le projet")](creating-tabbed-applications-images/solution-pad2.png)

 <a name="Creating_the_UI" />


#### <a name="creating-the-ui"></a>Création de l’interface utilisateur

Ensuite, nous allons créer une interface utilisateur simple pour chacune des vues de la ViewController à l’aide de Xamarin iOS concepteur.

Nous souhaitons faire glisser un `Label` et un `Button` sur ViewController1 à partir de la **boîte à outils** sur le côté droit. Ensuite, nous allons utiliser la zone de propriétés pour modifier le nom et le texte des contrôles à ce qui suit :

-  **Étiquette** : `Text`  =  **une**
-  **Bouton** : `Title`  =  **utilisateur exécute une Action initiale**


Nous contrôlera la visibilité du bouton de dans un `TouchUpInside` événement et nous devons faire référence dans le code-behind. Nous allons identifier avec le **nom** `aButton` dans la zone Propriétés, comme illustré dans la capture d’écran suivante :

[ ![](creating-tabbed-applications-images/abutton-properties.png "Le nom de la valeur aButton dans la zone de propriétés")](creating-tabbed-applications-images/abutton-properties.png)

Votre Surface de conception doit maintenant ressembler à la capture d’écran ci-dessous :

[ ![](creating-tabbed-applications-images/design-surface1.png "Votre Surface de conception doit maintenant ressembler à cette capture d’écran")](creating-tabbed-applications-images/design-surface1.png)

Vous allez ajouter un peu plus en détail à `ViewController2` et `ViewController3`, en ajoutant une étiquette à chaque et en modifiant le texte 'Deux' et 'Trois' respectivement. Cela met en surbrillance à l’utilisateur que nous examinons onglet/vue.

#### <a name="wiring-up-the-button"></a>Le bouton de câblage

Nous allons charger `ViewController1` lorsque l’application démarre tout d’abord. Quand l’utilisateur appuie sur le bouton, nous allons masquer le bouton et charger un `UITabBarController` avec la `ViewController1` instance dans le premier onglet.

Lorsque l’utilisateur relâche le `aButton`, nous souhaitons un événement TouchUpInside doit être déclenchée. Sélectionnez le bouton, puis, dans le **onglet événements** de la zone Propriétés, déclarez le Gestionnaire d’événements – `InitialActionCompleted` – afin qu’il peut apparaître dans le code. Ceci est illustré dans la capture d’écran ci-dessous :

[ ![](creating-tabbed-applications-images/event-handler.png "Lorsque l’utilisateur relâche l’aButton, déclencher un événement de TouchUpInside")](creating-tabbed-applications-images/event-handler.png)

Nous devons maintenant indiquer le contrôleur d’affichage pour masquer le bouton lorsque l’événement se déclenche `InitialActionCompleted`. Dans `ViewController1`, ajoutez la méthode partielle suivante :

```csharp
partial void InitialActionCompleted (UIButton sender)
    {
      aButton.Hidden = true;  
    }
```

Enregistrez le fichier et exécuter l’application. Nous devons voir écran un s’affiche et le bouton disparaissent sur retoucher.

#### <a name="adding-the-tab-bar-controller"></a>Ajout de la barre d’onglet contrôleur

Nous disposons de notre affichage Initial fonctionne comme prévu. Ensuite, nous souhaitons ajouter à un `UITabBarController`, ainsi que les vues 2 et 3. Ouvrez le plan conceptuel dans le concepteur.

Dans le **boîte à outils**, recherchez le **onglet barre contrôleur** contrôleurs de & objets déplacer sur l’aire de conception. Comme vous pouvez le voir dans la capture d’écran ci-dessous, le contrôleur de la barre d’onglet est sans interface utilisateur et met donc les deux contrôleurs de la vue avec lui par défaut :

[ ![](creating-tabbed-applications-images/tabbarcontroller.png "Ajoutez un contrôleur de barre d’onglet à la disposition")](creating-tabbed-applications-images/tabbarcontroller.png)

Supprimez ces nouveaux contrôleurs de vue en sélectionnant la barre noire au bas et sur la touche SUPPR.

Dans notre plan conceptuel, nous pouvons utiliser Segues pour traiter les transitions entre le TabBarController et les contrôleurs de la vue. Après l’interaction avec la vue initiale, nous souhaitons charger dans le TabBarController présentée à l’utilisateur. Nous allons configurer dans le concepteur.

**CTRL + clic** et **glisser** à partir du bouton à la TabBarController. Sur la souris, un menu contextuel s’affiche. Vous souhaitez utiliser un segue modale. 
 
Pour configurer chacun de ses onglets, **Ctrl + clic** à partir de la TabBarController à chacune de nos contrôleurs de la vue dans l’ordre d’un à trois, puis sélectionnez la relation **onglet** dans le menu contextuel, comme illustré ci-dessous :

[ ![](creating-tabbed-applications-images/context-menu.png "Sélectionnez l’onglet relation")](creating-tabbed-applications-images/context-menu.png)

Votre plan conceptuel doit ressembler à la capture d’écran ci-dessous :

[ ![](creating-tabbed-applications-images/segue-layout.png "Le plan conceptuel doit ressembler à cette capture d’écran")](creating-tabbed-applications-images/segue-layout.png)

Si nous cliquez sur l’un des éléments de barre d’onglet et Explorer le panneau Propriétés, vous pouvez voir un nombre de différentes options, comme illustré ci-dessous :

[ ![](creating-tabbed-applications-images/properties-panel.png "Définir les options de l’onglet dans l’Explorateur de propriétés")](creating-tabbed-applications-images/properties-panel.png)

Nous pouvons l’utiliser pour modifier certains attributs tels que le badge, le titre et le fichier iOS [identificateur](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/UIKitUICatalog/TabBarItem.html), entre autres

Si nous enregistrer et exécuter l’application maintenant, nous allons trouvé que le bouton s’affiche de nouveau lorsque l’instance de ViewController1 est chargé dans le TabBarController. Nous allons résoudre ce problème en vérifiant si la vue actuelle est un contrôleur de la vue parent. Dans ce cas, nous savons que nous sommes dans le TabBarController, et par conséquent, le bouton doit être masqué. Nous allons ajouter le code suivant à la classe ViewController1 :

```csharp
public override void ViewDidLoad ()
{
     if (ParentViewController != null){
       aButton.Hidden = true;
     }

}
```

Lorsque l’application s’exécute et l’utilisateur appuie sur le bouton dans le premier écran, le UITabBarController est chargé avec la vue dans le premier écran placé dans le premier onglet, comme indiqué ci-dessous :

[ ![](creating-tabbed-applications-images/first-view.png "L’exemple de sortie d’application")](creating-tabbed-applications-images/first-view.png)

<!--Save the files and run the application:

[ ![](creating-tabbed-applications-images/inital-screen-application.png "Save the files and run the application")](creating-tabbed-applications-images/inital-screen-application.png)-->

## <a name="summary"></a>Récapitulatif

Cet article expliqué comment utiliser un `UITabBarController` dans une application. Nous avons décrit comment charger des contrôleurs dans chaque onglet, ainsi que la définition des propriétés sur les onglets ce titre, image et badge. Nous avons examiné puis, à l’aide de plans conceptuels, comment charger un `UITabBarController` lors de l’exécution lorsqu’il n’est pas le `RootViewController` de la fenêtre.


## <a name="related-links"></a>Liens associés

- [Création d’Applications avec onglet (exemple)](https://developer.xamarin.com/samples/monotouch/CreatingTabbedApplications/)
- [Images.zip](https://github.com/xamarin/ios-samples/blob/master/CreatingTabbedApplications/Resources/images.zip?raw=true)
- [Référence de classe de UITabBarController](http://developer.apple.com/library/ios/#documentation/uikit/reference/UITabBarController_Class/Reference/Reference.html)
