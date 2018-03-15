---
title: "Résumé du chapitre 20. E/s asynchrones et fichier"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: D595862D-64FD-4C0D-B0AD-C1F440564247
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 0ac316bc2cef04a80958c047427845dbdcc4137f
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/15/2018
---
# <a name="summary-of-chapter-20-async-and-file-io"></a>Résumé du chapitre 20. E/s asynchrones et fichier

 Une interface graphique utilisateur doit répondre aux événements d’entrée d’utilisateur séquentiellement. Cela implique que tout le traitement des événements d’entrée d’utilisateur doit se produire dans un thread unique, souvent appelé le *thread principal* ou *thread d’interface utilisateur*.

Les utilisateurs attendent des interfaces utilisateur de répondre. Cela signifie qu’un programme doit traiter rapidement les événements d’entrée d’utilisateur. Si ce n’est pas possible, le traitement doit être réservées de premier ordre pour les threads secondaires de l’exécution.

Plusieurs exemples de programmes dans cet ouvrage ont utilisé le [ `WebRequest` ](https://developer.xamarin.com/api/type/System.Net.WebRequest/) classe. Dans cette classe le [ `BeginGetReponse` ](https://developer.xamarin.com/api/member/System.Net.WebRequest.BeginGetResponse/p/System.AsyncCallback/System.Object/) méthode démarre un thread de travail, qui appelle une fonction de rappel lorsqu’il est terminé. Toutefois, cette fonction de rappel s’exécute dans le thread de travail, afin que le programme doit appeler [ `Device.BeginInvokeOnMainThread` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.BeginInvokeOnMainThread/p/System.Action/) méthode pour accéder à l’interface utilisateur.

Une approche plus moderne pour le traitement asynchrone est disponible dans .NET et c#. Cela implique la [ `Task` ](https://developer.xamarin.com/api/type/System.Threading.Tasks.Task/) et [ `Task<TResult>` ](https://developer.xamarin.com/api/type/System.Threading.Tasks.Task%3CTResult%3E/) classes et autres types dans les [ `System.Threading` ](https://developer.xamarin.com/api/namespace/System.Threading/) et [ `System.Threading.Tasks` ](https://developer.xamarin.com/api/namespace/System.Threading.Tasks/) espaces de noms, ainsi que la version 5.0 c# `async` et `await` mots clés. C’est ce que ce chapitre traite.

## <a name="from-callbacks-to-await"></a>À partir des rappels pour attendre

La `Page` classe elle-même contient trois méthodes asynchrones pour afficher les zones de l’alerte :

- [`DisplayAlert`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert/p/System.String/System.String/System.String/) Retourne un `Task` objet
- [`DisplayAlert`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert/p/System.String/System.String/System.String/System.String/) Retourne un `Task<bool>` objet
- [`DisplayActionSheet`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayActionSheet/p/System.String/System.String/System.String/System.String[]/) Retourne un `Task<string>` objet

Le `Task` objets indiquent que ces méthodes implémentent le modèle asynchrone basé sur des tâches, appelé TAP. Ces `Task` objets sont retournés rapidement à partir de la méthode. Le `Task<T>` retournent des valeurs constituent une « promesse » qui une valeur de type `TResult` seront disponibles lorsque la tâche se termine. Le `Task` valeur renvoyée indique une action asynchrone seront complète mais sans valeur retournée.

Dans tous ces cas, le `Task` est terminée quand l’utilisateur ferme le message d’alerte.  

### <a name="an-alert-with-callbacks"></a>Une alerte avec des rappels

Le [ **AlertCallbacks** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertCallbacks) exemple montre comment gérer `Task<bool>` retournent des objets et `Device.BeginInvokeOnMainThread` appels à l’aide des méthodes de rappel.

### <a name="an-alert-with-lambdas"></a>Une alerte avec les expressions lambda

Le [ **AlertLambdas** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertLambdas) exemple illustre l’utilisation des fonctions lambda anonyme pour la gestion des `Task` et `Device.BeginInvokeOnMainThread` appels.  

### <a name="an-alert-with-await"></a>Une alerte avec await

Une approche plus simple implique la `async` et `await` mots clés introduits dans c# 5. Le [ **AlertAwait** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertAwait) exemple illustre son utilisation.

### <a name="an-alert-with-nothing"></a>Une alerte sans rien

Si la méthode asynchrone retourne `Task` plutôt que `Task<TResult>`, puis le programme n’a pas besoin d’utiliser une de ces techniques si elle n’a pas besoin de savoir quand la tâche asynchrone est terminée. Le [ **NothingAlert** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/NothingAlert) illustre cela.

### <a name="saving-program-settings-asynchronously"></a>Enregistrement des paramètres de programme en mode asynchrone

Le [ **SaveProgramChanges** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/SaveProgramSettings) exemple illustre l’utilisation de la [ `SavePropertiesAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.SavePropertiesAsync()/) méthode `Application` pour enregistrer les paramètres du programme lorsqu’elles changent sans substitution de la `OnSleep` (méthode).

### <a name="a-platform-independent-timer"></a>Un minuteur indépendant de la plateforme

Il est possible d’utiliser [ `Task.Delay` ](https://developer.xamarin.com/api/member/System.Threading.Tasks.Task.Delay/p/System.Int32/) pour créer un minuteur indépendant de la plateforme. Le [ **TaskDelayClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TaskDelayClock) illustre cela.

## <a name="file-inputoutput"></a>Fichier d’entrée/sortie

En règle générale, le .NET [ `System.IO` ](https://developer.xamarin.com/api/namespace/System.IO/) espace de noms a été la source de la prise en charge d’e/s de fichier. Bien que certaines méthodes dans cet espace de noms prennent en charge les opérations asynchrones, la plupart ne le font pas. L’espace de noms prend également en charge plusieurs appels de méthode simple qui exécutent des fonctions d’e/s de fichier sophistiquées.

### <a name="good-news-and-bad-news"></a>Une bonne et mauvaise nouvelle

Toutes les plateformes prises en charge par le stockage local d’application prise en charge Xamarin.Forms &mdash; stockage qui est privé pour l’application.

Les bibliothèques Xamarin.iOS et Xamarin.Android comprennent une version de .NET Xamarin a expressément adaptées à ces deux plateformes. Cela comprend les classes à partir de `System.IO` que vous pouvez utiliser pour effectuer d’e/s de fichier avec un stockage local d’application dans ces deux plateformes.

Toutefois, si vous recherchez ces `System.IO` classes dans une bibliothèque PCL Xamarin.Forms, vous ne trouverez pas les. Il s’agit de ce fichier Microsoft complètement repensée d’e/s pour les API Windows Runtime. Programmes qui ciblent Windows 8.1, Windows Phone 8.1 et la plateforme Windows universelle n’utilisent pas `System.IO` d’e/s de fichier.

Cela signifie que vous devez utiliser le [ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) (abordé en [ **chapitre 9. Appels d’API spécifiques à une plateforme** ](chapter09.md) pour implémenter des e/s de fichier.

### <a name="a-first-shot-at-cross-platform-file-io"></a>Une première capture inter-plateformes, fichier d’e/s

Le [ **TextFileTryout** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TextFileTryout) exemple définit un [ `IFileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/TextFileTryout/TextFileTryout/TextFileTryout/IFileHelper.cs) interface pour les implémentations de cette interface dans toutes les plateformes et d’e/s de fichier. Toutefois, les implémentations de Windows Runtime ne fonctionnent avec les méthodes dans cette interface, car les méthodes d’e/s de fichier Windows Runtime sont asynchrones.

### <a name="accommodating-windows-runtime-file-io"></a>Prise en charge d’e/s de fichier Windows Runtime

Les programmes s’exécutant sous Windows Runtime utilisent les classes dans le [ `Windows.Storage` ](https://msdn.microsoft.com/library/windows/apps/windows.storage.aspx) et [ `Windows.Storage.Streams` ](https://msdn.microsoft.com/library/windows/apps/windows.storage.streams.aspx) espaces de noms pour le fichier d’e/s, y compris le stockage local d’application. Étant donné que Microsoft déterminé que toute opération nécessitant que plus de 50 millisecondes doivent être asynchrones pour éviter de bloquer le thread d’interface utilisateur, ces méthodes d’e/s de fichier sont principalement asynchrones.

Le code qui illustre cette nouvelle approche sera dans une bibliothèque afin qu’il peut être utilisé par d’autres applications.

## <a name="platform-specific-libraries"></a>Bibliothèques propres à une plateforme

Il est préférable de stocker le code réutilisable dans des bibliothèques. Il s’agit évidemment plus difficile lorsque différentes parties du code réutilisable sont pour les systèmes d’exploitation totalement différentes.

Le [ **Xamarin.FormsBook.Platform** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform) solution illustre une approche. Cette solution contient des projets différents sept :

- [**Xamarin.FormsBook.Platform**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform), une bibliothèque de classes portables Xamarin.Forms normal
- [**Xamarin.FormsBook.Platform.iOS**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS), une bibliothèque de classes d’e/s
- [**Xamarin.FormsBook.Platform.Android**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android), une bibliothèque de classes Android
- [**Xamarin.FormsBook.Platform.UWP**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.UWP), une bibliothèque de classes Windows universelles
- [**Xamarin.FormsBook.Platform.Windows**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Windows), une bibliothèque PCL pour Windows 8.1.
- [**Xamarin.FormsBook.Platform.WinPhone**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinPhone), une bibliothèque PCL pour Windows Phone 8.1
- [**Xamarin.FormsBook.Platform.WinRT**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT), un projet partagé pour le code qui est commun à toutes les plateformes Windows

Tous les projets de plateforme individuels (à l’exception de **Xamarin.FormsBook.Platform.WinRT**) ont des références à **Xamarin.FormsBook.Platform**. Les trois projets Windows ont une référence à **Xamarin.FormsBook.Platform.WinRT**.

Tous les projets contiennent un statique `Toolkit.Init` pour s’assurer que la bibliothèque est chargée s’il n’est pas directement référencé par un projet dans une solution d’application Xamarin.Forms.

Le **Xamarin.FormsBook.Platform** projet contient le nouvel [ `IFileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/IFileHelper.cs) interface. Toutes les méthodes ont maintenant des noms `Async` suffixes et retour `Task` objets.

Le **Xamarin.FormsBook.Platform.WinRT** projet contient le [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/FileHelper.cs) classe pour le Windows Runtime.

Le **Xamarin.FormsBook.Platform.iOS** projet contient le [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/FileHelper.cs) classe pour iOS. Ces méthodes doivent maintenant être asynchrones. Certaines méthodes utilisent des versions asynchrones des méthodes définies dans `StreamWriter` et `StreamReader`: [ `WriteAsync` ](https://developer.xamarin.com/api/member/System.IO.StreamWriter.WriteAsync/p/System.String/) et [ `ReadToEndAsync` ](https://developer.xamarin.com/api/member/System.IO.StreamReader.ReadToEndAsync()/). D’autres convertissent en un résultat pour un `Task` à l’aide de l’objet le [ `FromResult` ](https://developer.xamarin.com/api/member/System.Threading.Tasks.Task.FromResult%7BTResult%7D/p/TResult/) (méthode).

Le **Xamarin.FormsBook.Platform.Android** projet contient un texte similaire [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/FileHelper.cs) classe pour Android.

Le **Xamarin.FormsBook.Platform** projet contient également un [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/FileHelper.cs) classe qui facilite l’utilisation de la `DependencyService` objet.

Pour utiliser ces bibliothèques, une solution d’application doit inclure tous les projets de la **Xamarin.FormsBook.Platform** solution et les projets d’application doivent avoir une référence à la bibliothèque correspondante dans  **Xamarin.FormsBook.Platform**.

Le [ **TextFileAsync** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TextFileAsync) solution montre comment utiliser le **Xamarin.FormsBook.Platform** bibliothèques. Chaque projet comporte un appel à `Toolkit.Init`. L’application se sert de l’e/s sur fichier asynchrones fonctions.

### <a name="keeping-it-in-the-background"></a>Conserver en arrière-plan

Méthodes dans les bibliothèques qui effectuent des appels à plusieurs méthodes asynchrones &mdash; tels que le `WriteFileAsync` et `ReadFileASync` méthodes dans le Windows Runtime [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/FileHelper.cs) classe &mdash; peut être effectuée quelque peu plus efficace à l’aide de la [ `ConfigureAwait` ](https://developer.xamarin.com/api/member/System.Threading.Tasks.Task%3CTResult%3E.ConfigureAwait/p/System.Boolean/) méthode afin d’éviter le basculement vers le thread d’interface utilisateur.

### <a name="dont-block-the-ui-thread"></a>Ne pas bloquer le thread d’interface utilisateur !

Il est parfois tentant d’éviter l’utilisation de `ContinueWith` ou `await` à l’aide de la [ `Result` ](https://developer.xamarin.com/api/property/System.Threading.Tasks.Task%3CTResult%3E.Result/) propriété sur les méthodes. Cela doit être évité pour bloquer le thread d’interface utilisateur ou de blocage même de l’application.

## <a name="your-own-awaitable-methods"></a>Vos propres méthodes pouvant être attendus

Vous pouvez exécuter du code de façon asynchrone en le passant à une de la [ `Task.Run` ](https://developer.xamarin.com/api/member/System.Threading.Tasks.Task.Run/p/System.Action/) méthodes. Vous pouvez appeler `Task.Run` au sein d’une méthode asynchrone qui gère une partie de la surcharge.

Les différentes `Task.Run` modèles sont décrites ci-dessous.

### <a name="the-basic-mandelbrot-set"></a>La base Mandelbrot

Pour dessiner le Mandelbrot défini en temps réel, le [ **Xamarin.Forms.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) bibliothèque a un [ `Complex` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/Complex.cs) structure similaire à celui de la `System.Numerics` espace de noms.

Le [ **MandelbrotSet** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotSet) exemple a un `CalculateMandeblotAsync` méthode dans son fichier code-behind qui calcule la base Mandelbrot noir et blanc [ `BmpMaker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BmpMaker.cs)à placer dans une image bitmap.

### <a name="marking-progress"></a>Le marquage de progression

Pour signaler la progression à partir d’une méthode asynchrone, vous pouvez instancier un [ `Progress<T>` ](https://developer.xamarin.com/api/type/System.Progress%3CT%3E/) de classe et de définir votre méthode asynchrone pour avoir un argument de type [ `IProgress<T>` ](https://developer.xamarin.com/api/type/System.IProgress%3CT%3E/). Cela est illustré dans le [ **MandelbrotProgress** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotProgress) exemple.

### <a name="cancelling-the-job"></a>Annulation de la tâche

Vous pouvez également écrire une méthode asynchrone pour être annulable. Commencer avec une classe nommée [ `CancellationTokenSource` ](https://developer.xamarin.com/api/type/System.Threading.CancellationTokenSource/). Le [ `Token` ](https://developer.xamarin.com/api/property/System.Threading.CancellationTokenSource.Token/) propriété est une valeur de type [ `CancellationToken` ](https://developer.xamarin.com/api/type/System.Threading.CancellationToken/). Il est passé à la fonction asynchrone. Un programme appelle la [ `Cancel` ](https://developer.xamarin.com/api/member/System.Threading.CancellationTokenSource.Cancel()/) méthode `CancellationTokenSource` (généralement en réponse à une action de l’utilisateur) pour annuler la fonction asynchrone.

La méthode asynchrone peut vérifier régulièrement la [ `IsCancellationRequested` ](https://developer.xamarin.com/api/property/System.Threading.CancellationToken.IsCancellationRequested/) propriété du `CancellationToken` et quitter si la propriété est `true`, ou simplement appeler le [ `ThrowIfCancellationRequested` ](https://developer.xamarin.com/api/member/System.Threading.CancellationToken.ThrowIfCancellationRequested()/) (méthode), dans auquel cas la méthode se termine par un [ `OperationCancelledException` ](https://developer.xamarin.com/api/type/System.OperationCanceledException/).

Le [ **MandelbrotCancellation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotCancellation) exemple illustre l’utilisation d’une fonction pouvant être annulée.

### <a name="an-mvvm-mandelbrot"></a>Un Mandelbrot MVVM

Le [ **MandelbrotXF** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotXF) exemple a une interface utilisateur plus étendue, et il est principalement basée sur un [ `MandelbrotModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/MandelbrotXF/MandelbrotXF/MandelbrotXF/MandelbrotModel.cs) et [ `MandelbrotViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/MandelbrotXF/MandelbrotXF/MandelbrotXF/MandelbrotViewModel.cs)classes :

[![Capture d’écran de triple de Mandelbrot X F](images/ch20fg13-small.png "MVVM Mandelbrot")](images/ch20fg13-large.png#lightbox "MVVM Mandelbrot")

## <a name="back-to-the-web"></a>Sur le web

Le [ `WebRequest` ](https://developer.xamarin.com/api/type/System.Net.WebRequest/) classe utilisée dans certains exemples utilise un protocole d’asynchrone traditionnelle appelé modèle de programmation asynchrone ou APM. Vous pouvez convertir une telle classe au protocole TAP modern à l’aide d’un de le `FromAsync` méthodes dans le [ `TaskFactory` ](https://developer.xamarin.com/api/type/System.Threading.Tasks.TaskFactory%3CTResult%3E/) classe. Le [ **ApmToTap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/ApmToTap) illustre cela.



## <a name="related-links"></a>Liens associés

- [Recherche en texte intégral chapitre 20 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch20-Apr2016.pdf)
- [Chapitre 20 échantillons](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20)
- [Utilisation de fichiers](~/xamarin-forms/app-fundamentals/files.md)
