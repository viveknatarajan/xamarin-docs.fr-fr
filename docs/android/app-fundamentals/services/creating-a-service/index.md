---
title: Création d'un service
ms.prod: xamarin
ms.assetid: A78A55E7-FB5C-4C42-8E3E-939B5E98F9EB
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 05/03/2018
ms.openlocfilehash: 00785ad161f5f05fd70b059bb0a3f1c8d6c31f97
ms.sourcegitcommit: daa089d41cfe1ed0456d6de2f8134cf96ae072b1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33850832"
---
# <a name="creating-a-service"></a>Création d'un service

Services de Xamarin.Android doivent respecter les deux règles inviolables de services Android :

* Ils doivent s’étendre la [ `Android.App.Service` ](https://developer.xamarin.com/api/type/Android.App.Service/).
* Elles doivent être décorées avec le [ `Android.App.ServiceAttribute` ](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute/).

Une autre exigence de services Android est qu’ils doivent être inscrits dans le **AndroidManifest.xml** et donné un nom unique. Xamarin.Android inscrire automatiquement le service dans le manifeste au moment de la génération avec l’attribut XML nécessaire.

Cet extrait de code est un exemple simple de création d’un service dans Xamarin.Android qui répond à ces deux exigences :  

```csharp
[Service]
public class DemoService : Service
{
    // Magical code that makes the service do wonderful things.
}
```

Au moment de la compilation, Xamarin.Android inscrira le service par l’injection de l’élément XML suivant dans **AndroidManifest.xml** (Notez que Xamarin.Android généré un nom aléatoire pour le service) :

```xml
<service android:name="md5a0cbbf8da641ae5a4c781aaf35e00a86.DemoService" />
```

Il est possible de partager un service avec d’autres applications Android par _exportation_ il. Cela est accompli en définissant le `Exported` propriété sur le `ServiceAttribute`. Lors de l’exportation d’un service, le `ServiceAttribute.Name` propriété doit également être définie pour fournir un nom significatif publique pour le service. Cet extrait de code montre comment exporter et d’un service de noms :

```csharp
[Service(Exported=true, Name="com.xamarin.example.DemoService")]
public class DemoService : Service
{
    // Magical code that makes the service do wonderful things.
}
```

Le **AndroidManifest.xml** élément pour ce service puis ressemble à ceci :

```xml
<service android:exported="true" android:name="com.xamarin.example.DemoService" />
```

Les services ont leur propre cycle de vie avec les méthodes de rappel qui sont appelées en tant que le service est créé. Exactement quelles méthodes sont appelées varie selon le type de service. Un service démarré doit implémenter les méthodes de cycle de vie différent à un service lié, lors d’un service hybride doit implémenter les méthodes de rappel pour un service démarré et un service lié. Ces méthodes sont tous les membres de la `Service` classe ; la façon dont le service est démarré déterminent les méthodes de cycle de vie qui seront appelés. Ces méthodes de cycle de vie seront abordés plus en détail ultérieurement.

Par défaut, un service démarre dans le même processus qu’une application Android. Il est possible de démarrer un service dans son propre processus, en définissant le `ServiceAttribute.IsolatedProcess` true à la propriété :

```csharp
[Service(IsolatedProcess=true)]
public class DemoService : Service
{
    // Magical code that makes the service do wonderful things, in it's own process!
}
```

L’étape suivante consiste à examiner comment démarrer un service et passez à examiner comment implémenter les trois différents types de services.

> [!NOTE]
> Un service s’exécute sur le thread d’interface utilisateur, donc si n’importe quel travail est à exécuter ce qui bloque l’interface utilisateur, le service doit utiliser des threads pour exécuter le travail.

## <a name="starting-a-service"></a>Démarrage d’un Service

La façon la plus simple pour démarrer un service dans Android consiste à distribuer un `Intent` qui contient des métadonnées pour aider à identifier le service qui doit être démarré. Il existe deux différents styles de modes qui peuvent être utilisés pour démarrer un service :

-   **Intention explicite** &ndash; un _intention explicite_ identifie exactement quel service doit être utilisé pour effectuer une action donnée. Une intention explicite peut être considérée comme une lettre qui dispose d’une adresse spécifique ; Android achemine l’intention au service qui est identifié de manière explicite. Cet extrait de code est un exemple d’utilisation d’une intention explicite pour démarrer un service appelé `DownloadService`:

    ```csharp
    // Example of creating an explicit Intent in an Android Activity
    Intent downloadIntent = new Intent(this, typeof(DownloadService));
    downloadIntent.data = Uri.Parse(fileToDownload);
    ```

-   **Intention implicite** &ndash; faiblement identifie de ce type de l’intention de l’action que l’utilisateur souhaite exécuter, mais le service exact pour effectuer cette action est inconnu. Une intention implicite peut être considérée comme une lettre est traité « Problème de mai à Whom informatique... ».
    Android est examiner le contenu de l’objectif et déterminer s’il existe un service existant, ce qui correspond à l’intention.

    Un _filtre intention_ est utilisée pour correspondre à l’intention implicite avec un service inscrit. Un filtre d’intention est un élément XML qui est ajouté à **AndroidManifest.xml** qui contient les métadonnées nécessaires pour mieux répondre à un Service avec l’intention implicite.

    ```csharp
    Intent sendIntent = new Intent("common.xamarin.DemoService");
    sendIntent.Data = Uri.Parse(fileToDownload);
    ```

Si Android a plusieurs correspondances possibles pour une intention implicite, il peut demander à l’utilisateur pour sélectionner les composants pour gérer l’action :

![Capture d’écran de boîte de dialogue ambiguïté](images/creating-a-service-01.png "capture d’écran de boîte de dialogue ambiguïté")

> [!IMPORTANT]
> Android 5.0 (niveau de l’Asie-Pacifique 21) à partir une intention implicite ne peut pas être utilisée pour démarrer un service.

Lorsque cela est possible, les applications doivent utiliser les modes explicites pour démarrer un service. Une intention implicite ne demande pas un service spécifique pour démarrer &ndash; il s’agit d’une demande pour un service installé sur l’appareil pour gérer la demande. Cette demande ambigüe peut entraîner un mauvais service gestion de la demande ou une autre application empêchera le démarrage (ce qui augmente la pression pour les ressources sur l’appareil).

Comment l’intention est distribuée dépend du type de service et sera abordée plus en détail plus loin dans les guides spécifiques à chaque type de service.


### <a name="creating-an-intent-filter-for-implicit-intents"></a>Création d’un filtre d’intention pour intentions implicites

Pour associer un service dans un but implicit, une application Android doit fournir des métadonnées pour identifier les fonctionnalités du service. Ces métadonnées sont fournie par _filtres intentionnels_. Les filtres intentionnels contiennent des informations, comme une action ou un type de données, qui doivent être présents dans un mode pour démarrer un service. Dans Xamarin.Android, le filtre intention est enregistré dans **AndroidManifest.xml** en décorant un service avec le [ `IntentFilterAttribute` ](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/). Par exemple, le code suivant ajoute un filtre d’intention avec l’action associée `com.xamarin.DemoService`:

```csharp
[Service]
[IntentFilter(new String[]{"com.xamarin.DemoService"})]
public class DemoService : Service
{
}
```

Cela entraîne une entrée qui est incluse dans le **AndroidManifest.xml** fichier &ndash; une entrée qui est fournie avec l’application d’une manière analogue à l’exemple suivant :

```xml
<service android:name="demoservice.DemoService">
    <intent-filter>
        <action android:name="com.xamarin.DemoService" />
    </intent-filter>
</service>
```

Les principes de base d’un service de Xamarin.Android disparaître, examinons les différents sous-types de services plus en détail.


## <a name="related-links"></a>Liens associés

- [Android.App.Service](https://developer.xamarin.com/api/type/Android.App.Service/)
- [Android.App.ServiceAttribute](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute/)
- [Android.App.Intent](https://developer.xamarin.com/api/type/Android.Content.Intent/)
- [Android.App.IntentFilterAttribute](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/)
