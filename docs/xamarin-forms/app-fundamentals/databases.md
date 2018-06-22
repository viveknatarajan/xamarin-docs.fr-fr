---
title: Bases de données locale de Xamarin.Forms
description: Xamarin.Forms prend en charge les applications pilotées par des base de données utilisant le moteur de base de données SQLite, ce qui rend possible charger et enregistrer les objets dans le code partagé. Cet article décrit comment les applications Xamarin.Forms peuvent lire et écrire des données dans une base de données SQLite local à l’aide de SQLite.Net.
ms.prod: xamarin
ms.assetid: F687B24B-7DF0-4F8E-A21A-A9BB507480EB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/21/2018
ms.openlocfilehash: feec4993a0719a083d713e084552b18aead8ee42
ms.sourcegitcommit: eac092f84b603958c761df305f015ff84e0fad44
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36310138"
---
# <a name="xamarinforms-local-databases"></a>Bases de données locale de Xamarin.Forms

_Xamarin.Forms prend en charge les applications pilotées par des base de données utilisant le moteur de base de données SQLite, ce qui rend possible charger et enregistrer les objets dans le code partagé. Cet article décrit comment les applications Xamarin.Forms peuvent lire et écrire des données dans une base de données SQLite local à l’aide de SQLite.Net._

## <a name="overview"></a>Vue d'ensemble

Xamarin.Forms les applications peuvent utiliser le [SQLite.NET PCL NuGet](https://www.nuget.org/packages/sqlite-net-pcl/) package à intégrer les opérations de base de données dans le code partagé en référençant le `SQLite` classes fournies dans NuGet. Opérations de base de données peuvent être définies dans le projet de bibliothèque .NET Standard de la solution Xamarin.Forms.

Les accompagnant [exemple d’application](https://github.com/xamarin/xamarin-forms-samples/tree/master/Todo) est une application simple de la liste de tâches. Les captures d’écran suivantes illustrent comment l’exemple s’affiche sur chaque plateforme :

[![Des captures d’écran exemple de base de données Xamarin.Forms](databases-images/todo-list-sml.png "TodoList première Page Screenshots")](databases-images/todo-list.png#lightbox "TodoList première Page Screenshots") [ ![ Des captures d’écran exemple de base de données Xamarin.Forms](databases-images/todo-list-sml.png "TodoList première Page Screenshots")](databases-images/todo-list.png#lightbox "TodoList première Page Screenshots")

<a name="Using_SQLite_with_PCL" />

## <a name="using-sqlite"></a>À l’aide de SQLite

Pour ajouter la prise en charge de SQLite dans une bibliothèque Xamarin.Forms .NET Standard, utiliser la fonction de recherche de NuGet pour rechercher **sqlite-net-pcl** et installez le dernier package :

![Ajouter un Package de bibliothèque PCL NuGet SQLite.NET](databases-images/vs2017-sqlite-pcl-nuget.png "ajouter un Package de bibliothèque PCL NuGet SQLite.NET")

Il existe un nombre de packages NuGet avec des noms similaires, le package possède ces attributs :

- **Créé par :** Frank A. Krueger
- **ID :** sqlite-net-pcl
- **Lien de NuGet :** [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)

> [!NOTE]
> Malgré le nom du package, utilisez le **sqlite-net-pcl** package NuGet même dans les projets .NET Standard.

Après avoir ajouté la référence, ajoutez une propriété à la `App` classe qui retourne un chemin d’accès de fichier local pour le stockage de la base de données :

```csharp
static TodoItemDatabase database;

public static TodoItemDatabase Database
{
  get
  {
    if (database == null)
    {
      database = new TodoItemDatabase(
        Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "TodoSQLite.db3"));
    }
    return database;
  }
}
```

Le `TodoItemDatabase` constructeur qui prend le chemin d’accès pour le fichier de base de données en tant qu’argument, est indiqué ci-dessous :

```csharp
public TodoItemDatabase(string dbPath)
{
  database = new SQLiteAsyncConnection(dbPath);
  database.CreateTableAsync<TodoItem>().Wait();
}
```

L’avantage d’exposition de la base de données comme un singleton est qu’une connexion de base de données unique est créée qui est maintenu ouvert lors de l’application s’exécute, par conséquent, ce qui évite les dépenses d’ouverture et de fermeture du fichier de base de données chaque fois qu’une opération de base de données est effectué.

Le reste de la `TodoItemDatabase` classe contient des requêtes de SQLite qui s’exécutent inter-plateformes. Exemple de code de requête est indiqué ci-dessous (vous trouverez plus d’informations sur la syntaxe dans le [à l’aide de SQLite.NET](~/cross-platform/app-fundamentals/index.md) article) :

```csharp
public Task<List<TodoItem>> GetItemsAsync()
{
  return database.Table<TodoItem>().ToListAsync();
}

public Task<List<TodoItem>> GetItemsNotDoneAsync()
{
  return database.QueryAsync<TodoItem>("SELECT * FROM [TodoItem] WHERE [Done] = 0");
}

public Task<TodoItem> GetItemAsync(int id)
{
  return database.Table<TodoItem>().Where(i => i.ID == id).FirstOrDefaultAsync();
}

public Task<int> SaveItemAsync(TodoItem item)
{
  if (item.ID != 0)
  {
    return database.UpdateAsync(item);
  }
  else {
    return database.InsertAsync(item);
  }
}

public Task<int> DeleteItemAsync(TodoItem item)
{
  return database.DeleteAsync(item);
}
```

> [!NOTE]
> L’avantage de l’utilisation de l’API SQLite.Net asynchrone est cette base de données opérations sont déplacées vers les threads d’arrière-plan. En outre, il n’est pas nécessaire d’écrire d’accès concurrentiel supplémentaire, la gestion du code, car l’API prend en charge de celui-ci.

## <a name="summary"></a>Récapitulatif

Xamarin.Forms prend en charge les applications pilotées par des base de données utilisant le moteur de base de données SQLite, ce qui rend possible charger et enregistrer les objets dans le code partagé.

Cet article se concentre sur **l’accès à** une base de données SQLite à l’aide de Xamarin.Forms. Pour plus d’informations sur l’utilisation des SQLite.Net lui-même, reportez-vous à la [SQLite.NET sur Android](~/android/data-cloud/data-access/using-sqlite-orm.md) ou [SQLite.NET sur iOS](~/ios/data-cloud/data/using-sqlite-orm.md) documentation.

## <a name="related-links"></a>Liens associés

- [Exemple de tâches](https://developer.xamarin.com/samples/xamarin-forms/Todo/)
- [Exemples Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Classeur de la base de données](https://developer.xamarin.com/workbooks/xamarin-forms/application-fundamentals/database/database.workbook)
