---
title: Résolution des problèmes
description: Conditions d’erreur courantes et comment les résoudre
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 63291951-7375-4CBF-BCC3-2E4AD157A2C8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: fbe4fb6fce52636b59a9637ee0150c4c19fcc9da
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60850436"
---
# <a name="troubleshooting"></a>Résolution des problèmes

_Conditions d’erreur courantes et comment les résoudre_

## <a name="error-unable-to-find-a-version-of-xamarinforms-compatible-with"></a>Erreur : « Impossible de trouver une version de Xamarin.Forms compatible avec... »

Les erreurs suivantes peuvent apparaître dans le **Package Console** fenêtre lors de la mise à jour tous les packages Nuget dans une solution Xamarin.Forms ou dans un projet d’application Android de Xamarin.Forms :

```csharp
Attempting to resolve dependency 'Xamarin.Android.Support.v7.AppCompat (= 23.3.0.0)'.
Attempting to resolve dependency 'Xamarin.Android.Support.v4 (= 23.3.0.0)'.
Looking for updates for 'Xamarin.Android.Support.v7.MediaRouter'...
Updating 'Xamarin.Android.Support.v7.MediaRouter' from version '23.3.0.0' to '23.3.1.0' in project 'Todo.Droid'.
Updating 'Xamarin.Android.Support.v7.MediaRouter 23.3.0.0' to 'Xamarin.Android.Support.v7.MediaRouter 23.3.1.0' failed.
Unable to find a version of 'Xamarin.Forms' that is compatible with 'Xamarin.Android.Support.v7.MediaRouter 23.3.0.0'.
```

### <a name="what-causes-this-error"></a>Ce qui provoque cette erreur ?

Visual Studio pour Mac (ou Visual Studio) peut indiquer que les mises à jour sont disponibles pour le package Xamarin.Forms Nuget *et toutes ses dépendances*. Dans de Xamarin Studio, la solution **Packages** nœud peut se présenter comme suit (les numéros de version peuvent être différents) :

![](images/updates-available.png "Dossier de Packages de projet Android")

Cette erreur peut se produire si vous tentez de mettre à jour _tous les_ les packages.

Il s’agit, car avec Android projets configurés pour une version cible/de compilation de Android 6.0 (API 23) ou ci-dessous, Xamarin.Forms a une dépendance dure *spécifique* versions d’Android prennent en charge les packages. Bien que des versions mises à jour de ces packages peuvent être disponibles, Xamarin.Forms n’est pas nécessairement compatible avec eux.

Dans ce cas vous devez mettre à jour _uniquement_ le **Xamarin.Forms** car cela garantit que les dépendances restent sur les versions compatibles du package. Autres packages que vous avez ajoutés à votre projet peuvent également être mis à jour individuellement tant qu’ils n’empêchent pas les packages de prise en charge Android mettre à jour.


> [!NOTE]
> Si vous n’utilisez pas Xamarin.Forms 2.3.4 ou une version ultérieure **et** Android version de votre projet cible/de compilation est définie sur Android 7.0 (API 24) ou une version ultérieure, puis appliquent les dépendances dures mentionnés ci-dessus n’est plus et vous pouvez mettre à jour de la prise en charge packages indépendamment le package Xamarin.Forms.


### <a name="fix-remove-all-packages-and-re-add-xamarinforms"></a>Correctif : Supprimer tous les packages et ajoutez de nouveau Xamarin.Forms

Si le **Xamarin.Android.Support** packages ont été mis à jour pour les versions incompatibles, la solution la plus simple consiste à :

1. Supprimez manuellement tous les packages Nuget dans le projet Android, puis
2. Ajoutez de nouveau le **Xamarin.Forms** package.

Cela télécharge automatiquement la *correct* versions des autres packages.

Pour afficher une vidéo de ce processus, reportez-vous à ce [forums post](https://forums.xamarin.com/discussion/comment/170012/#Comment_170012).
