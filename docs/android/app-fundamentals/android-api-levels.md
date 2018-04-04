---
title: Présentation des niveaux d’API Android
description: Xamarin.Android a plusieurs paramètres de niveau API Android qui déterminent la compatibilité de votre application avec plusieurs versions d’Android. Ce guide explique la signifient de ces paramètres, comment les configurer et effet qu’ils ont sur votre application en cours d’exécution.
ms.prod: xamarin
ms.assetid: 58CB7B34-3140-4BEB-BE2E-209928C1878C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 8f284fefd260764c6f09d78d2518bfd115782cd2
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="understanding-android-api-levels"></a>Présentation des niveaux d’API Android

_Xamarin.Android a plusieurs paramètres de niveau API Android qui déterminent la compatibilité de votre application avec plusieurs versions d’Android. Ce guide explique la signifient de ces paramètres, comment les configurer et effet qu’ils ont sur votre application en cours d’exécution._


## <a name="quick-start"></a>Démarrage rapide

Xamarin.Android expose trois paramètres de projet au niveau d’API Android :

-   [Infrastructure cible](#framework) &ndash; spécifie quelle version de .NET framework à utiliser lors de la génération de votre application. Ce niveau de l’API est utilisé au *compiler* en Xamarin.Android.

-   [La Version minimale Android](#minimum) &ndash; spécifie la version la plus ancienne Android que vous voulez que votre application pour prendre en charge. Ce niveau de l’API est utilisé au *exécuter* temps par Android.

-   [Cibler la Version Android](#target) &ndash; spécifie la version d’Android que votre application est destinée à s’exécuter. Ce niveau de l’API est utilisé au *exécuter* temps par Android.

Avant de pouvoir configurer un niveau de l’API pour votre projet, vous devez installer les composants de plateforme de kit de développement logiciel pour le niveau d’API. Pour plus d’informations sur le téléchargement et installation des composants du SDK Android, consultez [le programme d’installation du Kit de développement logiciel Android](~/android/get-started/installation/android-sdk.md).

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

En règle générale, les trois niveaux d’API de Xamarin.Android sont définies sur la même valeur. Sur le **Application** , définissez **compiler avec la version Android (Framework cible)** vers la dernière version d’API stable (ou, au minimum, la version Android qui a toutes les fonctionnalités que vous avez besoin).
Dans la capture d’écran suivante, le Framework cible est défini sur **7.1 Android (API niveau 25 - commande)**:

[![Cibler les valeurs par défaut de la version Framework compiler avec la version Android](android-api-levels-images/vs-defaults-sml.png)](android-api-levels-images/vs-defaults.png#lightbox)

Sur le **manifeste Android** page, définissez la version minimale Android **utilisez compiler à l’aide de la version du Kit de développement logiciel** et définissez la version cible Android sur la même valeur que la version cible de .NET Framework (dans l’exemple suivant capture d’écran, l’infrastructure Android cible a la valeur **7.1 Android (commande)**) :

[![Les versions minimale et cible Android définissent sur la version de Framework cible](android-api-levels-images/vs-manifest-defaults-sml.png)](android-api-levels-images/vs-manifest-defaults.png#lightbox)

Si vous souhaitez assurer la compatibilité descendante avec une version antérieure d’Android, définissez **version minimale Android à cibler** la version la plus ancienne d’Android que vous voulez que votre application pour prendre en charge. (Notez que le niveau d’API minimal requis pour API niveau 14 [services Google Play et prise en charge Firebase](https://android-developers.googleblog.com/2016/11/google-play-services-and-firebase-for-android-will-support-api-level-14-at-minimum.html).) L’exemple de configuration suivant prend en charge des versions Android à partir de l’API de niveau 14, via des API niveau 25 :

[![Compilez à l’aide du niveau de l’API 25 commande, la version minimale Android définie au niveau de l’API 14](android-api-levels-images/vs-minimum-sml.png)](android-api-levels-images/vs-minimum.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

En règle générale, les trois niveaux d’API de Xamarin.Android sont définies sur la même valeur. Définissez **framework cible** vers la dernière version d’API stable (ou, au minimum, la version Android qui a toutes les fonctionnalités que vous avez besoin). Pour définir le **framework cible**, accédez à **Générer > Général** dans les **Options du projet**. Dans la capture d’écran suivante, le Framework cible est défini sur **utiliser la dernière plateforme installée (8.0)**:

[![Framework cible par défaut à utiliser dernière installé l’appel](android-api-levels-images/xs-default-target-sml.png)](android-api-levels-images/xs-default-target.png#lightbox)

Les paramètres de version minimale et la cible Android peuvent être trouvées sous **Générer > Application Android** dans **Options du projet**. Définissez la version minimale Android **automatique - version du framework cible utilisez** et définissez la version cible Android sur la même valeur que la version cible de .NET Framework. Dans la capture d’écran suivante, le Framework Android cible est défini sur **8.0 Android (API niveau 26)** correspondant au paramètre de cible de .NET Framework ci-dessus :

[![Définir les niveaux de la cible et de l’infrastructure dans les Options du projet](android-api-levels-images/xs-default-app-sml.png)](android-api-levels-images/xs-default-app.png#lightbox)

Si vous souhaitez assurer la compatibilité descendante avec une version antérieure d’Android, modifiez **version minimale Android** la version la plus ancienne d’Android que vous voulez que votre application pour prendre en charge. Notez que le niveau d’API minimal requis pour API niveau 14 [services Google Play et prise en charge Firebase](https://android-developers.googleblog.com/2016/11/google-play-services-and-firebase-for-android-will-support-api-level-14-at-minimum.html).
Par exemple, la configuration suivante prend en charge les versions d’Android dès que le niveau d’API 14 :

[![Au minimum et cibler des versions définies sur automatique - utiliser la version du framework cible](android-api-levels-images/xs-minimum-sml.png)](android-api-levels-images/xs-minimum.png#lightbox)

-----


Si votre application prend en charge plusieurs versions d’Android, votre code doit inclure des vérifications à l’exécution pour vous assurer que votre application fonctionne avec le paramètre de version minimale Android (consultez [Runtime recherche les Versions d’Android](#runtimechecks) ci-dessous pour plus d’informations). Si vous consommation ou création d’une bibliothèque, consultez [niveaux d’API et les bibliothèques](#libraries) ci-dessous pour les meilleures pratiques de configuration des API de niveau paramètres pour les bibliothèques.



## <a name="android-versions-and-api-levels"></a>Les Versions d’Android et niveaux d’API

Comme la plateforme Android évolue et les nouvelles versions d’Android sont libérées, chaque version d’Android est attribuée à un identificateur entier unique, appelé le *niveau de l’API*. Par conséquent, chaque version d’Android correspond à un seul niveau d’API Android. Étant donné que les utilisateurs installent les applications sur des versions antérieures ainsi que la plus récentes d’Android, les applications Android doivent être conçues pour fonctionner avec plusieurs niveaux d’API Android.


### <a name="android-versions"></a>Versions d’Android

Chaque version d’Android passe par plusieurs noms de :

-   La version Android, tel que **7.1 Android**
-   Un nom, un code tel que _commande_
-   Niveau d’une API correspondante, tel que **niveau d’API 25**

Un nom de code Android peut correspondre à plusieurs versions et les niveaux d’API (comme indiqué dans la liste ci-dessous), mais chaque version d’Android correspond à un seul niveau de l’API.

De plus, définit les Xamarin.Android *générer des codes version* qui correspondent aux niveaux d’API Android connus. La liste suivante peut vous aider à les convertir entre le niveau de l’API, version d’Android, nom de code et code de version de build Xamarin.Android.

-   **API 27 (8.1 Android)** &ndash; _Oreo_publiée en décembre 2017. Générer du code de version `Android.OS.BuildVersionCodes.OMr1`

-   **API 26 (Android 8.0)** &ndash; _Oreo_publiée en août 2017. Générer du code de version `Android.OS.BuildVersionCodes.O`

-   **API 25 (Android 7.1)** &ndash; _commande_publiée en décembre 2016. Générer du code de version `Android.OS.BuildVersionCodes.NMr1`

-   **API 24 (Android 7.0)** &ndash; _commande_publiée en août 2016. Générer du code de version `Android.OS.BuildVersionCodes.N`

-   **API 23 (Android 6.0)** &ndash; _Marshmallow_publiée en août 2015. Générer du code de version `Android.OS.BuildVersionCodes.M`

-   **API 22 (Android 5.1)** &ndash; _Lollipop_publiée mars 2015. Générer du code de version `Android.OS.BuildVersionCodes.LollipopMr1`

-   **API 21 (Android 5.0)** &ndash; _Lollipop_publiée en novembre 2014. Générer du code de version `Android.OS.BuildVersionCodes.Lollipop`

-   **API 20 (Android 4.4W)** &ndash; _Kitkat espion_publiée en juin 2014. Générer du code de version `Android.OS.BuildVersionCodes.KitKatWatch`

-   **API 19 (Android 4.4)** &ndash; _Kitkat_publiée en octobre 2013. Générer du code de version `Android.OS.BuildVersionCodes.KitKat`

-   **API 18 (Android 4.3)** &ndash; _gelée Bean_publiée en juillet 2013. Générer du code de version `Android.OS.BuildVersionCodes.JellyBeanMr2`

-   **API 17 (Android 4.2-4.2.2)** &ndash; _gelée Bean_publiée en novembre 2012. Générer du code de version `Android.OS.BuildVersionCodes.JellyBeanMr1`

-   **API 16 (Android 4.1-4.1.1)** &ndash; _gelée Bean_publiée en juin 2012. Générer du code de version `Android.OS.BuildVersionCodes.JellyBean`

-   **API 15 (Android 4.0.3-4.0.4)** &ndash; _glace Sandwich_publiée en décembre 2011. Générer du code de version `Android.OS.BuildVersionCodes.IceCreamSandwichMr1`

-   **API 14 (Android 4.0-4.0.2)** &ndash; _glace Sandwich_publiée en octobre 2011. Générer du code de version `Android.OS.BuildVersionCodes.IceCreamSandwich`

-   **API 13 (Android 3.2)** &ndash; _nid_publiée en juin 2011. Générer du code de version `Android.OS.BuildVersionCodes.HoneyCombMr2`

-   **API 12 (Android 3.1.x)** &ndash; _nid_publiée en mai 2011. Générer du code de version `Android.OS.BuildVersionCodes.HoneyCombMr1`

-   **API 11 (Android 3.0.x)** &ndash; _nid_publiée en février 2011. Générer du code de version `Android.OS.BuildVersionCodes.HoneyComb`

-   **API 10 (Android 2.3.3-2.3.4)** &ndash; _personnage en pain_publiée en février 2011. Générer du code de version `Android.OS.BuildVersionCodes.GingerBreadMr1`

-   **API 9 (Android 2.3-2.3.2)** &ndash; _personnage en pain_publiée en novembre 2010. Générer du code de version `Android.OS.BuildVersionCodes.GingerBread`

-   **API 8 (Android 2.2.x)** &ndash; _Froyo_publiée en juin 2010. Générer du code de version `Android.OS.BuildVersionCodes.Froyo`

-   **API 7 (Android 2.1.x)** &ndash; _Eclair_publiée en janvier 2010. Générer du code de version `Android.OS.BuildVersionCodes.EclairMr1`

-   **API 6 (Android 2.0.1)** &ndash; _Eclair_publiée en décembre 2009. Générer du code de version `Android.OS.BuildVersionCodes.Eclair01`

-   **API 5 (Android 2.0)** &ndash; _Eclair_publiée en novembre 2009. Générer du code de version `Android.OS.BuildVersionCodes.Eclair`

-   **API 4 (Android 1.6)** &ndash; _anneau_publiée en septembre 2009. Générer du code de version `Android.OS.BuildVersionCodes.Donut`

-   **API 3 (Android 1.5)** &ndash; _petit gâteau_publiée en mai 2009. Générer du code de version `Android.OS.BuildVersionCodes.Cupcake`

-   **API 2 (Android 1.1)** &ndash; _Base_publiée en février 2009. Générer du code de version `Android.OS.BuildVersionCodes.Base11`

-   **API 1 (Android 1.0)** &ndash; _Base_publiée en octobre 2008. Générer du code de version `Android.OS.BuildVersionCodes.Base`


Comme cette liste indique, nouvelles Android versions fréquemment &ndash; parfois plusieurs versions d’un an. Par conséquent, l’univers des appareils Android qui peuvent s’exécuter votre application inclut un grand nombre de versions anciennes et récentes d’Android. Comment vous assurer que votre application s’exécutera manière cohérente et fiable sur un grand nombre de versions différentes d’Android ? Niveaux d’API d’Android peuvent vous aider à gérer ce problème.


### <a name="android-api-levels"></a>Niveaux d’API Android

Chaque appareil Android s’exécute à exactement *un* niveau de l’API &ndash; ce niveau de l’API est garanti être unique pour chaque version de la plateforme Android. Niveau de l’API permettent d’identifier précisément la version de l’ensemble d’API qui peut appeler votre application ; Il identifie la combinaison d’éléments de manifeste, des autorisations, etc. code par rapport à en tant que développeur. Système d’Android de niveaux d’API permet de déterminer si une application est compatible avec une image système Android avant d’installer l’application sur un appareil Android.

Quand une application est générée, elle contient les informations de niveau API suivantes :

-   Le *cible* niveau de l’API d’Android l’application est conçue pour fonctionner sur.

-   Le *minimale* niveau d’API Android dont un appareil Android doit disposer pour exécuter votre application. 

Ces paramètres sont utilisés pour s’assurer que les fonctionnalités nécessaires pour exécuter l’application correctement sont disponible sur l’appareil Android au moment de l’installation. Si ce n’est pas le cas, l’application en cours d’exécution sur l’appareil est bloquée. Par exemple, si le niveau de l’API d’un appareil Android est inférieur au niveau de l’API minimal que vous spécifiez pour votre application, l’appareil Android empêchera l’utilisateur à partir de l’installation de votre application.


## <a name="project-api-level-settings"></a>Paramètres de niveau de l’API de projet

Les sections suivantes expliquent comment utiliser le Gestionnaire du SDK pour préparer votre environnement de développement pour les niveaux d’API que vous souhaitez cibler, suivie des explications détaillées de la configuration *Framework cible*, *Minimum Android version*, et *cible Android version* paramètres dans Xamarin.Android.


### <a name="android-sdk-platforms"></a>Plateformes Android SDK

Avant de pouvoir sélectionner un niveau cible ou les API Minimum de Xamarin.Android, vous devez installer la version de la plateforme du kit SDK Android qui correspond à ce niveau de l’API. La plage des choix disponibles pour la cible de .NET Framework, version minimale Android et la version cible Android est limitée aux plage de Android versions du SDK que vous avez installée. Vous pouvez utiliser le Gestionnaire de kit de développement logiciel pour vérifier que les versions du SDK Android requis sont installées et que vous pouvez l’utiliser pour ajouter les nouveaux niveaux d’API dont vous avez besoin pour votre application. Si vous n’êtes pas familiarisé avec l’installation de niveaux d’API, consultez [le programme d’installation du Kit de développement logiciel Android](~/android/get-started/installation/android-sdk.md).

<a name="framework" />

### <a name="target-framework"></a>Framework cible

Le *Framework cible* (également appelé `compileSdkVersion`) est la version de framework Android spécifique (API niveau) que votre application est compilée pour au moment de la génération. Ce paramètre spécifie lesquels API de votre application *attend* à utiliser lors de son exécution, mais il n’a aucun effet sur lequel l’API sont réellement disponibles à votre application lorsqu’il est installé. Par conséquent, la modification du paramètre de la cible de .NET Framework ne modifie pas le comportement d’exécution.

Le Framework cible identifie les versions de bibliothèque que votre application est liée aux &ndash; ce paramètre détermine lesquelles API que vous pouvez utiliser dans votre application. Par exemple, si vous souhaitez utiliser le [NotificationBuilder.SetCategory](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetCategory/p/System.String/) méthode qui a été introduit dans l’interface Lollipop 5.0 Android, vous devez définir le Framework cible **API niveau 21 (Lollipop)** ou version ultérieure. Si vous définissez Framework cible de votre projet à une API telle que **API niveau 19 (KitKat)** et essayez d’appeler le `SetCategory` méthode dans votre code, vous obtiendrez une erreur de compilation.

Nous vous conseillons de toujours compiler avec la *dernière* version cible de .NET Framework disponible. Cela vous offre des messages d’avertissement utiles pour toutes les API déconseillées qui peuvent être appelés par votre code. À l’aide de la dernière version du Framework cible est particulièrement important lorsque vous utilisez les dernières versions de bibliothèque de prise en charge &ndash; chaque bibliothèque attend votre application doit être compilée au niveau d’API minimal de cette bibliothèque de prise en charge ou supérieur. 

> [!NOTE]
> À compter d’août 2018, la Console de lecture de Google nécessite que de nouvelles applications ciblent de niveau de l’API 26 (8.0 Android) ou une version ultérieure.
Les applications existantes seront requis pour cibler le niveau de l’API 26 ou supérieur à compter de novembre 2018. Pour plus d’informations, consultez [amélioration des performances sur Google Play pour les années à venir et sécurité de l’application](https://android-developers.googleblog.com/2017/12/improving-app-security-and-performance.html).


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Pour accéder au paramètre cible de .NET Framework dans Visual Studio, ouvrez les propriétés du projet dans **l’Explorateur de solutions** et sélectionnez le **Application** page :

[![Page de propriétés de projet d’application](android-api-levels-images/vs-target-framework-sml.png)](android-api-levels-images/vs-target-framework.png#lightbox)

Définir le Framework cible en sélectionnant un niveau de l’API dans le menu déroulant sous **compiler avec la version Android** comme indiqué ci-dessus.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Pour accéder au paramètre cible de .NET Framework dans Visual Studio pour Mac, cliquez sur le nom du projet et sélectionnez **Options**; il ouvre le **Options du projet** boîte de dialogue. Dans cette boîte de dialogue, accédez à **Générer > Général** comme indiqué ici :

[![Générer la section Général de la page Options du projet](android-api-levels-images/xs-target-framework-sml.png)](android-api-levels-images/xs-target-framework.png#lightbox)

Définir le Framework cible en sélectionnant un niveau de l’API dans le menu déroulant situé à droite de **framework cible** comme indiqué ci-dessus.

-----


<a name="minimum" />

### <a name="minimum-android-version"></a>Version minimale Android

Le *version minimale Android* (également appelé `minSdkVersion`) est la version la plus ancienne du système d’exploitation Android (autrement dit, le plus bas niveau de l’API) qui peut installer et exécuter votre application. Par défaut, une application peut uniquement être installé sur les appareils qui ne correspond au paramètre de la cible de .NET Framework ou ultérieure ; Si le paramètre de version minimale Android est *inférieure* à celle du paramètre de la cible de .NET Framework, votre application peut également s’exécuter sur les versions antérieures d’Android. Par exemple, si vous définissez le Framework cible **7.1 Android (commande)** et définissez la version minimale Android **Android 4.0.3 (glace Sandwich)**, votre application peut être installée sur n’importe quelle plateforme à partir de l’API niveau 15 au niveau de l’API 25, inclus.

Bien que votre application peut correctement générer et installer sur cette plage de plateformes, cela ne garantit pas qu’il sera correctement *exécuter* sur l’ensemble de ces plateformes. Par exemple, si votre application est installée sur **Android 5.0 (Lollipop)** et votre code appelle une API qui est disponible uniquement dans **7.1 Android (commande)** et les versions ultérieures, votre application obtient une erreur d’exécution et éventuellement se bloquer. Par conséquent, votre code doit vérifier &ndash; lors de l’exécution &ndash; qu’elle appelle des seules API prises en charge par le périphérique Android elle s’exécute sur. En d’autres termes, votre code doit inclure des vérifications à l’exécution explicite pour vous assurer que votre application utilise des API plus récente uniquement sur les appareils qui sont assez récentes pour prendre en charge.
[Runtime recherche les Versions d’Android](#runtimechecks), plus loin dans ce guide, explique comment ajouter ces vérifications à l’exécution de votre code.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Pour accéder au paramètre de version minimale Android dans Visual Studio, ouvrez les propriétés du projet dans **l’Explorateur de solutions** et sélectionnez le **manifeste Android** page. Dans le menu déroulant sous **version minimale Android** vous pouvez sélectionner la version minimale Android pour votre application :

[![Android minimale pour l’option cible de la valeur compiler à l’aide de la version du Kit de développement logiciel](android-api-levels-images/vs-minimum-version-sml.png)](android-api-levels-images/vs-minimum-version.png#lightbox)

Si vous sélectionnez **utilisez compiler à l’aide de la version du Kit de développement logiciel**, la version minimale Android doit être le même que le paramètre cible de .NET Framework.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Pour accéder au paramètre cible de .NET Framework dans Visual Studio pour Mac, cliquez sur le nom du projet et sélectionnez **Options**; il ouvre le **Options du projet** boîte de dialogue. Accédez à **Générer > Application Android**.
À l’aide de la liste déroulante à droite de **version minimale Android**, vous pouvez définir la version minimale Android pour votre application :

[![La version minimale Android défini sur automatique - utilisation version du framework cible](android-api-levels-images/xs-minimum-version-sml.png)](android-api-levels-images/xs-minimum-version.png#lightbox)

Si vous sélectionnez **automatique &ndash; utiliser la version du framework cible**, la version minimale Android doit être le même que le paramètre cible de .NET Framework.

-----


<a name="target" />

### <a name="target-android-version"></a>Version de la cible Android

Le *Version Android cible* (également appelé `targetSdkVersion`) est l’API de niveau de l’appareil Android où l’application s’attend à exécuter. Android utilise ce paramètre pour déterminer s’il faut activer les comportements de compatibilité &ndash; Cela garantit que votre application continue à fonctionner comme vous le souhaitez. Android utilise le paramètre de version cible Android de votre application pour déterminer les modifications de comportement peuvent être appliquées à votre application sans rupture (il s’agit comment Android fournit une compatibilité ascendante).

Le Framework cible et la version cible Android, tout en ayant des noms similaires, ne sont pas identiques. Le paramètre cible de .NET Framework communique des informations de niveau API cible à Xamarin.Android à un usage *compilation*, tandis que la version cible Android communique des informations de niveau API cible pour Android à un usage  *durée d’exécution* (quand l’application est installée et en cours d’exécution sur un appareil).

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Pour accéder à ce paramètre dans Visual Studio, ouvrez les propriétés du projet dans **l’Explorateur de solutions** et sélectionnez le **manifeste Android** page. Dans le menu déroulant sous **cible Android version** vous pouvez sélectionner la version cible Android pour votre application :

[![Version d’Android cible définie pour compiler à l’aide de la version du Kit de développement logiciel](android-api-levels-images/vs-target-version-sml.png)](android-api-levels-images/vs-target-version.png#lightbox)

Nous vous recommandons de définir explicitement la version cible Android vers la dernière version d’Android qui vous permet de tester votre application. Dans l’idéal, il doit être défini pour la dernière version du SDK Android &ndash; cela vous permet d’utiliser les nouvelles API avant le travail sur les modifications de comportement. Pour la plupart des développeurs, nous *pas* vous recommandons de définir la version cible Android **utilisez compiler à l’aide de la version du Kit de développement logiciel**.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Pour accéder au paramètre cible de .NET Framework dans Visual Studio pour Mac, cliquez sur le nom du projet et sélectionnez **Options**; il ouvre le **Options du projet** boîte de dialogue. Accédez à **Générer > Application Android**.
À l’aide de la liste déroulante à droite de **cible Android version**, vous pouvez définir la version Android cible pour votre application :

[![Version d’Android cible défini sur automatique - utilisation version du framework cible](android-api-levels-images/xs-target-version-sml.png)](android-api-levels-images/xs-target-version.png#lightbox)

Nous vous recommandons de définir explicitement la version cible Android vers la dernière version d’Android qui vous permet de tester votre application. Dans l’idéal, il doit être défini pour la dernière version du SDK Android disponible &ndash; cela vous permet d’utiliser les nouvelles API avant le travail sur les modifications de comportement. Pour la plupart des développeurs, nous ne recommandons pas dans la définition de la version cible Android sur **automatique - version du framework cible utilisation**.

-----

En général, la Version Android cible doit être délimitée par la Version Android Minimum et le Framework cible. Plus précisément :

**La Version minimale Android < = Version d’Android cible < = cible de .NET Framework**

Pour plus d’informations sur les niveaux de kit de développement logiciel, consultez le développeur Android [utilise-sdk](https://developer.android.com/guide/topics/manifest/uses-sdk-element.html) documentation.


<a name="runtimechecks" />

## <a name="runtime-checks-for-android-versions"></a>Vérifications à l’exécution pour les Versions d’Android

Comme chaque nouvelle version d’Android est publiée, l’infrastructure API est mise à jour pour fournir à nouveau ou la fonctionnalité de remplacement. À quelques exceptions près, les fonctionnalités d’API à partir de versions antérieures d’Android sont transmise dans les versions plus récentes d’Android sans modification. Par conséquent, si votre application s’exécute sur un niveau d’API Android particulier, il peut généralement à s’exécuter sur un niveau d’API Android ultérieure sans modification. Mais que se passe-t-il si vous souhaitez également exécuter votre application sur des versions antérieures d’Android ?

Si vous sélectionnez une version minimale Android *inférieure* à votre paramètre cible de .NET Framework, certaines API peut-être pas disponible pour votre application lors de l’exécution. Toutefois, votre application peut toujours s’exécuter sur un périphérique antérieur, mais avec des fonctionnalités réduites. Pour chaque API n’est pas disponible sur les plateformes Android correspondant à votre paramètre de version minimale Android, votre code doit explicitement vérifier la valeur de la `Android.OS.Build.VERSION.SdkInt` propriété pour déterminer le niveau de l’API de la plateforme de l’application est en cours d’exécution. Si le niveau de l’API est *inférieure* à la version minimale Android qui prend en charge l’API que vous souhaitez appeler, puis votre code doit trouver un moyen pour fonctionner correctement sans effectuer cet appel d’API.

Par exemple, supposons que vous voulez utiliser le [NotificationBuilder.SetCategory](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetCategory/p/System.String/) méthode pour classer une notification lors de l’exécution **Android 5.0 Lollipop** (et versions ultérieures), mais nous voulons toujours que notre application pour exécutez sur des versions antérieures d’Android comme **Bean de gelée Android 4.1** (où `SetCategory` n’est pas disponible). Faisant référence à la table de la version Android au début de ce guide, nous constatons que le code de version de build pour **Android 5.0 Lollipop** est `Android.OS.BuildVersionCodes.Lollipop`. Pour prendre en charge des versions antérieures de where Android `SetCategory` est non disponible, notre code peut détecter le niveau de l’API lors de l’exécution et appeler de manière conditionnelle `SetCategory` uniquement lorsque le niveau de l’API est supérieur ou égal au code de version de build Lollipop :

```csharp
if (Android.OS.Build.VERSION.SdkInt >= Android.OS.BuildVersionCodes.Lollipop)
{
    builder.SetCategory(Notification.CategoryEmail);
}
```

Dans cet exemple, cible Framework de notre application a la valeur **Android 5.0 (API niveau 21)** et sa version minimale Android est définie sur **Android 4.1 (API niveau 16)**. Étant donné que `SetCategory` est disponible dans le niveau de l’API `Android.OS.BuildVersionCodes.Lollipop` et versions ultérieures, cet exemple de code appelle `SetCategory` uniquement lorsqu’il est réellement disponible &ndash; il sera *pas* essaie d’appeler `SetCategory` lors de l’API niveau est 16, 17, 18, 19 ou 20. La fonctionnalité est réduite sur ces versions d’Android antérieures uniquement dans la mesure où les notifications ne sont pas triées correctement (car ils ne sont pas classés par type), mais les notifications sont toujours publiées pour avertir l’utilisateur. Notre application fonctionne toujours, mais ses fonctionnalités sont moindre légèrement.

En général, la vérification de version de build permet à votre code de décider lors de l’exécution entre la nouvelle méthode par rapport à l’ancienne méthode de faire quelque chose. Par exemple :

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

Il n’existe aucune règle simple et rapide qui explique comment réduire ou de modifier les fonctionnalités de votre application lorsqu’elle s’exécute sur les anciennes versions d’Android qui manquent à une ou plusieurs API. Dans certains cas (comme dans le `SetCategory` exemple ci-dessus), il suffit d’omettre l’appel d’API lorsqu’il n’est pas disponible. Toutefois, dans d’autres cas, vous devrez peut-être implémenter d’autres fonctionnalités lorsque `Android.OS.Build.VERSION.SdkInt` est détecté comme étant inférieure à l’API de niveau que votre application doit présenter son expérience optimale.

<a name="libraries" />

## <a name="api-levels-and-libraries"></a>Niveaux d’API et de bibliothèques

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Lorsque vous créez un projet de bibliothèque de Xamarin.Android (par exemple, une bibliothèque de classes ou une bibliothèque de liaisons), vous pouvez configurer uniquement le paramètre du Framework cible &ndash; la version minimale Android et les paramètres de la version cible Android ne sont pas disponibles. C’est parce qu’il existe aucune **manifeste Android** page :

[![La compilation à l’aide de l’option Android version est disponible](android-api-levels-images/vs-library-options-sml.png)](android-api-levels-images/vs-library-options.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Lorsque vous créez un projet de bibliothèque de Xamarin.Android, il est sans **Application Android** page où vous pouvez configurer la version minimale Android et la version cible Android &ndash; la version minimale Android et la cible Paramètres de la version Android ne sont pas disponibles.
C’est parce qu’il existe aucune **Générer > Application Android** page) :

[![Page Général sans les options de version minimale et la cible de build](android-api-levels-images/xs-library-options-sml.png)](android-api-levels-images/xs-library-options.png#lightbox)

-----

La version minimale Android et les paramètres de la version cible Android ne sont pas disponibles, car la bibliothèque résultante n’est pas une application autonome &ndash; la bibliothèque peut être exécutée sur n’importe quelle version d’Android, en fonction de l’application qu’il est fourni avec. Vous pouvez spécifier comment la bibliothèque doit être *compilé*, mais vous ne pouvez pas prédire quel niveau de l’API de plateforme la bibliothèque est exécutée sur. Dans cet esprit, les meilleures pratiques suivantes doivent être observées lors de la consommation ou de création de bibliothèques :

-   **Lors de l’utilisation d’une bibliothèque Android** &ndash; si vous consommez une bibliothèque Android dans votre application, veillez à définir le Framework cible du votre application paramètre à une API niveau qui est *au moins aussi élevée que* la cible Paramètre de cadre de la bibliothèque.

-   **Lors de la création d’une bibliothèque Android** &ndash; si vous créez une bibliothèque Android pour une utilisation par d’autres applications, veillez à définir son infrastructure cible au niveau de l’API minimal dont il a besoin pour compiler.

Ces méthodes sont recommandées pour éviter la situation où une bibliothèque tente d’appeler une API qui n’est pas disponible lors de l’exécution (ce qui peut entraîner l’application à se bloquer). Si vous êtes un développeur de bibliothèque, vous devez vous efforcer restreindre l’utilisation d’appels d’API à un sous-ensemble bien établi et de petite taille de la zone de surface API totale. Ainsi, pour vous assurer que votre bibliothèque peut être utilisée en toute sécurité entre les versions de plage de Android une plus large.


## <a name="summary"></a>Récapitulatif

Ce guide explique comment les niveaux d’API Android sont utilisés pour gérer la compatibilité des applications sur les différentes versions d’Android. Il fourni des instructions détaillées pour la configuration de la Xamarin.Android *Framework cible*, *version minimale Android*, et *cible Android version* paramètres du projet. Il fourni instructions sur l’utilisation du Gestionnaire de SDK Android pour installer les packages du Kit de développement logiciel, inclus des exemples montrant comment écrire du code pour gérer différents niveaux d’API lors de l’exécution et explique comment gérer les niveaux d’API lors de la création ou de consommation de bibliothèques Android. Il est également d’une liste complète qui lie les niveaux d’API pour les numéros de version d’Android (par exemple, Android 4.4), les noms de version d’Android (par exemple, Kitkat) et les codes de version de build Xamarin.Android.


## <a name="related-links"></a>Liens associés

- [Configuration du kit Android SDK](~/android/get-started/installation/android-sdk.md)
- [Outils du Kit de développement logiciel CLI change](~/android/troubleshooting/sdk-cli-tooling-changes.md)
- [Sélection de votre compileSdkVersion, minSdkVersion et targetSdkVersion](https://medium.com/google-developers/picking-your-compilesdkversion-minsdkversion-targetsdkversion-a098a0341ebd)
- [Qu’est le niveau de l’API ?](http://developer.android.com/guide/topics/manifest/uses-sdk-element.html#ApiLevels)
- [Noms, les balises et les numéros de Build](https://source.android.com/source/build-numbers)
