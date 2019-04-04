---
title: Ressources Android
description: Cet article présente le concept de ressources Android dans Xamarin.Android et documentons comment les utiliser. Il explique comment utiliser des ressources dans votre application Android pour prendre en charge la localisation d’applications et plusieurs appareils, y compris les densités et tailles d’écran.
ms.prod: xamarin
ms.assetid: C0DCC856-FA36-04CD-443F-68D26075649E
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/01/2018
ms.openlocfilehash: f14b3fd31fdda200f51f429367465677d389b1ca
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57666355"
---
# <a name="android-resources"></a>Ressources Android

_Cet article présente le concept de ressources Android dans Xamarin.Android et documentons comment les utiliser. Il explique comment utiliser des ressources dans votre application Android pour prendre en charge la localisation d’applications et plusieurs appareils, y compris les densités et tailles d’écran._


## <a name="overview"></a>Vue d'ensemble

Une application Android est rarement simplement le code source. Il existe souvent des nombreux autres fichiers qui composent une application : vidéo, des images, des polices et des fichiers audio n’en citer que quelques-uns. Collectivement, ces fichiers de code non source sont désignés comme des ressources et sont compilés (ainsi que le code source) pendant le processus de génération et présente sous la forme d’un APK pour la distribution et l’installation sur des appareils :

![Diagramme de l’empaquetage](images/packaging-diagram.png)

Ressources présentent plusieurs avantages à une application Android :

-  **Séparation de code** &ndash; sépare le code source à partir d’images, des chaînes, menus, des animations, couleurs, etc. Par conséquent, ressources peuvent aider considérablement lors de la localisation.

-  **Cibler plusieurs appareils** &ndash; fournit une prise en charge plus simple de différentes configurations d’appareils sans modification du code.

-  **Vérification de la compilation** &ndash; ressources sont statiques et compilé dans l’application. Cela permet l’utilisation des ressources à vérifier au moment de la compilation, quand il sera facile de repérer et de corriger les erreurs, par opposition à l’exécution lorsqu’il est plus difficile de localiser et coûteuses à corriger.

Quand un nouveau projet Xamarin.Android est démarré, un répertoire spécial appelé des ressources est créé, ainsi que certains sous-répertoires :

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Contenu et le dossier de ressources](images/resources-folder-vs.png)

Dans l’image ci-dessus, les ressources d’application sont organisées selon leur type dans ces sous-répertoires : images iront dans le **drawable** répertoire ; vues aller dans le **disposition** sous-répertoire, etc.
 
# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

![Contenu et le dossier de ressources](images/resources-folder-xs.png)

Dans l’image ci-dessus, les ressources d’application sont organisées selon leur type dans ces sous-répertoires : images iront dans le **mipmap** répertoire ; vues aller dans le **disposition** sous-répertoire, etc.
 
-----

Il existe deux façons d’accéder à ces ressources dans une application Xamarin.Android : *par programmation* dans le code et *déclarative* dans XML à l’aide d’une syntaxe XML spéciale.

Ces ressources sont appelés *des ressources par défaut* et sont utilisées par tous les appareils, sauf si une correspondance plus spécifique est spécifiée. En outre, chaque type de ressource peut éventuellement avoir *autres ressources* que Android peut utiliser pour cibler des appareils spécifiques. Par exemple, les ressources peuvent être fournis pour cibler les paramètres régionaux de l’utilisateur, la taille d’écran, ou si l’appareil est pivotée de 90 degrés de portrait à paysage, etc. Dans chacun de ces cas, Android chargera les ressources pour une utilisation par l’application sans aucun effort de codage supplémentaire par le développeur.

Autres ressources sont spécifiées en ajoutant une chaîne courte, appelée un *qualificateur*, à la fin du répertoire contenant un type donné de ressources.

Par exemple, **ressources/drawable-de** spécifieront les images pour les appareils qui sont définies sur des paramètres régionaux allemands, tandis que **ressources/drawable-fr** contiendrait des images pour les appareils définis sur les paramètres régionaux Français. Un exemple de fourniture de ressources de remplacement peut être consultée dans l’image ci-dessous où il est en cours d’exécution la même application avec simplement les paramètres régionaux de la modification de l’appareil :

![Exemples d’écrans pour différents paramètres régionaux](images/localized-screenshots.png)

Cet article prennent plus d’informations sur l’utilisation des ressources et couvrent les sujets suivants :

-  **Les principes des ressources Android** &ndash; à l’aide de ressources par défaut par programmation et de manière déclarative, ajout de types de ressources telles que les images et les polices à une application.

-  **Configurations d’appareils spécifiques** &ndash; prenant en charge les densités différentes résolutions d’écran dans une application.

-  **Localisation** &ndash; à l’aide de ressources pour prendre en charge les différentes régions, une application peut être utilisée.


## <a name="related-links"></a>Liens associés

- [Utilisation de ressources Android](~/android/app-fundamentals/resources-in-android/android-assets.md)
- [Notions de base des applications](https://developer.android.com/guide/topics/fundamentals.html)
- [Ressources d'application](https://developer.android.com/guide/topics/resources/index.html)
- [Prise en charge de plusieurs écrans](https://developer.android.com/guide/practices/screens_support.html)
