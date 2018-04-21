---
title: À l’aide de SQLite.NET avec Android
description: La bibliothèque SQLite.NET PCL NuGet fournit un mécanisme d’accès de données simple pour les applications de Xamarin.Android.
ms.prod: xamarin
ms.assetid: 3447B7EE-A320-489E-AF02-E5721097760A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/18/2018
ms.openlocfilehash: e8e6e98cb6ada8d8da494e408e8db66ad5038799
ms.sourcegitcommit: 797597d902330652195931dec9ac3e0cc00792c5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/20/2018
---
# <a name="using-sqlitenet-with-android"></a>À l’aide de SQLite.NET avec Android

La bibliothèque de SQLite.NET Xamarin recommande est un ORM très simple qui vous permet de stocker et récupérer des objets dans la base de données SQLite local sur un appareil Android. Représente le mappage relationnel objet ORM &ndash; une API qui vous permet d’enregistrer et de récupérer les « objets » à partir d’une base de données sans avoir à écrire des instructions SQL.

Pour inclure la bibliothèque SQLite.NET dans une application Xamarin, ajoutez le package NuGet suivant à votre projet :

- **Nom du package :** sqlite-net-pcl
- **Auteur :** Frank A. Krueger
- **ID :** sqlite-net-pcl
- **URL :** [nuget.org/packages/sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)

[![Package NuGet de SQLite.NET](using-sqlite-orm-images/image1a-sml.png "package NuGet de SQLite.NET")](using-sqlite-orm-images/image1a.png#lightbox)

> [!TIP]
> Il existe un nombre de packages de SQLite différents : veillez à choisir celle qui convient (il peut être le résultat supérieur dans la recherche).

Une fois que vous avez la bibliothèque SQLite.NET disponible, suivez ces trois étapes pour l’utiliser pour accéder à une base de données :

1.  **Ajouter un à l’aide instruction** &ndash; ajoutez l’instruction suivante pour les fichiers c# où l’accès aux données est requise :

    ```csharp
    using SQLite;
    ```

2.  **Créer une base de données vide** &ndash; une référence de base de données peut être créée en passant le chemin d’accès du constructeur de classe SQLiteConnection. Vous n’avez pas besoin de vérifier si le fichier existe déjà &ndash; il sera automatiquement créé si nécessaire, sinon le fichier existant de la base de données s’ouvre. Le `dbPath` variable doit être déterminée selon les règles décrites précédemment dans ce document :

    ```csharp
    var db = new SQLiteConnection (dbPath);
    ```

3.  **Enregistrer les données** &ndash; une fois que vous avez créé un objet SQLiteConnection, les commandes de base de données sont exécutées en appelant ses méthodes, telles que CREATE TABLE et insérer comme suit :

    ```csharp
    db.CreateTable<Stock> ();
    db.Insert (newStock); // after creating the newStock object
    ```

4.  **Récupérer des données** &ndash; pour récupérer un objet (ou une liste d’objets) utilisez la syntaxe suivante :

    ```csharp
    var stock = db.Get<Stock>(5); // primary key id of 5
    var stockList = db.Table<Stock>();
    ```

## <a name="basic-data-access-sample"></a>Exemple d’accès aux données de base

Le *DataAccess_Basic* exemple de code pour ce document ressemble à ceci lorsque vous exécutez sur Android. Le code montre comment effectuer des opérations simples SQLite.NET et affiche les résultats sous forme de texte dans la fenêtre principale de l’application.


**Android**

![Exemple d’Android SQLite.NET](using-sqlite-orm-images/image3.png "SQLite.NET Android exemple")

L’exemple de code suivant montre une interaction de base de données entière à l’aide de la bibliothèque SQLite.NET pour encapsuler l’accès de base de données sous-jacente.
Il montre :

1.  Création du fichier de base de données

2.  Insertion des données en créant des objets et enregistrez-les

3.  Interrogation des données

Vous devez inclure ces espaces de noms :

```csharp
using SQLite; // from the github SQLite.cs class
```

Il requiert que vous avez ajouté SQLite à votre projet. Notez que la table de base de données SQLite est définie en ajoutant des attributs à une classe (le `Stock` classe) au lieu d’une commande CREATE TABLE.

```csharp
[Table("Items")]
public class Stock {
    [PrimaryKey, AutoIncrement, Column("_id")]
    public int Id { get; set; }
    [MaxLength(8)]
    public string Symbol { get; set; }
}
public static void DoSomeDataAccess () {
       Console.WriteLine ("Creating database, if it doesn't already exist");
   string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "ormdemo.db3");
   var db = new SQLiteConnection (dbPath);
   db.CreateTable<Stock> ();
   if (db.Table<Stock> ().Count() == 0) {
        // only insert the data if it doesn't already exist
        var newStock = new Stock ();
        newStock.Symbol = "AAPL";
        db.Insert (newStock);
        newStock = new Stock ();
        newStock.Symbol = "GOOG";
        db.Insert (newStock);
        newStock = new Stock ();
        newStock.Symbol = "MSFT";
        db.Insert (newStock);
    }
    Console.WriteLine("Reading data");
    var table = db.Table<Stock> ();
    foreach (var s in table) {
        Console.WriteLine (s.Id + " " + s.Symbol);
    }
}
```

À l’aide de la `[Table]` attribut sans spécifier un paramètre de nom de table entraîne la table de base de données sous-jacente ont le même nom que la classe (dans ce cas, « Action »). Le nom de la table réelle est important si vous écrivez des requêtes SQL directement sur la base de données plutôt que d’utilisez les méthodes d’accès aux données ORM. De même la `[Column("_id")]` attribut est facultatif et si absent une colonne sera ajouté à la table avec le même nom que la propriété de la classe.

## <a name="sqlite-attributes"></a>Attributs de SQLite

Attributs courants que vous pouvez appliquer à vos classes pour contrôler la façon dont elles sont stockées dans la base de données sous-jacente sont les suivantes :

-   **[PrimaryKey]**  &ndash; Cet attribut peut être appliqué à une propriété entière pour le forcer à être la clé primaire de la table sous-jacente. Les clés primaires composites ne sont pas pris en charge.

-   **[AutoIncrement]**  &ndash; Cet attribut entraîne la valeur d’une propriété entière à incrémentation automatique pour chaque nouvel objet inséré dans la base de données

-   **[Column(name)]**  &ndash; En fournissant le paramètre facultatif `name` paramètre remplace la valeur par défaut du nom de la colonne de base de données sous-jacente (qui est identique à la propriété).

-   **[Table(name)]**  &ndash; Marque la classe comme pouvant être stockées dans une table de SQLite sous-jacente. En spécifiant le paramètre de nom facultatif remplace la valeur par défaut du nom de la table de base de données sous-jacente (qui est le même que le nom de classe).

-   **[MaxLength(value)]**  &ndash; Restreignent la longueur d’une propriété de texte, lors de la tentative d’insertion d’une base de données. Consommation du code doit effectuer cette vérification avant l’insertion de l’objet que cet attribut est uniquement « coché » lors de l’insertion d’une base de données ou l’opération de mise à jour est tentée.

-   **[Ignorer]**  &ndash; Provoque un SQLite.NET pour ignorer cette propriété.
    Cela est particulièrement utile pour les propriétés qui ont un type qui ne peut pas être stocké dans la base de données ou les propriétés que les collections du modèle qui ne peut pas être résolues automatiquement être SQLite.

-   **[Unique]**  &ndash; Garantit que les valeurs dans la colonne de base de données sous-jacente sont uniques.


La plupart de ces attributs sont facultatifs, SQLite utilise les valeurs par défaut pour les noms de table et de colonne. Vous devez toujours spécifier une clé primaire de type entier afin que les requêtes de sélection et la suppression peuvent être effectuées efficacement sur vos données.

## <a name="more-complex-queries"></a>Requêtes plus complexes.

Les méthodes suivantes sur `SQLiteConnection` peut être utilisé pour effectuer d’autres opérations de données :

-   **Insérer** &ndash; ajoute un nouvel objet à la base de données.

-   **Obtenir&lt;T&gt;**  &ndash; tente de récupérer un objet à l’aide de la clé primaire.

-   **Table&lt;T&gt;**  &ndash; renvoie tous les objets de la table.

-   **Supprimer** &ndash; supprime un objet à l’aide de sa clé primaire.

-   **Requête&lt;T&gt;**  &ndash; exécuter une requête SQL qui retourne un nombre de lignes (sous forme d’objets).

-   **Exécutez** &ndash; utiliser cette méthode (et non `Query`) lorsque vous ne prévoyez pas les lignes à partir de l’instruction SQL (par exemple, les instructions INSERT, UPDATE et DELETE).


### <a name="getting-an-object-by-the-primary-key"></a>Obtention d’un objet à la clé primaire

SQLite.Net fournit la méthode Get pour extraire un seul objet en fonction de sa clé primaire.

```csharp
var existingItem = db.Get<Stock>(3);
```

### <a name="selecting-an-object-using-linq"></a>Sélection d’un objet à l’aide de Linq

Prend en charge les méthodes qui retournent des collections `IEnumerable<T>` afin de pouvoir utiliser Linq pour interroger ou de trier le contenu d’une table. Le code suivant montre un exemple d’utilisation de Linq pour filtrer toutes les entrées qui commencent par la lettre « A » :

```csharp
var apple = from s in db.Table<Stock>()
    where s.Symbol.StartsWith ("A")
    select s;
Console.WriteLine ("-> " + apple.FirstOrDefault ().Symbol);
```

### <a name="selecting-an-object-using-sql"></a>Sélection d’un objet à l’aide de SQL

Même si SQLite.Net peut fournir basée sur un objet d’accès à vos données, parfois, vous devrez peut-être faire une requête plus complexe que ce qui permet de Linq (ou vous devrez peut-être améliorer les performances). Vous pouvez utiliser des commandes SQL avec la méthode de requête, comme indiqué ici :

```csharp
var stocksStartingWithA = db.Query<Stock>("SELECT * FROM Items WHERE Symbol = ?", "A");
foreach (var s in stocksStartingWithA) {
    Console.WriteLine ("a " + s.Symbol);
}
```

> [!NOTE]
> Lors de l’écriture d’instructions SQL directement, vous créez une dépendance sur les noms des tables et des colonnes dans votre base de données qui ont été générés à partir de vos classes et leurs attributs. Si vous modifiez ces noms dans votre code vous devez mettre à jour toutes les instructions SQL écrites manuellement.

### <a name="deleting-an-object"></a>Suppression d’un objet

La clé primaire est utilisée pour supprimer la ligne, comme illustré ici :

```csharp
var rowcount = db.Delete<Stock>(someStock.Id); // Id is the primary key
```

Vous pouvez vérifier le `rowcount` pour vérifier le nombre de lignes affecté (supprimé dans ce cas).

## <a name="using-sqlitenet-with-multiple-threads"></a>À l’aide de SQLite.NET avec plusieurs Threads

SQLite prend en charge trois modes de threads : *seul thread*, *multithread*, et *sérialisé*. Si vous souhaitez accéder à la base de données à partir de plusieurs threads sans aucune restriction, vous pouvez configurer SQLite à utiliser le **sérialisé** en mode de thread. Il est important de définir ce mode tôt dans votre application (par exemple, au début de la `OnCreate` méthode).

Pour modifier le mode de thread, appelez `SqliteConnection.SetConfig`. Par exemple, cette ligne de code configure SQLite pour **sérialisé** mode : 

```csharp
SqliteConnection.SetConfig(SQLiteConfig.Serialized);
```

La version Android de SQLite a une limite qui nécessite quelques étapes supplémentaires. Si l’appel à `SqliteConnection.SetConfig` génère une exception de SQLite comme `library used incorrectly`, vous devez utiliser la solution de contournement suivante :

1.  Lien vers natif **libsqlite.so** bibliothèque afin que les `sqlite3_shutdown` et `sqlite3_initialize` API est accessibles à l’application :

    ```csharp
    [DllImport("libsqlite.so")]
    internal static extern int sqlite3_shutdown();

    [DllImport("libsqlite.so")]
    internal static extern int sqlite3_initialize();
    ```


2.  Dès le début de la `OnCreate` (méthode), ajoutez le code d’arrêt SQLite, configurez-le pour **sérialisé** mode et réinitialisez SQLite :

    ```csharp
    sqlite3_shutdown();
    SqliteConnection.SetConfig(SQLiteConfig.Serialized);
    sqlite3_initialize();
    ```

Cette solution de contournement fonctionne également pour les `Mono.Data.Sqlite` bibliothèque. Pour plus d’informations sur SQLite et multi-threading, consultez [SQLite et plusieurs Threads](https://www.sqlite.org/threadsafe.html). 

## <a name="related-links"></a>Liens associés

- [DataAccess Basic (exemple)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess avancé (exemple)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Recettes de données Android](https://developer.xamarin.com/recipes/android/data/)
- [Accès aux données de Xamarin.Forms](~/xamarin-forms/app-fundamentals/databases.md)
