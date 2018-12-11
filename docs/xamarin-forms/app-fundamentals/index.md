---
title: Notions de base des applications Xamarin.Forms
description: Exploration des principes fondamentaux du développement d’applications Xamarin.Forms, y compris tous les concepts principaux nécessaires, jusqu’aux touches finales comme l’accessibilité et la localisation.
ms.prod: xamarin
ms.assetid: 7B516BBC-F7E1-4387-9779-7754E2E69723
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/03/2018
ms.openlocfilehash: 015a7dfa3d55b411e6626ae2ac70fd2930661e65
ms.sourcegitcommit: 01f93a34b466f8d4043cef68fab9b35cd8decee6
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2018
ms.locfileid: "52898990"
---
# <a name="xamarinforms-application-fundamentals"></a>Notions de base des applications Xamarin.Forms

## <a name="accessibilityaccessibilityindexmd"></a>[Accessibilité](accessibility/index.md)

Conseils pour incorporer des fonctionnalités accessibles (par exemple, la prise en charge des outils de lecture d’écran) avec Xamarin.Forms.

## <a name="app-classapplication-classmd"></a>[Classe d’application](application-class.md)

La classe `Application` est le point de départ de Xamarin.Forms : chaque application doit implémenter une sous-classe `App` pour définir la page initiale. Elle fournit aussi la collection `Properties` pour le stockage simple des données. Elle peut être définie en C# ou XAML.

## <a name="app-lifecycleapp-lifecyclemd"></a>[Cycle de vie d’application](app-lifecycle.md)

Les méthodes `OnStart`, `OnSleep`, et `OnResume` de la classe `Application`, ainsi que les événements de navigation modale, vous permettent de gérer les événements de cycle de vie des applications avec du code personnalisé.

## <a name="behaviorsbehaviorsindexmd"></a>[Comportements](behaviors/index.md)

Les contrôles d’interface utilisateur peuvent facilement être étendus sans utilisation de sous-classes à l’aide de comportements qui ajoutent des fonctionnalités.

## <a name="custom-rendererscustom-rendererindexmd"></a>[Renderers personnalisés](custom-renderer/index.md)

Les convertisseurs personnalisés permettent aux développeurs de « remplacer » le rendu par défaut des contrôles Xamarin.Forms pour personnaliser leur apparence et leur comportement sur chaque plateforme (avec des SDK natifs s’ils le souhaitent).

## <a name="data-bindingdata-bindingindexmd"></a>[Liaison de données](data-binding/index.md)

La liaison de données lie les propriétés de deux objets pour que les changements d’une propriété soient automatiquement répercutés sur l’autre propriété. La liaison de données fait partie intégrante de l’architecture d’application modèle-vue-vue modèle ([MVVM](~/xamarin-forms/enterprise-application-patterns/mvvm.md)).

## <a name="dependency-servicedependency-serviceindexmd"></a>[Service de dépendance](dependency-service/index.md)

Le `DependencyService` fournit un localisateur simple pour vous permettre de coder dans des interfaces dans votre code partagé, et fournit des implémentations propres à la plateforme qui sont résolues automatiquement pour simplifier la référence des fonctionnalités propres à la plateforme dans Xamarin.Forms.

## <a name="effectseffectsindexmd"></a>[Effets](effects/index.md)

Les effets permettent de personnaliser les contrôles natifs de chaque plateforme et sont généralement utilisés pour les petits changements de style.

## <a name="filesfilesmd"></a>[Fichiers](files.md)

La gestion de fichiers avec Xamarin.Forms est possible à l’aide de code dans une bibliothèque .NET Standard, ou en utilisant des ressources incorporées.

## <a name="gesturesgesturesindexmd"></a>[Gestes](gestures/index.md)

La classe Xamarin.Forms [`GestureRecognizer`](xref:Xamarin.Forms.GestureRecognizer) prend en charge les appuis, les pincements et les mouvements panoramiques sur les contrôles d’interface utilisateur.

## <a name="localizationlocalizationindexmd"></a>[Localisation](localization/index.md)

Le framework de localisation .NET intégré peut être utilisé pour créer des applications multilingues multiplateformes avec Xamarin.Forms.

## <a name="local-databasesdatabasesmd"></a>[Bases de données locales](databases.md)

Xamarin.Forms prend en charge les applications pilotées par base de données à l’aide du moteur de base de données SQLite, ce qui permet de charger et d’enregistrer des objets dans du code partagé.

## <a name="messaging-centermessaging-centermd"></a>[Centre de messagerie](messaging-center.md)

Le `MessagingCenter` de Xamarin.Forms permet aux modèles de vue et aux autres composants de communiquer sans avoir aucun lien entre eux autre qu’un simple contrat de message.

## <a name="navigationnavigationindexmd"></a>[Navigation](navigation/index.md)

Xamarin.Forms propose plusieurs expériences différentes de navigation dans les pages, selon le type de `Page` utilisé.

## <a name="shellshellmd"></a>[Shell](shell.md)

Xamarin.Forms Shell est un conteneur pour applications qui offre des fonctionnalités d’interface utilisateur clés nécessaires à la plupart des applications pour vous permettre de vous concentrer sur la charge de travail principale de l’application.

## <a name="templatestemplatesindexmd"></a>[Modèles](templates/index.md)

Les modèles de contrôle permettent de facilement créer et recréer des thèmes pour les pages d’application au moment de l’exécution, tandis que les modèles de données offrent la possibilité de définir la présentation des données sur les contrôles pris en charge.

## <a name="triggerstriggersmd"></a>[Déclencheurs](triggers.md)

Mettent à jour les contrôles en répondant aux changements de propriété et aux événements en XAML.


## <a name="related-links"></a>Liens associés

- [Introduction à Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
