---
title: Liaison des infrastructures natif
ms.prod: xamarin
ms.assetid: 91AE058A-3A1F-41A9-9DE4-4B96880A1869
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
ms.openlocfilehash: 52b845d9e062eea6292528c5a40a74aa67d8e1b7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="binding-native-frameworks"></a>Liaison des infrastructures natif

Parfois, une bibliothèque native est distribuée comme un [framework](https://developer.apple.com/library/mac/documentation/MacOSX/Conceptual/BPFrameworks/Concepts/WhatAreFrameworks.html). Objectif Sharpie fournit une fonctionnalité de commodité pour liaison correctement défini infrastructures via le `-framework` option.

Par exemple, la liaison la [Adobe Creative SDK Framework](https://creativesdk.adobe.com/downloads.html) pour iOS est simple :

<pre>$ <b>sharpie bind \
    -framework AdobeCreativeSDKFoundation.framework \
    -sdk iphoneos8.1</b></pre>

Dans certains cas, une infrastructure spécifie une **Info.plist** qui indique sur le Kit de développement logiciel de l’infrastructure doit être compilé. Si ces informations existent et non explicites `-sdk` option est passée, objectif Sharpie déduit à partir de l’infrastructure **Info.plist** (soit la `DTSDKName` clé ou une combinaison de la `DTPlatformName` et `DTPlatformVersion`clés).

Le `-framework` option n’autorise pas les fichiers d’en-tête explicite à passer. Le fichier d’en-tête couvrant est choisi par convention basée sur le nom d’infrastructure. Si un en-tête couvrant ne peut pas être trouvé, objectif Sharpie ne tente pas de l’infrastructure de liaison et vous devez effectuer manuellement la liaison en fournissant les fichiers d’en-tête couvrant correct à analyser, ainsi que les arguments de l’infrastructure pour clang (telles que la `-F`option de chemin d’accès de recherche de framework).

Dans les coulisses, en spécifiant `-framework` est simplement un raccourci. Les arguments de liaison suivants sont identiques à la `-framework` raccourcie ci-dessus.
Une importance particulière est le `-F .` chemin de recherche de framework fournie pour clang (Notez l’espace et la période, qui sont nécessaires dans le cadre de la commande).

<pre>$ <b>sharpie bind \
    -sdk iphoneos8.1 \
    AdobeCreativeSDKFoundation.framework/Headers/AdobeCreativeSDKFoundation.h \
    -scope AdobeCreativeSDKFoundation.framework/Headers \
    -c -F .</b></pre>

