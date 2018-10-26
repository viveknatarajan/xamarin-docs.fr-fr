---
title: À l’aide d’un ContentProvider
ms.prod: xamarin
ms.assetid: 251F7557-328D-0132-F39D-595920A28B87
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: d1ec628de3481820f320a5a8e6ef88fcbaab75a6
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50122112"
---
# <a name="using-a-contentprovider"></a>À l’aide d’un ContentProvider

CursorAdapters peut également être utilisé pour afficher des données à partir d’un ContentProvider.
Contentprovider vous permettre d’accéder aux données exposées par d’autres applications (y compris les données système Android comme contacts, les informations de support et de calendrier).

La meilleure façon d’accéder à un ContentProvider est avec un CursorLoader à l’aide de la LoaderManager. LoaderManager a été introduit dans 3.0 Android (API niveau 11, Honeycomb) pour déplacer les tâches de blocage du thread principal, ainsi que d’à l’aide d’un CursorLoader les données à charger dans un thread avant d’être lié à un ListView pour l’affichage.

Reportez-vous à [Introduction à Contentprovider](~/android/platform/content-providers/index.md) pour plus d’informations.

