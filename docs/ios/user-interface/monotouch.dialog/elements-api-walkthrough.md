---
title: 'Procédure pas à pas : création d’une application à l’aide de l’API d’éléments'
description: Cet article s’appuie sur les informations présentées dans l’Introduction de l’article de la boîte de dialogue MonoTouch. Il présente une procédure pas à pas qui montre comment utiliser le MonoTouch.Dialog (MT. D) éléments API pour commencer rapidement à créer une application avec MT. D.
ms.prod: xamarin
ms.assetid: F1124734-DF44-F1F3-0832-46F52A788CDC
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: e4fbf744c6f967d09e0033212024c2e2398fb768
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="walkthrough---creating-an-application-using-the-elements-api"></a>Procédure pas à pas : création d’une application à l’aide de l’API d’éléments

_Cet article s’appuie sur les informations présentées dans l’Introduction de l’article de la boîte de dialogue MonoTouch. Il présente une procédure pas à pas qui montre comment utiliser le MonoTouch.Dialog (MT. D) éléments API pour commencer rapidement à créer une application avec MT. D._

Dans cette procédure pas à pas, nous allons utiliser le MT. API d’éléments D pour créer un style maître / détail de l’application qui affiche une liste de tâches. Lorsque l’utilisateur sélectionne le <span class="ui"> + </span> bouton dans la barre de navigation, une nouvelle ligne est ajoutée à la table de la tâche. En sélectionnant la ligne permet d’accéder à l’écran de détails qui nous permet de mettre à jour la description de la tâche et la date d’échéance, comme illustré ci-dessous :

 [![](elements-api-walkthrough-images/01-task-list-app.png "En sélectionnant la ligne pour accéder à l’écran de détails qui nous permet de mettre à jour la description de la tâche et la date d’échéance")](elements-api-walkthrough-images/01-task-list-app.png#lightbox)

 <a name="Elements_API_Walkthrough" />


## <a name="elements-api-walkthrough"></a>API de procédure pas à pas les éléments

Dans le [Introduction à la boîte de dialogue MonoTouch](~/ios/user-interface/monotouch.dialog/index.md) article, nous acquis une connaissance approfondie des différentes parties du MT. D. Nous allons utiliser l’API d’éléments à placer tous les éléments dans une application.

 <a name="Setting_up_the_Multi-Screen_Application" />


## <a name="setting-up-the-multi-screen-application"></a>La configuration de l’Application d’écrans multiples

Pour démarrer le processus de création d’écran, MonoTouch.Dialog crée un `DialogViewController`, puis ajoute un `RootElement`.

Pour créer une application d’écran multiples avec MonoTouch.Dialog, nous devons :

1.  Créer un  `UINavigationController.`
1.  Créer un  `DialogViewController.`
1.  Ajouter le `DialogViewController` comme racine de la  `UINavigationController.` 
1.  Ajouter un `RootElement` à la  `DialogViewController.`
1.  Ajouter `Sections` et `Elements` à la  `RootElement.` 


 <a name="Using_A_UINavigationController" />


### <a name="using-a-uinavigationcontroller"></a>À l’aide d’un UINavigationController

Pour créer une application de style de navigation, nous devons créer un `UINavigationController`, puis l’ajouter en tant que le `RootViewController` dans le `FinishedLaunching` méthode de la `AppDelegate`. Pour rendre le `UINavigationController` fonctionnent avec MonoTouch.Dialog, nous ajoutons un `DialogViewController` à le `UINavigationController` comme indiqué ci-dessous :

```csharp
public override bool FinishedLaunching (UIApplication app, 
        NSDictionary options)
{
        _window = new UIWindow (UIScreen.MainScreen.Bounds);
            
        _rootElement = new RootElement ("To Do List"){new Section ()};

        // code to create screens with MT.D will go here …

        _rootVC = new DialogViewController (_rootElement);
        _nav = new UINavigationController (_rootVC);
        _window.RootViewController = _nav;
        _window.MakeKeyAndVisible ();
            
        return true;
}
```

Le code ci-dessus crée une instance d’un `RootElement` et la passe le `DialogViewController`. Le `DialogViewController` a toujours un `RootElement` en haut de sa hiérarchie. Dans cet exemple, le `RootElement` est créée avec la chaîne « Liste des tâches, » qui sert de titre dans la barre de navigation du contrôleur de navigation. À ce stade, l’application en cours d’exécution présente l’écran ci-dessous :

 [![](elements-api-walkthrough-images/02-to-do-list-screen-.png "Exécution de l’application présente l’écran illustré ici")](elements-api-walkthrough-images/02-to-do-list-screen-.png#lightbox)

Nous allons voir comment utiliser la structure hiérarchique de MonoTouch.Dialog de `Sections` et `Elements` pour ajouter plusieurs écrans.

 <a name="Creating_the_Dialog_Screens" />


### <a name="creating-the-dialog-screens"></a>Création d’écrans de la boîte de dialogue

A `DialogViewController` est un `UITableViewController` sous-classe MonoTouch.Dialog utilise pour ajouter des écrans. MonoTouch.Dialog crée des écrans en ajoutant un `RootElement` à un `DialogViewController`, comme nous l’avons vu ci-dessus. Le `RootElement` peut avoir `Section` instances qui représentent les sections d’une table.
Les sections sont constituées d’éléments, les autres sections, ou même les autres `RootElements`. En imbriquant `RootElements`, MonoTouch.Dialog crée automatiquement une application de style de navigation, comme nous allons le voir.

 <a name="Using_DialogViewController" />


### <a name="using-dialogviewcontroller"></a>À l’aide de DialogViewController

Le `DialogViewController`, qui est un `UITableViewController` sous-classe, a un `UITableView` comme sa vue. Dans cet exemple, nous souhaitons ajouter des éléments à la table chaque fois que le <span class="ui"> + </span> bouton. Étant donné que la `DialogViewController` a été ajouté à un `UINavigationController`, nous pouvons utiliser le `NavigationItem`de `RightBarButton` propriété à ajouter le <span class="ui"> + </span> bouton, comme indiqué ci-dessous :

```csharp
_addButton = new UIBarButtonItem (UIBarButtonSystemItem.Add);
_rootVC.NavigationItem.RightBarButtonItem = _addButton;
```

Lorsque nous avons créé la `RootElement` précédemment, nous le passé un seul `Section` instance afin que nous pouvons ajouter des éléments comme la <span class="ui"> + </span> bouton est tapée par l’utilisateur. Nous pouvons utiliser le code suivant pour accomplir ce dans l’événement de gestionnaire pour le bouton :

```csharp
_addButton.Clicked += (sender, e) => {
                
        ++n;
                
        var task = new Task{Name = "task " + n, DueDate = DateTime.Now};
                
        var taskElement = new RootElement (task.Name){
                new Section () {
                        new EntryElement (task.Name, 
                                "Enter task description", task.Description)
                },
                new Section () {
                        new DateElement ("Due Date", task.DueDate)
                }
        };
        _rootElement [0].Add (taskElement);
};
```

Ce code crée un nouveau `Task` objet chaque fois que le bouton est activé par un clic. L’exemple suivant montre l’implémentation simple de la `Task` classe :

```csharp
public class Task
{   
        public Task ()
        {
        }
        
        public string Name { get; set; }
        
        public string Description { get; set; }

        public DateTime DueDate { get; set; }
}
```

 []()

La tâche `Name` propriété est utilisée pour créer le `RootElement`de légende avec une variable de compteur nommée `n` qui est incrémenté pour chaque nouvelle tâche. MonoTouch.Dialog Active les éléments dans les lignes sont ajoutées à la `TableView` lorsque chaque `taskElement` est ajouté.

 <a name="Presenting_and_Managing_Dialog_Screens" />


## <a name="presenting-and-managing-dialog-screens"></a>Présentation et la gestion des écrans de la boîte de dialogue

Nous avons utilisé un `RootElement` afin que MonoTouch.Dialog serait automatiquement créer un nouvel écran de détails de chaque tâche et accédez à celui-ci lorsqu’une ligne est sélectionnée.

L’écran de détails de tâche elle-même se compose de deux sections. chacune de ces sections contient un élément unique. Le premier élément est créé à partir une `EntryElement` pour fournir une ligne modifiable pour la tâche `Description` propriété. Lorsque l’élément est sélectionné, un clavier pour modifier le texte est présenté comme indiqué ci-dessous :

 [![](elements-api-walkthrough-images/03-create-task.png "Lorsque l’élément est sélectionné, un clavier pour modifier le texte est présenté comme")](elements-api-walkthrough-images/03-create-task.png#lightbox)

La deuxième section contient un `DateElement` qui nous permet de gérer de la tâche `DueDate` propriété. Sélection de la date de charge automatiquement un sélecteur de dates comme indiqué :

 [![](elements-api-walkthrough-images/04-date-picker.png "Sélection de la date de charge automatiquement un sélecteur de dates en tant que")](elements-api-walkthrough-images/04-date-picker.png#lightbox)

À la fois dans le `EntryElement` et `DateElement` cas (ou pour tout élément d’entrée de données dans MonoTouch.Dialog), les modifications apportées aux valeurs sont automatiquement conservées. Nous pouvons illustrer cela en modifiant la date et de puis d’y accéder dans les deux sens entre l’écran de la racine et divers détails de la tâche, où les valeurs dans les écrans de détails sont conservées.

 <a name="Summary" />


## <a name="summary"></a>Récapitulatif

Cet article présenté une procédure pas à pas qui vous a montré comment utiliser l’API d’éléments MonoTouch.Dialog. Il couvert les étapes de base pour créer une application d’écran multiples MT. D, y compris comment utiliser un `DialogViewController` et comment ajouter des éléments et des Sections pour créer des écrans. En outre, il vous a montré comment utiliser MT. D conjointement avec un `UINavigationController`.


## <a name="related-links"></a>Liens associés

- [MTDWalkthrough (exemple)](https://developer.xamarin.com/samples/MTDWalkthrough/)
- [Capture - Miguel de Icaza crée un écran de connexion iOS avec MonoTouch.Dialog](http://youtu.be/3butqB1EG0c)
- [Capture - créer facilement des interfaces utilisateur iOS avec MonoTouch.Dialog](http://youtu.be/j7OC5r8ZkYg)
- [Introduction aux MonoTouch.Dialog](~/ios/user-interface/monotouch.dialog/index.md)
- [Procédure pas à pas API de réflexion](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [Procédure pas à pas JSON élément](~/ios/user-interface/monotouch.dialog/json-element-walkthrough.md)
- [Boîte de dialogue MonoTouch sur Github](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [Application de TweetStation](https://github.com/migueldeicaza/TweetStation)
- [Référence de classe de UITableViewController](http://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [Référence de classe de UINavigationController](http://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
