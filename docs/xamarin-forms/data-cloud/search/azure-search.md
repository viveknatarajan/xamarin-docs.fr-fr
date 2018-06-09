---
title: Rechercher les données avec Azure Search
description: Cet article explique comment utiliser la bibliothèque de recherche de Microsoft Azure d’intégration d’Azure Search à une application de Xamarin.Forms.
ms.prod: xamarin
ms.assetid: A4AEF233-3672-4174-9DBA-15BEE3030C0B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/05/2016
ms.openlocfilehash: bb1ebec25d747f1188f39e9c9032145bcdc3cb97
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35242126"
---
# <a name="searching-data-with-azure-search"></a>Rechercher les données avec Azure Search

_Azure Search est un service cloud qui fournit l’indexation et l’interrogation des fonctionnalités pour les données téléchargées. Cette opération supprime les exigences de l’infrastructure et la recherche algorithme complexité traditionnellement dans l’implémentation de la fonctionnalité de recherche dans une application. Cet article explique comment utiliser la bibliothèque de recherche de Microsoft Azure d’intégration d’Azure Search à une application de Xamarin.Forms._

## <a name="overview"></a>Vue d'ensemble

En tant qu’index et documents, les données sont stockées dans Azure Search. Un *index* est un magasin de données qui peuvent être recherchés par le service Azure Search et est conceptuellement semblable à une table de base de données. A *document* est une unité de données de recherche dans un index unique et est conceptuellement semblable à une ligne de base de données. Si téléchargement de documents et soumettre des requêtes de recherche d’Azure Search, les demandes sont effectuées à un index spécifique dans le service de recherche.

Chaque demande adressée à Azure Search doit inclure le nom du service et une clé d’API. Il existe deux types de clé d’API :

- *Clés d’administration* accorder des droits d’accès complets à toutes les opérations. Cela inclut la gestion du service, la création et la suppression des index et des sources de données.
- *Clés de requête* accorder l’accès en lecture seule aux index et documents et doit être utilisée par les applications qui génèrent des requêtes de recherche.

Les requêtes les plus courantes pour Azure Search doit exécuter une requête. Il existe deux types de requête qui peut être envoyé :

- A *recherche* requête recherche pour un ou plusieurs éléments dans tous les champs de recherche dans un index. Les requêtes de recherche sont générées à l’aide de la syntaxe simplifiée, ou la syntaxe de requête Lucene. Pour plus d’informations, consultez [syntaxe de requête Simple dans Azure Search](/rest/api/searchservice/Simple-query-syntax-in-Azure-Search/), et [syntaxe de requête Lucene dans Azure Search](/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search/).
- A *filtre* requête évalue une expression booléenne dans tous les champs filtrables dans un index. Filtrer les requêtes sont créées à l’aide d’un sous-ensemble du langage de filtre OData. Pour plus d’informations, consultez [syntaxe d’Expression OData pour Azure Search](/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search/).

Les requêtes de recherche et de filtrer les requêtes peuvent être utilisés ensemble ou séparément. Utilisés conjointement, la requête de filtre est appliquée en premier pour l’intégralité de l’index, et ensuite la requête de recherche est effectuée sur les résultats de la requête de filtre.

Azure Search prend également en charge la récupération des suggestions en fonction de l’entrée de recherche. Pour plus d’informations, consultez [Suggestion requêtes](#suggestions).

## <a name="setup"></a>Installation

Le processus d’intégration d’Azure Search dans une application de Xamarin.Forms est la suivante :

1. Créer un service Azure Search. Pour plus d’informations, consultez [créer un service Azure Search à l’aide du portail Azure](/azure/search/search-create-service-portal/).
1. Supprimez Silverlight en tant que cible de .NET framework à partir de la solution Xamarin.Forms bibliothèque de classes Portable (PCL). Cela peut être accompli en modifiant le profil de bibliothèque PCL à n’importe quel profil qui prend en charge le développement multiplateforme, mais ne prend pas en charge Silverlight, telles que le profil 151 ou 92.
1. Ajouter le [bibliothèque de recherche de Microsoft Azure](https://www.nuget.org/packages/Microsoft.Azure.Search) package NuGet pour le projet de bibliothèque de classes portables dans la solution Xamarin.Forms.

Après avoir effectué ces étapes, l’API de bibliothèque Microsoft Search peut être utilisé pour gérer les sources de données et les index de recherche, télécharger et gérer des documents et exécuter des requêtes.

## <a name="creating-the-azure-search-index"></a>Création de l’Index Azure Search

Un schéma d’index doit être défini qui mappe à la structure des données à rechercher. Cela peut être effectué dans le portail Azure, ou par programme à l’aide la `SearchServiceClient` classe. Cette classe gère les connexions à Azure Search et peut être utilisée pour créer un index. L’exemple de code suivant montre comment créer une instance de cette classe :

```csharp
var searchClient =
  new SearchServiceClient(Constants.SearchServiceName, new SearchCredentials(Constants.AdminApiKey));
```

Le `SearchServiceClient` surcharge de constructeur prend un nom de service de recherche et un `SearchCredentials` en tant qu’arguments, de l’objet avec la `SearchCredentials` habillage de l’objet le *clé d’administration* pour le service Azure Search. Le *clé d’administration* est nécessaire pour créer un index.

> [!NOTE]
>  Un seul `SearchServiceClient` instance doit être utilisée dans une application afin d’éviter l’ouverture de connexions trop d’Azure Search.

Un index est défini par le `Index` de l’objet, comme illustré dans l’exemple de code suivant :

```csharp
static void CreateSearchIndex()
{
  var index = new Index()
  {
    Name = Constants.Index,
    Fields = new[]
    {
      new Field("id", DataType.String) { IsKey = true, IsRetrievable = true },
      new Field("name", DataType.String) { IsRetrievable = true, IsFilterable = true, IsSortable = true, IsSearchable = true },
      new Field("location", DataType.String) { IsRetrievable = true, IsFilterable = true, IsSortable = true, IsSearchable = true },
      new Field("details", DataType.String) { IsRetrievable = true, IsFilterable = true, IsSearchable = true },
      new Field("imageUrl", DataType.String) { IsRetrievable = true }
    },
    Suggesters = new[]
    {
      new Suggester("nameSuggester", SuggesterSearchMode.AnalyzingInfixMatching, new[] { "name" })
    }
  };

  searchClient.Indexes.Create(index);
}
```

Le `Index.Name` propriété doit être définie sur le nom de l’index et le `Index.Fields` propriété doit être définie dans un tableau de `Field` objets. Chaque `Field` instance spécifie un nom, un type et toutes les propriétés qui spécifient la façon dont le champ est utilisé. Ces propriétés sont les suivantes :

- `IsKey` : indique si le champ est la clé de l’index. Qu’un seul champ dans l’index, de type `DataType.String`, doit être désigné en tant que le champ de clé.
- `IsFacetable` : indique s’il est possible d’effectuer une navigation par facettes sur ce champ. La valeur par défaut est `false`.
- `IsFilterable` : indique si le champ peut être utilisé dans les requêtes de filtre. La valeur par défaut est `false`.
- `IsRetrievable` : indique si le champ peut être récupéré dans les résultats de la recherche. La valeur par défaut est `true`.
- `IsSearchable` : indique si le champ est inclus dans les recherches en texte intégral. La valeur par défaut est `false`.
- `IsSortable` : indique si le champ peut être utilisé dans `OrderBy` expressions. La valeur par défaut est `false`.

> [!NOTE]
> La modification d’un index après son déploiement implique la reconstruction et de rechargement des données.

Un `Index` objet peut éventuellement spécifier un `Suggesters` propriété, qui définit les champs dans l’index à utiliser pour prendre en charge la saisie semi-automatique ou suggestion requêtes de recherche. Le `Suggesters` propriété doit être définie dans un tableau de `Suggester` objets qui définissent les champs qui sont utilisés pour générer des résultats des suggestions de la recherche.

Après avoir créé le `Index` de l’objet, l’index est créé en appelant `Indexes.Create` sur la `SearchServiceClient` instance.

> [!NOTE]
> Lors de la création d’un index à partir d’une application qui doit être conservé réactive, utilisez le `Indexes.CreateAsync` (méthode).

Pour plus d’informations, consultez [créer un index Azure Search à l’aide du SDK .NET](/azure/search/search-create-index-dotnet/).

## <a name="deleting-the-azure-search-index"></a>Suppression de l’Index de recherche Azure

Un index peut être supprimé en appelant `Indexes.Delete` sur la `SearchServiceClient` instance :

```csharp
searchClient.Indexes.Delete(Constants.Index);
```

## <a name="uploading-data-to-the-azure-search-index"></a>Transfert de données à l’Index de recherche Azure

Après avoir défini l’index, les données peuvent être téléchargées à l’aide d’un des deux modèles :

- **Modèle d’extraction** : données sont intégrées régulièrement à partir de la base de données Azure Cosmos, base de données SQL Azure, stockage d’objets Blob Azure ou SQL Server hébergé dans une Machine virtuelle Azure.
- **Modèle push** – données sont envoyées par programme à l’index. Il s’agit du modèle adopté dans cet article.

A `SearchIndexClient` instance doit être créée pour importer des données dans l’index. Cela peut être accompli en appelant le `SearchServiceClient.Indexes.GetClient` méthode, comme illustré dans l’exemple de code suivant :

```csharp
static void UploadDataToSearchIndex()
{
  var indexClient = searchClient.Indexes.GetClient(Constants.Index);

  var monkeyList = MonkeyData.Monkeys.Select(m => new
  {
    id = Guid.NewGuid().ToString(),
    name = m.Name,
    location = m.Location,
    details = m.Details,
    imageUrl = m.ImageUrl
  });

  var batch = IndexBatch.New(monkeyList.Select(IndexAction.Upload));
  try
  {
    indexClient.Documents.Index(batch);
  }
  catch (IndexBatchException ex)
  {
    // Sometimes when the Search service is under load, indexing will fail for some
    // documents in the batch. Compensating actions like delaying and retrying should be taken.
    // Here, the failed document keys are logged.
    Console.WriteLine("Failed to index some documents: {0}",
      string.Join(", ", ex.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
  }
}
```

Données à importer dans l’index sont empaquetées comme un `IndexBatch` objet qui encapsule une collection de `IndexAction` objets. Chaque `IndexAction` instance contient un document et une propriété qui indique à Azure Search à l’action à effectuer sur le document. Dans l’exemple de code ci-dessus, le `IndexAction.Upload` action est spécifiée, les résultats dans le document est inséré dans l’index s’il est nouveau, ou remplacée s’il existe déjà. Le `IndexBatch` objet est ensuite envoyé à l’index en appelant le `Documents.Index` méthode sur le `SearchIndexClient` objet. Pour plus d’informations sur les autres actions d’indexation, consultez [décidez de l’action d’indexation à utiliser](/azure/search/search-import-data-dotnet#ii-decide-which-indexing-action-to-use).

> [!NOTE]
> Uniquement 1 000 documents peuvent être inclus dans une même demande d’indexation.

Notez que dans l’exemple de code ci-dessus, le `monkeyList` collection est créée en tant qu’objet anonyme à partir d’une collection de `Monkey` objets. Cela crée les données pour le `id` champ et résout le mappage de casse Pascal `Monkey` en casse mixte, les noms de propriété Rechercher les noms de champs d’index. Ou bien, ce mappage également possible en ajoutant le `[SerializePropertyNamesAsCamelCase]` attribut le `Monkey` classe.

Pour plus d’informations, consultez [télécharger des données vers Azure Search à l’aide du SDK .NET](/azure/search/search-import-data-dotnet/).

## <a name="querying-the-azure-search-index"></a>Interrogation de l’Index de recherche Azure

A `SearchIndexClient` instance doit être créée pour un index de requête. Lorsqu’une application exécute des requêtes, il est recommandé de suivre le principe du privilège minimum et créer un `SearchIndexClient` directement, en passant le *clé de requête* en tant qu’argument. Ceci garantit que les utilisateurs ont accès en lecture seule aux index et aux documents. Cette approche est illustrée dans l’exemple de code suivant :

```csharp
SearchIndexClient indexClient =
  new SearchIndexClient(Constants.SearchServiceName, Constants.Index, new SearchCredentials(Constants.QueryApiKey));
```

Le `SearchIndexClient` surcharge de constructeur prend un nom de service de recherche, le nom de l’index et un `SearchCredentials` en tant qu’arguments, de l’objet avec la `SearchCredentials` habillage de l’objet le *clé de requête* pour le service Azure Search.

### <a name="search-queries"></a>Requêtes de recherche

L’index peut être interrogé en appelant le `Documents.SearchAsync` méthode sur le `SearchIndexClient` de l’instance, comme illustré dans l’exemple de code suivant :

```csharp
async Task AzureSearch(string text)
{
  Monkeys.Clear();

  var searchResults = await indexClient.Documents.SearchAsync<Monkey>(text);
  foreach (SearchResult<Monkey> result in searchResults.Results)
  {
    Monkeys.Add(new Monkey
    {
      Name = result.Document.Name,
      Location = result.Document.Location,
      Details = result.Document.Details,
      ImageUrl = result.Document.ImageUrl
    });
  }
}
```

Le `SearchAsync` méthode prend un argument de texte de recherche et éventuellement un `SearchParameters` objet qui peut être utilisé pour affiner la requête. Une requête de recherche est spécifiée comme argument de texte de recherche, tandis que la requête de filtre peut être spécifiée en définissant le `Filter` propriété de la `SearchParameters` argument. L’exemple de code suivant illustre que les deux types de requête :

```csharp
var parameters = new SearchParameters
{
  Filter = "location ne 'China' and location ne 'Vietnam'"
};
var searchResults = await indexClient.Documents.SearchAsync<Monkey>(text, parameters);
```

Cette requête de filtre est appliquée à la totalité de l’index et supprime les documents à partir des résultats où les `location` champ n’est pas égale à la Chine et non égal à Vietnam. Après le filtrage, la requête de recherche est effectuée sur les résultats de la requête de filtre.

> [!NOTE]
> Pour filtrer sans rechercher, passer `*` en tant que l’argument de texte de recherche.

Le `SearchAsync` méthode retourne un `DocumentSearchResult` objet qui contient les résultats de requête. Cet objet est énuméré, avec chaque `Document` de l’objet en cours de création en tant qu’un `Monkey` de l’objet et ajouté à la `Monkeys` `ObservableCollection` pour l’affichage. Les captures d’écran Afficher requête résultats suivants retournés à partir d’Azure Search :

![](azure-search-images/search.png "Résultats de la recherche")

Pour plus d’informations sur la recherche et le filtrage, consultez [interroger votre index Azure Search à l’aide du SDK .NET](/azure/search/search-query-dotnet/).

<a name="suggestions" />

### <a name="suggestion-queries"></a>Requêtes de suggestion

Azure Search permet des suggestions à être demandés basé sur une requête de recherche, en appelant le `Documents.SuggestAsync` méthode sur le `SearchIndexClient` instance. Cela est illustré dans l’exemple de code suivant :

```csharp
async Task AzureSuggestions(string text)
{
  Suggestions.Clear();

  var parameters = new SuggestParameters()
  {
    UseFuzzyMatching = true,
    HighlightPreTag = "[",
    HighlightPostTag = "]",
    MinimumCoverage = 100,
    Top = 10
  };

  var suggestionResults =
    await indexClient.Documents.SuggestAsync<Monkey>(text, "nameSuggester", parameters);

  foreach (var result in suggestionResults.Results)
  {
    Suggestions.Add(new Monkey
    {
      Name = result.Text,
      Location = result.Document.Location,
      Details = result.Document.Details,
      ImageUrl = result.Document.ImageUrl
    });
  }
}
```

Le `SuggestAsync` méthode prend un argument de texte de recherche, le nom du Générateur de suggestion à utiliser (qui est défini dans l’index), et éventuellement un `SuggestParameters` objet qui peut être utilisé pour affiner la requête. Le `SuggestParameters` instance définit les propriétés suivantes :

- `UseFuzzyMatching` – Lorsque la valeur `true`, Azure Search trouve des suggestions même s’il existe un caractère erroné ou manquant dans le texte de recherche.
- `HighlightPreTag` – la balise qui est ajoutée devant les correspondances de suggestion.
- `HighlightPostTag` – la balise qui est ajoutée à l’accès de suggestion.
- `MinimumCoverage` : représente le pourcentage de l’index doit être couvert par une requête de suggestion de la requête pour être signalé un succès. La valeur par défaut est 80.
- `Top` – le nombre de suggestions à récupérer. Il doit être un entier compris entre 1 et 100, avec 5 comme valeur par défaut.

L’effet global est que les 10 premiers résultats à partir de l’index seront retournés avec la mise en surbrillance d’accès et les résultats doivent inclure les documents qui contiennent la même orthographié termes de recherche.

Le `SuggestAsync` méthode retourne un `DocumentSuggestResult` objet qui contient les résultats de requête. Cet objet est énuméré, avec chaque `Document` de l’objet en cours de création en tant qu’un `Monkey` de l’objet et ajouté à la `Monkeys` `ObservableCollection` pour l’affichage. Les captures d’écran suivantes illustrent les résultats de suggestion retournés à partir d’Azure Search :

![](azure-search-images/suggest.png "Résultats de la suggestion")

Notez que dans l’exemple d’application, le `SuggestAsync` méthode est appelée uniquement lorsque l’utilisateur termine la saisie d’un terme à rechercher. Toutefois, il peut également être utilisé pour prendre en charge les requêtes de recherche de saisie semi-automatique par l’exécution de chaque keypress.

## <a name="summary"></a>Récapitulatif

Cet article vous a montré comment utiliser la bibliothèque de recherche de Microsoft Azure d’intégration d’Azure Search à une application de Xamarin.Forms. Azure Search est un service cloud qui fournit l’indexation et l’interrogation des fonctionnalités pour les données téléchargées. Cette opération supprime les exigences de l’infrastructure et la recherche algorithme complexité traditionnellement dans l’implémentation de la fonctionnalité de recherche dans une application.


## <a name="related-links"></a>Liens associés

- [Azure Search (exemple)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/AzureSearch/)
- [Documentation Azure Search](/azure/search/)
- [Bibliothèque de recherche de Microsoft Azure](https://www.nuget.org/packages/Microsoft.Azure.Search/)
