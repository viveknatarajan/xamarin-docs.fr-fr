---
title: Présentation des niveaux d’API Android
description: Xamarin.Android a plusieurs paramètres de niveau API Android qui déterminent la compatibilité de votre application avec plusieurs versions d’Android. Ce guide explique la signifient de ces paramètres, comment les configurer et effet qu’ils ont sur votre application en cours d’exécution.
ms.prod: xamarin
ms.assetid: 58CB7B34-3140-4BEB-BE2E-209928C1878C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/21/2018
ms.openlocfilehash: 8690be7551046a26339f58029da5f3f58e18cd15
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57672584"
---
# <a name="understanding-android-api-levels"></a>Présentation des niveaux d’API Android

_Xamarin.Android a plusieurs paramètres de niveau API Android qui déterminent la compatibilité de votre application avec plusieurs versions d’Android. Ce guide explique la signifient de ces paramètres, comment les configurer et effet qu’ils ont sur votre application en cours d’exécution._


## <a name="quick-start"></a>Démarrage rapide

Xamarin.Android expose trois paramètres de projet au niveau d’API Android :

-   [Cible de Framework](#framework) &ndash; indique l’infrastructure à utiliser dans la création de votre application. Ce niveau d’API est utilisé au *compiler* temps par Xamarin.Android.

-   [Version minimale d’Android](#minimum) &ndash; spécifie la version Android plus ancien que vous souhaitez que votre application pour prendre en charge. Ce niveau d’API est utilisé au *exécuter* temps par Android.

-   [Cibler Android Version](#target) &ndash; spécifie la version d’Android utilisée par votre application est conçue pour s’exécuter. Ce niveau d’API est utilisé au *exécuter* temps par Android.

Avant de pouvoir configurer un niveau d’API pour votre projet, vous devez installer les composants de plateforme de kit de développement logiciel pour ce niveau d’API. Pour plus d’informations sur le téléchargement et installation des composants du Kit Android SDK, consultez [le programme d’installation du Kit de développement logiciel Android](~/android/get-started/installation/android-sdk.md).

> [!NOTE]
> Depuis août 2018, la Console Google Play nécessite que les nouvelles applications ciblent niveau d’API 26 (Android 8.0) ou une version ultérieure.
Les applications existantes seront requis pour cibler le niveau d’API 26 ou début plus élevé en novembre 2018. Pour plus d’informations, consultez [amélioration des performances sur Google Play pour les années à venir et sécurité de l’application](https://android-developers.googleblog.com/2017/12/improving-app-security-and-performance.html).

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Normalement, toutes les trois niveaux d’API de Xamarin.Android sont définies sur la même valeur. Sur le **Application** , définissez **compiler à l’aide de la version d’Android (Framework cible)** vers la dernière version d’API stable (ou, au minimum, à la version Android qui a toutes les fonctionnalités que vous avez besoin).
Dans la capture d’écran suivante, le Framework cible est défini sur **Android 7.1 (API niveau 25 - Nougat)**:

[![Cibler les valeurs par défaut de la version Framework compiler à l’aide de la version d’Android](android-api-levels-images/vs-defaults-sml.png)](android-api-levels-images/vs-defaults.png#lightbox)

Sur le **manifeste Android** page, définissez la version minimale d’Android sur **utilisez compiler à l’aide de la version SDK** et définissez la version Android cible sur la même valeur que la version de Framework cible (dans l’exemple suivant capture d’écran, la version Android cible est définie sur **Android 7.1 (Nougat)**) :

[![Les versions minimale et cible Android définissent sur la version de Framework cible](android-api-levels-images/vs-manifest-defaults-sml.png)](android-api-levels-images/vs-manifest-defaults.png#lightbox)

Si vous souhaitez assurer la compatibilité descendante avec une version antérieure d’Android, définissez **version minimale d’Android à cibler** la version la plus ancienne d’Android que vous souhaitez que votre application pour prendre en charge. (Notez que le niveau d’API 14 est le niveau d’API minimal requis pour [services Google Play et prise en charge Firebase](https://android-developers.googleblog.com/2016/11/google-play-services-and-firebase-for-android-will-support-api-level-14-at-minimum.html).) L’exemple de configuration suivant prend en charge des versions Android à partir de l’API niveau 14 par le biais de niveau d’API 25 :

[![Compilez à l’aide du niveau d’API 25 Nougat, version minimale d’Android définie au niveau de l’API 14](android-api-levels-images/vs-minimum-sml.png)](android-api-levels-images/vs-minimum.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

Normalement, toutes les trois niveaux d’API de Xamarin.Android sont définies sur la même valeur. Définissez **framework cible** vers la dernière version d’API stable (ou, au minimum, à la version Android qui a toutes les fonctionnalités que vous avez besoin). Pour définir le **framework cible**, accédez à **Générer > Général** dans le **Options du projet**. Dans la capture d’écran suivante, le Framework cible est défini sur **utiliser la dernière plateforme installée (8.0)**:

[![Framework cible par défaut à utiliser la plateforme installée plus récente](android-api-levels-images/xs-default-target-sml.png)](android-api-levels-images/xs-default-target.png#lightbox)

Les paramètres de version minimale et cible Android peuvent être trouvées sous **Générer > Application Android** dans **Options du projet**. La valeur est la version minimale d’Android **automatique - Utilisez la version target framework** et définissez la version Android cible sur la même valeur que la version du Framework cible. Dans la capture d’écran suivante, la version Android cible est définie sur **Android 8.0 (niveau d’API 26)** correspondant au paramètre du Framework cible ci-dessus :

[![Définir les niveaux de la cible et le framework dans Options du projet](android-api-levels-images/xs-default-app-sml.png)](android-api-levels-images/xs-default-app.png#lightbox)

Si vous souhaitez assurer la compatibilité descendante avec une version antérieure d’Android, modifiez **version minimale d’Android** la version la plus ancienne d’Android que vous souhaitez que votre application pour prendre en charge. Notez que le niveau d’API 14 est le niveau d’API minimal requis pour [services Google Play et prise en charge Firebase](https://android-developers.googleblog.com/2016/11/google-play-services-and-firebase-for-android-will-support-api-level-14-at-minimum.html).
Par exemple, la configuration suivante prend en charge les versions d’Android dès que le niveau d’API 14 :

[![Au minimum et cibler des versions définies sur automatique - utiliser la version du framework cible](android-api-levels-images/xs-minimum-sml.png)](android-api-levels-images/xs-minimum.png#lightbox)

-----


Si votre application prend en charge plusieurs versions d’Android, votre code doit inclure des vérifications à l’exécution pour vous assurer que votre application fonctionne avec le paramètre de version minimale d’Android (consultez [Runtime recherche les Versions Android](#runtimechecks) ci-dessous pour plus d’informations). Si vous êtes consommation ou création d’une bibliothèque, consultez [niveaux d’API et bibliothèques](#libraries) ci-dessous pour obtenir des recommandations de la configuration des API de niveau paramètres pour les bibliothèques.



## <a name="android-versions-and-api-levels"></a>Versions Android et niveaux d’API

Comme la plateforme Android évolue et de nouvelles versions Android sont publiées, chaque version d’Android est affectée à un identificateur entier unique, appelé le *niveau d’API*. Par conséquent, chaque version d’Android correspond à un seul niveau d’API Android. Étant donné que les utilisateurs installent des applications sur les versions antérieures ainsi que les plus récentes d’Android, les applications Android doivent être conçues pour fonctionner avec plusieurs niveaux d’API Android.


### <a name="android-versions"></a>Versions d’Android

Chaque version d’Android est publiée par plusieurs noms :

-   La version d’Android, tel que **9.0 Android**
-   Un code (ou dessert) nom, tel que _à secteurs_
-   Niveau d’une API correspondante, comme **niveau d’API 28**

Un nom de code Android peut correspondre à plusieurs versions et les niveaux d’API (comme indiqué dans le tableau ci-dessous), mais chaque version d’Android correspond à un seul niveau d’API.

En outre, Xamarin.Android définit *générer des codes de version* qui correspondent aux niveaux d’API Android connus. Le tableau suivant peut vous aider à traduire entre le niveau d’API, version d’Android, nom de code et code de version de build Xamarin.Android (codes de version de build sont définis dans le `Android.OS` espace de noms) :

[!include[](~/android/includes/api-levels.md)]

Comme l’indique, cette table nouvelles Android versions fréquemment &ndash; parfois plus d’une version par an. Par conséquent, l’univers des appareils Android qui peuvent s’exécuter votre application comprend un large éventail de versions anciennes et récentes d’Android. Comment garantir que votre application s’exécutera cohérente et fiable sur tellement différentes versions d’Android ? Niveaux d’API d’Android peuvent vous aider à gérer ce problème.


### <a name="android-api-levels"></a>Niveaux d’API Android

Chaque appareil Android s’exécute à exactement *un* niveau d’API &ndash; ce niveau d’API est garanti être unique pour chaque version de la plateforme Android. Niveau de l’API identifie précisément la version de l’ensemble d’API qui peut appeler votre application ; Il identifie la combinaison des éléments de manifeste, autorisations, etc. ce que vous codez en tant que développeur. Système d’Android de niveaux d’API permet de déterminer si une application est compatible avec une image système Android avant d’installer l’application sur un appareil Android.

Quand une application est générée, elle contient les informations de niveau API suivantes :

-   Le *cible* niveau d’API d’Android utilisée par l’application est générée pour s’exécuter sur.

-   Le *minimale* niveau d’API Android qu’un appareil Android doit avoir pour exécuter votre application. 

Ces paramètres sont utilisés pour vous assurer que les fonctionnalités nécessaires pour exécuter l’application correctement sont disponible sur l’appareil Android au moment de l’installation. Si ce n’est pas le cas, l’application est bloquée en cours d’exécution sur l’appareil. Par exemple, si le niveau de l’API d’un appareil Android est inférieur au niveau d’API minimale que vous spécifiez pour votre application, l’appareil Android empêche l’utilisateur d’installer votre application.


## <a name="project-api-level-settings"></a>Paramètres au niveau du projet d’API

Les sections suivantes expliquent comment utiliser le Gestionnaire de kit de développement logiciel pour préparer votre environnement de développement pour les niveaux d’API que vous souhaitez cibler, suivie des explications détaillées de la configuration *Framework cible*, *Minimum Version d’Android*, et *version Android cible* paramètres dans Xamarin.Android.


### <a name="android-sdk-platforms"></a>Plateformes de kit de développement logiciel Android

Avant de pouvoir sélectionner un niveau cible ou les API Minimum dans Xamarin.Android, vous devez installer la version de plateforme du Kit Android SDK qui correspond à ce niveau d’API. La plage des choix disponibles pour le Framework cible, la version minimale d’Android et version Android cible est limitée aux plage de Android versions du SDK que vous avez installée. Vous pouvez utiliser le Gestionnaire du Kit de développement logiciel pour vérifier que les versions du Kit Android SDK requises sont installées et que vous pouvez l’utiliser pour ajouter les nouveaux niveaux d’API dont vous avez besoin pour votre application. Si vous n’êtes pas familiarisé avec l’installation de niveaux d’API, consultez [le programme d’installation du Kit de développement logiciel Android](~/android/get-started/installation/android-sdk.md).

<a name="framework" />

### <a name="target-framework"></a>Framework cible

Le *Framework cible* (également appelé `compileSdkVersion`) est la version de framework Android spécifique (niveau API) que votre application est compilée au moment de la génération. Ce paramètre spécifie les API que votre application *attend* à utiliser lors de son exécution, mais il n’a aucun effet sur lequel les API sont réellement disponibles pour votre application lorsqu’il est installé. Par conséquent, la modification du paramètre du Framework cible ne change pas comportement d’exécution.

Le Framework cible identifie les versions de bibliothèque que votre application est liée par rapport à &ndash; ce paramètre détermine les API que vous pouvez utiliser dans votre application. Par exemple, si vous souhaitez utiliser le [NotificationBuilder.SetCategory](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetCategory/p/System.String/) méthode qui a été introduite dans Android Lollipop 5.0, vous devez définir le Framework cible sur **API niveau 21 (Lollipop)** ou version ultérieure. Si vous définissez Framework cible de votre projet à une API niveau comme **API niveau 19 (KitKat)** et essayez d’appeler le `SetCategory` méthode dans votre code, vous obtiendrez une erreur de compilation.

Nous vous conseillons de toujours compiler avec la *dernière* version de Framework cible disponible. Cela vous offre les messages d’avertissement utiles pour toutes les API déconseillées peuvent être appelés par votre code. À l’aide de la dernière version de Framework cible est particulièrement important lorsque vous utilisez les dernières versions de bibliothèque de prise en charge &ndash; chaque bibliothèque attend votre application compilée au niveau d’API minimale de cette bibliothèque de prise en charge ou supérieur. 


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Pour accéder au paramètre du Framework cible dans Visual Studio, ouvrez les propriétés du projet dans **l’Explorateur de solutions** et sélectionnez le **Application** page :

[![Page de propriétés de projet d’application](android-api-levels-images/vs-target-framework-sml.png)](android-api-levels-images/vs-target-framework.png#lightbox)

Définir le Framework cible en sélectionnant un niveau d’API dans le menu déroulant sous **compiler à l’aide de la version d’Android** comme indiqué ci-dessus.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

Pour accéder au paramètre du Framework cible dans Visual Studio pour Mac, cliquez sur le nom du projet et sélectionnez **Options**; cette ouvre le **Options du projet** boîte de dialogue. Dans cette boîte de dialogue, accédez à **Générer > Général** comme indiqué ici :

[![Générer la section Général de la page Options du projet](android-api-levels-images/xs-target-framework-sml.png)](android-api-levels-images/xs-target-framework.png#lightbox)

Définir le Framework cible en sélectionnant un niveau d’API dans le menu déroulant à droite de **framework cible** comme indiqué ci-dessus.

-----


<a name="minimum" />

### <a name="minimum-android-version"></a>Version minimale d’Android

Le *version minimale d’Android* (également appelé `minSdkVersion`) est la version la plus ancienne du système d’exploitation Android (autrement dit, le plus bas niveau de l’API) qui peut installer et exécuter votre application. Par défaut, une application peut uniquement être installé sur les appareils qui ne correspond au paramètre du Framework cible ou ultérieure ; Si le paramètre de version minimale d’Android est *inférieure* à celle du paramètre du Framework cible, votre application peut également s’exécuter sur des versions antérieures d’Android. Par exemple, si vous définissez le Framework cible sur **Android 7.1 (Nougat)** et définissez la version minimale d’Android sur **Android 4.0.3 (Ice Cream Sandwich)**, votre application peut être installée sur n’importe quelle plateforme à partir de l’API niveau 15 au niveau d’API 25, inclusif.

Bien que votre application peut correctement générer et installer sur cet éventail de plateformes, cela ne garantit pas ce qu’elle soit correctement *exécuter* sur toutes ces plateformes. Par exemple, si votre application est installée sur **Android 5.0 (Lollipop)** et votre code appelle une API qui est disponible uniquement dans **Android 7.1 (Nougat)** et les versions ultérieures, votre application obtient une erreur d’exécution et éventuellement se bloquer. Par conséquent, votre code doit vérifier &ndash; lors de l’exécution &ndash; qu’elle appelle des seules API prises en charge par l’appareil Android qu’elle s’exécute sur. En d’autres termes, votre code doit inclure des vérifications à l’exécution explicite pour vous assurer que votre application utilise des API plus récente uniquement sur les appareils qui sont suffisamment récentes pour les prendre en charge.
[Runtime recherche les Versions Android](#runtimechecks), plus loin dans ce guide, explique comment ajouter ces vérifications à l’exécution de votre code.


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Pour accéder au paramètre de version minimale d’Android dans Visual Studio, ouvrez les propriétés du projet dans **l’Explorateur de solutions** et sélectionnez le **manifeste Android** page. Dans le menu déroulant sous **version minimale d’Android** vous pouvez sélectionner la version minimale d’Android pour votre application :

[![Version Android minimale à l’option cible définie pour être compilé à l’aide de la version SDK](android-api-levels-images/vs-minimum-version-sml.png)](android-api-levels-images/vs-minimum-version.png#lightbox)

Si vous sélectionnez **utilisez compiler à l’aide de la version SDK**, la version minimale d’Android doit être le même que le paramètre du Framework cible.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

Pour accéder à la version minimale d’Android dans Visual Studio pour Mac, cliquez sur le nom du projet et sélectionnez **Options**; cette ouvre le **Options du projet** boîte de dialogue. Accédez à **Générer > Application Android**.
À l’aide du menu déroulant à droite de **version minimale d’Android**, vous pouvez définir la version minimale d’Android pour votre application :

[![Version minimale d’Android défini sur automatique - Utilisez la version framework cible](android-api-levels-images/xs-minimum-version-sml.png)](android-api-levels-images/xs-minimum-version.png#lightbox)

Si vous sélectionnez **automatique &ndash; utiliser la version du framework cible**, la version minimale d’Android doit être le même que le paramètre du Framework cible.

-----


<a name="target" />

### <a name="target-android-version"></a>Version Android cible

Le *Version Android cible* (également appelé `targetSdkVersion`) est l’API de niveau de l’appareil Android où l’application est censée s’exécuter. Android utilise ce paramètre pour déterminer s’il faut activer les comportements de compatibilité &ndash; Cela garantit que votre application continue à fonctionner comme prévu. Android utilise le paramètre de version Android cible de votre application pour déterminer les modifications de comportement peuvent être appliquées à votre application sans rupture (il s’agit comment Android fournit une compatibilité ascendante).

Le Framework cible et la version Android cible, tout en ayant des noms similaires, ne sont pas la même chose. Le paramètre du Framework cible communique des informations de niveau API cible pour Xamarin.Android pour une utilisation au *moment de la compilation*, tandis que la version Android cible communique des informations de niveau API cible à Android à un usage  *durée d’exécution* (lorsque l’application est installée et en cours d’exécution sur un appareil).

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Pour accéder à ce paramètre dans Visual Studio, ouvrez les propriétés du projet dans **l’Explorateur de solutions** et sélectionnez le **manifeste Android** page. Dans le menu déroulant sous **version Android cible** vous pouvez sélectionner la version Android cible pour votre application :

[![Version Android cible définie pour être compilé à l’aide de la version SDK](android-api-levels-images/vs-target-version-sml.png)](android-api-levels-images/vs-target-version.png#lightbox)

Nous vous recommandons de définir explicitement la version Android cible vers la dernière version d’Android que vous utilisez pour tester votre application. Dans l’idéal, il doit être défini avec la dernière version du Kit Android SDK &ndash; cela vous permet d’utiliser les nouvelles API avant d’étudier les modifications de comportement. Pour la plupart des développeurs, nous *ne le faites pas* vous recommandons de définir la version Android cible **utilisez compiler à l’aide de la version SDK**.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

Pour accéder à ce paramètre dans Visual Studio pour Mac, cliquez sur le nom du projet et sélectionnez **Options**; cette ouvre le **Options du projet** boîte de dialogue. Accédez à **Générer > Application Android**. À l’aide du menu déroulant à droite de **version Android cible**, vous pouvez définir la version Android cible pour votre application :

[![Version Android cible défini sur automatique - Utilisez la version framework cible](android-api-levels-images/xs-target-version-sml.png)](android-api-levels-images/xs-target-version.png#lightbox)

Nous vous recommandons de définir explicitement la version Android cible vers la dernière version d’Android que vous utilisez pour tester votre application. Dans l’idéal, il doit être défini sur la dernière version du Kit Android SDK disponible &ndash; cela vous permet d’utiliser les nouvelles API avant d’étudier les modifications de comportement. Pour la plupart des développeurs, nous ne recommandons pas dans la définition de la version Android cible **automatique - Utilisez la version target framework**.

-----

En règle générale, la Version Android cible doit être limitée par la Version Android minimale et le Framework cible. Plus précisément :

**Version minimale d’Android < = Version Android cible < = Framework cible**

Pour plus d’informations sur les niveaux de kit de développement logiciel, consultez le développeur Android [utilise-sdk](https://developer.android.com/guide/topics/manifest/uses-sdk-element.html) documentation.


<a name="runtimechecks" />

## <a name="runtime-checks-for-android-versions"></a>Runtime recherche les versions Android

Comme chaque nouvelle version d’Android est publiée, l’API de framework est mis à jour pour fournir à nouveau ou la fonctionnalité de remplacement. À quelques exceptions près, les fonctionnalités de l’API à partir de versions antérieures d’Android sont transférée dans les versions Android plus récentes sans modification. Par conséquent, si votre application s’exécute sur un niveau d’API Android particulier, il sera généralement en mesure d’exécuter sur un niveau d’API Android ultérieur sans modification. Mais que se passe-t-il si vous souhaitez également exécuter votre application sur des versions antérieures d’Android ?

Si vous sélectionnez une version minimale d’Android *inférieure* que votre paramètre de Framework cible, certaines API peut-être pas disponibles pour votre application lors de l’exécution. Toutefois, votre application peut toujours s’exécuter sur un appareil antérieures, mais avec des fonctionnalités réduites. Pour chaque API n’est pas disponible sur les plateformes Android correspondant à votre paramètre de version minimale d’Android, votre code doit vérifier explicitement la valeur de la `Android.OS.Build.VERSION.SdkInt` propriété afin de déterminer le niveau de l’API de la plateforme de l’application est en cours d’exécution. Si le niveau de l’API est *inférieure* à la version minimale d’Android qui prend en charge de l’API que vous souhaitez appeler, puis votre code doit trouver un moyen pour fonctionner correctement sans effectuer cet appel d’API.

Par exemple, supposons que nous souhaitons utiliser la [NotificationBuilder.SetCategory](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetCategory/p/System.String/) méthode pour classer une notification lors de l’exécution **Android 5.0 Lollipop** (et versions ultérieures), mais nous voulons toujours que notre application pour exécuter sur les versions antérieures d’Android comme **Android 4.1 Jelly Bean** (où `SetCategory` n’est pas disponible). Faisant référence à la table des versions Android au début de ce guide, nous voyons que le code de version de build pour **Android 5.0 Lollipop** est `Android.OS.BuildVersionCodes.Lollipop`. Pour prendre en charge les versions antérieures de l’emplacement où Android `SetCategory` est non disponible, notre code peut détecter le niveau de l’API lors de l’exécution et appeler de manière conditionnelle `SetCategory` uniquement lorsque le niveau d’API est supérieur ou égal au code de version de build (lollipop) :

```csharp
if (Android.OS.Build.VERSION.SdkInt >= Android.OS.BuildVersionCodes.Lollipop)
{
    builder.SetCategory(Notification.CategoryEmail);
}
```

Dans cet exemple, Target Framework de notre application est défini sur **Android 5.0 (API niveau 21)** et sa version minimale d’Android est définie sur **Android 4.1 (niveau API 16)**. Étant donné que `SetCategory` est disponible dans le niveau d’API `Android.OS.BuildVersionCodes.Lollipop` et versions ultérieures, cet exemple de code appellera `SetCategory` uniquement lorsqu’il est réellement disponible &ndash; il sera *pas* tentative d’appel `SetCategory` lors de l’API niveau est 16, 17, 18, 19 ou 20. La fonctionnalité est réduite sur ces versions Android précédemment uniquement dans la mesure où notifications ne sont pas triées correctement (car ils ne sont pas classés par type), mais les notifications sont toujours publiées pour avertir l’utilisateur. Notre application fonctionne toujours, mais sa fonctionnalité est légèrement moindre.

En règle générale, le contrôle de version de build permet à votre code de décider lors de l’exécution entre la nouvelle méthode par rapport à l’ancienne méthode de faire quelque chose. Exemple :

```csharp
if (Android.OS.Build.VERSION.SdkInt >= Android.OS.BuildVersionCodes.Lollipop)
{
    // Do things the Lollipop way
}
else
{
    // Do things the pre-Lollipop way
}
```

Il n’existe aucune règle simple et rapide qui explique comment réduire ou de modifier les fonctionnalités de votre application lorsqu’elle s’exécute sur les versions antérieures d’Android qui manquent à une ou plusieurs API. Dans certains cas (comme dans le `SetCategory` exemple ci-dessus), il suffit d’omettre l’appel d’API lorsqu’il n’est pas disponible. Toutefois, dans d’autres cas, vous devrez peut-être implémenter d’autres fonctionnalités quand `Android.OS.Build.VERSION.SdkInt` est détecté comme étant inférieure à l’API au niveau que votre application a besoin présenter son expérience optimale.

<a name="libraries" />

## <a name="api-levels-and-libraries"></a>Bibliothèques et des niveaux d’API

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Lorsque vous créez un projet de bibliothèque de Xamarin.Android (par exemple, une bibliothèque de classes ou une bibliothèque de liaisons), vous pouvez configurer uniquement le paramètre du Framework cible &ndash; la version minimale d’Android et les paramètres de version Android cible ne sont pas disponibles. C’est, car il n’est pas **manifeste Android** page :

[![La compilation à l’aide d’option Android version est disponible](android-api-levels-images/vs-library-options-sml.png)](android-api-levels-images/vs-library-options.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

Lorsque vous créez un projet de bibliothèque de Xamarin.Android, il existe aucune **Application Android** page où vous pouvez configurer la version minimale d’Android et la version Android cible &ndash; la version minimale d’Android et la cible Paramètres de la version d’Android ne sont pas disponibles.
Qui est, car il n’est pas **Générer > Application Android** page :

[![Page Général sans options de version minimale et cible de build](android-api-levels-images/xs-library-options-sml.png)](android-api-levels-images/xs-library-options.png#lightbox)

-----

La version minimale d’Android et les paramètres de la version Android cible ne sont pas disponibles, car la bibliothèque qui en résulte n’est pas une application autonome &ndash; la bibliothèque peut être exécutée sur n’importe quelle version d’Android, en fonction de l’application à laquelle il est fourni avec. Vous pouvez spécifier la manière dont la bibliothèque doit être *compilé*, mais vous ne pouvez pas prédire le niveau d’API plateforme sur laquelle la bibliothèque est exécutée sur. Dans cet esprit, les meilleures pratiques suivantes doivent être respectées lors de la consommation ou de création de bibliothèques :

-   **Lors de l’utilisation d’une bibliothèque Android** &ndash; si vous consommez une bibliothèque Android dans votre application, veillez à définir le Framework cible de votre application paramètre à une API au niveau *au moins aussi élevée que* la cible Paramètre du Framework de la bibliothèque.

-   **Lors de la création d’une bibliothèque Android** &ndash; si vous créez une bibliothèque Android pour une utilisation par d’autres applications, veillez à définir son Framework cible au niveau d’API minimal dont il a besoin pour compiler.

Ces méthodes sont recommandées pour éviter la situation où une bibliothèque tente d’appeler une API qui n’est pas disponible lors de l’exécution (ce qui peut entraîner l’application se bloque). Si vous êtes un développeur de bibliothèque, vous devez vous efforcer limiter votre utilisation d’appels d’API à un sous-ensemble de petite et bien établi de la zone de surface d’API totale. Ainsi, pour vous assurer que votre bibliothèque peut être utilisée en toute sécurité entre les versions de plage de Android un plus large.


## <a name="summary"></a>Récapitulatif

Ce guide a expliqué comment les niveaux d’API Android permet de gérer la compatibilité des applications entre différentes versions d’Android. Il fourni des instructions détaillées pour la configuration de la Xamarin.Android *Framework cible*, *version minimale d’Android*, et *version Android cible* paramètres du projet. Il fourni des instructions pour l’utilisation du gestionnaire Android SDK pour installer les packages du Kit de développement logiciel, inclus des exemples montrant comment écrire du code pour gérer différents niveaux d’API lors de l’exécution et a expliqué comment gérer les niveaux d’API pendant la création ou l’utilisation des bibliothèques Android. Elle également une liste complète qui lie les niveaux d’API pour les numéros de version Android (par exemple, Android 4.4), les noms de version d’Android (par exemple, Kitkat) et les codes de version de build de Xamarin.Android.


## <a name="related-links"></a>Liens associés

- [Configuration du kit Android SDK](~/android/get-started/installation/android-sdk.md)
- [Outils SDK CLI change](~/android/troubleshooting/sdk-cli-tooling-changes.md)
- [Choisir votre compileSdkVersion, minSdkVersion et targetSdkVersion](https://medium.com/google-developers/picking-your-compilesdkversion-minsdkversion-targetsdkversion-a098a0341ebd)
- [Qu’est le niveau d’API ?](https://developer.android.com/guide/topics/manifest/uses-sdk-element.html#ApiLevels)
- [Noms, les balises et les numéros de Build](https://source.android.com/source/build-numbers)
