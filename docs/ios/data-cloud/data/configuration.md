---
title: Configuration
ms.prod: xamarin
ms.assetid: E5582F4B-AD74-420F-9E6D-B07CFB420B3A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/11/2016
ms.openlocfilehash: f4a1bc65fbba68b59196702978633eaf46bc44c5
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="configuration"></a>Configuration

Pour utiliser SQLite dans votre application Xamarin.iOS, vous devez déterminer l’emplacement de fichier approprié pour votre fichier de base de données.

## <a name="database-file-path"></a>Chemin d’accès du fichier de base de données

Quelle que soit la méthode accès aux données utilisée, vous devez créer un fichier de base de données avant que les données peuvent être stockées avec SQLite. En fonction de la plate-forme cible, l’emplacement du fichier seront différents. Pour iOS, vous pouvez utiliser classe Environment pour construire un chemin d’accès valide, comme indiqué dans l’extrait de code suivant :

```csharp
string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "database.db3");
// dbPath contains a valid file path for the database file to be stored
```

Autres éléments sont à prendre en considération lorsque vous décidez où stocker le fichier de base de données. Sur iOS, vous pouvez choisir la base de données sauvegardées sont automatiquement (ou pas).

Si vous souhaitez utiliser un emplacement différent sur chaque plateforme dans votre application multiplateforme vous pouvez utiliser une directive de compilateur comme indiqué pour générer un chemin d’accès différent pour chaque plateforme :

```csharp
var sqliteFilename = "MyDatabase.db3";
#if __ANDROID__
// Just use whatever directory SpecialFolder.Personal returns
string libraryPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal); ;
#else
// we need to put in /Library/ on iOS5.1+ to meet Apple's iCloud terms
// (they don't want non-user-generated data in Documents)
string documentsPath = Environment.GetFolderPath (Environment.SpecialFolder.Personal); // Documents folder
string libraryPath = Path.Combine (documentsPath, "..", "Library"); // Library folder instead
#endif
var path = Path.Combine (libraryPath, sqliteFilename);
```

Reportez-vous à la [fonctionne avec le système de fichiers](~/ios/app-fundamentals/file-system.md) article pour plus d’informations sur les emplacements de fichier à utiliser sur iOS. Consultez le [génération d’Applications Cross-Platform](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) document pour plus d’informations sur l’utilisation de directives de compilateur pour écrire du code spécifique à chaque plateforme.

## <a name="threading"></a>Thread

Vous ne devez pas utiliser la même connexion de base de données SQLite entre plusieurs threads. Veillez à ouvrir et utiliser, puis fermez toutes les connexions que vous créez sur le même thread.

Pour vous assurer que votre code n’essaie pas à accéder à la base de données SQLite à partir de plusieurs threads en même temps, mettre manuellement un verrou chaque fois que vous vous apprêtez à accéder à la base de données, comme suit :

```csharp
object locker = new object(); // class level private field
// rest of class code
lock (locker){
    // Do your query or insert here
}
```

Tous les accès de base de données (lectures, écritures, les mises à jour, etc.) doivent être encapsulées avec le même verrou. Doit veiller à éviter une situation de blocage en vérifiant que le travail à l’intérieur de la clause de verrou est conservé simple et n’appelle pas à d’autres méthodes qui peuvent également prendre un verrou !


## <a name="related-links"></a>Liens associés

- [DataAccess Basic (exemple)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess avancé (exemple)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS recettes de données](https://developer.xamarin.com/recipes/ios/data/sqlite/)
- [Accès aux données de Xamarin.Forms](~/xamarin-forms/app-fundamentals/databases.md)
