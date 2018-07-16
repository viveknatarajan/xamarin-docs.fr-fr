---
title: Introduction à iOS 12
description: Ce document fournit une description générale de certaines API iOS 12 pour la version préliminaire de quels Xamarin version fournit des liaisons c#.
ms.prod: xamarin
ms.assetid: 99EA7090-315D-493C-87D3-26AB73D9E1A9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/08/2018
ms.openlocfilehash: 865a06e9fa430e195ce4ea3c6088785d9513dbf6
ms.sourcegitcommit: cfb72be633e335147d156af3ef9527151b9e31d9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39030702"
---
# <a name="introduction-to-ios-12"></a>Introduction à iOS 12

![Preview](~/media/shared/preview.png)

> [!WARNING]
> Prise en charge 12 de Xamarin iOS est actuellement en version préliminaire, ce qui signifie qu’il peut contenir des bogues, n’est pas complet, et peut changer. Utilisez-le pour l’expérimentation uniquement.

> [!NOTE]
> - Examinez le [mise en route](get-started.md) guide pour obtenir des instructions sur la façon de créer des applications iOS 12 avec Xamarin.
> - Pour plus d’informations, consultez la préversion Xamarin [mise en production de billet de blog](https://releases.xamarin.com/preview-release-xcode-10-beta-3/).

Ce document fournit une description générale de certaines API iOS 12 pour la version préliminaire de quels Xamarin version fournit des liaisons c#.

## <a name="arkit-2"></a>ARKit 2

ARKit est l’infrastructure de réalité augmentée inclus avec iOS. ARKit 2 permet à plusieurs utilisateurs d’interagir avec eux dans une scène de réalité augmentée, rend possible conserver des objets dans l’espace et de retourner leur ultérieurement, reconnaissance d’images 2D et fournit la reconnaissance de l’objet suivi et 3D. iOS 12 fournit également de rechercher AR rapide, un moyen pour restituer usdz modèles AR dans vos applications.

## <a name="siri-shortcuts"></a>Raccourcis de Siri

Raccourcis de Siri permettent aux développeurs d’intégrer plus étroitement leurs applications avec Siri. Avec les raccourcis de Siri, les utilisateurs peuvent utiliser des commandes vocales pour ouvrir le contenu ou de lancer des tâches dans leurs applications. Siri apprendrez lorsque certains d'entre eux sont plus susceptibles d’être utilisés et les proposer à l’utilisateur via des notifications.

## <a name="core-ml-2"></a>Core ML 2

Core ML 2 réduit la taille de l’application par le biais de quantification de modèle et de modèles flexibles, améliore les performances des applications avec une nouvelle API de prédiction de lot et utilise des modèles personnalisés pour prendre en charge des progrès dans l’apprentissage.

## <a name="notification-improvements"></a>Amélioration de la notification

Dans iOS 12, notifications groupées rendent possible pour envoyer des notifications utilisateur présents dans l’application ou des regroupements liés aux threads. Texte du résumé peut être utilisé pour fournir davantage d’informations sur un groupe de notification.

Extensions de contenu de notification dans iOS 12 permettent des interfaces utilisateur personnalisées et les actions dynamiques. Ces fonctionnalités permettent des expériences plus riches et plus pertinentes dans les notifications à l’utilisateur.

## <a name="natural-language-framework"></a>Framework de langage naturel

L’infrastructure de langage naturel permet aux applications d’effectuer différents types d’analyse linguistique. Par exemple, il peut être utilisé pour identifier les parties du discours et de déterminer la langue représentée par un bloc de texte.

## <a name="carplay"></a>CarPlay

Dans iOS 12, les applications tierces peuvent offrir maps et obtenir des instructions étape par étape navigation dans CarPlay à l’aide de la nouvelle infrastructure CarPlay.

## <a name="automatic-strong-passwords"></a>Mots de passe forts automatique

iOS 12 suggère et stocker les noms d’utilisateur et mots de passe forts pour les applications contenant un écran de compte de créer. Les mots de passe suggérées peuvent être générés selon un format par défaut, 20 caractères ou en fonction des règles de mot de passe spécifiés par les développeurs. Cette fonctionnalité rend utiliser de domaines associés et spécifié les types de contenu sur le nouveau nom d’utilisateur et de nouveaux champs de mot de passe.

## <a name="autofill-credential-provider-extensions"></a>Extensions de fournisseur d’informations d’identification de remplissage automatique

Avec iOS 12, les applications du Gestionnaire de mot de passe par des tiers peuvent fournir une extension pour fournir des valeurs de nom d’utilisateur et mot de passe dans les champs de connexion.

## <a name="healthkit-updates"></a>Mises à jour HealthKit

iOS 11.3 introduit [médicaux](https://www.apple.com/healthcare/health-records/), ce qui permet aux utilisateurs de télécharger leur intégrité enregistrer des informations à partir de différents établissements de soins de santé et l’afficher sur leurs appareils iOS. iOS 12 ajoute des API qui permettent à des applications tierces à accéder en toute sécurité ces données.

## <a name="imessage-app-presentation-contexts"></a>iMessage contextes de présentation d’application

Dans iOS 12, iMessage applications prennent en charge les contextes de présentation, qui autorise les applications à exécuter comme application iMessage normal ou dans le contexte d’une photo ou d’effet vidéo.

## <a name="related-links"></a>Liens connexes

- [Préparez-vous pour iOS 12 (Apple)](https://developer.apple.com/ios/)
- [iOS 12 Preview (Apple)](https://www.apple.com/ios/ios-12-preview/)
- Aperçu de Xamarin [mise en production de billet de blog](https://releases.xamarin.com/preview-release-xcode-10-beta-3/)
