---
title: Présentation de l’exemple
description: Cette rubrique fournit une procédure pas à pas de l’exemple d’application Xamarin.Forms qui montre comment communiquer avec les services web différents. Alors que chaque service web utilise une application d’exemple distincts, ils sont fonctionnellement similaires et partagent des classes courantes.
ms.prod: xamarin
ms.assetid: A3FEB262-0D79-42E6-8F8B-A565618C490B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/28/2017
ms.openlocfilehash: 703441e3fc58beeb33e519f3781387a59c1c1cef
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="understanding-the-sample"></a>Présentation de l’exemple

_Cette rubrique fournit une procédure pas à pas de l’exemple d’application Xamarin.Forms qui montre comment communiquer avec les services web différents. Alors que chaque service web utilise une application d’exemple distincts, ils sont fonctionnellement similaires et partagent des classes courantes._

L’exemple d’application de liste des tâches décrite ci-dessous est utilisé pour illustrer comment accéder aux différents types de serveurs principaux de service web avec Xamarin.Forms. Il fournit une fonctionnalité pour :

- Afficher la liste des tâches.
- Ajouter, modifier et supprimer des tâches.
- Définir l’état d’une tâche à 'done'.
- Présenter les champs nom et les notes de la tâche.

Dans tous les cas, les tâches sont stockées dans un service principal qui est accessible via un service web.

Lorsque l’application est lancée, une page s’affiche qui répertorie les tâches extraites du service web et permet à l’utilisateur créer une nouvelle tâche. En cliquant sur une tâche accède à l’application à une deuxième page où la tâche peut être modifiée, enregistrée, supprimée et la prononciation. L’application finale est indiquée ci-dessous :

![](walkthrough-images/app-example-1.png "Application de TODO : première page")
![](walkthrough-images/app-example-2.png "Todo application - deuxième page")

Chaque rubrique de ce guide fournit un lien vers une *différents* version de l’application qui illustre un type spécifique de principal du service web. Télécharger le code exemple pertinentes sur la page relative à chaque service web de style.

## <a name="understanding-the-application-anatomy"></a>Présentation de la composition de l’Application

Le projet de bibliothèque de classes portables pour chaque exemple d’application se compose de trois dossiers principaux :

|Dossier|Objectif|
|--- |--- |
|Données|Contient les classes et les interfaces utilisées pour gérer les éléments de données et de communiquer avec le service web. Au minimum, cela inclut la `TodoItemManager` (classe), qui est exposé via une propriété dans la `App` classe pour appeler des opérations de service web.|
|Modèles|Contient les classes de modèle de données pour l’application. Au minimum, cela inclut la `TodoItem` classe, qui modélise un seul élément de données utilisées par l’application. Le dossier peut également inclure toutes les autres classes utilisées pour modéliser les données utilisateur.|
|Affichages|Contient les pages de l’application. Il se compose généralement de la `TodoListPage` et `TodoItemPage` classes et des classes supplémentaires utilisés à des fins d’authentification.|

Le projet de bibliothèque de classes portables pour chaque application comprend également un certain nombre de fichiers importants :

|Fichier|Objectif|
|--- |--- |
|Constants.cs|Le `Constants` (classe), qui spécifie les constantes utilisées par l’application pour communiquer avec le service web. Ces constantes nécessitent des mises à jour pour accéder à votre service principal personnel créé sur un fournisseur.|
|ITextToSpeech.cs|Le `ITextToSpeech` interface, qui spécifie que le `Speak` (méthode) doit être fournie par les classes d’implémentation.|
|TODO.cs|Le `App` classe qui est responsable de l’instanciation à la fois la première page qui sera affichée par l’application sur chaque plateforme, et la `TodoItemManager` classe qui est utilisée pour appeler des opérations de service web.|

### <a name="viewing-pages"></a>Affichage des Pages

La plupart des exemples d’applications contiennent au moins deux pages :

- **TodoListPage** : cette page affiche une liste de `TodoItem` instances et une icône de coche si le `TodoItem.Done` propriété est `true`. En cliquant sur un élément pour atteindre la `TodoItemPage`. En outre, les nouveaux éléments peuvent être créés en cliquant sur le *+* symbole.
- **TodoItemPage** : cette page affiche les détails pour le `TodoItem`et lui permet d’être modifié, enregistré, supprimés et parlée.

En outre, certains exemples d’applications contiennent des pages supplémentaires qui sont utilisés pour gérer le processus d’authentification utilisateur.

### <a name="modeling-the-data"></a>Modélisation de données

Chaque exemple d’application utilise le `TodoItem` classe pour modéliser les données qui s’affiche et est envoyées au service web pour le stockage. L’exemple de code suivant illustre la classe `TodoItem` :

```csharp
public class TodoItem
{
    public string ID { get; set; }
    public string Name { get; set; }
    public string Notes { get; set; }
    public bool Done { get; set; }
}
```

Le `ID` propriété est utilisée pour identifier de façon unique chaque `TodoItem` de l’instance et est utilisé par chaque service web pour identifier les données mises à jour ou à supprimer.

### <a name="invoking-web-service-operations"></a>Appeler des opérations de Service Web

Opérations de service Web sont accessibles via la `TodoItemManager` classe et une instance de la classe est accessible via la `App.TodoManager` propriété. La `TodoItemManager` classe fournit les méthodes suivantes pour appeler des opérations de service web :

- **GetTasksAsync** – cette méthode est utilisée pour remplir la `ListView` control sur la `TodoListPage` avec la `TodoItem` instances récupérées à partir du service web.
- **SaveTaskAsync** – cette méthode est utilisée pour créer ou mettre à jour un `TodoItem` instance sur le service web.
- **DeleteTaskAsync** – cette méthode est utilisée pour supprimer un `TodoItem` instance sur le service web.

En outre, certains exemples d’applications contiennent des méthodes supplémentaires dans le `TodoItemManager` (classe), qui sont utilisés pour gérer le processus d’authentification utilisateur.

Plutôt que d’appeler les opérations de service web directement, les `TodoItemManager` méthodes appellent des méthodes sur une classe dépendante qui est injecté dans le `TodoItemManager` constructeur. Par exemple, un exemple d’application injecte le `RestService` classe dans le `TodoItemManager` constructeur pour fournir l’implémentation qui utilise l’API REST pour accéder aux données.

### <a name="translating-text-to-speech"></a>Conversion de texte par synthèse vocale

La plupart des exemples d’applications contiennent les fonctionnalités de synthèse vocale parler les valeurs de la `TodoItem.Name` et `TodoItem.Notes` propriétés. Cela est accompli par le `OnSpeakActivated` Gestionnaire d’événements dans le `TodoItemPage` de classe, comme indiqué dans l’exemple de code suivant :

```csharp
void OnSpeakActivated (object sender, EventArgs e)
{
    var todoItem = (TodoItem)BindingContext;
    App.Speech.Speak(todoItem.Name + " " + todoItem.Notes);
}
```

Cette méthode appelle simplement la `Speak` méthode implémentée par un spécifique à la plateforme `Speech` classe. Chaque `Speech` la classe implémente le `ITextToSpeech` interface, et le code de démarrage spécifiques à la plateforme crée une instance de la `Speech` classe qui est accessible via la `App.Speech` propriété.

## <a name="summary"></a>Récapitulatif

Cette rubrique fourni une procédure pas à pas de l’exemple d’application Xamarin.Forms qui est utilisé pour illustrer comment communiquer avec les services web différents. Alors que chaque service web utilise une application d’exemple distinct, elles sont toutes basées sur la même interface utilisateur et la logique métier comme décrit ci-dessus - uniquement le mécanisme de stockage données web service est différent.


## <a name="related-links"></a>Liens associés

- [Version ASMX (exemple)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoASMX)
- [Version WCF (exemple)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoWCF)
- [Version REST (exemple)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoREST)
- [Version de Microsoft Azure (exemple)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzure)
