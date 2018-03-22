---
title: "À l’aide d’ADO.NET"
ms.topic: article
ms.prod: xamarin
ms.assetid: 79078A4D-2D24-44F3-9543-B50418A7A000
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 8d7c942e0be52ed12fc20bc838e693af81dd989f
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
---
# <a name="using-adonet"></a>À l’aide d’ADO.NET

Xamarin prend en charge pour la base de données SQLite est disponible sur iOS, exposées à l’aide de la syntaxe classique d’ADO.NET de type. À l’aide de ces API vous oblige à écrire des instructions SQL qui sont traitées par SQLite, tel que `CREATE TABLE`, `INSERT` et `SELECT` instructions.

## <a name="assembly-references"></a>Références d'assembly

Pour utiliser l’accès SQLite via ADO.NET, vous devez ajouter `System.Data` et `Mono.Data.Sqlite` fait référence à votre projet iOS, comme illustré ici (pour obtenir des exemples dans Visual Studio pour Mac et Visual Studio) :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

 ![](using-adonet-images/image4.png "Références d’assembly dans Visual Studio pour Mac")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  ![](using-adonet-images/image6.png "Références d’assembly dans Visual Studio")

-----

Avec le bouton droit **références > modifier les références...**  , puis cliquez pour sélectionner les assemblys requis.

## <a name="about-monodatasqlite"></a>À propos de Mono.Data.Sqlite

Nous allons utiliser la `Mono.Data.Sqlite.SqliteConnection` classe pour créer un fichier de base de données vide, puis à instancier `SqliteCommand` objets que nous pouvons utiliser pour exécuter des instructions SQL par rapport à la base de données.


1. **Création d’une base de données vide** -appelez le `CreateFile` méthode avec un nom (ie. accessible en écriture) le chemin d’accès au fichier. Vous devez vérifier si le fichier existe déjà avant d’appeler cette méthode, sinon une nouvelle base de données (vide) est créé sur la partie supérieure de l’ancienne, et les données de l’ancien fichier seront perdues :

    `Mono.Data.Sqlite.SqliteConnection.CreateFile (dbPath);`

    > [!NOTE]
    > Le `dbPath` variable doit être déterminée selon les règles décrites précédemment dans ce document.

2. **Création d’une connexion de base de données** - une fois le fichier de base de données SQLite a été créé. vous pouvez créer un objet de connexion pour accéder aux données. La connexion est construite avec une chaîne de connexion qui prend la forme de `Data Source=file_path`, comme illustré ici :

    ```csharp
    var connection = new SqliteConnection ("Data Source=" + dbPath);
    connection.Open();
    // do stuff
    connection.Close();
    ```

    Comme mentionné précédemment, une connexion doit jamais être réutilisée entre les différents threads. En cas de doute, créer la connexion en fonction des besoins et la fermer lorsque vous avez terminé ; mais penser effectuant le plus souvent que nécessaire en trop.
    
3. **Création et exécution d’une commande de base de données** - une fois que nous avons une connexion, nous pouvons exécuter des commandes SQL arbitraires par rapport à elle. Le code suivant montre une instruction CREATE TABLE en cours d’exécution.

    ```csharp
    using (var command = connection.CreateCommand ()) {
        command.CommandText = "CREATE TABLE [Items] ([_id] int, [Symbol] ntext, [Name] ntext);";
        var rowcount = command.ExecuteNonQuery ();
    }
    ```

Lors de l’exécution SQL directement sur la base de données, que vous devez prendre les précautions normales d’utiliser des demandes non valides, tels que la tentative de création d’une table qui existe déjà. Effectuer le suivi de la structure de votre base de données afin que vous ne provoquent pas une SqliteException tels que « table d’erreur SQLite [éléments] existe déjà ».

## <a name="basic-data-access"></a>Accès aux données de base

Le *DataAccess_Basic* exemple de code pour ce document ressemble à ceci lorsque vous exécutez sur iOS :

 ![](using-adonet-images/image9.png "exemple d’ADO.NET iOS")

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

SQLite autorisant des commandes SQL arbitraires à exécuter sur les données, vous pouvez effectuer tout ce que créer, insérer, mettre à jour, supprimer ou que des instructions SELECT. Vous pouvez lire sur les commandes SQL prises en charge par SQLite sur le site Web de Sqlite. Les instructions SQL sont exécutées à l’aide d’une des trois méthodes sur un objet SqliteCommand :

-  **ExecuteNonQuery** : généralement utilisé pour l’insertion de données ou de la création de la table. La valeur de retour pour certaines opérations est le nombre de lignes affectées, sinon -1.
-  **ExecuteReader** – utilisé lors d’une collection de lignes doit être retournée comme un `SqlDataReader` .
-  **ExecuteScalar** : récupère une valeur unique (par exemple un agrégat).


### <a name="executenonquery"></a>EXECUTENONQUERY

Les instructions INSERT, UPDATE et DELETE retourne le nombre de lignes affectées. Toutes les autres instructions SQL retourne -1.

```csharp
using (var c = connection.CreateCommand ()) {
    c.CommandText = "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('1', 'APPL')";
    var rowcount = c.ExecuteNonQuery (); // rowcount will be 1
}
```

### <a name="executereader"></a>EXECUTEREADER

La méthode suivante montre une clause WHERE dans l’instruction SELECT. Étant donné que le code consiste à créer une instruction SQL complète il doit veiller à la séquence d’échappement les caractères réservés tels que l’apostrophe (') autour de chaînes.

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

La méthode ExecuteReader retourne un objet SqliteDataReader. En plus de la méthode Read indiquée dans l’exemple, les autres propriétés utiles sont les suivantes :

-  **RowsAffected** – nombre de lignes affectées par la requête.
-  **HasRows** : indique si toutes les lignes ont été retournées.


### <a name="executescalar"></a>EXECUTESCALAR

Utiliser pour les instructions SELECT qui retournent une valeur unique (par exemple, un agrégat).

```csharp
using (var contents = connection.CreateCommand ()) {
    contents.CommandText = "SELECT COUNT(*) FROM [Items] WHERE Symbol <> 'MSFT'";
    var i = contents.ExecuteScalar ();
}
```

Le `ExecuteScalar` est de type de retour de la méthode `object` – vous devez caster le résultat en fonction de la requête de base de données. Le résultat peut être un entier à partir d’une requête de nombre ou une chaîne à partir d’une requête de sélection de colonne unique. Notez que cela est différent à d’autres méthodes Execute qui retournent un objet de lecteur ou le nombre de lignes affectées.


## <a name="related-links"></a>Liens associés

- [DataAccess Basic (exemple)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess avancé (exemple)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS recettes de données](https://developer.xamarin.com/recipes/ios/data/sqlite/)
- [Accès aux données de Xamarin.Forms](~/xamarin-forms/app-fundamentals/databases.md)
