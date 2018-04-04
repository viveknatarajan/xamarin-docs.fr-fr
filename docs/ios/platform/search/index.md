---
title: Nouvelles API de recherche
description: Cet article couvre l’utilisation de nouvelles API de recherche application fournies par iOS 9 pour permettre aux utilisateurs de rechercher des informations et des fonctionnalités à l’intérieur de vos applications Xamarin.iOS.
ms.prod: xamarin
ms.assetid: 7323EB3D-A78F-4BF0-9990-3160C7E83CF0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 5dcd3d9665befaa82fd0f5677a4a662f633ed45b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="new-search-apis"></a>Nouvelles API de recherche

_Cet article couvre l’utilisation de nouvelles API de recherche application fournies par iOS 9 pour permettre aux utilisateurs de rechercher des informations et des fonctionnalités à l’intérieur de vos applications Xamarin.iOS._

Recherche a été développée dans iOS 9 pour fournir de nouvelles façons d’accéder aux informations et des fonctionnalités à l’intérieur d’une application Xamarin.iOS. Contenu de l’application à l’aide de la nouvelle API de recherche application, est effectué recherche Spotlight et Safari résultats de recherche, remise et les rappels de Siri et les Suggestions. Cela permet aux utilisateurs d’accéder rapidement aux activités et informations approfondies dans votre application.

En outre, les nouvelles API de recherche facilitent intégrer la recherche dans votre application sans expérience de mise en œuvre de recherche antérieure. Pour cette raison, Apple revendications qu’il prend généralement quelques heures pour rendre le contenu de l’application iOS 9 universellement consultable à l’aide de la recherche de l’application.

[![](images/intro01.png "Un exemple de contenu de l’application iOS 9 universellement consultable à l’aide de la recherche de l’application")](images/intro01.png#lightbox)

Recherche de l’application est composé de trois API :

1. [**NSUserActivity** ](nsuseractivity.md) -il s’agit d’une extension de l’API de remise qui Apple iOS 8 publiée. Il est utilisé pour effectuer l’historique application interaction des recherches à la fois publiques et privées) par l’utilisateur.

2. [**Galerie de base** ](corespotlight.md) -permet à une application de son contenu à être présentés dans les résultats de la recherche d’index. Il fonctionne comme une base de données API dans lequel les éléments peuvent être ajoutées et supprimées et la meilleure façon d’indexer le contenu privé au sein d’une application.

3. [**WebMarkup** ](web-markup.md) - pour les applications qui fournissent l’accès à leur contenu via une interface web (pas uniquement à partir de l’application). Le contenu Web peut être marqué avec liens spéciaux qui seront analysés par Apple et fournissent un lien profond à votre application sur son appareil iOS 9.

## <a name="selecting-an-app-search-approach"></a>Choix d’une méthode de recherche de l’application

Déterminer laquelle de ces méthodes à implémenter dépend des types d’interaction fourni par votre application et le type de contenu, qu'il s’affiche.

Utilisez les instructions suivantes :

- [**NSUserActivity** ](nsuseractivity.md) – utiliser cette infrastructure pour fournir des possibilités de recherche de contenu public et privé et également les possibilités de recherche de points de navigation au sein de votre application.

- [**Galerie de base** ](corespotlight.md) – utiliser cette infrastructure pour fournir des possibilités de recherche pour les données confidentielles stockées sur l’appareil.

- [**Web balisage** ](web-markup.md) – permet de fournir des possibilités de recherche pour les applications qui présentent leur contenu, pas uniquement à partir de l’application, mais à partir du site Web de l’application ainsi cette infrastructure.

De la recherche de l’application approches sont distincts et peuvent être utilisés individuellement, toutefois Apple conçues pour fonctionner ensemble. Lorsque vous utilisez une plusieurs approche à un élément spécifique d’index, assurez-vous d’utiliser le même **ID d’élément** sur chaque approche, par conséquent, cette personne fonctionnent les liens entre eux.

L’utilisation de plus d’une approche non seulement permet de s’assurer que votre contenu sera trouvé par l’utilisateur final, mais contribue également à améliorer le classement de votre objet de recherche.

Pendant le processus de classification dans totalement transparente pour le développeur, l’interaction utilisateur avec un élément donné pèse fortement à ce classement (par exemple l’utilisateur touchant un lien).
En fournissant des éléments informatifs riches, vous pouvez vous assurer qu’un utilisateur s’est conduit pour interagir avec votre contenu, élève donc son classement.

## <a name="what-content-to-index"></a>Le contenu à l’Index

Apple offre des suggestions suivantes concernant le contenu et des actions pour fournir des index de recherche pour dans votre application :

 - Tout contenu affiché, créé ou organisé par l’utilisateur à partir de votre application.
 - Points de navigation et les fonctionnalités au sein de l’application.
 - Éléments comme des nouveaux messages, de contenu ou d’autres types d’éléments affichées par votre application qui ont récemment été téléchargées sur l’appareil.

## <a name="app-search-enhancements"></a>Améliorations de la recherche de l’application

Spotlight Core dans iOS 10 apporte plusieurs améliorations à la recherche de l’application tel que :

- **Popularité de lien ciblé Crowdsourced (avec confidentialité différentielle)** -permet de promouvoir le contenu lié en profondeur l’application dans les résultats de la recherche.
- **Dans l’application recherche** -utiliser la nouvelle `CSSearchQuery` classe afin de fournir la possibilité de recherche Spotlight dans l’application similaire au fonctionnement des applications de messagerie, les Messages et les Notes.
- **Rechercher la Continuation** : permet à un utilisateur de lancer une recherche dans la galerie ou Safari, puis ouvrir une application et continuer la recherche.
- **Visualisation des résultats de la Validation** -Apple [outil de Validation API application recherche](https://search.developer.apple.com/appsearch-validation-tool) affiche maintenant une représentation visuelle du balisage d’un site Web et lien profond lors de la réalisation des tests.
- **Message d’application de partage Image** -permet des images dans l’application populaires fournis pour le partage dans les Messages (via une Extension d’application Message) pour qu’il apparaisse dans les recherches de la galerie.

Pour plus d’informations, consultez notre [application recherche améliorations](~/ios/platform/search/app-search-enhancements.md) guide.

### <a name="proactive-suggestions"></a>Suggestions proactive

iOS 10 présente les nouvelles façons d’implication conduite à une application en autorisant le système de façon proactive présente des informations utiles automatiquement à l’utilisateur au moment opportun. Comme d’iOS 9 fourni la possibilité d’ajouter une recherche approfondie à l’application à l’aide de la galerie, remise et des Suggestions de Siri, avec iOS 10, qu'une application peut exposer des fonctionnalités qui peuvent être présentées à l’utilisateur par le système à partir des emplacements suivants :

- Le sélecteur d’application
- L’écran de verrouillage
- CarPlay
- Maps
- Interactions de Siri
- Suggestions de QuickType 

Une application expose cette fonctionnalité dans le système à l’aide d’une collection de technologies telles que [NSUserActivity](https://developer.xamarin.com/api/type/Foundation.NSUserActivity/), balisage web, Core Spotlight, MapKit, lecteur multimédia et UIKit.

Pour plus d’informations, consultez notre [Suggestions Proactive](~/ios/platform/search/proactive-suggestions.md) guide.

## <a name="summary"></a>Récapitulatif

Cet article a couvert les nouvelles fonctionnalités de l’API de recherche qu’iOS 9 fournit pour les applications de Xamarin.iOS. Couvert [NSUserActivity](nsuseractivity.md), [Core Spotlight](corespotlight.md) et [Web balisage](web-markup.md) méthodes pour l’indexation de contenu. Il est terminé avec une brève présentation des quand une approche de recherche donnée doit être utilisée et les types de contenu doivent être indexée.



## <a name="related-links"></a>Liens associés

- [Exemples d’iOS 9](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 pour les développeurs](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Guide de programmation de recherche de l’application](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
