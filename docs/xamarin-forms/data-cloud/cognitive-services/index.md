---
title: "Ajout de l’Intelligence avec les Services cognitifs"
description: "Services cognitifs Microsoft sont un ensemble d’API, les kits de développement logiciel et les services disponibles aux développeurs pour rendre leurs applications plus intelligente en ajoutant des fonctionnalités telles que la reconnaissance des visages, la reconnaissance vocale et compréhension de la langue. Cet article fournit une introduction à l’application d’exemple qui montre comment appeler des API des services Microsoft cognitifs."
ms.topic: article
ms.prod: xamarin
ms.assetid: 74121ADB-1322-4C1E-A103-F37257BC7CB0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 650f8dceebb088b3601c21c1f5373fc4ae8c76dc
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/28/2018
---
# <a name="adding-intelligence-with-cognitive-services"></a>Ajout de l’Intelligence avec les Services cognitifs

_Services cognitifs Microsoft sont un ensemble d’API, les kits de développement logiciel et les services disponibles aux développeurs pour rendre leurs applications plus intelligente en ajoutant des fonctionnalités telles que la reconnaissance des visages, la reconnaissance vocale et compréhension de la langue. Cet article fournit une introduction à l’application d’exemple qui montre comment appeler des API des services Microsoft cognitifs._

## <a name="overview"></a>Vue d'ensemble

L’exemple fourni est une application de liste todo qui fournit une fonctionnalité pour :

- Afficher la liste des tâches.
- Ajouter et modifier des tâches via le clavier logiciel, ou en effectuant la reconnaissance vocale avec l’API de reconnaissance vocale Bing. Pour plus d’informations sur l’exécution de la reconnaissance vocale, consultez [la reconnaissance vocale à l’aide de l’API de reconnaissance vocale Bing](speech-recognition.md).
- L’orthographe dans les tâches de vérification à l’aide de l’API de vérification orthographique Bing. Pour plus d’informations, consultez [orthographique à l’aide de l’API de vérification orthographique Bing](spell-check.md).
- Traduire des tâches de l’anglais, allemand à l’aide de l’API du traducteur. Pour plus d’informations, consultez [traduction de texte à l’aide de l’API du traducteur](text-translation.md).
- Supprimer des tâches.
- Définir l’état d’une tâche à 'done'.
- Taux de l’application avec la reconnaissance émotion, à l’aide de l’API émotion. Pour plus d’informations, consultez [émotion la reconnaissance à l’aide de l’API émotion](emotion-recognition.md).

Les tâches sont stockées dans une base de données local SQLite. Pour plus d’informations sur l’utilisation d’une base de données SQLite local, consultez [fonctionne avec une base de données locale](~/xamarin-forms/app-fundamentals/databases.md).

Le `TodoListPage` s’affiche lorsque l’application est lancée. Cette page affiche une liste de toutes les tâches stockées dans la base de données locale et permet à l’utilisateur pour créer une nouvelle tâche ou du taux de l’application :

![](images/sample-application-1.png "TodoListPage")

Nouveaux éléments peuvent être créés en cliquant sur le  *+*  bouton qui accède à la `TodoItemPage`. Cette page peut également être parcourue à en sélectionnant une tâche :

![](images/sample-application-2.png "TodoItemPage")

Le `TodoItemPage` permet de tâches doit être créée, modifiée, une vérification orthographique, traduit, enregistrés et supprimés. La reconnaissance vocale peut être utilisée pour créer ou modifier une tâche. Cela est possible en appuyant sur le bouton microphone pour commencer l’enregistrement et en appuyant sur le même bouton une deuxième fois pour arrêter l’enregistrement, qui envoie l’enregistrement à l’API de reconnaissance vocale Bing.

En cliquant sur le bouton smilies sur le `TodoListPage` accède à la `RateAppPage`, qui sert à effectuer une reconnaissance émotion sur une image d’une expression faciale :

![](images/sample-application-3.png "RateAppPage")

Le `RateAppPage` permet à l’utilisateur de prendre une photo de leur face, est soumis à l’API émotion avec l’émotion retournée affichée.

## <a name="understanding-the-application-anatomy"></a>Présentation de la composition de l’Application

Le projet de bibliothèque de classes Portable (PCL) pour l’exemple d’application se compose de cinq dossiers principaux :

<table>
    <thead>
        <tr><td><strong>Folder</strong></td><td><strong>Purpose</strong></td></tr>
    </thead>
    <tbody>
        <tr>
            <td><strong>Modèles</strong></td>
            <td>Contient les classes de modèle de données pour l’application. Cela inclut la <code>TodoItem</code> classe, qui modélise un seul élément de données utilisées par l’application. Le dossier comprend également des classes utilisées pour les réponses JSON de modèle retournées à partir de différentes API de Service cognitifs Microsoft.</td>
        </tr>
        <tr>
            <td><strong>Repositories</strong></td>
                        <td>Contient le <code>ITodoItemRepository</code> interface et <code>TodoItemRepository</code> classe qui sont utilisés pour effectuer des opérations de base de données.</td>
        </tr>
        <tr>
            <td><strong>Services</strong></td>
                        <td>Contient les interfaces et les classes qui permettent d’accéder aux différentes Microsoft cognitifs API de Service, ainsi que les interfaces qui sont utilisées par la <code>DependencyService</code> classe pour localiser les classes qui implémentent les interfaces dans les projets de plateforme.</td>
        </tr>
        <tr>
            <td><strong>Utils</strong></td>
            <td>Contient le <code>Timer</code> (classe), qui est utilisée par la <code>AuthenticationService</code> classe pour renouveler un jeton d’accès JWT toutes les minutes 9.</td>
        </tr>
        <tr>
            <td><strong>Vues</strong></td>
            <td>Contient les pages de l’application.</td>
        </tr>
    </tbody>
</table>

Projet de bibliothèque PCL contient également certains fichiers importants :

<table>
    <thead>
      <tr><td><strong>Fichier</strong></td><td><strong>Purpose</strong></td></tr>
    <thead>
    <tbody>
        <tr>
            <td><strong>Constants.cs</strong></td>
            <td>Le <code>Constants</code> (classe), qui spécifie les clés de l’API et les points de terminaison pour l’API Service cognitifs Microsoft qui sont appelées. Les constantes de clé API nécessitent la mise à jour pour accéder aux API de Service cognitifs différents.
        </tr>
        <tr>
          <td><strong>App.xaml.cs</strong></td>
          <td>Le <code>App</code> classe est responsable de l’instanciation à la fois la première page qui sera affichée par l’application sur chaque plateforme, et la <code>TodoManager</code> classe qui est utilisée pour appeler des opérations de base de données.</td>
        </tr>
    </tbody>
</table>

### <a name="nuget-packages"></a>Packages NuGet

L’exemple d’application utilise les packages NuGet suivants :

- `Microsoft.Net.Http` : fournit la `HttpClient` classe pour effectuer des requêtes sur HTTP.
- `Newtonsoft.Json` : fournit une infrastructure JSON pour .NET.
- `Microsoft.ProjectOxford.Emotion` – une bibliothèque cliente pour accéder à l’API émotion.
- `PCLStorage` : fournit un ensemble d’API de l’e/s d’un fichier local inter-plateformes.
- `sqlite-net-pcl` : fournit le stockage de base de données SQLite.
- `Xam.Plugin.Media` : fournit la prise de photo multiplateforme et l’API de prélèvement.

En outre, ces packages NuGet installent leurs dépendances.

### <a name="modeling-the-data"></a>Modélisation de données

L’exemple d’application utilise le `TodoItem` classe pour modéliser les données qui s’affiche et est stockées dans la base de données local SQLite. L’exemple de code suivant illustre la classe `TodoItem` :

```csharp
public class TodoItem
{
  [PrimaryKey, AutoIncrement]
  public int ID { get; set; }
  public string Name { get; set; }
  public bool Done { get; set; }
}
```

Le `ID` propriété est utilisée pour identifier de façon unique chaque `TodoItem` instance et elle est décorée avec des attributs de SQLite qui rendent la propriété d’une clé primaire auto-incrémentée dans la base de données.

### <a name="invoking-database-operations"></a>Appeler des opérations de base de données

Le `TodoItemRepository` classe implémente les opérations de base de données, et une instance de la classe est accessible via la `App.TodoManager` propriété. La `TodoItemRepository` classe fournit les méthodes suivantes pour appeler des opérations de base de données :

- **GetAllItemsAsync** – récupère tous les éléments à partir de la base de données local SQLite.
- **GetItemAsync** – récupère un élément spécifié à partir de la base de données local SQLite.
- **SaveItemAsync** – crée ou met à jour un élément dans la base de données local SQLite.
- **DeleteItemAsync** : supprime l’élément spécifié de la base de données local SQLite.

### <a name="platform-project-implementations"></a>Implémentations de projet de plateforme

Le `Services` dossier dans le projet de bibliothèque de classes portables contient le `IFileHelper` et `IAudioRecorderService` interfaces qui sont utilisées par la `DependencyService` classe pour localiser les classes qui implémentent les interfaces dans les projets de plateforme.

Le `IFileHelper` interface est implémentée par la `FileHelper` classe dans chaque projet de plateforme. Cette classe se compose d’une méthode unique, `GetLocalFilePath`, qui retourne un chemin d’accès de fichier local pour le stockage de la base de données SQLite.

Le `IAudioRecorderService` interface est implémentée par la `AudioRecorderService` classe dans chaque projet de plateforme. Cette classe se compose de `StartRecording`, `StopRecording`et la prise en charge des méthodes que vous utilisent les API de la plateforme pour enregistrer des données audio de microphone de l’appareil et enregistrez-le sous forme de fichier wav. Sur iOS, le `AudioRecorderService` utilise le `AVFoundation` API pour l’enregistrement audio. Sur Android, le `AudioRecordService` utilise le `AudioRecord` API pour l’enregistrement audio. Sur la plate-forme de Windows universelle (UWP), le `AudioRecorderService` utilise le `AudioGraph` API pour l’enregistrement audio.

### <a name="invoking-cognitive-services"></a>Appel de Services cognitifs

L’exemple d’application appelle les Services cognitifs Microsoft suivants :

- Vocale Bing API. Pour plus d’informations, consultez [la reconnaissance vocale à l’aide de l’API de reconnaissance vocale Bing](speech-recognition.md).
- Vérification de l’orthographe Bing API. Pour plus d’informations, consultez [orthographique à l’aide de l’API de vérification orthographique Bing](spell-check.md).
- Traduire des API. Pour plus d’informations, consultez [traduction de texte à l’aide de l’API du traducteur](text-translation.md).
- Émotion API. Pour plus d’informations, consultez [émotion la reconnaissance à l’aide de l’API émotion](emotion-recognition.md).


## <a name="related-links"></a>Liens associés

- [Documentation de Microsoft Services cognitifs](https://www.microsoft.com/cognitive-services/documentation)
- [Services de troubles cognitifs TODO (exemple)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
