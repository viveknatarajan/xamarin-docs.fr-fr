---
title: Travailler avec le manifeste Android
ms.prod: xamarin
ms.assetid: CB7CCF60-FEF1-3B28-215F-159391E74347
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/05/2018
ms.openlocfilehash: 5e354f8271257ab21a855bdf5d576ce3062fadc7
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60957388"
---
# <a name="working-with-the-android-manifest"></a>Travailler avec le manifeste Android


## <a name="overview"></a>Vue d'ensemble

**AndroidManifest.xml** est un fichier puissant dans la plateforme Android qui vous permet de décrire les fonctionnalités et les exigences de votre application pour Android. Toutefois, travailler avec elle n’est pas facile. Xamarin.Android permet de réduire cette difficulté en vous permettant d’ajouter des attributs personnalisés à vos classes, qui seront ensuite servir pour générer automatiquement le manifeste pour vous. Notre objectif est que les 99 % de nos utilisateurs ne doivent jamais avoir à modifier manuellement **AndroidManifest.xml**. 

**AndroidManifest.xml** est généré en tant que partie du processus de génération et le code XML trouvé dans **Properties/Androidmanifest.XML** est fusionné avec le code XML qui est généré à partir des attributs personnalisés. Le résultat fusionné **AndroidManifest.xml** réside dans le **obj** sous-répertoire ; par exemple, il réside à **obj/Debug/android/AndroidManifest.xml** pour les versions Debug . Le processus de fusion est simple : il utilise les attributs personnalisés dans le code pour générer des éléments XML, et *insère* ces éléments dans **AndroidManifest.xml**. 



## <a name="the-basics"></a>Principes de base

Au moment de la compilation, les assemblys sont analysés pour non -`abstract` classes qui dérivent [activité](https://developer.xamarin.com/api/type/Android.App.Activity/) et ont le [ `[Activity]` ](https://developer.xamarin.com/api/type/Android.App.ActivityAttribute/) attribut déclaré sur ces derniers. Il utilise ensuite ces classes et les attributs pour générer le manifeste. Considérons par exemple le code suivant : 

```csharp
namespace Demo
{
    public class MyActivity : Activity
    {
    }
}
```

Ainsi, rien n’est en cours de génération dans **AndroidManifest.xml**. Si vous souhaitez un `<activity/>` puisse être généré, vous devez utiliser la [`[Activity]`](https://developer.xamarin.com/api/type/Android.App.Activity/Attribute) 
attribut personnalisé : 

```csharp
namespace Demo
{
    [Activity]
    public class MyActivity : Activity
    {
    }
}
```

Cet exemple montre le fragment xml suivant à ajouter à **AndroidManifest.xml**:

```xml
<activity android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity" />
```

Le `[Activity]` attribut n’a aucun effet `abstract` types ; `abstract` types sont ignorés.



### <a name="activity-name"></a>Nom de l'activité

À compter de Xamarin.Android 5.1, le nom de type d’une activité repose sur le MD5SUM du nom qualifié d’assembly du type en cours d’exportation. Ainsi, le même nom qualifié complet être fournis à partir de deux assemblys différents et pas obtenir une erreur d’empaquetage. (Avant Xamarin.Android 5.1, le nom de type par défaut de l’activité a été créé à partir de l’espace de noms en minuscules et le nom de classe.) 

Si vous souhaitez remplacer cette valeur par défaut et spécifier explicitement le nom de votre activité, utilisez le [ `Name` ](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.Name/) propriété : 

```csharp
[Activity (Name="awesome.demo.activity")]
public class MyActivity : Activity
{
}
```

Cet exemple produit le fragment xml suivant :

```xml
<activity android:name="awesome.demo.activity" />
```

*Remarque*: vous devez utiliser le `Name` propriété uniquement pour des raisons de compatibilité descendante, changement de nom en tant que tel peut ralentir la recherche du type lors de l’exécution. Si vous avez du code hérité qui attend le nom de type par défaut de l’activité doit être basé sur l’espace de noms en minuscules et le nom de classe, consultez [Android Callable Wrapper d’affectation de noms](https://developer.xamarin.com/releases/android/xamarin.android_5/xamarin.android_5.1/#Android_Callable_Wrapper_Naming) pour obtenir des conseils sur la conservation de la compatibilité. 


### <a name="activity-title-bar"></a>Barre de titre d’activité

Par défaut, Android donne à votre application une barre de titre lorsqu’elle est exécutée. La valeur utilisée pour cela est [ `/manifest/application/activity/@android:label` ](https://developer.android.com/guide/topics/manifest/activity-element.html#label). Dans la plupart des cas, cette valeur diffère du nom de votre classe. Pour spécifier l’étiquette de votre application sur la barre de titre, utilisez la [ `Label` ](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.Label/) propriété.
Exemple : 

```csharp
[Activity (Label="Awesome Demo App")]
public class MyActivity : Activity
{
}
```

Cet exemple produit le fragment xml suivant :

```xml
<activity android:label="Awesome Demo App" 
          android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity" />
```


### <a name="launchable-from-application-chooser"></a>Accessible à partir du sélecteur d’Application

Par défaut, votre activité s’afficheront pas dans l’écran de lancement d’application d’Android. Il s’agit, car il y aura probablement de nombreuses activités dans votre application, et vous ne voulez pas une icône pour chacun. Pour spécifier celle qui doit être accessible depuis le Lanceur d’applications, utilisez la [ `MainLauncher` ](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.MainLauncher/) propriété. Exemple : 

```csharp
[Activity (Label="Awesome Demo App", MainLauncher=true)] 
public class MyActivity : Activity
{
}
```

Cet exemple produit le fragment xml suivant :

```xml
<activity android:label="Awesome Demo App" 
          android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity">
  <intent-filter>
    <action android:name="android.intent.action.MAIN" />
    <category android:name="android.intent.category.LAUNCHER" />
  </intent-filter>
</activity>
```



### <a name="activity-icon"></a>Icône d’activité

Par défaut, votre activité recevra l’icône de lancement par défaut fourni par le système. Pour utiliser une icône personnalisée, ajoutez d’abord votre **.png** à **ressources/drawable**, définissez son Action de génération **AndroidResource**, puis utiliser le [ `Icon` ](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.Icon/) propriété pour spécifier l’icône à utiliser. Exemple : 

```csharp
[Activity (Label="Awesome Demo App", MainLauncher=true, Icon="@drawable/myicon")] 
public class MyActivity : Activity
{
}
```

Cet exemple produit le fragment xml suivant :

```xml
<activity android:icon="@drawable/myicon" android:label="Awesome Demo App" 
          android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity">
  <intent-filter>
    <action android:name="android.intent.action.MAIN" />
    <category android:name="android.intent.category.LAUNCHER" />
  </intent-filter>
</activity>
```


### <a name="permissions"></a>Autorisations

Lorsque vous ajoutez des autorisations pour le manifeste Android (comme décrit dans [ajouter des autorisations au manifeste Android](https://github.com/xamarin/recipes/tree/master/Recipes/android/general/projects/add_permissions_to_android_manifest)), ces autorisations sont enregistrées dans **Properties/Androidmanifest.XML**. Par exemple, si vous définissez la `INTERNET` autorisation, l’élément suivant est ajouté à **Properties/Androidmanifest.XML**: 

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

Les versions Debug définir automatiquement certaines autorisations pour faciliter le débogage (tel que `INTERNET` et `READ_EXTERNAL_STORAGE`) &ndash; ces paramètres sont définis dans le texte généré **obj/Debug/android/AndroidManifest.xml** et ne sont pas affiché comme activé dans le **autorisations requises** paramètres. 

Par exemple, si vous examinez le fichier de manifeste généré au **obj/Debug/android/AndroidManifest.xml**, vous pouvez voir ce qui suit ajouté des éléments d’autorisation : 

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
```

Dans la version générer la version du manifeste (à **obj/Debug/android/AndroidManifest.xml**), ces autorisations sont *pas* configurées automatiquement. Si vous trouvez que le passage à une version Release incite votre application perd une autorisation qui était disponible dans la version Debug, vérifiez que vous avez défini explicitement cette autorisation le **autorisations requises** paramètres pour votre application (voir  **Générer > Application Android** dans Visual Studio pour Mac ; consultez **Propriétés > manifeste Android** dans Visual Studio). 




## <a name="advanced-features"></a>Fonctionnalités avancées


### <a name="intent-actions-and-features"></a>Fonctionnalités et les Actions intent

Le manifeste Android offre un moyen pour vous permettent de décrire les fonctionnalités de votre activité. Cette opération est effectuée [intentions](https://developer.android.com/guide/topics/manifest/intent-filter-element.html) et le [`[IntentFilter]`](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/) 
attribut personnalisé. Vous pouvez spécifier les actions qui sont appropriées pour votre activité avec le [`IntentFilter`](https://developer.xamarin.com/api/constructor/Android.App.IntentFilterAttribute.IntentFilterAttribute/p/System.String[]/) 
constructeur, et les catégories dans lesquelles sont appropriés avec les [`Categories`](https://developer.xamarin.com/api/property/Android.App.IntentFilterAttribute.Categories/) 
. Au moins une activité doit être fournie (qui est la raison pour laquelle les activités sont fournies dans le constructeur). `[IntentFilter]` peuvent être fournis à plusieurs reprises, et chaque utilisation entraîne un distinct `<intent-filter/>` élément dans le `<activity/>`. Exemple :

```csharp
[Activity (Label="Awesome Demo App", MainLauncher=true, Icon="@drawable/myicon")] 
[IntentFilter (new[]{Intent.ActionView}, 
        Categories=new[]{Intent.CategorySampleCode, "my.custom.category"})]
public class MyActivity : Activity
{
}
```

Cet exemple produit le fragment xml suivant :

```xml
<activity android:icon="@drawable/myicon" android:label="Awesome Demo App" 
          android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity">
  <intent-filter>
    <action android:name="android.intent.action.MAIN" />
    <category android:name="android.intent.category.LAUNCHER" />
  </intent-filter>
  <intent-filter>
    <action android:name="android.intent.action.VIEW" />
    <category android:name="android.intent.category.SAMPLE_CODE" />
    <category android:name="my.custom.category" />
  </intent-filter>
</activity>
```


### <a name="application-element"></a>Élément d’application

Le manifeste Android fournit également un moyen pour vous permettent de déclarer des propriétés pour votre application entière. Cette opération est effectuée le `<application>` élément et son équivalent, la [Application](https://developer.xamarin.com/api/type/Android.App.ApplicationAttribute/) attribut personnalisé. Notez qu’il s’agit de paramètres d’application (assembly à l’échelle) plutôt que les paramètres par activité. En général, vous déclarez `<application>` propriétés pour votre application entière, puis vous remplacez ces paramètres (si nécessaire) sur une base par activité. 

Par exemple, ce qui suit `Application` attribut est ajouté à **AssemblyInfo.cs** pour indiquer que l’application peut être déboguée, que son nom lisible par l’utilisateur est **My App**, et qu’il utilise le `Theme.Light` style comme le thème par défaut pour toutes les activités : 

```csharp
[assembly: Application (Debuggable=true,   
                        Label="My App",   
                        Theme="@android:style/Theme.Light")]
```

Cette déclaration provoque le fragment XML suivant doit être généré dans **obj/Debug/android/AndroidManifest.xml**:

```xml
<application android:label="My App" 
             android:debuggable="true" 
             android:theme="@android:style/Theme.Light"
                ... />
```
Dans cet exemple, toutes les activités dans l’application par défaut seront le `Theme.Light` style. Si vous définissez le thème d’une activité sur `Theme.Dialog`, uniquement qui activité utilisera le `Theme.Dialog` style tandis que toutes les autres activités dans votre application par défaut seront le `Theme.Light` style tel que défini dans le `<application>` élément. 

Le `Application` élément n’est pas la seule façon de configurer `<application>` attributs. Alternativement, vous pouvez insérer des attributs directement dans le `<application>` élément de **Properties/Androidmanifest.XML**. Ces paramètres sont fusionnées dans la dernière `<application>` élément qui se trouve dans **obj/Debug/android/AndroidManifest.xml**. Notez que le contenu de **Properties/Androidmanifest.XML** toujours remplacer les données fournies par les attributs personnalisés. 

Il existe de nombreux attributs de l’application que vous pouvez configurer dans le `<application>` élément ; pour plus d’informations sur ces paramètres, consultez le [propriétés publiques](https://developer.xamarin.com/api/type/Android.App.ApplicationAttribute/#Public_Properties) section de [ApplicationAttribute](https://developer.xamarin.com/api/type/Android.App.ApplicationAttribute/). 



## <a name="list-of-custom-attributes"></a>Liste d’attributs personnalisés

-   [Android.App.ActivityAttribute](https://developer.xamarin.com/api/type/Android.App.ActivityAttribute/) : Génère un [/manifest/application/activity](https://developer.android.com/guide/topics/manifest/activity-element.html) fragment XML 
-   [Android.App.ApplicationAttribute](https://developer.xamarin.com/api/type/Android.App.ApplicationAttribute/) : Génère un [/manifeste/application](https://developer.android.com/guide/topics/manifest/application-element.html) fragment XML 
-   [Android.App.InstrumentationAttribute](https://developer.xamarin.com/api/type/Android.App.InstrumentationAttribute/) : Génère un [/manifeste/instrumentation](https://developer.android.com/guide/topics/manifest/instrumentation-element.html) fragment XML 
-   [Android.App.IntentFilterAttribute](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/) : Génère un [//intent-filter](https://developer.android.com/guide/topics/manifest/intent-filter-element.html) fragment XML 
-   [Android.App.MetaDataAttribute](https://developer.xamarin.com/api/type/Android.App.MetaDataAttribute/) : Génère un [//meta-data](https://developer.android.com/guide/topics/manifest/meta-data-element.html) fragment XML 
-   [Android.App.PermissionAttribute](https://developer.xamarin.com/api/type/Android.App.PermissionAttribute/) : Génère un [//permission](https://developer.android.com/guide/topics/manifest/permission-element.html) fragment XML 
-   [Android.App.PermissionGroupAttribute](https://developer.xamarin.com/api/type/Android.App.PermissionGroupAttribute/) : Génère un [//permission-group](https://developer.android.com/guide/topics/manifest/permission-group-element.html) fragment XML 
-   [Android.App.PermissionTreeAttribute](https://developer.xamarin.com/api/type/Android.App.PermissionTreeAttribute/) : Génère un [//permission-tree](https://developer.android.com/guide/topics/manifest/permission-tree-element.html) fragment XML 
-   [Android.App.ServiceAttribute](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute/) : Génère un [/manifest/application/service](https://developer.android.com/guide/topics/manifest/service-element.html) fragment XML 
-   [Android.App.UsesLibraryAttribute](https://developer.xamarin.com/api/type/Android.App.UsesLibraryAttribute/) : Génère un [/manifest/application/uses-library](https://developer.android.com/guide/topics/manifest/uses-library-element.html) fragment XML 
-   [Android.App.UsesPermissionAttribute](https://developer.xamarin.com/api/type/Android.App.UsesPermissionAttribute/) : Génère un [/manifest/uses-permission](https://developer.android.com/guide/topics/manifest/uses-permission-element.html) fragment XML 
-   [Android.Content.BroadcastReceiverAttribute](https://developer.xamarin.com/api/type/Android.Content.BroadcastReceiverAttribute/) : Génère un [/manifest/application/receiver](https://developer.android.com/guide/topics/manifest/receiver-element.html) fragment XML 
-   [Android.Content.ContentProviderAttribute](https://developer.xamarin.com/api/type/Android.Content.ContentProviderAttribute/) : Génère un [/manifest/application/provider](https://developer.android.com/guide/topics/manifest/provider-element.html) fragment XML 
-   [Android.Content.GrantUriPermissionAttribute](https://developer.xamarin.com/api/type/Android.Content.GrantUriPermissionAttribute/) : Génère un [/manifest/application/provider/grant-uri-permission](https://developer.android.com/guide/topics/manifest/grant-uri-permission-element.html) fragment XML

