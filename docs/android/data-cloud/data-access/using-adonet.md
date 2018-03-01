---
title: "À l’aide d’ADO.NET"
ms.topic: article
ms.prod: xamarin
ms.assetid: F6ABCEF1-951E-40D8-9EA9-DD79123C2650
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: 6c5b01f30bf2bbdf7ad8a7fbca2500b220a38d01
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="using-adonet"></a>À l’aide d’ADO.NET

Xamarin prend en charge pour la base de données SQLite est disponible sur Android et permettre être exposée à l’aide de la syntaxe classique d’ADO.NET de type. À l’aide de ces API vous oblige à écrire des instructions SQL qui sont traitées par SQLite, tel que `CREATE TABLE`, `INSERT` et `SELECT` instructions.

## <a name="assembly-references"></a>Références d'assembly

Pour utiliser l’accès SQLite via ADO.NET, vous devez ajouter `System.Data` et `Mono.Data.Sqlite` fait référence à votre projet Android, comme illustré ici :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin) 

![Références Android dans Visual Studio](using-adonet-images/image7.png "Android références dans Visual Studio") 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac) 

![Références Android dans Visual Studio pour Mac](using-adonet-images/image5.png "Android fait référence à Visual Studio pour Mac") 

-----


Avec le bouton droit **références > modifier les références...**  , puis cliquez pour sélectionner les assemblys requis.

## <a name="about-monodatasqlite"></a>À propos de Mono.Data.Sqlite

Nous allons utiliser la `Mono.Data.Sqlite.SqliteConnection` classe pour créer un fichier de base de données vide, puis à instancier `SqliteCommand` objets que nous pouvons utiliser pour exécuter des instructions SQL par rapport à la base de données.

**Création d’une base de données vide** &ndash; appeler le `CreateFile` méthode avec valide (ie. accessible en écriture) le chemin d’accès au fichier. Vous devez vérifier si le fichier existe déjà avant d’appeler cette méthode, sinon une nouvelle base de données (vide) est créé sur la partie supérieure de l’ancienne, et les données de l’ancien fichier seront perdues.
`Mono.Data.Sqlite.SqliteConnection.CreateFile (dbPath);` Le `dbPath` variable doit être déterminée selon les règles décrites précédemment dans ce document.

**Création d’une connexion de base de données** &ndash; une fois que le fichier de base de données SQLite a été créé, vous pouvez créer un objet de connexion pour accéder aux données. La connexion est construite avec une chaîne de connexion qui prend la forme de `Data Source=file_path`, comme illustré ici :

```csharp
var connection = new SqliteConnection ("Data Source=" + dbPath);
connection.Open();
// do stuff
connection.Close();
```

Comme mentionné précédemment, une connexion doit jamais être réutilisée entre les différents threads. En cas de doute, créer la connexion en fonction des besoins et la fermer lorsque vous avez terminé ; mais penser effectuant le plus souvent que nécessaire en trop.

**Création et exécution d’une commande de base de données** &ndash; une fois une connexion, nous pouvons exécuter des commandes SQL arbitraires par rapport à elle. Le code ci-dessous montre un `CREATE TABLE` instruction en cours d’exécution.

```csharp
using (var command = connection.CreateCommand ()) {
    command.CommandText = "CREATE TABLE [Items] ([_id] int, [Symbol] ntext, [Name] ntext);";
    var rowcount = command.ExecuteNonQuery ();
}
```

Lors de l’exécution SQL directement sur la base de données, que vous devez prendre les précautions normales d’utiliser des demandes non valides, tels que la tentative de création d’une table qui existe déjà. Effectuer le suivi de la structure de votre base de données afin que vous ne provoquent pas un `SqliteException` comme **table d’erreur SQLite [éléments] existe déjà**.

## <a name="basic-data-access"></a>Accès aux données de base

Le *DataAccess_Basic* exemple de code pour ce document ressemble à ceci lorsque vous exécutez sur Android :

![Exemple d’ADO.NET Android](using-adonet-images/image8.png "Android ADO.NET (exemple)")

Le code ci-dessous montre comment effectuer des opérations simples de SQLite et affiche les résultats sous forme de texte dans la fenêtre principale de l’application.

Vous devez inclure ces espaces de noms :

```csharp
using System;
using System.IO;
using Mono.Data.Sqlite;
```

L’exemple de code suivant montre une interaction de base de données entière :

1.  Création du fichier de base de données
2.  Insertion des données
3.  Interrogation des données

Ces opérations généralement apparaît dans plusieurs emplacements dans votre code, par exemple vous pouvez créer le fichier de base de données et des tables au premier démarrage de votre application et effectuer des lectures de données et les écritures dans les écrans individuels dans votre application. Dans l’exemple ci-dessous ont été regroupées dans une seule méthode pour cet exemple :

```csharp
public static SqliteConnection connection;
public static string DoSomeDataAccess ()
{
    // determine the path for the database file
    string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "adodemo.db3");

    bool exists = File.Exists (dbPath);

    if (!exists) {
        Console.WriteLine("Creating database");
        // Need to create the database before seeding it with some data
        Mono.Data.Sqlite.SqliteConnection.CreateFile (dbPath);
        connection = new SqliteConnection ("Data Source=" + dbPath);

        var commands = new[] {
            "CREATE TABLE [Items] (_id ntext, Symbol ntext);",
            "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('1', 'AAPL')",
            "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('2', 'GOOG')",
            "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('3', 'MSFT')"
        };
        // Open the database connection and create table with data
        connection.Open ();
        foreach (var command in commands) {
            using (var c = connection.CreateCommand ()) {
                c.CommandText = command;
                var rowcount = c.ExecuteNonQuery ();
                Console.WriteLine("\tExecuted " + command);
            }
        }
    } else {
        Console.WriteLine("Database already exists");
        // Open connection to existing database file
        connection = new SqliteConnection ("Data Source=" + dbPath);
        connection.Open ();
    }

    // query the database to prove data was inserted!
    using (var contents = connection.CreateCommand ()) {
        contents.CommandText = "SELECT [_id], [Symbol] from [Items]";
        var r = contents.ExecuteReader ();
        Console.WriteLine("Reading data");
        while (r.Read ())
            Console.WriteLine("\tKey={0}; Value={1}",
                              r ["_id"].ToString (),
                              r ["Symbol"].ToString ());
    }
    connection.Close ();
}

```

## <a name="more-complex-queries"></a>Requêtes plus complexes.

SQLite autorisant des commandes SQL arbitraires à exécuter sur les données, vous pouvez effectuer tout ce qui `CREATE`, `INSERT`, `UPDATE`, `DELETE`, ou `SELECT` instructions que vous le souhaitez. Vous pouvez lire sur les commandes SQL prises en charge par SQLite sur le site Web de Sqlite. Les instructions SQL sont exécutées à l’aide d’une des trois méthodes sur un `SqliteCommand` objet :

-   **ExecuteNonQuery** &ndash; généralement utilisé pour l’insertion de données ou de la création de la table. La valeur de retour pour certaines opérations est le nombre de lignes affectées, sinon -1.

-   **ExecuteReader** &ndash; utilisé lors d’une collection de lignes doit être retournée comme un `SqlDataReader`.

-   **ExecuteScalar** &ndash; récupère une valeur unique (par exemple un agrégat).


### <a name="executenonquery"></a>EXECUTENONQUERY

`INSERT`, `UPDATE`, et `DELETE` instructions renvoie le nombre de lignes affectées. Toutes les autres instructions SQL retourne -1.

```csharp
using (var c = connection.CreateCommand ()) {
    c.CommandText = "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('1', 'APPL')";
    var rowcount = c.ExecuteNonQuery (); // rowcount will be 1
}
```

### <a name="executereader"></a>EXECUTEREADER

L’exemple de méthode suivant un `WHERE` clause dans la `SELECT` instruction.
Étant donné que le code consiste à créer une instruction SQL complète il doit veiller à la séquence d’échappement les caractères réservés tels que l’apostrophe (') autour de chaînes.

```csharp
public static string MoreComplexQuery ()
{
    var output = "";
    output += "\nComplex query example: ";
    string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal), "ormdemo.db3");

    connection = new SqliteConnection ("Data Source=" + dbPath);
    connection.Open ();
    using (var contents = connection.CreateCommand ()) {
        contents.CommandText = "SELECT * FROM [Items] WHERE Symbol = 'MSFT'";
        var r = contents.ExecuteReader ();
        output += "\nReading data";
        while (r.Read ())
            output += String.Format ("\n\tKey={0}; Value={1}",
                    r ["_id"].ToString (),
                    r ["Symbol"].ToString ());
    }
    connection.Close ();

    return output;
}
```

La méthode `ExecuteReader` retourne un objet `SqliteDataReader`. Outre la `Read` méthode illustrée dans l’exemple, incluent d’autres propriétés utiles :

-   **RowsAffected** &ndash; nombre de lignes affectées par la requête.

-   **HasRows** &ndash; si toutes les lignes ont été retournées.


### <a name="executescalar"></a>EXECUTESCALAR

Utilisez cette option pour `SELECT` instructions qui retournent une valeur unique (par exemple, un agrégat).

```csharp
using (var contents = connection.CreateCommand ()) {
    contents.CommandText = "SELECT COUNT(*) FROM [Items] WHERE Symbol <> 'MSFT'";
    var i = contents.ExecuteScalar ();
}
```

Le `ExecuteScalar` est de type de retour de la méthode `object` &ndash; vous devez caster le résultat en fonction de la requête de base de données. Le résultat peut être un entier compris entre un `COUNT` requête ou une chaîne à partir d’une seule colonne `SELECT` requête. Notez que cela est différent des autres `Execute` méthodes qui retournent un objet de lecteur ou le nombre de lignes affectées.



## <a name="related-links"></a>Liens associés

- [DataAccess Basic (exemple)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess avancé (exemple)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Recettes de données Android](https://developer.xamarin.com/recipes/android/data/)
- [Accès aux données de Xamarin.Forms](~/xamarin-forms/app-fundamentals/databases.md)
