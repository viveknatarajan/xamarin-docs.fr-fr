---
title: "À l’aide des données dans une application"
ms.topic: article
ms.prod: xamarin
ms.assetid: D5932AEB-0B6E-4F37-8B32-9BE4775AEE85
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/11/2016
ms.openlocfilehash: b9892ce129beaf168d8a091ff9b894db8bf66cf5
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="using-data-in-an-app"></a>À l’aide des données dans une application

Le **DataAccess_Adv** montre une application opérationnelle qui autorise les entrées d’utilisateur et *CRUD* les fonctionnalités de base de données (création, lecture, mise à jour et suppression). L’application se compose de deux écrans : une liste et un formulaire d’entrée de données. Tout le code d’accès aux données est réutilisable dans iOS et Android sans modification.

Après avoir ajouté des données, les écrans de l’application ressembler à ceci sur iOS :

 ![](using-data-in-an-app-images/image9.png "liste d’exemples iOS")

 ![](using-data-in-an-app-images/image10.png "détails d’échantillon iOS")

IOS projet est illustré ci-dessous, le code présenté dans cette section est contenu dans le **Orm** active :

 ![](using-data-in-an-app-images/image13.png "arborescence du projet iOS")

Le code de l’interface utilisateur natif pour les ViewControllers dans iOS est hors de portée de ce document.
Reportez-vous à la [iOS utilisation de Tables et des cellules](~/ios/user-interface/controls/tables/index.md) guide pour plus d’informations sur les contrôles d’interface utilisateur.

## <a name="read"></a>Lecture

Il existe quelques opérations de lecture dans l’exemple :

-  Lecture de la liste
-  Lors de la lecture des enregistrements individuels


Les deux méthodes dans la `StockDatabase` classe sont :

```csharp
public IEnumerable<Stock> GetStocks ()
{
    lock (locker) {
        return (from i in Table<Stock> () select i).ToList ();
    }
}
public Stock GetStock (int id)
{
    lock (locker) {
        return Table<Stock>().FirstOrDefault(x => x.Id == id);
    }
}
```

iOS restitue les données différemment, comme un `UITableView`.

## <a name="create-and-update"></a>Créer et mettre à jour

Pour simplifier le code d’application, une seule méthode de sauvegarde est fournie qui effectue une insertion ou mise à jour selon que la clé primaire a été défini. Étant donné que la `Id` propriété est marquée avec un `[PrimaryKey]` attribut, vous devez lui affecter pas dans votre code.
Cette méthode détecte si la valeur a été précédente enregistrée (par la vérification de la propriété de clé primaire) et insérer ou mettre à jour en conséquence de l’objet :

```csharp
public int SaveStock (Stock item)
{
    lock (locker) {
        if (item.Id != 0) {
            Update (item);
            return item.Id;
    } else {
            return Insert (item);
        }
    }
}
```



Applications réelles nécessitent généralement des validations (telles que les champs obligatoires, les longueurs minimales ou les autres règles d’entreprise).
Bonne des applications multiplateformes implémentent autant de la validation logique que possible dans le code partagé, en passant des erreurs de validation de sauvegarder sur l’interface utilisateur pour l’affichage en fonction des capacités de la plate-forme.

## <a name="delete"></a>Supprimer

Contrairement à la `Insert` et `Update` méthodes, le `Delete<T>` méthode peut accepter uniquement la valeur de clé primaire plutôt que complète `Stock` objet.
Dans cet exemple un `Stock` objet est passé à la méthode, mais seule la propriété Id est passée à la `Delete<T>` (méthode).

```csharp
public int DeleteStock(Stock stock)
{
    lock (locker) {
        return Delete<Stock> (stock.Id);
    }
}
```

## <a name="using-a-pre-populated-sqlite-database-file"></a>À l’aide d’un fichier de base de données SQLite prédéfinie

Certaines applications sont livrées avec une base de données déjà remplie de données.
Vous pouvez le faire facilement dans votre application mobile par l’envoi d’un fichier de base de données SQLite existant avec votre application et en le copiant dans un répertoire accessible en écriture avant d’y accéder. Étant donné que SQLite est un format de fichier standard qui est utilisé sur de nombreuses plateformes, il existe plusieurs outils permettant de créer un fichier de base de données SQLite :

-  **Gestionnaire de SQLite Firefox Extension** – fonctionne sur Mac et Windows et génère des fichiers qui sont compatibles avec iOS et Android.
-  **Ligne de commande** – consultez [www.sqlite.org/sqlite.html](http://www.sqlite.org/sqlite.html) .


Lorsque vous créez un fichier de base de données de distribution avec votre application, soyez prudent avec la dénomination des tables et des colonnes pour s’assurer qu’ils correspondent à ce qu’attend votre code, surtout si vous utilisez SQLite.NET qui attend les noms pour correspondre à vos classes c# et les propriétés (ou attributs personnalisés associés).

Pour iOS, incluez le fichier sqlite dans votre application et vérifiez qu’il est marqué avec **Action de génération : contenu**. Placez le code dans le `FinishedLaunching` pour copier le fichier dans un répertoire accessible en écriture *avant* vous appelez les méthodes de données. Le code suivant copie une base de données appelée **data.sqlite**, uniquement si elle n’existe pas déjà.

```csharp
// Copy the database across (if it doesn't exist)
var appdir = NSBundle.MainBundle.ResourcePath;
var seedFile = Path.Combine (appdir, "data.sqlite");
if (!File.Exists (Database.DatabaseFilePath))
{
  File.Copy (seedFile, Database.DatabaseFilePath);
}
```

N’importe quel code d’accès aux données (Si ADO.NET ou à l’aide de SQLite.NET) qui s’exécute une fois que cela a seront terminé ont accès aux données déjà remplis.


## <a name="related-links"></a>Liens associés

- [DataAccess Basic (exemple)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess avancé (exemple)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS recettes de données](https://developer.xamarin.com/recipes/ios/data/sqlite/)
- [Accès aux données de Xamarin.Forms](~/xamarin-forms/app-fundamentals/databases.md)
