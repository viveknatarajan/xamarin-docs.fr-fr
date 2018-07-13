---
title: Principes de base Application Xamarin.Forms
description: Explorer les principes fondamentaux du développement d’applications Xamarin.Forms, y compris tous les concepts principaux requis via à finitions telles que l’accessibilité et la localisation.
ms.prod: xamarin
ms.assetid: 7B516BBC-F7E1-4387-9779-7754E2E69723
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/28/2017
ms.openlocfilehash: 515dbd2683619cfcfb7a6c8ecac6bc147265ef7d
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995610"
---
# <a name="xamarinforms-application-fundamentals"></a>Principes de base Application Xamarin.Forms

## <a name="accessibilityaccessibilityindexmd"></a>[Accessibilité](accessibility/index.md)

Conseils pour incorporer des fonctionnalités accessibles (par exemple, la prise en charge des outils de lecture d’écran) avec Xamarin.Forms.

## <a name="app-classapplication-classmd"></a>[Classe d’application](application-class.md)

Le `Application` classe est le point de départ pour Xamarin.Forms : chaque application doit implémenter une sous-classe `App` pour définir la page initiale. Il fournit également la `Properties` collection pour le stockage de données simple. Elle peut être définie en c# ou XAML.

## <a name="app-lifecycleapp-lifecyclemd"></a>[Cycle de vie d’application](app-lifecycle.md)

Le `Application` classe `OnStart`, `OnSleep`, et `OnResume` méthodes, ainsi que les événements de navigation modale, vous permettent de gérer les événements de cycle de vie d’application avec du code personnalisé.

## <a name="behaviorsbehaviorsindexmd"></a>[Comportements](behaviors/index.md)

Contrôles d’interface utilisateur peuvent facilement être étendues sans sous-classement à l’aide de comportements pour ajouter des fonctionnalités.

## <a name="custom-rendererscustom-rendererindexmd"></a>[Renderers personnalisés](custom-renderer/index.md)

Les convertisseurs personnalisés permettent aux développeurs 'override' le rendu par défaut des contrôles Xamarin.Forms pour personnaliser leur apparence et leur comportement sur chaque plateforme (à l’aide de kits de développement natifs si vous le souhaitez).

## <a name="data-bindingdata-bindingindexmd"></a>[Liaison de données](data-binding/index.md)

Liaison de données lie les propriétés de deux objets, ce qui permet des modifications dans une propriété à automatiquement répercutées dans l’autre propriété. Liaison de données fait partie intégrante de Model-View-ViewModel ([MVVM](~/xamarin-forms/enterprise-application-patterns/mvvm.md)) architecture de l’application.

## <a name="dependency-servicedependency-serviceindexmd"></a>[Service de dépendance](dependency-service/index.md)

Le `DependencyService` fournit un localisateur simple afin que vous puissiez aux interfaces de code dans votre code partagé et fournissent des implémentations spécifiques à la plateforme qui sont résolues automatiquement, ce qui vous permet de référencer des fonctionnalités spécifiques à la plateforme dans Xamarin.Forms.

## <a name="effectseffectsindexmd"></a>[Effets](effects/index.md)

Les effets permettent les contrôles natifs sur chaque plateforme pour être personnalisées et sont généralement utilisés pour les styles de petites modifications.

## <a name="filesfilesmd"></a>[Fichiers](files.md)

Gestion avec Xamarin.Forms de fichiers est possible à l’aide de code dans une bibliothèque .NET Standard, ou en utilisant des ressources incorporées.

## <a name="gesturesgesturesindexmd"></a>[Gestes](gestures/index.md)

Xamarin.Forms [ `GestureRecognizer` ](xref:Xamarin.Forms.GestureRecognizer) tap, pincement et des mouvements de panoramique prend en charge la classe sur les contrôles d’interface utilisateur.

## <a name="localizationlocalizationindexmd"></a>[Localisation](localization/index.md)

L’infrastructure de localisation .NET intégrée peut être utilisé pour générer des applications multilingues multiplateforme avec Xamarin.Forms.

## <a name="local-databasesdatabasesmd"></a>[Bases de données locales](databases.md)

Xamarin.Forms prend en charge les applications pilotées par base de données en utilisant le moteur de base de données SQLite, ce qui rend possible charger et enregistrer des objets dans le code partagé.

## <a name="messaging-centermessaging-centermd"></a>[Centre de messagerie](messaging-center.md)

Xamarin.Forms `MessagingCenter` permet d’afficher des modèles et autres composants de communiquer avec sans avoir rien à savoir sur eux en dehors d’un contrat de Message simple.

## <a name="navigationnavigationindexmd"></a>[Navigation](navigation/index.md)

Xamarin.Forms fournit un nombre d’expériences de navigation de page différente, selon le `Page` en cours d’utilisation.

## <a name="templatestemplatesindexmd"></a>[Modèles](templates/index.md)

Modèles de contrôle permettent de facilement des thèmes et re-theme pages d’application lors de l’exécution, tandis que les modèles de données offrent la possibilité de définir la présentation des données sur les contrôles pris en charge.

## <a name="triggerstriggersmd"></a>[Déclencheurs](triggers.md)

Mettre à jour des contrôles en répondant aux modifications apportées aux propriétés et événements dans XAML.


## <a name="related-links"></a>Liens associés

- [Introduction à Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
