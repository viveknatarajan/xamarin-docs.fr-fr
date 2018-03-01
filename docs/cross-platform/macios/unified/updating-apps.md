---
title: "Mise à jour des applications existantes à l’API unifiée"
ms.topic: article
ms.prod: xamarin
ms.assetid: 8A654C95-5DCA-4BB5-A582-F96C2BECC81C
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: b1b6338494b9be98e677cf9d338410eae759feb8
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="updating-existing-apps-to-the-unified-api"></a>Mise à jour des applications existantes à l’API unifiée

> [!IMPORTANT]
> **Désapprobation de profil classique :** que de nouvelles plateformes sont ajoutés dans Xamarin.iOS nous avons commencé à déconseiller progressivement des fonctionnalités à partir du profil classique (monotouch.dll). Par exemple, l’option non-NRC (nouvelle-ref-count) a été supprimée. NRC a toujours été activé pour unifiée de toutes les applications (c'est-à-dire non NRC n’a jamais été une option) et ne présente aucun problème connu. Les versions futures supprime la possibilité d’utiliser Boehm en tant que le garbage collector. Cela était également une option jamais disponible pour les applications unifiées. La suppression complète de la prise en charge classique est planifiée pour automne suivant avec la version de Xamarin.iOS 10.0.




## <a name="how-to-update-your-apps"></a>Comment mettre à jour vos applications

Xamarin University a une vidéo disponible gratuitement **mise à niveau vers les API unifiée iOS**. Visitez [Xamarin University éclair conférences](http://university.xamarin.com/lightninglectures) à surveiller.

[ ![](updating-apps-images/xamu-video-sml.png "Université de Xamarin")](http://university.xamarin.com/lightninglectures)

Il existe trois étapes pour mettre à jour vos applications :

1. Résoudre les avertissements du compilateur dans votre code existant, en particulier celles qui concernent les API déconseillées.

2. Pour mettre à jour vos fichiers projet et les espaces de noms, utilisez l’outil de Migration intégrée à Visual Studio pour Mac.

3. Corrigez les erreurs du compilateur relatives au nouveau restantes [64-types](~/cross-platform/macios/nativetypes.md) et [autres API](~/cross-platform/macios/unified/index.md#deprecated-typos) qui ont été modifiés. Extraire [ces conseils](~/cross-platform/macios/unified/updating-tips.md) pour plus d’informations sur les mises à jour manuelles qui peuvent être requises.

Repères spécifiques sont disponibles pour chaque produit pour vous aider à mettre à jour vos applications aux API unifiée et prise en charge 64 bits :

### <a name="xamarinios-appscross-platformmaciosunifiedupdating-ios-appsmd"></a>[Applications Xamarin.iOS](~/cross-platform/macios/unified/updating-ios-apps.md)

Les applications Xamarin.iOS existantes peuvent être mis à jour à l’API unifiée à l’aide de l’outil de migration automatisée intégré à Visual Studio pour Mac. Certains correctifs supplémentaires peuvent ensuite être requis, comme expliqué dans [ces instructions](~/cross-platform/macios/unified/updating-ios-apps.md) et [conseils](~/cross-platform/macios/unified/updating-tips.md).

###  <a name="xamarinmac-appscross-platformmaciosunifiedupdating-mac-appsmd"></a>[Applications Xamarin.Mac](~/cross-platform/macios/unified/updating-mac-apps.md)

Les applications Xamarin.Mac existantes peuvent être mis à jour à l’API unifiée à l’aide de l’outil de migration automatisée intégré à Visual Studio pour Mac. Certains correctifs supplémentaires peuvent ensuite être requis, comme expliqué dans [ces instructions](~/cross-platform/macios/unified/updating-mac-apps.md) et [conseils](~/cross-platform/macios/unified/updating-tips.md).

###  <a name="xamarinforms-appscross-platformmaciosunifiedupdating-xamarin-forms-appsmd"></a>[Applications de Xamarin.Forms](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)

Suivez ces instructions pour mettre à jour d’une solution Xamarin.Forms existante avec un projet iOS à utiliser l’API unifiée. Prise en charge des API unifiée est uniquement disponible dans Xamarin.Forms 1.3 et versions ultérieures, donc [les instructions](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md) expliquent également comment mettre à jour votre application Xamarin.Forms pour la version 1.3. Ces [conseils](~/cross-platform/macios/unified/updating-tips.md) peut aider à la mise à jour n’importe quel code iOS natif dans les convertisseurs personnalisés ou des services de dépendance.

## <a name="working-with-native-types-in-cross-platform-appscross-platformmaciosnativetypesmd"></a>[Utilisation des Types natifs dans des applications multiplateformes](~/cross-platform/macios/nativetypes.md)

Cet article couvre l’utilisation de la nouvelle iOS types unifiée l’API Native (nint, nuint, nfloat) dans une application multiplateforme où le code est partagé avec des périphériques non-iOS Android ou des systèmes d’exploitation Windows Phone. Il permet de comprendre quand les types natifs doivent être utilisées et offre plusieurs solutions possibles pour les cas où le nouveau type doit être utilisé avec code multiplateforme.

## <a name="update-bindings-to-the-unified-api"></a>Mettre à jour des liaisons à l’API unifiée

Les clients qui ont créé des liaisons à des bibliothèques de Objective-C devez mettre à jour le projet de liaison pour refléter les modifications dans l’API sous-jacente (où certains types sont maintenant être 64 bits).
Suivez ces instructions pour [mettre à jour un projet existant de la liaison pour prendre en charge l’API unifiée](~/cross-platform/macios/unified/update-binding.md).




## <a name="related-links"></a>Liens associés

- [Mise à jour des applications iOS](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Mise à jour les applications Mac](~/cross-platform/macios/unified/updating-mac-apps.md)
- [Mise à jour des applications de Xamarin.Forms](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)
- [Mise à jour des liaisons](~/cross-platform/macios/unified/update-binding.md)
- [Conseils de mise à jour](~/cross-platform/macios/unified/updating-tips.md)
- [Différences d’API unifiée de vs classiques](http://developer.xamarin.comhttps://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
