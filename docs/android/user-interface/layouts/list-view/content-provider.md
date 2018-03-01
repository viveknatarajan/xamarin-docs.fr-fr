---
title: "À l’aide d’un ContentProvider"
ms.topic: article
ms.prod: xamarin
ms.assetid: 251F7557-328D-0132-F39D-595920A28B87
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 5c90b140719e0dafb36dad1da75f52ba27e75e25
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="using-a-contentprovider"></a>À l’aide d’un ContentProvider

CursorAdapters peut également être utilisé pour afficher des données à partir d’un ContentProvider.
ContentProviders vous permettre d’accéder aux données exposées par d’autres applications (y compris les données système Android comme contacts, les informations de support et de calendrier).

La meilleure façon d’accéder à un ContentProvider est avec un CursorLoader à l’aide de la LoaderManager. LoaderManager a été introduit dans 3.0 Android (API niveau 11, nid) pour déplacer les tâches de blocage du thread principal, et à l’aide d’un CursorLoader permet aux données à charger dans un thread avant d’être lié à un ListView pour l’affichage.

Reportez-vous à [Introduction à ContentProviders](~/android/platform/content-providers/index.md) pour plus d’informations.

