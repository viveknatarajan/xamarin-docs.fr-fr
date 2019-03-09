---
title: 'Erreur de génération Android : échouée inattendu de la tâche LinkAssemblies le'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: EB3BE685-CB72-48E3-89D7-C845E76B9FA2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/07/2019
ms.openlocfilehash: f517aaa770fa7b2f1463954638f0afc95168bf65
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57669672"
---
# <a name="android-build-error--the-linkassemblies-task-failed-unexpectedly"></a>Erreur de génération Android : échouée inattendu de la tâche LinkAssemblies le

Vous pouvez voir un message d’erreur `The "LinkAssemblies" task failed unexpectedly` lorsque la génération d’un projet Xamarin.Android qui utilise les formulaires. Cela se produit lorsque l’éditeur de liens est active (en général sur un *version* build pour réduire la taille du package d’application) ; et il se produit car les cibles Android ne sont pas mis à jour vers la dernière version de framework. (Plus d’informations : [Xamarin.Forms pour Android exigences](~/get-started/requirements.md#android))

La résolution de ce problème consiste à s’assurer que vous avez les dernières versions du Kit Android SDK pris en charge et définissez le **Framework cible** à la dernière plateforme installée. Il est également recommandé que vous avez défini le **Version Android cible** à la dernière plateforme installée et le **version Android minimale** supérieure ou égale à 19 d’API. Cela est considéré comme la configuration prise en charge.

## <a name="setting-in-visual-studio-for-mac"></a>Configuration dans Visual Studio pour Mac

1.  Cliquez avec le bouton droit sur le projet Android, puis sélectionnez **Options** dans le menu.
2.  Dans le **Options du projet** boîte de dialogue, accédez à **Générer > Général**.
3.  Définir le **compiler à l’aide de la version d’Android : (Framework cible)**  à la dernière plateforme installée.
4.  Dans le **Options du projet** boîte de dialogue, accédez à **Générer > Application Android**.
5.  Définir le **version minimale d’Android** au niveau d’API 19 ou une version ultérieure et le **version Android cible** à la dernière plateforme installée que vous avez choisi dans (3).

## <a name="setting-in-visual-studio"></a>Paramètre dans Visual Studio

1.  Cliquez avec le bouton droit sur le projet Android, puis sélectionnez **propriétés** dans le menu.
2.  Dans les propriétés du projet, accédez à **Application**.
3.  Définir le **compiler à l’aide de la version d’Android : (Framework cible)**  à la dernière plateforme installée.
4.  Dans les propriétés du projet, accédez à **manifeste Android**.
5.  Définir le **version minimale d’Android** au niveau d’API 19 ou une version ultérieure et le **version Android cible** à la dernière plateforme installée que vous avez choisi dans (3).

Une fois que vous avez mis à jour ces paramètres, Veuillez nettoyer et régénérer votre projet pour vérifier que vos changements sont récupérés.
