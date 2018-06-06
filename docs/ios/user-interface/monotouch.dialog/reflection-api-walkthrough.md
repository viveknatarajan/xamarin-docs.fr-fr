---
title: Création d’une application Xamarin.iOS à l’aide de l’API de réflexion
description: Ce document décrit le MonoTouch.Dialog basée sur l’attribut API de réflexion, ce qui crée l’interface utilisateur basée sur les classes décorés avec des attributs.
ms.prod: xamarin
ms.assetid: C0F923D2-300E-DB9D-F390-9FA71B22DFD6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: c60d2a8ca58fe807dc340125d0db21b6f9ddff9f
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790507"
---
# <a name="creating-a-xamarinios-application-using-the-reflection-api"></a>Création d’une application Xamarin.iOS à l’aide de l’API de réflexion

Le MT. API de réflexion D permet aux classes être décoré avec des attributs que MT. D utilise pour créer des écrans automatiquement. L’API de réflexion fournit une liaison entre ces classes et ce qui est affiché sur l’écran. Cette API ne fournit pas le contrôle précis que les API d’éléments, il réduit la complexité en générant automatiquement à la hiérarchie des éléments en fonction de la décoration de la classe.

 <a name="Getting_Started_with_the_Reflection_API" />


## <a name="getting-started-with-the-reflection-api"></a>Prise en main de l’API de réflexion

À l’aide de l’API de réflexion est aussi simple que :

1.  Création d’une classe décorée avec MT. Attributs de D.
1.  Création d’un `BindingContext` instance, en lui passant une instance de la classe ci-dessus. 
1.  Création d’un `DialogViewController` , en lui passant le `BindingContext’s` `RootElement` . 


Examinons un exemple pour illustrer l’utilisation de l’API de réflexion. Dans cet exemple, nous allons construire un écran d’entrée de données simple comme indiqué ci-dessous :

 [![](reflection-api-walkthrough-images/01-expense-entry.png "Dans cet exemple, nous allons construire un écran d’entrée de données simple comme indiqué ici")](reflection-api-walkthrough-images/01-expense-entry.png#lightbox)

 <a name="Creating_a_Class_with_MT.D_Attributes" />

## <a name="creating-a-class-with-mtd-attributes"></a>Création d’une classe avec MT. Attributs de D

La première chose que nous devons utiliser l’API de réflexion est une classe décorée avec des attributs. Ces attributs seront utilisés par MT. D en interne pour créer des objets à partir de l’API d’éléments. Par exemple, considérez la définition de classe suivante :

```csharp
public class Expense
{
        [Section("Expense Entry")]

        [Entry("Enter expense name")]
        public string Name;
        
        [Section("Expense Details")]
  
        [Caption("Description")]
        [Entry]
        public string Details;
        
        [Checkbox]
        public bool IsApproved = true;
}
```

Le `SectionAttribute` entraîne des sections de la `UITableView` créé, avec l’argument de chaîne utilisé pour remplir l’en-tête de la section. Une fois qu’une section est déclarée, tous les champs qui le suit seront inclus dans cette section, jusqu'à ce qu’une autre section est déclarée.
Le type d’élément d’interface utilisateur créé pour le champ varient en fonction du type de champ et le MT. Attribut D décorant.

Par exemple, le `Name` champ est un `string` et elle est décorée avec un `EntryAttribute`. Cela entraîne une ligne ajoutée à la table avec un champ d’entrée de texte et la légende spécifiée. De même, la `IsApproved` champ est un `bool` avec un `CheckboxAttribute`, se traduisant par une ligne de table avec une case à cocher située à droite de la cellule du tableau. MT. D utilise le nom du champ, ajouter automatiquement un espace, pour créer la légende dans ce cas, car il n’est pas spécifié dans un attribut.

 <a name="Adding_the_BindingContext" />


## <a name="adding-the-bindingcontext"></a>Ajout de BindingContext

Pour utiliser le `Expense` (classe), nous devons créer un `BindingContext`. A `BindingContext` est une classe qui sera lier les données à partir de la classe avec attributs pour créer la hiérarchie d’éléments. Pour en créer un, nous avons simplement instancier et passer une instance de la classe avec attributs au constructeur.

Par exemple, pour ajouter une interface utilisateur que nous avons déclaré à l’aide de l’attribut dans le `Expense` de classe, incluez le code suivant dans le `FinishedLaunching` méthode de la `AppDelegate`:

```csharp
var expense = new Expense ();
var bctx = new BindingContext (null, expense, "Create a task");
```

Ensuite, nous devons faire pour créer l’interface utilisateur d’ajouter le `BindingContext` à la `DialogViewController` et définissez-le en tant que le `RootViewController` de la fenêtre, comme indiqué ci-dessous :

```csharp
UIWindow window;

public override bool FinishedLaunching (UIApplication app, 
        NSDictionary options)
{
   
        window = new UIWindow (UIScreen.MainScreen.Bounds);
            
        var expense = new Expense ();
        var bctx = new BindingContext (null, expense, "Create a task");
        var dvc = new DialogViewController (bctx.Root);
            
        window.RootViewController = dvc;
        window.MakeKeyAndVisible ();
            
        return true;
}
```

Dans l’écran affiché au-dessus de l’affichage des résultats maintenant l’application en cours d’exécution.

 <a name="Adding_a_UINavigationController" />


### <a name="adding-a-uinavigationcontroller"></a>Ajout d’un UINavigationController

Notez toutefois que le titre « crée une tâche » qui nous passé à la `BindingContext` n’est pas affichée. C’est parce que le `DialogViewController` est ne font pas partie d’un `UINavigatonController`. Nous allons modifier le code pour ajouter un `UINavigationController` en tant que la fenêtre `RootViewController,` et ajoutez le `DialogViewController` comme racine de la `UINavigationController` comme indiqué ci-dessous :

```csharp
nav = new UINavigationController(dvc);
window.RootViewController = nav;
```

Maintenant lorsque nous exécutons l’application, le titre s’affiche dans le `UINavigationController’s` barre de navigation en tant que la capture d’écran ci-dessous montre :

 [![](reflection-api-walkthrough-images/02-create-task.png "Lorsque nous exécutons l’application, le titre apparaît désormais dans la barre de navigation UINavigationControllers")](reflection-api-walkthrough-images/02-create-task.png#lightbox)

En incluant un `UINavigationController`, nous pouvons à présent tirer parti d’autres fonctionnalités de MT. D pour lesquels la navigation est nécessaire. Par exemple, nous pouvons ajouter une énumération pour la `Expense` classe pour définir la catégorie des dépenses et MT. D créera automatiquement un écran de sélection. Pour illustrer, modifiez le `Expense` classe pour inclure un `ExpenseCategory` champ comme suit :

```csharp
public enum Category
{
        Travel,
        Lodging,
        Books
}
        
public class Expense
{
        …

        [Caption("Category")]
        public Category ExpenseCategory;
}
```

Exécution de l’application maintenant aboutit dans une nouvelle ligne dans la table pour la catégorie :

 [![](reflection-api-walkthrough-images/03-set-details.png "Exécution de l’application maintenant aboutit dans une nouvelle ligne dans la table pour la catégorie")](reflection-api-walkthrough-images/03-set-details.png#lightbox)

En sélectionnant la ligne entraîne l’application accédant à un nouvel écran avec des lignes correspondant à l’énumération, comme indiqué ci-dessous :

 [![](reflection-api-walkthrough-images/04-set-category.png "En sélectionnant la ligne entraîne la navigation application vers un nouvel écran avec les lignes correspondant à l’énumération")](reflection-api-walkthrough-images/04-set-category.png#lightbox)

 <a name="Summary" />


## <a name="summary"></a>Récapitulatif

Cet article présenté une procédure pas à pas de l’API de réflexion. Nous vous avons montré comment ajouter des attributs à une classe pour contrôler ce qui est affiché. Il explique également comment utiliser un `BindingContext` pour lier des données à partir d’une classe à la hiérarchie de l’élément qui est créée, ainsi que comment MT d’utilisation. D avec un `UINavigationController`.


## <a name="related-links"></a>Liens associés

- [MTDReflectionWalkthrough (exemple)](https://developer.xamarin.com/samples/MTDReflectionWalkthrough/)
- [Capture - Miguel de Icaza crée un écran de connexion iOS avec MonoTouch.Dialog](http://youtu.be/3butqB1EG0c)
- [Capture - créer facilement des interfaces utilisateur iOS avec MonoTouch.Dialog](http://youtu.be/j7OC5r8ZkYg)
- [Présentation de la boîte de dialogue MonoTouch](~/ios/user-interface/monotouch.dialog/index.md)
- [API de procédure pas à pas les éléments](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [Procédure pas à pas JSON élément](~/ios/user-interface/monotouch.dialog/monotouch.dialog-json-markup.md)
- [Boîte de dialogue MonoTouch sur Github](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [Application de TweetStation](https://github.com/migueldeicaza/TweetStation)
- [Référence de classe de UITableViewController](http://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [Référence de classe de UINavigationController](http://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
