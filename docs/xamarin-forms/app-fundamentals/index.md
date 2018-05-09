---
title: Notions de base des applications
description: Exploration des notions de base du développement de Xamarin.Forms
ms.prod: xamarin
ms.assetid: 7B516BBC-F7E1-4387-9779-7754E2E69723
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/28/2017
ms.openlocfilehash: 2bd050e6eba33b543c52a80bb42b184200164c14
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
---
# <a name="application-fundamentals"></a>Notions de base des applications

## <a name="accessibilityaccessibilityindexmd"></a>[Accessibilité](accessibility/index.md)

Conseils pour intégrer des fonctionnalités accessibles (tels que la prise en charge des outils de la lecture de l’écran) avec Xamarin.Forms.

## <a name="app-classapplication-classmd"></a>[Classe d’application](application-class.md)

Le `Application` classe est le point de départ pour Xamarin.Forms – chaque application doit implémenter une sous-classe `App` pour définir la page initiale. Il fournit également la `Properties` collection pour le stockage de données simple. Elle peut être définie en c# ou XAML.

## <a name="app-lifecycleapp-lifecyclemd"></a>[Cycle de vie d’application](app-lifecycle.md)

Le `Application` classe `OnStart`, `OnSleep`, et `OnResume` méthodes, ainsi que les événements de navigation modale, vous permettent de gérer les événements de cycle de vie d’application avec du code personnalisé.

## <a name="behaviorsbehaviorsindexmd"></a>[Comportements](behaviors/index.md)

Contrôles d’interface utilisateur peuvent facilement être étendus sans le sous-classement en utilisant les comportements pour ajouter des fonctionnalités.

## <a name="custom-rendererscustom-rendererindexmd"></a>[Renderers personnalisés](custom-renderer/index.md)

Les convertisseurs personnalisés permettent aux développeurs de 'override' le rendu par défaut des contrôles de Xamarin.Forms pour personnaliser leur apparence et leur comportement sur chaque plateforme (à l’aide de kits de développement logiciel natives si vous le souhaitez).

## <a name="data-bindingdata-bindingindexmd"></a>[Liaison de données](data-binding/index.md)

Liaison de données lie les propriétés de deux objets, autoriser les modifications sur une propriété à automatiquement répercutées dans l’autre propriété. Liaison de données fait partie intégrante de Model-View-ViewModel ([MVVM](~/xamarin-forms/enterprise-application-patterns/mvvm.md)) architecture de l’application.

## <a name="dependency-servicedependency-serviceindexmd"></a>[Service de dépendance](dependency-service/index.md)

Le `DependencyService` fournit une recherche simple afin que vous pouvez aux interfaces de code dans votre code partagé et fournir des implémentations de spécifique à la plateforme qui sont résolues automatiquement, ce qui permet de référencer des fonctionnalités spécifiques à la plateforme dans Xamarin.Forms.

## <a name="effectseffectsindexmd"></a>[Effets](effects/index.md)

Autoriser les effets des contrôles natifs sur chaque plateforme pour être personnalisées et sont généralement utilisés pour le style de petites modifications.

## <a name="gesturesgesturesindexmd"></a>[Gestes](gestures/index.md)

Le Xamarin.Forms [ `GestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GestureRecognizer/) tap, pincement et des mouvements de panoramique prend en charge la classe sur les contrôles d’interface utilisateur.

## <a name="localizationlocalizationindexmd"></a>[Localisation](localization/index.md)

L’infrastructure de localisation intégré .NET permet de créer des applications multilingues inter-plateformes avec Xamarin.Forms.

## <a name="local-databasesdatabasesmd"></a>[Bases de données locales](databases.md)

Xamarin.Forms prend en charge les applications pilotées par des base de données utilisant le moteur de base de données SQLite, ce qui rend possible charger et enregistrer les objets dans le code partagé.

## <a name="messaging-centermessaging-centermd"></a>[Centre de messagerie](messaging-center.md)

Xamarin.Forms `MessagingCenter` permet d’afficher des modèles et autres composants de communiquer avec et sans avoir à connaître l’autre en dehors d’un contrat de Message simple.

## <a name="navigationnavigationindexmd"></a>[Navigation](navigation/index.md)

Xamarin.Forms contient un nombre d’expériences de navigation de page différente, selon la `Page` en cours d’utilisation.

## <a name="templatestemplatesindexmd"></a>[Modèles](templates/index.md)

Modèles de contrôle permettent de facilement et re pages d’application lors de l’exécution, tandis que les modèles de données fournissent la possibilité de définir la présentation des données sur les contrôles pris en charge.

## <a name="triggerstriggersmd"></a>[Déclencheurs](triggers.md)

Mettre à jour des contrôles en réponse aux modifications apportées aux propriétés et les événements en XAML.


## <a name="related-links"></a>Liens associés

- [Introduction à Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
