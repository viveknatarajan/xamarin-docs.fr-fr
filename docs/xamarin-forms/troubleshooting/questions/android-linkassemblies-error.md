---
title: 'Erreur de génération Android : échouée inattendu de la tâche LinkAssemblies le'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: EB3BE685-CB72-48E3-89D7-C845E76B9FA2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: 552ad4d2de6e2560dc4301a9b5cc0ce6a5edb348
ms.sourcegitcommit: d62732ce6f3f9d8dc929d72d4acac3e592cba073
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57197107"
---
# <a name="android-build-error--the-linkassemblies-task-failed-unexpectedly"></a>Erreur de génération Android : échouée inattendu de la tâche LinkAssemblies le

Vous pouvez voir un message d’erreur `The "LinkAssemblies" task failed unexpectedly` lorsque la génération d’un projet Xamarin.Android qui utilise les formulaires. Cela se produit lorsque l’éditeur de liens est active (en général sur un *version* build pour réduire la taille du package d’application) ; et il se produit car les cibles Android ne sont pas mis à jour vers la dernière version de framework. (Plus d’informations : [Xamarin.Forms pour Android exigences](~/get-started/requirements.md#android))

La résolution de ce problème consiste à s’assurer que vous avez les dernières versions du Kit Android SDK pris en charge et définissez le **Framework cible** à **utiliser la plateforme plus récente installée**. Il est également recommandé que vous avez défini le **Version Android cible** à **Version du Framework cible utilisation** et le **version Android minimale** supérieure ou égale à 15 de l’API. Cela est considéré comme la configuration prise en charge.

## <a name="setting-in-visual-studio-for-mac"></a>Configuration dans Visual Studio pour Mac

1.  Cliquez avec le bouton droit sur le projet Android.
2.  Accédez à **Générer > Général > ciblent Framework**.
3.  Définir le **ciblent Framework : Utiliser la plateforme plus récente installée**.
4.  Toujours dans les options du projet, accédez à **Générer > Application Android**.
5.  Définir le **version minimale d’Android** au niveau d’API 15 ou version ultérieure & le **version Android cible** à **automatique - Utilisez la version target framework**.

## <a name="setting-in-visual-studio"></a>Paramètre dans Visual Studio

1.  Cliquez avec le bouton droit sur le projet Android.
2.  Accédez à **Application** dans les options du projet.
3.  Définir le **compiler à l’aide de la version d’Android** & **version Android cible** paramètres **utiliser la dernière plateforme** / **utilisation Compilez à l’aide de la version SDK**.
4.  Définir le **minimale d’Android à cibler** définition d’API 19 ou une valeur supérieure.

Une fois que vous avez mis à jour ces paramètres, Veuillez nettoyer et régénérer votre projet pour vérifier que vos changements sont récupérés.
