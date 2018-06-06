---
title: Bases de données locales
description: Xamarin.Forms prend en charge les applications pilotées par des base de données utilisant le moteur de base de données SQLite, ce qui rend possible charger et enregistrer les objets dans le code partagé. Cet article décrit comment les applications Xamarin.Forms peuvent lire et écrire des données dans une base de données SQLite local à l’aide de SQLite.Net.
ms.prod: xamarin
ms.assetid: F687B24B-7DF0-4F8E-A21A-A9BB507480EB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/31/2018
ms.openlocfilehash: d97fc792e2eb14f7e432d377811d1318c99b9602
ms.sourcegitcommit: a4c2a63ba76b839cda99e4474e7ab46fe307cd39
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34689446"
---
# <a name="local-databases"></a>Bases de données locales

_Xamarin.Forms prend en charge les applications pilotées par des base de données utilisant le moteur de base de données SQLite, ce qui rend possible charger et enregistrer les objets dans le code partagé. Cet article décrit comment les applications Xamarin.Forms peuvent lire et écrire des données dans une base de données SQLite local à l’aide de SQLite.Net._

## <a name="overview"></a>Vue d'ensemble

Xamarin.Forms les applications peuvent utiliser le [SQLite.NET PCL NuGet](https://www.nuget.org/packages/sqlite-net-pcl/) package à intégrer les opérations de base de données dans le code partagé en référençant le `SQLite` classes fournies dans NuGet. Opérations de base de données peuvent être définies dans le projet de bibliothèque .NET Standard de la solution Xamarin.Forms, avec des projets spécifiques à une plateforme retournant un chemin d’accès à la base de données où est stocké.

Les accompagnant [exemple d’application](https://github.com/xamarin/xamarin-forms-samples/tree/master/Todo) est une application simple de la liste de tâches. Les captures d’écran suivantes illustrent comment l’exemple s’affiche sur chaque plateforme :

[![Des captures d’écran exemple de base de données Xamarin.Forms](databases-images/todo-list-sml.png "TodoList première Page Screenshots")](databases-images/todo-list.png#lightbox "TodoList première Page Screenshots") [ ![ Des captures d’écran exemple de base de données Xamarin.Forms](databases-images/todo-list-sml.png "TodoList première Page Screenshots")](databases-images/todo-list.png#lightbox "TodoList première Page Screenshots")

<a name="Using_SQLite_with_PCL" />

## <a name="using-sqlite"></a>À l’aide de SQLite

Cette section montre comment ajouter des packages SQLite.Net NuGet à une solution Xamarin.Forms, écrire des méthodes pour effectuer des opérations de base de données et utiliser le [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md) pour déterminer un emplacement pour stocker la base de données sur chaque plateforme.

<a name="XamarinForms_PCL_Project" />

### <a name="xamarinsforms-net-standard-or-pcl-project"></a>Xamarins.Forms .NET Standard ou un projet de bibliothèque de classes portables

Pour ajouter la prise en charge de SQLite à un projet de Xamarin.Forms, utiliser la fonction de recherche de NuGet pour rechercher **sqlite-net-pcl** et installez le package :

![Ajouter un Package de bibliothèque PCL NuGet SQLite.NET](databases-images/vs2017-sqlite-pcl-nuget.png "ajouter un Package de bibliothèque PCL NuGet SQLite.NET")

Il existe un nombre de packages NuGet avec des noms similaires, le package possède ces attributs :

- **Créé par :** Frank A. Krueger
- **ID :** sqlite-net-pcl
- **Lien de NuGet :** [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)

> [!TIP]
> Utilisez le **sqlite-net-pcl** NuGet même dans les projets .NET Standard.

Une fois que la référence a été ajoutée, écrivez une interface pour faire abstraction de la fonctionnalité spécifique à la plateforme, qui consiste à déterminer l’emplacement du fichier de base de données. L’interface utilisée dans l’exemple définit une méthode unique :

```csharp
public interface IFileHelper
{
  string GetLocalFilePath(string filename);
}
```

Une fois que l’interface a été défini, utilisez le [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md) pour obtenir une implémentation et obtenir un chemin d’accès du fichier local (Notez que cette interface n’a pas encore été implémentée). Le code suivant obtient une implémentation le `App.Database` propriété :

```csharp
static TodoItemDatabase database;

public static TodoItemDatabase Database
{
  get
  {
    if (database == null)
    {
      database = new TodoItemDatabase(DependencyService.Get<IFileHelper>().GetLocalFilePath("TodoSQLite.db3"));
    }
    return database;
  }
}
```

Le `TodoItemDatabase` constructeur est indiqué ci-dessous :

```csharp
public TodoItemDatabase(string dbPath)
{
  database = new SQLiteAsyncConnection(dbPath);
  database.CreateTableAsync<TodoItem>().Wait();
}
```

Cette approche crée une connexion de base de données unique qui est maintenue ouverte pendant l’exécution de l’application, par conséquent, ce qui évite les dépenses d’ouverture et de fermeture du fichier de base de données chaque fois qu’une opération de base de données est effectuée.

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

Tous les le code d’accès aux données est écrit dans le projet de bibliothèque de classes portables à partager entre toutes les plateformes. Obtention d’un chemin d’accès local uniquement pour la base de données nécessite que du code de spécifique à la plateforme, comme indiqué dans les sections suivantes.

<a name="PCL_iOS" />

### <a name="ios-project"></a>iOS projet

Pour configurer l’application iOS, ajoutez le même package NuGet dans le projet iOS en utilisant le *NuGet* fenêtre :

![Ajouter un Package de bibliothèque PCL NuGet SQLite.NET](databases-images/vsmac-sqlite-nuget.png "ajouter un Package de bibliothèque PCL NuGet SQLite.NET")

Le seul code requis est le `IFileHelper` implémentation qui détermine le chemin d’accès du fichier de données. Le code suivant place le fichier de base de données SQLite dans le **/bases de données bibliothèque** dossier dans un sandbox de l’application. Consultez le [iOS fonctionne avec le système de fichiers](~/ios/app-fundamentals/file-system.md) documentation pour plus d’informations sur les répertoires différents qui sont disponibles pour le stockage.

```csharp
[assembly: Dependency(typeof(FileHelper))]
namespace Todo.iOS
{
  public class FileHelper : IFileHelper
  {
    public string GetLocalFilePath(string filename)
    {
      string docFolder = Environment.GetFolderPath(Environment.SpecialFolder.Personal);
      string libFolder = Path.Combine(docFolder, "..", "Library", "Databases");

      if (!Directory.Exists(libFolder))
      {
        Directory.CreateDirectory(libFolder);
      }

      return Path.Combine(libFolder, filename);
    }
  }
}
```

Notez que le code inclut le `assembly:Dependency` attribut afin que cette implémentation est détectable par le `DependencyService`.

<a name="PCL_Android" />

### <a name="android-project"></a>Projet Android

Pour configurer l’application Android, ajoutez le même package NuGet pour le projet Android à l’aide de la *NuGet* fenêtre :

![](databases-images/vsmac-sqlite-nuget.png "Ajouter un Package de bibliothèque PCL NuGet SQLite.NET")

Une fois que cette référence a été ajoutée, le seul code requis est le `IFileHelper` implémentation qui détermine le chemin d’accès du fichier de données.

```csharp
[assembly: Dependency(typeof(FileHelper))]
namespace Todo.Droid
{
  public class FileHelper : IFileHelper
  {
    public string GetLocalFilePath(string filename)
    {
        string path = Environment.GetFolderPath(Environment.SpecialFolder.Personal);
        return Path.Combine(path, filename);
    }
  }
}
```

<a name="PCL_UWP" />

### <a name="windows-10-universal-windows-platform-uwp"></a>Plateforme Windows universelle (UWP) Windows 10

Pour configurer l’application UWP, ajoutez le même package NuGet dans le projet UWP en utilisant le *NuGet* fenêtre :

![Ajouter un Package de bibliothèque PCL NuGet SQLite.NET](databases-images/vs2017-sqlite-uwp-nuget.png "ajouter un Package de bibliothèque PCL NuGet SQLite.NET")

Une fois que la référence est ajoutée, implémenter la `IFileHelper` à l’aide de la plateforme spécifique de l’interface `Windows.Storage` API afin de déterminer le chemin d’accès du fichier de données.

```csharp
using Windows.Storage;
...

[assembly: Dependency(typeof(FileHelper))]
namespace Todo.UWP
{
  public class FileHelper : IFileHelper
  {
    public string GetLocalFilePath(string filename)
    {
      return Path.Combine(ApplicationData.Current.LocalFolder.Path, filename);
    }
  }
}
```

## <a name="summary"></a>Récapitulatif

Xamarin.Forms prend en charge les applications pilotées par des base de données utilisant le moteur de base de données SQLite, ce qui rend possible charger et enregistrer les objets dans le code partagé.

Cet article se concentre sur **l’accès à** une base de données SQLite à l’aide de Xamarin.Forms. Pour plus d’informations sur l’utilisation des SQLite.Net lui-même, reportez-vous à la [SQLite.NET sur Android](~/android/data-cloud/data-access/using-sqlite-orm.md) ou [SQLite.NET sur iOS](~/ios/data-cloud/data/using-sqlite-orm.md) documentation. La plupart du code SQLite.Net est partageable sur toutes les plateformes ; Configurez uniquement l’emplacement du fichier de base de données SQLite requiert des fonctionnalités spécifiques à la plateforme.

## <a name="related-links"></a>Liens associés

- [Exemple de tâches](https://developer.xamarin.com/samples/xamarin-forms/Todo/)
- [Exemples Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Classeur de la base de données](https://developer.xamarin.com/workbooks/xamarin-forms/application-fundamentals/database/database.workbook)
