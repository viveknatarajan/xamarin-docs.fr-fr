---
title: System.Data
ms.topic: article
ms.prod: xamarin
ms.assetid: F10C0C57-7BDE-A3F3-B011-9839949D15C8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 145a0692ed9761944eec4c7ece1f098a584f2d54
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
---
# <a name="systemdata"></a>System.Data

Xamarin.iOS 8.10 ajoute la prise en charge de [System.Data](https://developer.xamarin.com/api/namespace/System.Data/), y compris le `Mono.Data.Sqlite.dll` fournisseur ADO.NET. Prise en charge inclut l’ajout des éléments suivants [assemblys](~/cross-platform/internals/available-assemblies.md):

-  `System.Data.dll`
-  `System.Data.Service.Client.dll`
-  `System.Transactions.dll`
-  `Mono.Data.Tds.dll`
-  `Mono.Data.Sqlite.dll`


<a name="Example" />

## <a name="example"></a>Exemple

Le programme suivant crée une base de données `Documents/mydb.db3`, et si la base de données inexistante précédemment qu’il sont rempli avec des exemples de données. La base de données est ensuite interrogée, la sortie écrite dans `stderr`.

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

Le code suivant montre comment utiliser des paramètres de commande pour insérer du texte d’entré par l’utilisateur en toute sécurité dans la base de données (même si le texte contient des caractères spéciaux SQL comme unique-apostrophe) :

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

Les fonctionnalités manquantes à partir de **System.Data.dll** se compose de :

-  Tout nécessitant [System.CodeDom](https://developer.xamarin.com/api/namespace/System.CodeDom/) (par exemple)  [System.Data.TypedDataSetGenerator](https://developer.xamarin.com/api/type/System.Data.TypedDataSetGenerator/) )
-  Fichier de configuration de XML prise en charge (par exemple)  [System.Data.Common.DbProviderConfigurationHandler](https://developer.xamarin.com/api/type/System.Data.Common.DbProviderConfigurationHandler/) )
-   [System.Data.Common.DbProviderFactories](https://developer.xamarin.com/api/type/System.Data.Common.DbProviderFactories/) (dépend de la prise en charge des fichiers de configuration XML)
-   [System.Data.OleDb](https://developer.xamarin.com/api/namespace/System.Data.OleDb/)
-   [System.Data.Odbc](https://developer.xamarin.com/api/namespace/System.Data.Odbc/)
-  Le `System.EnterpriseServices.dll` dépendance a été *supprimé* de `System.Data.dll` , se traduisant par la suppression de la [SqlConnection.EnlistDistributedTransaction(ITransaction)](https://developer.xamarin.com/api/member/System.Data.SqlClient.SqlConnection.EnlistDistributedTransaction/(System.EnterpriseServices.ITransaction)) (méthode).


<a name="Mono.Data.Sqlite" />

### <a name="monodatasqlite"></a>Mono.Data.Sqlite

Pendant ce temps, **Mono.Data.Sqlite.dll** ont subi aucune modification de code source, mais au lieu de cela peut être l’hôte d’un certain nombre de *runtime* émet depuis `Mono.Data.Sqlite.dll` lie SQLite 3.5. iOS 8, fourni pendant ce temps, avec SQLite 3.8.5. Suffit dire que, certaines choses ont changé entre les deux versions.

Une version plus ancienne d’e/s sont fournis avec les versions suivantes de SQLite :

- **iOS 7** -version 3.7.13.
- **iOS 6** -version 3.7.13.
- **iOS 5** -version 3.7.7.
- **e/s 4** -version 3.6.22.

Détermination d’apparaissent des problèmes les plus courants liés à l’interrogation de schéma de base de données, par exemple, lors de l’exécution qui existent de colonnes sur une table donnée, tel que `Mono.Data.Sqlite.SqliteConnection.GetSchema` (substitution [DbConnection.GetSchema](https://developer.xamarin.com/api/member/System.Data.Common.DbConnection.GetSchema/)) et `Mono.Data.Sqlite.SqliteDataReader.GetSchemaTable` () substitution de [DbDataReader.GetSchemaTable](https://developer.xamarin.com/api/member/System.Data.Common.DbDataReader.GetSchemaTable/)). En bref, il semble que n’est pas défini à l’aide [DataTable](https://developer.xamarin.com/api/type/System.Data.DataTable/) est peu probable fonctionner.

<a name="Data_Binding" />

## <a name="data-binding"></a>Liaison de données

Liaison de données n’est pas prise en charge pour l’instant.

