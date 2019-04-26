---
title: Utilisation d’un ContentProvider
ms.prod: xamarin
ms.assetid: 251F7557-328D-0132-F39D-595920A28B87
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: d1ec628de3481820f320a5a8e6ef88fcbaab75a6
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61189727"
---
# <a name="using-a-contentprovider"></a>Utilisation d’un ContentProvider

CursorAdapters peut également être utilisé pour afficher des données à partir d’un ContentProvider.
Contentprovider vous permettre d’accéder aux données exposées par d’autres applications (y compris les données système Android comme contacts, les informations de support et de calendrier).

La meilleure façon d’accéder à un ContentProvider est avec un CursorLoader à l’aide de la LoaderManager. LoaderManager a été introduit dans 3.0 Android (API niveau 11, Honeycomb) pour déplacer les tâches de blocage du thread principal, ainsi que d’à l’aide d’un CursorLoader les données à charger dans un thread avant d’être lié à un ListView pour l’affichage.

Reportez-vous à [Introduction à Contentprovider](~/android/platform/content-providers/index.md) pour plus d’informations.

