---
title: System.Data dans Xamarin.iOS
description: Ce document décrit comment utiliser System.Data et Mono.Data.Sqlite.dll pour accéder aux données SQLite dans une application Xamarin.iOS.
ms.prod: xamarin
ms.assetid: F10C0C57-7BDE-A3F3-B011-9839949D15C8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 183079c150ad4df05424d4dbf2980a307a889352
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997196"
---
# <a name="systemdata-in-xamarinios"></a>System.Data dans Xamarin.iOS

Xamarin.iOS 8.10 ajoute la prise en charge de [System.Data](xref:System.Data), y compris le `Mono.Data.Sqlite.dll` fournisseur ADO.NET. Prise en charge inclut l’ajout des éléments suivants [assemblys](~/cross-platform/internals/available-assemblies.md):

-  `System.Data.dll`
-  `System.Data.Service.Client.dll`
-  `System.Transactions.dll`
-  `Mono.Data.Tds.dll`
-  `Mono.Data.Sqlite.dll`

<a name="Example" />

## <a name="example"></a>Exemple

Le programme suivant crée une base de données `Documents/mydb.db3`, et si la base de données n’existe encore qu’il sont rempli avec des exemples de données. La base de données est ensuite interrogée, avec la sortie écrite dans `stderr`.

### <a name="add-references"></a>Ajouter des références

Tout d’abord, avec le bouton droit sur le **références** nœud et choisissez **modifier les références...**  puis sélectionnez `System.Data` et `Mono.Data.Sqlite`:

[![](system.data-images/edit-references-sml.png "Ajout de nouvelles références")](system.data-images/edit-references.png#lightbox)

### <a name="sample-code"></a>Exemple de code

Le code suivant montre un exemple simple de création d’une table et l’insertion de lignes à l’aide des commandes SQL incorporées :

```csharp
using System;
using System.Data;
using System.IO;
using Mono.Data.Sqlite;

class Demo {
    static void Main (string [] args)
    {
        var connection = GetConnection ();
        using (var cmd = connection.CreateCommand ()) {
            connection.Open ();
            cmd.CommandText = "SELECT * FROM People";
            using (var reader = cmd.ExecuteReader ()) {
                while (reader.Read ()) {
                    Console.Error.Write ("(Row ");
                    Write (reader, 0);
                    for (nint i = 1; i < reader.FieldCount; ++i) {
                        Console.Error.Write(" ");
                        Write (reader, i);
                    }
                    Console.Error.WriteLine(")");
                }
            }
            connection.Close ();
        }
    }

    static SqliteConnection GetConnection()
    {
        var documents = Environment.GetFolderPath (
                Environment.SpecialFolder.Personal);
        string db = Path.Combine (documents, "mydb.db3");
        bool exists = File.Exists (db);
        if (!exists)
            SqliteConnection.CreateFile (db);
        var conn = new SqliteConnection("Data Source=" + db);
        if (!exists) {
            var commands = new[] {
            "CREATE TABLE People (PersonID INTEGER NOT NULL, FirstName ntext, LastName ntext)",
            // WARNING: never insert user-entered data with embedded parameter values
            "INSERT INTO People (PersonID, FirstName, LastName) VALUES (1, 'First', 'Last')",
            "INSERT INTO People (PersonID, FirstName, LastName) VALUES (2, 'Dewey', 'Cheatem')",
            "INSERT INTO People (PersonID, FirstName, LastName) VALUES (3, 'And', 'How')",
            };
            conn.Open ();
            foreach (var cmd in commands) {
                using (var c = conn.CreateCommand()) {
                    c.CommandText = cmd;
                    c.CommandType = CommandType.Text;
                    c.ExecuteNonQuery ();
                }
            }
            conn.Close ();
        }
        return conn;
    }

    static void Write(SqliteDataReader reader, int index)
    {
        Console.Error.Write("({0} '{1}')",
                reader.GetName(index),
                reader [index]);
    }
}
```

> [!IMPORTANT]
> Comme indiqué dans l’exemple de code ci-dessus, il est déconseillé d’incorporer des chaînes dans les commandes SQL, car elle rend votre code vulnérable aux [injection SQL](http://en.wikipedia.org/wiki/SQL_injection).


### <a name="using-command-parameters"></a>À l’aide des paramètres de commande

Le code suivant montre comment utiliser les paramètres de commande pour insérer du texte d’entré par l’utilisateur en toute sécurité dans la base de données (même si le texte contient des caractères SQL spéciaux comme unique-apostrophe) :

```csharp
// user input from Textbox control
var fname = fnameTextbox.Text;
var lname = lnameTextbox.Text;
// use command parameters to safely insert into database
using (var addCmd = conn.CreateCommand ()) {
    addCmd.CommandText = "INSERT INTO [People] (PersonID, FirstName, LastName) VALUES (@COL1, @COL2, @COL3)";
    addCmd.CommandType = System.Data.CommandType.Text;
    addCmd.AddParameterWithValue ("@COL1", 1);
    addCmd.AddParameterWithValue ("@COL2", fname);
    addCmd.AddParameterWithValue ("@COL3", lname);
    addCmd.ExecuteNonQuery ();
}
```

<a name="Missing_Functionality" />

## <a name="missing-functionality"></a>Fonctionnalités manquantes

Les deux **System.Data** et **Mono.Data.Sqlite** manque certaines fonctionnalités.

<a name="System.Data" />

### <a name="systemdata"></a>System.Data

Fonctionnalités manquantes à partir de **System.Data.dll** se compose de :

-  Quoi que ce soit nécessitant [System.CodeDom](xref:System.CodeDom) (par exemple)  [System.Data.TypedDataSetGenerator](xref:System.Data.TypedDataSetGenerator) )
-  Fichier de configuration de XML prise en charge (par exemple)  [System.Data.Common.DbProviderConfigurationHandler](xref:System.Data.Common.DbProviderConfigurationHandler) )
-   [System.Data.Common.DbProviderFactories](xref:System.Data.Common.DbProviderFactories) (dépend de la prise en charge des fichiers de configuration XML)
-   [System.Data.OleDb](xref:System.Data.OleDb)
-   [System.Data.Odbc](xref:System.Data.Odbc)
-  Le `System.EnterpriseServices.dll` dépendance a été *supprimé* de `System.Data.dll` , qui entraîne la suppression de la [SqlConnection.EnlistDistributedTransaction(ITransaction)](xref:System.Data.SqlClient.SqlConnection.EnlistDistributedTransaction*) (méthode).


<a name="Mono.Data.Sqlite" />

### <a name="monodatasqlite"></a>Mono.Data.Sqlite

Pendant ce temps, **Mono.Data.Sqlite.dll** ont subi aucune modification de code source, mais au lieu de cela peut être l’hôte d’un nombre de *runtime* émet depuis `Mono.Data.Sqlite.dll` lie SQLite 3.5. iOS 8, est fourni dans le même temps, avec SQLite 3.8.5. Je me contenterais dire que, certaines choses ont changé entre les deux versions.

Une version antérieure d’e/s fournis avec les versions suivantes de SQLite :

- **iOS 7** -version 3.7.13.
- **iOS 6** -version 3.7.13.
- **iOS 5** -version 3.7.7.
- **iOS 4** -version 3.6.22.

Détermination d’apparaissent des problèmes les plus courants d’être liées à l’interrogation de schéma de base de données, par exemple, lors de l’exécution qui existe de colonnes sur une table donnée, tel que `Mono.Data.Sqlite.SqliteConnection.GetSchema` (substitution de [DbConnection.GetSchema](xref:System.Data.Common.DbConnection.GetSchema) et `Mono.Data.Sqlite.SqliteDataReader.GetSchemaTable` (substitution [DbDataReader.GetSchemaTable](xref:System.Data.Common.DbDataReader.GetSchemaTable). En bref, il semble que quoi que ce soit à l’aide [DataTable](xref:System.Data.DataTable) fonctionne.

<a name="Data_Binding" />

## <a name="data-binding"></a>Liaison de données

Liaison de données n’est pas pris en charge pour l’instant.

