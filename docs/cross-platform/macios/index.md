---
title: Plateforme Apple (iOS et Mac)
description: Dans cette section nous présente les stratégies pour partager du code entre vos projets Xamarin.iOS et Xamarin.Mac.
ms.prod: xamarin
ms.assetid: 67246203-D78E-4DCC-9E55-7D3D93968E54
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 3c01ff4af699dd0374729b638470d1ef34aa7022
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2018
---
# <a name="apple-platform-ios-and-mac"></a>Plateforme Apple (iOS et Mac)

_Dans cette section nous présente les stratégies pour partager du code entre vos projets Xamarin.iOS et Xamarin.Mac._

## <a name="code-sharing"></a>Partage de code

Pour les éléments de votre code dont aucun élément d’interface utilisateur de la meilleure façon de partager le code entre iOS et Mac est toujours l’utilisation de [bibliothèques de classes portables](~/cross-platform/app-fundamentals/pcl.md).

Pour le code qui doit effectuer certaines tâches d’interface utilisateur et encore, vous souhaitez partager, vous devez utiliser [les projets partagés](~/cross-platform/app-fundamentals/shared-projects.md) vous permettent de placer le code pour le partage dans un projet unique et qu’il compilée avec des Mac et iOS lorsque référencé.

##  <a name="unified-apiunifiedindexmd"></a>[API unifiée](unified/index.md)

L’API unifiée pour iOS et les projets de Mac utilise les mêmes espaces de noms pour les infrastructures afin que le même fichier de code peut être utilisé entre les deux plateformes, transparente de partage de code. Il active également pour les versions 32 et 64 bits. L’API unifiée est la valeur par défaut du modèle depuis 2015 anticipée et est recommandée pour tous les nouveaux projets - *uniquement* les projets API unifiée qui peuvent être envoyées à l’App Store.

### <a name="classic-apis"></a>API classique

> [!NOTE]
> **Désapprobation de profil classique :** que de nouvelles plateformes sont ajoutés dans Xamarin.iOS nous avons commencé à déconseiller progressivement des fonctionnalités à partir du profil classique (monotouch.dll). Par exemple, l’option non-NRC (nouvelle-ref-count) a été supprimée. NRC a toujours été activé pour unifiée de toutes les applications (c'est-à-dire non NRC n’a jamais été une option) et ne présente aucun problème connu. Les versions futures supprime la possibilité d’utiliser Boehm en tant que le garbage collector. Cela était également une option jamais disponible pour les applications unifiées. La suppression complète de la prise en charge classique est planifiée pour automne 2016 avec la version de Xamarin.iOS 10.0.

Les Xamarin.iOS de (non-unifiée) d’origine et Xamarin.Mac APIs apportées partage de code plus difficile, car les infrastructures natifs soit `MonoTouch.` ou `MonoMac.` les préfixes d’espace de noms.  Nous vous avons fourni des espaces de noms vide qui permet aux développeurs de partager du code en ajoutant `using` instructions faisant référence à la fois MonoMac et MonoTouch des espaces de noms sur le même fichier, mais cela a été quelques inconvénients. L’API classique doit uniquement continuer à être utilisé dans les applications héritées qui sont distribuées en interne (mise à niveau vers l’API unifiée recommandé).


### <a name="updating-from-classic-to-the-unified-api"></a>Mise à jour à partir de classique à l’API unifiée

Il existe des instructions détaillées pour la mise à jour n’importe quelle application à partir de la classique à l’API unifiée.

## <a name="binding-objective-c-librariesbindingindexmd"></a>[Liaison de bibliothèques Objective-C](binding/index.md)

Xamarin vous permet de placer des bibliothèques natives dans vos applications avec des liaisons. Cette section décrit :

- fonctionnement des liaisons,
- comment créer manuellement un projet de liaison qui vous permet de remettre le code Objective-C dans Xamarin, et
- l’utilisation de nos **objectif Sharpie** outil pour aider à automatiser le processus.

## <a name="native-referencesnative-referencesmd"></a>[Références natives](native-references.md)



##  <a name="macios-native-typesnativetypesmd"></a>[Types natifs Mac/iOS](nativetypes.md)

Pour prendre en charge de 32 et code 64 bits en toute transparence à partir de c# et F #, nous vous présentons les nouveaux types de données.   En savoir plus sur les ici.

##  <a name="building-32-and-64-bit-apps32-and-64indexmd"></a>[Applications de construction 32 et 64 bits](32-and-64/index.md)

Ce que vous devez savoir pour prendre en charge pour les applications 32 et 64 bits.

## <a name="working-with-native-types-in-cross-platform-appsnative-types-cross-platformmd"></a>[Utilisation de types natifs dans des applications multiplateformes](native-types-cross-platform.md)

Cet article couvre l’utilisation de la nouvelle iOS types natifs des API unifiée (`nint`, `nuint`, `nfloat`) dans une application multiplateforme où le code est partagé avec des périphériques non-iOS Android ou des systèmes d’exploitation Windows Phone.
Il permet de comprendre quand les types natifs doivent être utilisées et offre plusieurs solutions possibles pour les cas où le nouveau type doit être utilisé avec code multiplateforme.


## <a name="httpclient-stack-and-ssltls-implementation-selectorhttp-stackmd"></a>[Pile HttpClient et sélecteur d’implémentation de SSL/TLS](http-stack.md)

Le nouveau sélecteur de pile HttpClient détermine quelle implémentation HttpClient à utiliser dans votre application Xamarin.iOS, Xamarin.tvOS et Xamarin.Mac. Vous pouvez maintenant basculez vers une implémentation qui utilise d’iOS, de tvOS ou les transports natifs à la place du système d’exploitation (`NSUrlSession` ou `CFNetwork` selon le système d’exploitation).

SSL (Secure Socket Layer) et son successeur, TLS (Transport Layer Security), prennent en charge HTTP et d’autres connexions réseau via `System.Net.Security.SslStream`. La nouvelle option de génération de mise en œuvre de SSL/TLS bascule entre pile TLS de Mono et par la pile TLS d’Apple dans Mac et iOS.
