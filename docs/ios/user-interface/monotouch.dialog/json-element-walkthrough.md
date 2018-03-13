---
title: "Procédure pas à pas : Utilisation d’un élément de JSON pour créer une Interface utilisateur"
description: "MonoTouch.Dialog (MT. D) inclut la prise en charge pour la génération dynamique de l’interface utilisateur via les données JSON. Dans ce didacticiel, nous examinerons comment utiliser un JSONElement pour créer une interface utilisateur à partir de JSON qui est inclus dans une application, soit chargé à partir d’une Url distante."
ms.topic: article
ms.prod: xamarin
ms.assetid: E353DF14-51D7-98E3-59EA-16683C770C23
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 1a5e4ab73c9090cbb743dc8cb339d6f50c5ff527
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="walkthrough-using-a-json-element-to-create-a-user-interface"></a>Procédure pas à pas : Utilisation d’un élément de JSON pour créer une Interface utilisateur

_MonoTouch.Dialog (MT. D) inclut la prise en charge pour la génération dynamique de l’interface utilisateur via les données JSON. Dans ce didacticiel, nous examinerons comment utiliser un JSONElement pour créer une interface utilisateur à partir de JSON qui est inclus dans une application, soit chargé à partir d’une Url distante._


MT. D prend en charge la création d’interfaces utilisateur déclaré dans JSON. Lorsque les éléments sont déclarés à l’aide de JSON, MT. D crée les éléments associés pour vous automatiquement. Le JSON peut être chargé à partir d’un fichier local, un analysé `JsonObject` instance, ou même une Url distante.

MT. D prend en charge la gamme complète des fonctionnalités qui sont disponibles dans l’API d’éléments lors de l’utilisation de JSON. Par exemple, l’application dans la capture d’écran suivante est déclarée complètement à l’aide de JSON :

[![](json-element-walkthrough-images/01-load-from-file.png "Par exemple, l’application dans cette capture d’écran est complètement déclarée à l’aide de JSON") ](json-element-walkthrough-images/01-load-from-file.png#lightbox) [ ![ ] (json-element-walkthrough-images/01-load-from-file.png ", par exemple, l’application dans cette capture d’écran est complètement déclarée à l’aide de JSON")](json-element-walkthrough-images/01-load-from-file.png#lightbox)

Nous allons revisiter l’exemple de la [API de procédure pas à pas les éléments](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md) didacticiel, montrant comment ajouter un écran de détails de tâche à l’aide de JSON.

## <a name="json-walkthrough"></a>Procédure pas à pas JSON

L’exemple de cette procédure pas à pas permet la création de tâches. Lorsqu’une tâche est sélectionnée dans le premier écran, un écran de détails est présenté comme indiqué :

 [![](json-element-walkthrough-images/03-task-list.png "Lorsqu’une tâche est sélectionnée dans le premier écran, un écran de détails est présenté comme")](json-element-walkthrough-images/03-task-list.png#lightbox)

## <a name="creating-the-json"></a>Création de l’objet JSON

Pour cet exemple, nous allons charger le fichier JSON à partir d’un fichier dans le projet nommé `task.json`. MT. D attend le JSON pour se conformer à une syntaxe qui reflète l’API d’éléments. Comme à l’aide de l’API d’éléments à partir du code, lorsque vous utilisez JSON, nous déclarons sections et dans ces sections nous ajoutons des éléments. Pour déclarer des sections et des éléments au format JSON, nous utilisons les chaînes « sections » et les « éléments » respectivement comme clés. Pour chaque élément, le type d’élément associé est défini à l’aide de la `type` clé. Chaque autre propriété d’éléments est définie avec le nom de propriété comme clé.

Par exemple, le texte JSON suivant décrit les sections et les éléments pour les détails de la tâche :

```csharp
{
    "title": "Task",
    "sections": [
        {
          "elements" : [
            {
                "id" : "task-description",
                "type": "entry",
                "placeholder": "Enter task description"
            },
            {
                "id" : "task-duedate",
                "type": "date",
                "caption": "Due Date",
                "value": "00:00"
            }
         ]
        }
    ]
  }
```

Notez le JSON ci-dessus inclut un id pour chaque élément. N’importe quel élément peut inclure un id, pour le référencer lors de l’exécution. Nous allons voir comment il est utilisé dans un moment lorsque nous montrent comment charger l’objet JSON dans le code.

 <a name="Loading_the_JSON_in_Code" />


## <a name="loading-the-json-in-code"></a>Le chargement de l’objet JSON dans le Code

Une fois que l’objet JSON a été défini, nous devons charger dans Mt. D à l’aide de la `JsonElement` classe. En supposant que d’un fichier avec le JSON que nous avons créés ci-dessus a été ajouté au projet avec le nom sample.json et donné d’une action de génération du contenu, le chargement du `JsonElement` est aussi simple que l’appel de la ligne de code suivante :

```csharp
var taskElement = JsonElement.FromFile ("task.json");
```

Étant donné que nous l’ajoutons à la demande à chaque fois qu’une tâche est créée, nous pouvons modifier le bouton cliqué à partir de l’exemple précédent éléments API comme suit :

```csharp
_addButton.Clicked += (sender, e) => {

        ++n;

        var task = new Task{Name = "task " + n, DueDate = DateTime.Now};

        var taskElement = JsonElement.FromFile ("task.json");

        _rootElement [0].Add (taskElement);
};
```

 <a name="Accessing_Elements_at_Runtime" />


## <a name="accessing-elements-at-runtime"></a>L’accès aux éléments lors de l’exécution

Rappelez-vous que nous avons ajouté un id pour les deux éléments lorsque nous déclarés dans le fichier JSON. Nous pouvons utiliser la propriété id pour accéder à chaque élément lors de l’exécution à modifier leurs propriétés dans le code. Par exemple, le code suivant fait référence aux éléments d’entrée et la date pour définir les valeurs à partir de l’objet de tâche :

```csharp
_addButton.Clicked += (sender, e) => {

        ++n;

        var task = new Task{Name = "task " + n, DueDate = DateTime.Now};

        var taskElement = JsonElement.FromFile ("task.json");

        taskElement.Caption = task.Name;

        var description = taskElement ["task-description"] as EntryElement;

        if (description != null) {
                description.Caption = task.Name;
                description.Value = task.Description;       
        }

        var duedate = taskElement ["task-duedate"] as DateElement;

        if (duedate != null) {                
                duedate.DateValue = task.DueDate;
        }
        _rootElement [0].Add (taskElement);
};
```

 <a name="Loading_JSON_from_a_Url" />


## <a name="loading-json-from-a-url"></a>Le chargement de JSON à partir d’une Url

MT. D prend également en charge le chargement dynamique JSON à partir d’une Url externe par le fait de passer au constructeur de l’Url de la `JsonElement`. MT. D développe la hiérarchie déclarée dans le JSON à la demande lorsque vous naviguez entre les écrans. Par exemple, considérez un fichier JSON comme celle ci-dessous situé à la racine du serveur web local :

```csharp
{
    "type": "root",
    "title": "home",
    "sections": [
       {
         "header": "Nested view!",
         "elements": [
           {
             "type": "boolean",
             "caption": "Just a boolean",
             "id": "first-boolean",
             "value": false
           },
           {
             "type": "string",
             "caption": "Welcome to the nested controller"
           }
         ]
       }
     ]
}
```

Vous pouvez le charger à l’aide de la `JsonElement` comme dans le code suivant :

```csharp
_rootElement = new RootElement ("Json Example"){
        new Section (""){ new JsonElement ("Load from url",
                "http://localhost/sample.json")
        }
};
```

Lors de l’exécution, le fichier sera récupéré et analysé par MT. D lorsque l’utilisateur navigue vers la seconde vue, comme indiqué dans la capture d’écran ci-dessous :

 [![](json-element-walkthrough-images/04-json-web-example.png "Le fichier sera récupéré et analysé par MT. D lorsque l’utilisateur navigue vers la seconde vue")](json-element-walkthrough-images/04-json-web-example.png#lightbox)

 <a name="Summary" />


## <a name="summary"></a>Récapitulatif

Cet article a montré comment créer un à l’aide de l’interface avec MT. D à partir de JSON. Il vous a montré comment charger JSON inclus dans un fichier avec l’application, ainsi qu’à partir d’une Url distante. Il a également montré comment accéder aux éléments décrits dans JSON lors de l’exécution.


## <a name="related-links"></a>Liens associés

- [MTDJsonDemo (sample)](https://developer.xamarin.com/samples/MTDJsonDemo/)
- [Capture - Miguel de Icaza crée un écran de connexion iOS avec MonoTouch.Dialog](http://youtu.be/3butqB1EG0c)
- [Capture - créer facilement des interfaces utilisateur iOS avec MonoTouch.Dialog](http://youtu.be/j7OC5r8ZkYg)
- [Introduction aux MonoTouch.Dialog](~/ios/user-interface/monotouch.dialog/index.md)
- [API de procédure pas à pas les éléments](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [Procédure pas à pas API de réflexion](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [Boîte de dialogue MonoTouch sur Github](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [Application de TweetStation](https://github.com/migueldeicaza/TweetStation)
- [Référence de classe de UITableViewController](http://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [Référence de classe de UINavigationController](http://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
