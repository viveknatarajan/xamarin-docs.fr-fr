---
title: "Vue d’ensemble d’async"
description: "La dernière version du langage c# – version 5 – introduit deux nouveaux mots-clés pour exprimer des opérations asynchrones : async et await. Ces mots clés vous permettent d’écrire du code simple qui utilise la bibliothèque parallèle de tâches pour exécuter des opérations longues (par exemple, l’accès réseau) dans un autre thread et d’accéder facilement les résultats à la fin. Les dernières versions de Xamarin.iOS et Xamarin.Android prennent en charge async et await : ce document fournit des explications et un exemple d’utilisation de la nouvelle syntaxe avec Xamarin."
ms.topic: article
ms.prod: xamarin
ms.assetid: F87BF587-AB64-4C60-84B1-184CAE36ED65
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: 4762612075a866581339b198552b8e26c9a1f6fa
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="async-support-overview"></a>Présentation de la prise en charge asynchrone

_La dernière version du langage c# – version 5 – introduit deux nouveaux mots-clés pour exprimer des opérations asynchrones : async et await. Ces mots clés vous permettent d’écrire du code simple qui utilise la bibliothèque parallèle de tâches pour exécuter des opérations longues (par exemple, l’accès réseau) dans un autre thread et d’accéder facilement les résultats à la fin. Les dernières versions de Xamarin.iOS et Xamarin.Android prennent en charge async et await : ce document fournit des explications et un exemple d’utilisation de la nouvelle syntaxe avec Xamarin._

Prise en charge de Xamarin Async repose sur la Fondation Mono 3.0 et le profil de l’API de la mise à niveau à partir d’une version adaptés aux appareils mobiles de Silverlight pour être une version adaptés aux appareils mobiles de .NET 4.5 en cours.

## <a name="overview"></a>Vue d'ensemble

Ce document présente la nouvelle async et await mots clés, présente des exemples simples implémentant les méthodes asynchrones dans Xamarin.iOS et Xamarin.Android.

Pour une description plus complète des nouvelles fonctionnalités asynchrones de c# 5 (y compris un grand nombre d’échantillons et les différents scénarios d’utilisation), reportez-vous à la documentation MSDN [programmation asynchrone avec Async et Await](http://msdn.microsoft.com/en-us/library/vstudio/hh191443.aspx).

L’exemple d’application effectue une requête web asynchrone simple (sans bloquer le thread principal), puis met à jour l’interface utilisateur avec le html téléchargé et le nombre de caractères.

 [![](async-images/AsyncAwait_427x368.png "L’exemple d’application effectue une requête web asynchrone simple sans bloquer le thread principal, puis met à jour l’interface utilisateur avec le html téléchargé et le nombre de caractères")](async-images/AsyncAwait.png#lightbox)

Prise en charge de Xamarin async repose sur la Fondation Mono 3.0 et le profil de l’API de la mise à niveau à partir d’une version adaptés aux appareils mobiles de Silverlight pour être une version adaptés aux appareils mobiles de .NET 4.5 en cours.

## <a name="requirements"></a>Configuration requise

Les fonctionnalités de c# 5 requièrent 3.0 Mono qui est inclus dans 6.4 de Xamarin.iOS et Xamarin.Android 4.8. Vous devrez mettre à niveau votre Mono, Xamarin.iOS, Xamarin.Android et Xamarin.Mac pour tirer parti de celui-ci.

## <a name="using-async-amp-await"></a>L’utilisation d’async &amp; await

 `async` et `await` sont des nouvelles fonctionnalités du langage c# qui fonctionnent conjointement avec la bibliothèque parallèle de tâches pour le rendre facile d’écrire du code multithread pour effectuer des tâches longues sans bloquer le thread principal de votre application.

## <a name="async"></a>async

### <a name="declaration"></a>Déclaration

Le `async` (mot clé) est placé dans une déclaration de méthode (ou sur une expression lambda ou une méthode anonyme) pour indiquer qu’il contient du code qui peut s’exécuter de façon asynchrone, ie. ne bloque pas le thread de l’appelant.

Une méthode marquée avec `async` doit contenir au moins une expression ou une instruction d’attente. Si aucun `await`s sont présents dans la méthode, puis elle s’exécute de façon synchrone (le même comme s’il existait aucune `async` modificateur). Cela entraîne également un avertissement du compilateur (mais pas une erreur).

### <a name="return-types"></a>Types de retours

Une méthode asynchrone doit retourner un `Task`, `Task<TResult>` ou `void`.

Spécifiez le `Task` type de retour si la méthode ne retourne pas de toute autre valeur.

Spécifier `Task<TResult>` si la méthode doit retourner une valeur, où `TResult` est le type qui est retourné (par exemple un `int`, par exemple).

Le `void` type de retour est utilisé principalement pour les gestionnaires d’événements qui ont besoin. Le code qui appelle les méthodes asynchrones retournant void ne peut pas `await` sur le résultat.

### <a name="parameters"></a>Paramètres

Impossible de déclarer les méthodes asynchrones `ref` ou `out` paramètres.

## <a name="await"></a>await

L’opérateur await peut être appliqué à une tâche à l’intérieur d’une méthode marquée comme asynchrone. Il provoque la méthode pour arrêter l’exécution à ce stade et attendez que la tâche se termine.

À l’aide d’await ne bloque pas le thread de l’appelant – plutôt le contrôle est retourné à l’appelant. Cela signifie que le thread appelant n’est pas bloqué, donc par exemple, l’utilisateur thread d’interface n’est pas bloquée lors de l’attente d’une tâche.

Lorsque la tâche se termine, la méthode reprend l’exécution sur le même point dans le code. Cela inclut le retour à la portée de réessayer d’un bloc try-catch-finally (le cas échéant). await ne peut pas être utilisé dans un bloc catch ou le bloc finally.

En savoir plus sur [await sur MSDN](http://msdn.microsoft.com/en-us/library/vstudio/hh156528.aspx).

## <a name="exception-handling"></a>Gestion des exceptions

Les exceptions qui se produisent à l’intérieur d’une méthode async sont stockées dans la tâche et levées quand la tâche est `await`ed. Ces exceptions peuvent être interceptées et gérées à l’intérieur d’un bloc try-catch.

## <a name="cancellation"></a>Annulation

Les méthodes asynchrones qui prennent beaucoup de temps doivent prendre en charge l’annulation. En règle générale, l’annulation est appelée comme suit :

- A `CancellationTokenSource` objet est créé.
- Le `CancellationTokenSource.Token` instance est passée à une méthode asynchrone pouvant être annulée.
- L’annulation est demandée en appelant le `CancellationTokenSource.Cancel` (méthode).

Ensuite, la tâche annule et accuse réception de l’annulation.

Pour plus d’informations sur l’annulation, consultez [comment annuler une tâche asynchrone](http://msdn.microsoft.com/en-us/library/vstudio/jj155761.aspx) sur MSDN.

## <a name="example"></a>Exemple

Téléchargez le [exemple solution Xamarin](https://developer.xamarin.com/samples/mobile/AsyncAwait/) (pour iOS et Android) pour voir un exemple d’utilisation de `async` et `await` dans les applications mobiles. L’exemple de code est abordée plus en détail dans cette section.

### <a name="writing-an-async-method"></a>Écriture d’une méthode async

La méthode suivante montre comment le code une `async` méthode avec un `await`les tâches ed :

```csharp
public async Task<int> DownloadHomepage()
{
    var httpClient = new HttpClient(); // Xamarin supports HttpClient!

    Task<string> contentsTask = httpClient.GetStringAsync("http://xamarin.com"); // async method!

    // await! control returns to the caller and the task continues to run on another thread
    string contents = await contentsTask;

    ResultEditText.Text += "DownloadHomepage method continues after async call. . . . .\n";

    // After contentTask completes, you can calculate the length of the string.
    int exampleInt = contents.Length;

    ResultEditText.Text += "Downloaded the html and found out the length.\n\n\n";

    ResultEditText.Text += contents; // just dump the entire HTML

    return exampleInt; // Task<TResult> returns an object of type TResult, in this case int
}
```

Notez ces points :

-  La déclaration de méthode inclut la `async` (mot clé).
-  Le type de retour est `Task<int>` pour le code appelant peut accéder à la `int` valeur calculée dans cette méthode.
-  L’instruction return est `return exampleInt;` qui est un objet entier, le fait que la méthode retourne `Task<int>` fait partie des améliorations de langage.


### <a name="calling-an-async-method-1"></a>Appel d’une méthode async 1

Événements click de ce bouton Gestionnaire se trouve dans l’application Android pour appeler la méthode ci-dessus :

```csharp
GetButton.Click += async (sender, e) => {

    Task<int> sizeTask = DownloadHomepage();

    ResultTextView.Text = "loading...";
    ResultEditText.Text = "loading...\n";

    // await! control returns to the caller
    var intResult = await sizeTask;

    // when the Task<int> returns, the value is available and we can display on the UI
    ResultTextView.Text = "Length: " + intResult ;
    // "returns" void, since it's an event handler
};
```

Remarques :

-  Le délégué anonyme a le préfixe de mot clé async.
-  La méthode asynchrone DownloadHomepage retourne une tâche <int> qui est stocké dans la variable sizeTask.
-  Le code attend dans la variable sizeTask.  *Cela* est l’emplacement que la méthode est interrompue et le contrôle est retourné au code appelant jusqu'à ce que la tâche asynchrone se termine sur son propre thread.
-  L’exécution est *pas* suspendre lorsque la tâche est créée sur la première ligne de la méthode, en dépit de la tâche qui est créée. Le mot clé await indique l’emplacement où l’exécution est suspendue.
-  Lorsque la tâche asynchrone se termine, intResult est défini et l’exécution se poursuit sur le thread d’origine, à partir de la ligne await.


### <a name="calling-an-async-method-2"></a>Appel d’une méthode async 2

Dans l’exemple d’application iOS l’exemple est écrit légèrement différemment pour illustrer une approche alternative. Plutôt que d’utiliser un délégué anonyme cet exemple déclare un `async` Gestionnaire d’événements qui est attribué à un gestionnaire d’événements standard :

```csharp
GetButton.TouchUpInside += HandleTouchUpInside;
```

La méthode de gestionnaire d’événements est alors définie comme indiqué ici :

```csharp
async void HandleTouchUpInside (object sender, EventArgs e)
{
    ResultLabel.Text = "loading...";
    ResultTextView.Text = "loading...\n";

    // await! control returns to the caller
    var intResult = await DownloadHomepage();

    // when the Task<int> returns, the value is available and we can display on the UI
    ResultLabel.Text = "Length: " + intResult ;
}
```

Points importants :

-  La méthode est marquée en tant que `async` , mais retourne `void` . Cela s’effectue généralement uniquement pour les gestionnaires d’événements (dans le cas contraire, vous devez retourner un `Task` ou `Task<TResult>` ).
-  Le code `await` s sur le `DownloadHomepage` (méthode) directement sur une assignation à une variable ( `intResult` ) contrairement à l’exemple précédent où nous avons utilisé un intermédiaire `Task<int>` variable pour faire référence à la tâche.  *Cela* est l’emplacement où contrôle est retourné à l’appelant jusqu'à ce que la méthode asynchrone est terminée sur un autre thread.
-  Lorsque la méthode asynchrone se termine et retourne, l’exécution reprend à le `await` ce qui signifie que le résultat de l’entier est retourné et est rendue dans un widget de l’interface utilisateur.


## <a name="summary"></a>Récapitulatif

L’utilisation d’async et await simplifie considérablement le code requis pour générer dynamiquement des longues opérations sur les threads d’arrière-plan sans bloquer le thread principal. Ils facilitent également accéder aux résultats lorsque la tâche est terminée.

Ce document a fourni une vue d’ensemble de nouveaux mots clés de langage et des exemples de Xamarin.iOS et Xamarin.Android.



## <a name="related-links"></a>Liens associés

- [AsyncAwait (exemple)](https://developer.xamarin.com/samples/mobile/AsyncAwait/)
- [Rappels en tant que notre générations atteindre la déclaration](http://tirania.org/blog/archive/2013/Aug-15.html)
- [Données (iOS) (exemple)](https://developer.xamarin.com/samples/monotouch/Data/)
- [HttpClient (iOS) (exemple)](https://developer.xamarin.com/samples/monotouch/HttpClient/)
- [MapKitSearch (iOS) (sample)](https://github.com/xamarin/monotouch-samples/tree/master/MapKitSearch)
- [Séminaire Web : C# Async sur iOS et Android (vidéo)](http://xamarin.wistia.com/medias/k27mc627xz)
- [Programmation asynchrone avec Async et Await (MSDN)](http://msdn.microsoft.com/en-us/library/vstudio/hh191443.aspx)
- [Ajustement de votre application Async (MSDN)](http://msdn.microsoft.com/en-us/library/vstudio/jj155761.aspx)
- [Await et l’interface utilisateur et les interblocages ! Mon Dieu ! (MSDN)](http://blogs.msdn.com/b/pfxteam/archive/2011/01/13/10115163.aspx)
- [Le traitement des tâches terminées (MSDN)](http://blogs.msdn.com/b/pfxteam/archive/2012/08/02/processing-tasks-as-they-complete.aspx)
- [Modèle asynchrone basé sur les tâches (TAP, Task-based Asynchronous Pattern)](http://msdn.microsoft.com/en-us/library/hh873175.aspx)
- [Comportement asynchrone dans c# 5 (blog de Eric Lippert) : à propos de l’introduction des mots clés](http://blogs.msdn.com/b/ericlippert/archive/2010/11/11/whither-async.aspx)
