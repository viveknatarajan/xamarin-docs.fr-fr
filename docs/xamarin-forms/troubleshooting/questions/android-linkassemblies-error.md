---
title: Erreur de génération Android – tâche LinkAssemblies l’échec inattendu
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: EB3BE685-CB72-48E3-89D7-C845E76B9FA2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: de6e0b66cac688955d27ba2d0165d5a059d36c38
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30789538"
---
# <a name="android-build-error--the-linkassemblies-task-failed-unexpectedly"></a>Erreur de génération Android – tâche LinkAssemblies l’échec inattendu

Vous pouvez voir un message d’erreur `The "LinkAssemblies" task failed unexpectedly` lorsque la génération d’un projet Xamarin.Android qui utilise les formulaires. Cela se produit lorsque l’éditeur de liens est active (en général sur un *version* build pour réduire la taille du package d’application) ; et il se produit car les cibles Android ne sont pas mis à jour vers la dernière version de framework. (Plus d’informations : [Xamarin.Forms pour la configuration requise Android](~/xamarin-forms/get-started/installation.md#android))

La solution à ce problème consiste à s’assurer que vous avez les dernières versions du SDK Android pris en charge et la **Framework cible** à **plate-forme utiliser dernière installé**. Il est également recommandé de définir la **Version Android cible** à **Version du Framework cible utilisation** et le **la version minimale Android** supérieure ou égale à 15 de l’API. Il s’agit de la configuration prise en charge.

## <a name="setting-in-visual-studio-for-mac"></a>Paramètre dans Visual Studio pour Mac

1.  Cliquez avec le bouton droit sur le projet Android.
2.  Accédez à **Générer > Général > infrastructure cible**.
3.  Définir le **Framework cible : utiliser dernière installé l’appel**.
4.  Toujours dans les options du projet, accédez à **Générer > Application Android**.
5.  Définir le **version minimale Android** au niveau de l’API 15 ou version ultérieure & le **cible Android version** à **automatique - version du framework cible utilisation**.

## <a name="setting-in-visual-studio"></a>Paramètre dans Visual Studio

1.  Cliquez avec le bouton droit sur le projet Android.
2.  Accédez à **Application** dans les options de projet.
3.  Définir le **compiler avec la version Android** & **version cible Android** paramètres **utilisez plateforme la plus récente** / **utilisation Compiler avec la version du Kit de développement logiciel**.
4.  Définir le **Minimum Android à cibler** définition supérieure ou égale à 15 de l’API.

Une fois que vous avez mis à jour ces paramètres, Veuillez nettoyer et régénérer votre projet pour vérifier que vos modifications sont appliquées.
