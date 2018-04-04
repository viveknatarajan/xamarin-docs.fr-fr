---
title: Ressources Android
description: Cet article présente le concept de ressources Android dans Xamarin.Android et allez expliquer comment les utiliser. Elle couvre l’utilisation des ressources dans votre application Android pour prendre en charge la localisation des applications et plusieurs appareils, y compris les densités et tailles d’écran.
ms.prod: xamarin
ms.assetid: C0DCC856-FA36-04CD-443F-68D26075649E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/01/2018
ms.openlocfilehash: 7b6ba9cdc222019bfa2e1cb9a61b54e290e69bba
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="android-resources"></a>Ressources Android

_Cet article présente le concept de ressources Android dans Xamarin.Android et allez expliquer comment les utiliser. Elle couvre l’utilisation des ressources dans votre application Android pour prendre en charge la localisation des applications et plusieurs appareils, y compris les densités et tailles d’écran._


## <a name="overview"></a>Vue d'ensemble

Une application Android est rarement simplement le code source. Il existe souvent des divers autres fichiers qui composent une application : vidéo, des images, des polices et des fichiers audio uniquement à titre d’exemple. Collectivement, ces fichiers de code source non sont appelées ressources et sont compilés (ainsi que le code source) pendant le processus de génération et empaquetés comme un APK de distribution et d’installation sur des périphériques :

![Diagramme d’empaquetage](images/packaging-diagram.png)

Ressources offrent plusieurs avantages pour une application Android :

-  **Séparation de code** &ndash; sépare le code source à partir d’images, chaînes, menus, des animations, couleurs, etc. Par conséquent les ressources peuvent considérablement aider lors de la localisation.

-  **Cibler plusieurs périphériques** &ndash; fournit une prise en charge plus simple des configurations de différents périphériques sans modification du code.

-  **Vérification de la compilation** &ndash; ressources sont statiques et compilé dans l’application. Cela permet l’utilisation des ressources pour être vérifiées au moment de la compilation, quand il sera facile de repérer et de corriger les erreurs, par opposition à l’exécution lorsqu’il est plus difficile de localiser et plus coûteux à corriger.

Lorsqu’un nouveau projet Xamarin.Android est démarré, un répertoire spécial appelé ressources est créé, ainsi que certains ses sous-répertoires :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Contenu et le dossier de ressources](images/resources-folder-vs.png)

Dans l’image ci-dessus, les ressources d’application sont organisées en fonction de leur type dans ces sous-répertoires : images iront dans le **drawable** répertoire ; go dans les vues du **disposition** sous-répertoire, etc.
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

![Contenu et le dossier de ressources](images/resources-folder-xs.png)

Dans l’image ci-dessus, les ressources d’application sont organisées en fonction de leur type dans ces sous-répertoires : images iront dans le **mipmap** répertoire ; go dans les vues du **disposition** sous-répertoire, etc.
 
-----

Il existe deux façons d’accéder à ces ressources dans une application Xamarin.Android : *par programmation* dans le code et *déclarative* dans XML à l’aide d’une syntaxe XML spéciale.

Ces ressources sont appelés *des ressources par défaut* et sont utilisés par tous les appareils, sauf si une correspondance plus spécifique est spécifiée. En outre, chaque type de ressource peut-être éventuellement *autres ressources* que Android peut utiliser pour cibler des appareils spécifiques. Par exemple, les ressources peuvent être fournis pour cibler des paramètres régionaux de l’utilisateur, la taille de l’écran, ou si le périphérique est pivotée de 90 degrés à partir de portrait, paysage, etc. Dans chacun de ces cas, Android chargera les ressources pour une utilisation par l’application sans effort de codage supplémentaire par le développeur.

Ressources de remplacement sont spécifiées en ajoutant une chaîne courte, appelée un *qualificateur*, à la fin du répertoire contenant un type donné de ressources.

Par exemple, **ressources/drawable-de** spécifie les images pour les appareils qui sont définies sur des paramètres régionaux allemands, tandis que **ressources/drawable-fr** contiendrait les images pour les appareils défini sur les paramètres régionaux Français. Un exemple de fournir des ressources de remplacement sont consultables dans l’image ci-dessous où la même application est exécutée avec uniquement les paramètres régionaux de la modification de l’appareil :

![Exemples d’écrans pour différents paramètres régionaux](images/localized-screenshots.png)

Cet article prend une vision détaillée de l’utilisation des ressources et couvre les rubriques suivantes :

-  **Principes de base Android ressource** &ndash; à l’aide de ressources par défaut par programmation et de manière déclarative, ajout de types de ressources telles que les images et les polices à une application.

-  **Configurations spécifiques d’appareil** &ndash; prenant en charge les densités différentes résolutions d’écran dans une application.

-  **Localisation** &ndash; à l’aide des ressources pour prendre en charge les différentes régions, une application peut être utilisée.


## <a name="related-links"></a>Liens associés

- [Utilisation de ressources Android](~/android/app-fundamentals/resources-in-android/android-assets.md)
- [Notions de base des applications](http://developer.android.com/guide/topics/fundamentals.html)
- [Ressources d'application](http://developer.android.com/guide/topics/resources/index.html)
- [Prise en charge de plusieurs écrans](http://developer.android.com/guide/practices/screens_support.html)
