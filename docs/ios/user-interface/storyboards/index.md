---
title: Introduction à des plans conceptuels
description: Un Storyboard est une représentation visuelle de l’apparence et les flux de votre application. Xamarin propose un concepteur pour permettre aux applications de Xamarin.iOS tirer parti des plans conceptuels, afin de pouvoir concevoir visuellement de l’écran de votre application et accéder aux vues, contrôleurs et est parfait pour avec c# pour plus de contrôle.
ms.prod: xamarin
ms.assetid: A3339BD2-9F56-7965-25F5-4B7C991EB775
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 647bd7d339dc56978752f7ab29de30cf8acb7e07
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-storyboards"></a>Introduction à des plans conceptuels

_Un Storyboard est une représentation visuelle de l’apparence et les flux de votre application. Xamarin propose un concepteur pour permettre aux applications de Xamarin.iOS tirer parti des plans conceptuels, afin de pouvoir concevoir visuellement de l’écran de votre application et accéder aux vues, contrôleurs et est parfait pour avec c# pour plus de contrôle._

Dans ce guide, nous allons expliquer l’un Storyboard est et examiner certains des principaux composants, tels que Segues. Nous allons examiner comment les plans conceptuels peuvent être créés et utilisés, et quels sont les avantages dont ils disposent d’un développeur.

Avant que le format de fichier Storyboard a été introduit par Apple en une représentation visuelle de l’interface utilisateur d’une application iOS, les développeurs créé pour chaque contrôleur d’affichage des fichiers XIB et programmer la navigation entre chaque vue manuellement.  À l’aide d’un Storyboard permet au développeur de définir des contrôleurs de la vue et la navigation entre eux sur une aire de conception et permet la modification WYSIWYG de l’interface utilisateur de l’application.

Un plan conceptuel peut être créé, ouverts et modifié avec Xamarin iOS concepteur. Ce guide effectue également une procédure pas à pas comment utiliser le concepteur pour créer vos plans conceptuels lors de l’utilisation de c# pour programmer le volet de navigation.


## <a name="requirements"></a>Spécifications

Storyboards utilisable avec le concepteur dans Visual Studio pour Mac iOS ou avec Visual Studio 2015 et 2017 avec les charges de travail Xamarin installés.

## <a name="what-is-a-storyboard"></a>Qu’est une table de montage séquentiel ?

Un Storyboard est la représentation visuelle de tous les écrans dans une application. Il contient une séquence d’arrière-plan, chacun représentant de scène un *View Controller* et son *vues*. Ces vues peuvent contenir des objets et [contrôles](~/ios/user-interface/controls/index.md) qui permet à votre utilisateur d’interagir avec votre application. Cette collection de contrôles et des vues (ou *sous-vues*) est appelé un *contenu afficher la hiérarchie*. En arrière-plan est connectés par segue objets qui représentent une transition entre les contrôleurs de la vue. Normalement, cela est possible en créant un segue entre un objet sur la vue initiale et la vue de connexion. Les relations sur l’aire de conception sont illustrées dans l’image ci-dessous :

 [![](images/storyboardsview.png "Les relations sur l’aire de conception sont illustrées dans cette image")](images/storyboardsview.png#lightbox)

Comme indiqué, le plan conceptuel chacun de vos scènes sera composer avec du contenu déjà et illustre les connexions entre eux.  Il est important de noter à ce stade, lorsque nous parlons en arrière-plan sur un iPhone, il est risqué de supposer qu’un *scène* sur le plan conceptuel est égale à un *écran* du contenu sur le périphérique. Toutefois, avec un iPad, qu'il est possible d’avoir plusieurs scènes apparaissent à la fois, par exemple, à l’aide un contrôleur de vue Popover.

Il existe de nombreux avantages à l’utilisation de tables de montage séquentiel pour créer l’interface utilisateur de votre application, surtout lorsque vous utilisez Xamarin. Tout d’abord, il s’agit d’une représentation visuelle de l’interface utilisateur, comme tous les objets, y compris [contrôles personnalisés](~/ios/user-interface/designer/ios-designable-controls-overview.md) – sont rendus au moment du design. Cela signifie qu’avant la génération ou le déploiement de votre application, vous pouvez visualiser son apparence et le flux. Prenez l’image ci-dessus, par exemple. Nous pouvons le constater à partir d’un coup de œil comment tout est lié de la conception de la surface d’exposition scènes combien il et la disposition de chaque vue. C’est ce que fait Storyboards aussi puissant.

Les événements sont plus faciles à gérer avec des plans conceptuels, surtout si vous utilisez le concepteur iOS. La plupart des contrôles d’interface utilisateur possède une liste des événements possibles dans la zone de propriétés. Le Gestionnaire d’événements peut être ajouté ici et exécutée dans une méthode partielle dans la classe d’affichage des contrôleurs...

Le contenu d’un storyboard est stocké dans un fichier XML. AT heure de création, tout `.storyboard` fichiers sont compilés dans des fichiers binaires appelés plumes. Lors de l’exécution, ces plumes initialisées et instanciés pour créer des vues.

## <a name="segues"></a>Est parfait pour

A *Segue*, ou *Segue un objet*, est utilisé dans le développement d’iOS pour représenter une transition entre les scènes. Pour créer un segue, maintenez la **Ctrl** clé et cliquez sur, faites glisser une scène à un autre. Lorsque nous faites glisser la souris, un connecteur bleu s’affiche, indiquant où le segue entraîne comme illustré dans l’image ci-dessous :

 [![](images/createsegue.png "Un connecteur bleu s’affiche, indiquant où le segue entraîne comme illustré dans cette image")](images/createsegue.png#lightbox)

Sur la souris, un menu s’affiche permet de choisir l’action pour notre segue. Il peut ressembler aux images ci-dessous : 

**Préliminaire iOS 8 et taille des Classes de**:

[![](images/segue1.png "La liste de déroulante Action Segue sans taille de Classes")](images/segue1.png#lightbox)

**Lors de l’utilisation des Classes de taille et est parfait pour Adaptive**:

[![](images/16new.png "La liste déroulante de Segue d’Action avec les Classes de taille")](images/16new.png#lightbox)

> [!IMPORTANT]
> Si vous utilisez VMWare pour votre Machine virtuelle de Windows, Ctrl + clic est mappé en tant que le _avec le bouton droit_ bouton par défaut. Pour créer un Segue, modifiez vos préférences de clavier via **préférences** > **clavier et souris** > **raccourcis souris** et remapper votre **Bouton secondaire** comme illustré ci-dessous :
> 
> [![](images/image22.png "Paramètres de préférence de clavier et souris")](images/image22.png#lightbox)
> 
> Vous devez maintenant être en mesure d’ajouter un segue entre vos contrôleurs de la vue comme d’habitude.

Il existe différents types de transitions, chaque contrôle donnant sur la manière dont un nouveau contrôleur de la vue est présenté à l’utilisateur et leur interaction avec d’autres contrôleurs de la vue dans le plan conceptuel. Ceux-ci sont expliquées ci-dessous. Il est également possible de sous-classe d’un objet segue pour implémenter une transition personnalisée :

-  **Afficher / Push** – un push segue ajoute le contrôleur de la vue à la pile de navigation. Il suppose que le contrôleur de vue le push d’origine fait partie du même contrôleur que le contrôleur de vue qui est ajouté à la pile de navigation. Cela produit la même chose que `pushViewController` et est généralement utilisé lorsqu’il existe une relation entre les données sur les écrans. À l’aide de l’installation poussée du segue vous donne le luxe de disposer d’une barre de navigation avec un bouton Précédent et le titre ajoutés à la vue sur la pile, ce qui permet la navigation dans la hiérarchie d’exploration.
-  **Modal** – un segue modale créer une relation entre des contrôleurs de deux vue dans votre projet, avec la possibilité d’une transition animée est affichée. Le contrôleur de la vue enfant masquera complètement le contrôleur de vue parent lors de la mise en vue. Contrairement à une notification push segue, qui ajoute un bouton Précédent pour nous ; Lorsque à l’aide de modal segue `DismissViewController` doit être utilisée pour retourner au contrôleur de vue précédente.
-  **Personnalisé** – tout personnalisé segue peut être créé comme une sous-classe de ` UIStoryboardSegue`.
-  **Déroulement** : déroulement segue peut être utilisé pour naviguer émetteur ou modal segue – par exemple, en fermant le contrôleur présentées de façon modale l’affichage. En outre, vous pouvez dérouler via pas qu’un seul, mais une série de push et modal est parfait pour et revenir plusieurs étapes dans votre hiérarchie de navigation avec une seule action de déroulement. Pour comprendre comment utiliser un déroulement segue dans iOS, lire la [création de déroulement est parfait pour](https://developer.xamarin.com/recipes/ios/general/storyboard/unwind_segue/) recette.
-  **Sans source** – un segue sans source indique la séquence contenant le contrôleur de la vue initiale et par conséquent, ce qui permet d’afficher l’utilisateur verrez tout d’abord. Il est représenté par la segue indiqué ci-dessous :  

    [![](images/sourcelesssegue.png "Un segue sans source")](images/sourcelesssegue.png#lightbox)

### <a name="adaptive-segue-types"></a>Types de Segue adaptative

 iOS 8 introduit [taille Classes](~/ios/user-interface/storyboards/unified-storyboards.md#size-classes) pour autoriser un fichier de plan conceptuel iOS travailler avec toutes les tailles d’écran disponible, permettant aux développeurs de créer une interface utilisateur pour tous les appareils iOS. Par défaut, toutes les nouvelles applications Xamarin.iOS utilise les classes de taille. Pour utiliser les classes de taille à partir d’un projet antérieur, reportez-vous à la [présentation unifiée des animations](~/ios/user-interface/storyboards/unified-storyboards.md) guide. 
 
N’importe quelle application à l’aide des Classes de taille utiliseront la nouvelle [ *est parfait pour Adaptive*](~/ios/user-interface/storyboards/unified-storyboards.md). Lorsque vous utilisez les classes de taille, n’oubliez pas que nous ne directement en spécifiant s’il convient, nous utilisons un iPhone ou un iPad. En d’autres termes, nous allons créer une interface utilisateur que recherche toujours la même, quelle que soit la quantité immobilier il doit fonctionner avec. Travail Segues adaptative en évaluer l’environnement et déterminer la meilleure façon de présenter le contenu. L’est parfait pour Adaptive figurent ci-dessous : 

[![](images/adaptivesegue.png "La liste déroulante est parfait pour adaptative")](images/adaptivesegue.png#lightbox)

|Segue|Description|
|--- |--- |
|Afficher|Cela est très similaire à une notification Push segue, mais il prend le contenu de l’écran en compte.|
|Afficher les détails|Si l’application affiche une vue maître / détail (par exemple, dans un contrôleur de vue de division sur un iPad), le contenu remplace la vue de détail. Si l’application affiche uniquement la principale ou détail, le contenu remplace le haut de la pile de contrôleur d’affichage.|
|La présentation|Cela est similaire à la segue modale et autorise la sélection de styles de présentation et de transition.|
|Présentation de popover|Cela présente le contenu sous la forme d’un popover|

### <a name="transferring-data-with-segues"></a>Transfert de données avec est parfait pour

Les avantages d’un segue ne se terminent avec les transitions. Ils peuvent également servir à gérer le transfert de données entre les contrôleurs de la vue. Cela est possible en remplaçant le `PrepareForSegue` méthode sur le contrôleur de la vue initiale et la gestion des données nous-mêmes. Au déclenchement de le segue – par exemple, avec un appui sur le bouton – l’application appelle cette méthode, ce qui permet de préparer le nouveau contrôleur de vue *avant* aucune navigation se produit. Le code ci-dessous, à partir de la [Phoneword](https://developer.xamarin.com/samples/monotouch/Hello_iOS/) exemple, illustre cela : 


```csharp
public override void PrepareForSegue (UIStoryboardSegue segue, 
NSObject sender)
{
    base.PrepareForSegue (segue, sender);

    var callHistoryContoller = segue.DestinationViewController 
                                  as CallHistoryController;

    if (callHistoryContoller != null) {
        callHistoryContoller.PhoneNumbers = PhoneNumbers;
    }
}
```

Dans cet exemple, le `PrepareForSegue` méthode sera appelée lorsque le segue est déclenchée par l’utilisateur. Nous devons d’abord créer une instance du contrôleur d’affichage « réception » et de définir cette valeur en tant que destination de la segue contrôleur d’affichage. Cela est effectué par la ligne de code ci-dessous :

```csharp
var callHistoryContoller = segue.DestinationViewController as CallHistoryController;
```

La méthode a désormais la possibilité de définir des propriétés sur le `DestinationViewController`. Dans cet exemple, nous avons pris tirer parti en passant une liste appelée `PhoneNumbers` à la `CallHistoryController` et en l’assignant à un objet du même nom :

```csharp
if (callHistoryContoller != null) {
        callHistoryContoller.PhoneNumbers = PhoneNumbers;
    }
```

Une fois la transition terminée, l’utilisateur voit le `CallHistoryController` avec la liste.

## <a name="adding-a-storyboard-to-a-non-storyboard-project"></a>Ajout d’un plan conceptuel à un projet en table de montage séquentiel

Il arrive que vous devrez peut-être ajouter une animation à un fichier précédemment non-plan conceptuel. Cette opération qu’une seule fois dans Visual Studio pour Mac peut être affinée en suivant les étapes ci-dessous :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1. Créer un nouveau fichier de plan conceptuel en naviguant vers **fichier > nouveau fichier > iOS > Storyboard**, comme illustré ci-dessous : 
    
    [![](images/new-storyboard-xs.png "La nouvelle boîte de dialogue")](images/new-storyboard-xs.png#lightbox)

2. Ajoutez votre nom de la table de montage séquentiel à la **Interface principale** section de la **Info.plist**, comme indiqué ci-dessous :
    
    [![](images/infoplist.png "L’éditeur de fichier Info.plist.")](images/infoplist.png#lightbox)
    
    Il effectue l’équivalent de l’instanciation le contrôleur de la vue initiale dans la `FinishedLaunching` méthode dans le délégué de l’application. Avec cette option, l’application instancie une fenêtre (voir ci-dessous), charge le storyboard principal et affecte une instance de contrôleur d’affichage Initial de la table de montage séquentiel (situé en regard de la Segue sans source) en tant que le `RootViewController` propriété de la fenêtre, puis rend la fenêtre est visible à l’écran.

3. Dans le `AppDelegate`, remplacez la valeur par défaut `Window` méthode, avec le code suivant pour implémenter la propriété de la fenêtre :
        
        public override UIWindow Window {
            get;
            set;
            }
            
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Créer un nouveau fichier de table de montage séquentiel en cliquant sur le projet à **Ajouter > nouveau fichier > iOS > Storyboard vide**, comme illustré ci-dessous : 
    
    [![](images/new-storyboard-vs.png "La boîte de dialogue Nouvel élément")](images/new-storyboard-vs.png#lightbox)

2. Ajoutez votre nom de la table de montage séquentiel à la **Interface principale** section de l’application iOS Application, comme indiqué ci-dessous :
    
    [![](images/ios-app.png "L’éditeur de fichier Info.plist.")](images/ios-app.png#lightbox)
    
    Il effectue l’équivalent de l’instanciation le contrôleur de la vue initiale dans la `FinishedLaunching` méthode dans le délégué de l’application. Avec cette option, l’application instancie une fenêtre (voir ci-dessous), charge le storyboard principal et affecte une instance de contrôleur d’affichage Initial de la table de montage séquentiel (situé en regard de la Segue sans source) en tant que le `RootViewController` propriété de la fenêtre, puis rend la fenêtre est visible à l’écran.

3. Dans le `AppDelegate`, remplacez la valeur par défaut `Window` méthode, avec le code suivant pour implémenter la propriété de la fenêtre :

        public override UIWindow Window {
            get;
            set;
            }
            
-----

## <a name="creating-a-storyboard-with-the-ios-designer"></a>Création d’une table de montage séquentiel avec le concepteur iOS

Un plan conceptuel peut être créé à l’aide du Concepteur de Xamarin pour iOS, ce qui a été intégré en toute transparence à Visual Studio pour Mac et Visual Studio.

Pour commencer à utiliser le concepteur iOS pour créer des animations, suivez les [Hello, iOS Multiscreen](~/ios/get-started/hello-ios-multiscreen/index.md) guide. Dans cette procédure pas à pas, vous allez Explorer navigation entre les contrôleurs de la vue à l’aide de Segues et comment gérer les événements de vos contrôles.

## <a name="instantiate-storyboards-manually"></a>Instancier des animations manuellement

Storyboards remplacent totalement les fichiers XIB individuels dans votre projet, mais les contrôleurs vue individuelle dans une table de montage séquentiel peuvent toujours être instanciés à l’aide `Storyboard.InstantiateViewController`.

Parfois, les applications ont des exigences spéciales qui ne peuvent pas être gérées avec les transitions de plan conceptuel intégrés fournies par le concepteur. Par exemple, si vous deviez créer une application qui lance différents écrans à partir du bouton de même, selon l’état actuel d’une application, nous pouvons souhaitez instancier l’affichage des contrôleurs manuellement et un programme de la transition nous-mêmes.

La capture d’écran ci-dessous montre les deux contrôleurs de la vue sur l’aire de conception de nos sans aucune segue entre eux. La section suivante vous guide dans la manière dont cette transition peut être configurée dans le code.

 [![](images/viewcontrollerspink.png "Cette capture d’écran montre deux contrôleurs de la vue sur l’aire de conception sans aucune segue entre eux")](images/viewcontrollerspink.png#lightbox)

1. Ajouter un _vide iPhone Storyboard_ à un projet existant de projet :
    
    [![](images/add-storyboard1.png "Ajout de table de montage séquentiel")](images/add-storyboard1.png#lightbox)

2. Double-cliquez sur la table de montage séquentiel qui vient d’être créé pour l’ouvrir et ajouter un nouveau **Navigation contrôleur** à l’aire de conception. Comme le contrôleur de Navigation est sans interface utilisateur, par défaut, celle-ci sera accompagnée d’un contrôleur de vue racine, comme illustré ci-dessous :

    [![](images/uinavigationcontroller.png "Affichage des contrôleurs avec est parfait pour")](images/uinavigationcontroller.png#lightbox)

3. Sélectionnez le _View Controller_ en cliquant sur la barre noire en bas. Dans du concepteur **remplissage de la propriété**, sous **identité** nous pouvons spécifier une classe personnalisée ainsi que d’un ID unique pour le contrôleur de la vue. Définir le **nom de la classe** et **ID de la table de montage séquentiel** à `MainViewController`.

    [![](images/identitypanelnew.png "Spécifiez la classe personnalisée")](images/identitypanelnew.png#lightbox)

4. Une version ultérieure, nous allons devoir instancier nos contrôleurs de la vue à partir de la table de montage séquentiel et utilisez l’ID de la table de montage séquentiel pour les référencer dans votre code. L’ID de la restauration pour faire correspondre l’ID de la table de montage séquentiel garantit que le contrôleur de la vue obtient recréé correctement si l’état doit être restaurée.

5. Nous avons actuellement seulement un seul contrôleur de la vue. Faites glisser un autre contrôleur de la vue sur l’aire de conception. Dans le **remplissage de la propriété**, sous l’identité, définissez la classe et l’ID de plan conceptuel à `PinkViewController`, comme illustré ci-dessous :

    [![](images/pinkvcnew.png "La propriété de remplissage")](images/pinkvcnew.png#lightbox)
    
    L’IDE crée ces classes personnalisées pour les contrôleurs de vue. Elles peuvent être consultées dans le **remplissage de la Solution**, comme illustré dans la capture d’écran ci-dessous :
    
    [![](images/solution-pad.png "Remplissage de la solution")](images/solution-pad.png#lightbox)

6. Dans la `PinkViewController`, sélectionnez la vue en cliquant sur vers le centre du frame du contrôleur. Dans la zone Propriétés, sous affichage Modifier le **arrière-plan** à Magenta :
    
    [![](images/pinkcontroller.png "Définir la couleur d’arrière-plan")](images/pinkcontroller.png#lightbox)

7. Enfin, faites glisser un bouton à partir de la **boîte à outils** sur la `MainViewController`. Dans la zone Propriétés, attribuez-lui le nom `PinkButton` et le titre GoToPink, comme illustré ci-dessous :

    [![](images/pinkbutton.png "Définir le nom du bouton")](images/pinkbutton.png#lightbox)

Le plan conceptuel est terminé, mais si nous déployer le projet maintenant, nous obtenons un écran vide. C’est parce que nous devons toujours demander à l’IDE à utiliser notre plan conceptuel et pour configurer un contrôleur de vue racine pour servir de la première vue. Normalement cela est possible via notre Options du projet, comme indiqué ci-dessus. Toutefois dans cet exemple nous allons atteint le même résultat dans le code, en ajoutant le code suivant à la **AppDelegate**:

```csharp
public partial class AppDelegate : UIApplicationDelegate
    {
        UIWindow window;
        public static UIStoryboard Storyboard = UIStoryboard.FromName ("MainStoryboard", null);
        public static UIViewController initialViewController;

        public override bool FinishedLaunching (UIApplication app, NSDictionary options)
        {
            window = new UIWindow (UIScreen.MainScreen.Bounds);

            initialViewController = Storyboard.InstantiateInitialViewController () as UIViewController;

            window.RootViewController = initialViewController;
            window.MakeKeyAndVisible ();
            return true;
        }

    }
```

Beaucoup de code, mais uniquement quelques lignes sont inconnues. Tout d’abord, nous enregistrons notre plan conceptuel avec la **AppDelegate** en passant le nom de la table de montage séquentiel, **MainStoryboard**. Ensuite, nous indiquer à l’application pour instancier un contrôleur de la vue initiale à partir de la table de montage séquentiel en appelant `InstantiateInitialViewController` sur notre plan conceptuel, et affecter ce contrôleur de la vue en tant que contrôleur de notre application racine de la vue. Cette méthode détermine le premier écran que l’utilisateur voit, et crée une nouvelle instance de ce contrôleur de la vue.

Notez que dans le volet de la solution que l’IDE a créé un `MainViewcontroller.cs` (classe) et ses `corresponding designer.cs` lorsque nous avons ajouté le nom de classe à la zone de propriétés à l’étape 4. Nous pouvons voir cette classe créée un constructeur spécial qui inclut une classe de base :

```csharp
public MainViewController (IntPtr handle) : base (handle) 
{
}
```


Lorsque vous créez un plan conceptuel à l’aide du concepteur, l’IDE ajoute automatiquement le [[Enregistrer]](https://developer.xamarin.com/api/type/Foundation.RegisterAttribute/) attribut en haut de la `designer.cs` classe, puis passez dans un identificateur de chaîne, qui est identique à l’ID de plan conceptuel spécifié dans le étape précédente. Cela permet de lier c# à la scène pertinentes dans le plan conceptuel.

À un moment donné vous souhaiterez peut-être ajouter une classe existante qui a été **pas** créés dans le concepteur. Dans ce cas, vous pouvez enregistrer cette classe normalement :

```csharp
[Register ("MainViewController")]
public partial class MainViewController : UIViewController
{
public MainViewController (IntPtr handle) : base (handle) 
{
}

...
}
```

Pour plus d’informations sur l’inscription des classes et méthodes, reportez-vous à la [bureau d’enregistrement de Type](http://docs.xamarin.com/guides/ios/advanced_topics/registrar/) documentation.

La dernière étape de cette classe consiste à associer le bouton et la transition vers le contrôleur de vue rose. Nous allons instancier le `PinkViewController` à partir de la table de montage séquentiel ; ensuite, nous programmerez push de segue avec `PushViewController`, comme illustré dans l’exemple de code ci-dessous :

```csharp
public partial class MainViewController : UIViewController
{
    UIViewController pinkViewController;

    public MainViewController (IntPtr handle) : base (handle)
    {

    }

    public override void AwakeFromNib ()
    {
    // Called when loaded from xib or storyboard.

    this.Initialize ();
    }

    public void Initialize(){

    //Instantiating View Controller with Storyboard ID 'PinkViewController'
    pinkViewController = Storyboard.InstantiateViewController ("PinkViewController") as PinkViewController;
    }

    public override void ViewDidLoad ()
    {
    base.ViewDidLoad ();

    //When we push the button, we will push the pinkViewController onto our current Navigation Stack
    PinkButton.TouchUpInside += (o, e) =&gt; {
        this.NavigationController.PushViewController (pinkViewController, true);
    };
    }

}
```

Exécution de l’application génère une application d’écran de 2 :

![](images/finishedstoryboard.png "Exemple d’application exécuter écrans")

## <a name="conditional-segues"></a>Conditionnelle est parfait pour

Souvent, le déplacement d’un contrôleur de vue à l’autre est dépendant d’une certaine condition. Par exemple, si nous devions apporter un écran de connexion simple nous seulement voulons déplacer à l’écran suivant *si* le nom d’utilisateur et un mot de passe qui avaient été vérifiés.

Dans l’exemple suivant, nous allons ajouter un champ de mot de passe à l’exemple ci-dessus. L’utilisateur pourrez accéder à la *PinkViewController* s’il a entré le mot de passe, sinon une erreur s’affiche.

Avant de commencer, suivez les étapes 1 à 8 ci-dessus. Dans ces étapes, nous allons créer notre plan conceptuel, commencer à créer notre interface utilisateur et indiquer à quel contrôleur d’affichage à utiliser nos délégué de l’application car il est RootViewController.

1. Maintenant, nous allons construire notre interface utilisateur et ajouter des affichages supplémentaires répertoriés à la `MainViewController` pour lui donner l’apparence que la capture d’écran ci-dessous :

    - UITextField
        - Nom : PasswordTextField
        - Espace réservé : entrez le mot de passe Secret « »
    - UILabel
        - Texte : « erreur : incorrect de mot de passe. Vous ne transmet pas ! »
        - Couleur : rouge
        - Alignement : centre
        - Lignes : 2
        - 'Hidden' case à cocher activée 
        
    [![](images/passwordvc.png "Centre les lignes")](images/passwordvc.png#lightbox)
    
2. Créer un Segue entre le bouton Accédez rose et le contrôleur de la vue en faisant glisser de Ctrl de le *PinkButton* à la *PinkViewController*et en sélectionnant **Push** sur la souris . 

3. Cliquez sur le Segue et lui donner le *identificateur* `SegueToPink`:

    [![](images/namesegue.png "Cliquez sur le Segue et lui donner l’identificateur SegueToPink")](images/namesegue.png#lightbox)  
    

4. Enfin, ajoutez la méthode ShouldPerformSegue suivante à la `MainViewController` classe :

    ```csharp
    public override bool ShouldPerformSegue (string segueIdentifier, NSObject sender)
    {
        
        if(segueIdentifier == "SegueToPink"){
            if (PasswordTextField.Text == "password") {
                PasswordTextField.ResignFirstResponder ();
                return true;
            }
            else{
                ErrorLabel.Hidden = false;
                return false;
            }
        }
        return base.ShouldPerformSegue (segueIdentifier, sender);
    }
    ```

Dans ce code nous avons mis en correspondance la segueIdentifier à notre `SegueToPink` segue, donc nous pouvons ensuite tester une condition ; un mot de passe valide dans ce cas. Si la condition retourne `true`, effectuera le Segue et présente la `PinkViewController`. Si `false`, le nouveau contrôleur de vue s’affiche pas.

Nous pouvons appliquer cette approche à n’importe quel Segue sur ce contrôleur d’affichage en vérifiant l’argument segueIdentifier à la méthode ShouldPerformSegue. Dans ce cas nous avons un identificateur Segue – `SegueToPink`.

Reportez-vous à la solution Storyboards.Conditional dans les [exemple de manuel Storyboards](https://developer.xamarin.com/samples/monotouch/ManualStoryboard/) pour obtenir un exemple.

<a name="Using-Storyboard-References" />

## <a name="using-storyboard-references"></a>Utilisation de références à la table de montage séquentiel

Une référence de table de montage séquentiel vous permet de prendre une conception de plan conceptuel volumineuses et complexe et de décomposer en plus petits Storyboards obtient référencé à partir de la version d’origine, donc supprimer suppression de la complexité et d’apporter résultant des Storyboards plus facile à la conception et mettre à jour.

En outre, une référence de table de montage séquentiel peut fournir une _ancre_ à une autre séquence au sein de la même table de montage séquentiel ou une scène spécifique sur un autre.

<a name="Referencing-an-External-Storyboard" />

### <a name="referencing-an-external-storyboard"></a>Faisant référence à une table de montage séquentiel externe

Pour ajouter une référence à une table de montage séquentiel externe, procédez comme suit :

1. Dans le **l’Explorateur de solutions**, avec le bouton droit sur le nom du projet et sélectionnez **ajouter** > **nouveau fichier...**   >  **iOS** > **Storyboard**. Entrez un **nom** pour la table de montage séquentiel et cliquez sur le **nouveau** bouton :
    
    [![](images/ref01.png "La nouvelle boîte de dialogue")](images/ref01.png#lightbox)
    
2. Concevoir la disposition de l’arrière-plan de la nouvelle table de montage séquentiel aurait normalement et enregistrer vos modifications : 
    
    [![](images/ref02.png "La disposition de la nouvelle séquence")](images/ref02.png#lightbox)
    
3. Ouvrez le plan conceptuel que vous allez ajouter la référence dans le concepteur iOS.

4. Faites glisser un **Storyboard référence** à partir de la **boîte à outils** sur l’aire de conception : 
    
    [![](images/ref03.png "Une référence de table de montage séquentiel")](images/ref03.png#lightbox)
    
5. Dans le **Widget** onglet de la **l’Explorateur de propriétés**, sélectionnez le nom de la **Storyboard** que vous avez créé précédemment : 

    [![](images/ref04.png "L’onglet du Widget")](images/ref04.png#lightbox)
    
6. En cliquant sur un Widget de l’interface utilisateur (par exemple, un bouton) sur une scène existante et créer un nouveau Segue à la **référence de table de montage séquentiel** que vous venez de créer : 

    [![](images/ref05.png "Création d’un segue")](images/ref05.png#lightbox) 
    
7. Dans le menu contextuel, sélectionnez **afficher** pour terminer le Segue : 

    [![](images/ref06.png "Sélectionnez Afficher pour terminer le Segue")](images/ref06.png#lightbox) 
    
8. Enregistrez vos modifications dans la table de montage séquentiel.

Lorsque l’application est exécutée et que l’utilisateur clique sur l’élément d’interface utilisateur que vous avez créé le Segue à partir, le contrôleur de la vue initiale à partir de la table de montage séquentiel externe spécifié dans la référence de table de montage séquentiel seront affichera.

<a name="Referencing-a-Specific-Scene-in-an-External-Storyboard" />

### <a name="referencing-a-specific-scene-in-an-external-storyboard"></a>Faisant référence à une séquence spécifique dans une table de montage séquentiel externe

Pour ajouter une référence à une séquence spécifique un Storyboard externe (et pas le contrôleur d’affichage Initial), procédez comme suit :

1. Dans le **l’Explorateur de solutions**, double-cliquez sur le plan conceptuel externe pour l’ouvrir pour le modifier.

2. Ajouter une nouvelle séquence et sa disposition de conception comme vous le feriez normalement : 

    [![](images/ref07.png "La nouvelle disposition de la scène")](images/ref07.png#lightbox)
    
3. Dans le **Widget** onglet de la **l’Explorateur de propriétés**, entrez un **ID de la table de montage séquentiel** pour-View-Controller de cette nouvelle séquence : 

    [![](images/ref08.png "Entrez un ID de table de montage séquentiel pour le nouveau contrôleur de vue en arrière-plan")](images/ref08.png#lightbox)
    
3. Ouvrez le plan conceptuel que vous allez ajouter la référence dans le concepteur iOS.

4. Faites glisser un **Storyboard référence** à partir de la **boîte à outils** sur l’aire de conception : 

    [![](images/ref03.png "Une référence de table de montage séquentiel")](images/ref03.png#lightbox)
    
5. Dans le **Widget** onglet de la **l’Explorateur de propriétés**, sélectionnez le nom de la **Storyboard** et le **ID de référence** (ID de la table de montage séquentiel) de la Scène que vous avez créé précédemment : 

    [![](images/ref09.png "L’onglet du Widget ")](images/ref09.png#lightbox)
    
6. En cliquant sur un Widget de l’interface utilisateur (par exemple, un bouton) sur une scène existante et créer un nouveau Segue à la **référence de table de montage séquentiel** que vous venez de créer : 

    [![](images/ref10.png "Création d’un segue")](images/ref10.png#lightbox) 
    
7. Dans le menu contextuel, sélectionnez **afficher** pour terminer le Segue : 

    [![](images/ref06.png "Sélectionnez Afficher pour terminer le Segue")](images/ref06.png#lightbox) 
    
8. Enregistrez vos modifications dans la table de montage séquentiel.

Lorsque l’application est exécutée et que l’utilisateur clique sur l’élément d’interface utilisateur que vous avez créé le Segue à partir de la scène avec la donnée **ID de la table de montage séquentiel** à partir de la table de montage séquentiel externe spécifié dans la référence de table de montage séquentiel s’affichera.

<a name="Referencing-a-Specific-Scene-in-the-Same-Storyboard" />

### <a name="referencing-a-specific-scene-in-the-same-storyboard"></a>Faisant référence à une séquence spécifique dans la même table de montage séquentiel

Pour ajouter une référence à une séquence spécifique de la même table de montage séquentiel, procédez comme suit :

1. Dans le **l’Explorateur de solutions**, double-cliquez sur le plan conceptuel pour l’ouvrir pour le modifier.

2. Ajouter une nouvelle séquence et sa disposition de conception comme vous le feriez normalement : 

    [![](images/ref11.png "La nouvelle disposition de la scène")](images/ref11.png#lightbox)

3. Dans le **Widget** onglet de la **l’Explorateur de propriétés**, entrez un **ID de la table de montage séquentiel** pour-View-Controller de cette nouvelle séquence : 

    [![](images/ref12.png "L’onglet du Widget")](images/ref12.png#lightbox)
    
3. Faites glisser un **Storyboard référence** à partir de la **boîte à outils** sur l’aire de conception : 

    [![](images/ref03.png "Une référence de table de montage séquentiel")](images/ref03.png#lightbox)
    
5. Dans le **Widget** onglet de la **l’Explorateur de propriétés**, sélectionnez **ID de référence** (ID de la table de montage séquentiel) de la scène que vous avez créé précédemment : 

    [![](images/ref13.png "L’onglet du Widget")](images/ref13.png#lightbox)
    
6. En cliquant sur un Widget de l’interface utilisateur (par exemple, un bouton) sur une scène existante et créer un nouveau Segue à la **référence de table de montage séquentiel** que vous venez de créer : 

    [![](images/ref14.png "Création d’un segue")](images/ref14.png#lightbox) 
    
7. Dans le menu contextuel, sélectionnez **afficher** pour terminer le Segue : 

    [![](images/ref06.png "Sélectionnez Afficher pour terminer le Segue")](images/ref06.png#lightbox) 
    
8. Enregistrez vos modifications dans la table de montage séquentiel.

Lorsque l’application est exécutée et que l’utilisateur clique sur l’élément d’interface utilisateur que vous avez créé le Segue à partir de la scène avec la donnée **ID de la table de montage séquentiel** dans le même Storyboard spécifié dans la référence de table de montage séquentiel s’affiche.

## <a name="summary"></a>Récapitulatif

Cet article présente le concept des plans conceptuels et comment ils peuvent être utiles pour le développement d’applications iOS. Il traite des scènes, affichage des contrôleurs, vues et des hiérarchies et comment les scènes sont liés avec différents types de Segues.  Elle explore également instanciation de l’affichage des contrôleurs manuellement à partir d’une table de montage séquentiel et création Segues conditionnel.



## <a name="related-links"></a>Liens associés

- [Animation manuelle (exemple)](https://developer.xamarin.com/samples/ManualStoryboard/)
- [Introduction au Concepteur d’iOS](~/ios/user-interface/designer/introduction.md)
- [Conversion à des plans conceptuels](http://developer.apple.com/library/ios/#releasenotes/Miscellaneous/RN-AdoptingStoryboards/)
- [Référence de classe de UIStoryboard](https://developer.apple.com/library/ios/#documentation/UIKit/Reference/UIStoryboard_Class/Reference/Reference.html)
