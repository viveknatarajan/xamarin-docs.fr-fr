---
title: Création d'un service
ms.prod: xamarin
ms.assetid: A78A55E7-FB5C-4C42-8E3E-939B5E98F9EB
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/03/2018
ms.openlocfilehash: 8c2086025ccb5fe41b3ffddc9cd650c1e0c81fbc
ms.sourcegitcommit: f890b5ec9b7c2702875070859e1a8cbf6e870e46
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/28/2018
ms.locfileid: "53813983"
---
# <a name="creating-a-service"></a>Création d'un service

Services de Xamarin.Android doivent respecter deux règles inviolables de services Android :

* Ils doivent étendre la [ `Android.App.Service` ](https://developer.xamarin.com/api/type/Android.App.Service/).
* Elles doivent être décorées avec le [ `Android.App.ServiceAttribute` ](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute/).

Une autre exigence de services Android est qu’ils doivent être inscrits dans le **AndroidManifest.xml** et un nom unique donné. Xamarin.Android s’inscriront automatiquement le service dans le manifeste au moment de la génération avec l’attribut XML nécessaires.

Cet extrait de code est un exemple simple de création d’un service dans Xamarin.Android qui répond à ces deux exigences :  

```csharp
[Service]
public class DemoService : Service
{
    // Magical code that makes the service do wonderful things.
}
```

Au moment de la compilation, Xamarin.Android inscrira le service par l’injection de l’élément XML suivant dans **AndroidManifest.xml** (Notez que Xamarin.Android a généré un nom aléatoire pour le service) :

```xml
<service android:name="md5a0cbbf8da641ae5a4c781aaf35e00a86.DemoService" />
```

Il est possible de partager un service avec d’autres applications Android par _exportation_ il. Cela s’effectue en définissant le `Exported` propriété sur le `ServiceAttribute`. Lors de l’exportation d’un service, le `ServiceAttribute.Name` propriété doit également être définie pour fournir un nom explicite publique pour le service. Cet extrait de code montre comment exporter et nommez un service :

```csharp
[Service(Exported=true, Name="com.xamarin.example.DemoService")]
public class DemoService : Service
{
    // Magical code that makes the service do wonderful things.
}
```

Le **AndroidManifest.xml** élément pour ce service puis ressemblera à :

```xml
<service android:exported="true" android:name="com.xamarin.example.DemoService" />
```

Les services ont leur propre cycle de vie avec des méthodes de rappel qui sont appelés comme le service est créé. Exactement quelles méthodes sont appelées varie selon le type de service. Un service démarré doit implémenter les méthodes de cycle de vie différents qu’un service lié, pendant un service hybride doit implémenter les méthodes de rappel pour un service de prise en main et un service lié. Ces méthodes sont tous les membres de la `Service` classe ; la façon dont le service est démarré détermine quelles méthodes de cycle de vie seront appelés. Ces méthodes de cycle de vie nous reviendrons plus en détail ultérieurement.

Par défaut, un service démarre dans le même processus qu’une application Android. Il est possible de démarrer un service dans son propre processus en définissant le `ServiceAttribute.IsolatedProcess` true à la propriété :

```csharp
[Service(IsolatedProcess=true)]
public class DemoService : Service
{
    // Magical code that makes the service do wonderful things, in it's own process!
}
```

L’étape suivante consiste à examiner comment démarrer un service et passez ensuite à examiner comment implémenter les trois différents types de services.

> [!NOTE]
> Un service s’exécute sur le thread d’interface utilisateur, donc si n’importe quel travail est à effectuer ce qui bloque l’interface utilisateur, le service doit utiliser des threads pour effectuer le travail.

## <a name="starting-a-service"></a>Démarrage d’un Service

La façon la plus simple pour démarrer un service dans Android consiste à distribuer un `Intent` qui contient des métadonnées pour aider à identifier le service qui doit être démarré. Il existe deux styles différents de Intents qui peuvent être utilisées pour démarrer un service :

-   **Intention explicite** &ndash; un _intention explicite_ identifie exactement quel service doit être utilisé pour effectuer une action donnée. Une intention explicite peut être considérée comme une lettre qui a une adresse spécifique ; Android achemine l’intention au service qui est identifié explicitement. Cet extrait de code est un exemple d’utilisation d’une intention explicite pour démarrer un service appelé `DownloadService`:

    ```csharp
    // Example of creating an explicit Intent in an Android Activity
    Intent downloadIntent = new Intent(this, typeof(DownloadService));
    downloadIntent.data = Uri.Parse(fileToDownload);
    ```

-   **Intent implicite** &ndash; ce type d’intention faiblement identifie l’action que l’utilisateur souhaite effectuer, mais le service exact pour effectuer cette action est inconnu. Une intention implicite peut être considérée comme une lettre est traité « Problème de mai à Whom informatique... ».
    Android sera examiner le contenu de l’objectif et déterminer s’il existe un service existant qui correspond à l’intention.

    Un _filtre intent_ est utilisé pour mieux répondre à l’intention implicite avec un service inscrit. Un filtre d’intention est un élément XML qui est ajouté à **AndroidManifest.xml** qui contient les métadonnées nécessaires pour mieux répondre à un Service avec une intention implicite.

    ```csharp
    Intent sendIntent = new Intent("common.xamarin.DemoService");
    sendIntent.Data = Uri.Parse(fileToDownload);
    ```

Si Android a plusieurs correspondances possibles pour une intention implicite, il peut demander à l’utilisateur pour sélectionner le composant pour gérer l’action :

![Capture d’écran d’une boîte de dialogue de levée d’ambiguïté](images/creating-a-service-01.png "capture d’écran d’une boîte de dialogue de levée d’ambiguïté")

> [!IMPORTANT]
> À partir de Android 5.0 (niveau 21 d’AP) une intention implicite ne peut pas être utilisée pour démarrer un service.

Si possible, les applications doivent utiliser les intentions explicites pour démarrer un service. Une intention implicite ne demande pas d’un service spécifique Démarrer &ndash; il s’agit une demande pour un service installé sur l’appareil pour gérer la demande. Cette demande AMBIGUE peut entraîner un mauvais service gère la requête ou une autre application inutilement démarrage (ce qui augmente la pression pour les ressources sur l’appareil).

Comment l’intention est distribuée dépend du type de service et sera abordée plus en détail plus loin dans les guides spécifiques à chaque type de service.


### <a name="creating-an-intent-filter-for-implicit-intents"></a>Création d’un filtre d’intention d’intentions implicites

Pour associer un service avec une intention implicite, une application Android doit fournir des métadonnées pour identifier les fonctionnalités du service. Ces métadonnées sont fournie par _filtres intentionnels_. Filtres intentionnels contiennent certaines informations, telles que d’une action ou un type de données, qui doivent être présents dans une intention pour démarrer un service. Dans Xamarin.Android, le filtre d’intention est inscrit dans **AndroidManifest.xml** en décorant un service avec le [ `IntentFilterAttribute` ](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/). Par exemple, le code suivant ajoute un filtre d’intention avec l’action associée `com.xamarin.DemoService`:

```csharp
[Service]
[IntentFilter(new String[]{"com.xamarin.DemoService"})]
public class DemoService : Service
{
}
```

Il en résulte une entrée qui est incluse dans le **AndroidManifest.xml** fichier &ndash; une entrée qui est fournie avec l’application d’une manière analogue à l’exemple suivant :

```xml
<service android:name="demoservice.DemoService">
    <intent-filter>
        <action android:name="com.xamarin.DemoService" />
    </intent-filter>
</service>
```

Principes de base d’un service de Xamarin.Android disparaître, examinons les différents sous-types de services plus en détail.


## <a name="related-links"></a>Liens associés

- [Android.App.Service](https://developer.xamarin.com/api/type/Android.App.Service/)
- [Android.App.ServiceAttribute](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute/)
- [Android.App.Intent](https://developer.xamarin.com/api/type/Android.Content.Intent/)
- [Android.App.IntentFilterAttribute](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/)
