---
title: Liaison de frameworks natifs
description: Ce document décrit comment utiliser l’objectif Sharpie - framework permet de créer une liaison vers une bibliothèque distribué en tant qu’infrastructure.
ms.prod: xamarin
ms.assetid: 91AE058A-3A1F-41A9-9DE4-4B96880A1869
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
ms.openlocfilehash: 0da40918c8ae36c4ab3d4c41128429b49706d653
ms.sourcegitcommit: bf18425f97b48661ab6b775195eac76b356eeba0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2019
ms.locfileid: "64977655"
---
# <a name="binding-native-frameworks"></a>Liaison de frameworks natifs

Parfois, une bibliothèque native est distribuée comme un [framework](https://developer.apple.com/library/mac/documentation/MacOSX/Conceptual/BPFrameworks/Concepts/WhatAreFrameworks.html). Objectif Sharpie fournit une fonctionnalité pratique de liaison correctement définie infrastructures via la `-framework` option.

Par exemple, la liaison la [Adobe Creative SDK Framework](https://creativesdk.adobe.com/downloads.html) pour iOS est simple :

<pre>$ <b>sharpie bind \
    -framework AdobeCreativeSDKFoundation.framework \
    -sdk iphoneos8.1</b></pre>

Dans certains cas, une infrastructure spécifierez un **Info.plist** qui indique sur quel Kit de développement logiciel de l’infrastructure doit être compilé. Si ces informations existent et explicites ne `-sdk` est passée à l’option, l’objectif Sharpie déduira à partir de l’infrastructure **Info.plist** (soit la `DTSDKName` clé ou une combinaison de la `DTPlatformName` et `DTPlatformVersion`clés).

Le `-framework` option ne permet pas de fichiers d’en-tête explicite à passer. Le fichier d’en-tête PARAPLUIE est choisi par convention, en fonction du nom du framework. Si un en-tête PARAPLUIE est introuvable, objectif Sharpie ne tente pas de lier l’infrastructure et vous devez effectuer manuellement la liaison en fournissant les fichiers d’en-tête PARAPLUIE correct à analyser, ainsi que les arguments de framework pour clang (telles que la `-F`option de chemin d’accès de recherche de framework).

Sous le capot, en spécifiant `-framework` est simplement un raccourci. Les arguments de liaison suivants sont identiques à la `-framework` raccourci ci-dessus.
D’une importance particulière est le `-F .` framework recherche chemin d’accès fourni à clang (Notez l’espace et la période, qui sont nécessaires dans le cadre de la commande).

<pre>$ <b>sharpie bind \
    -sdk iphoneos8.1 \
    AdobeCreativeSDKFoundation.framework/Headers/AdobeCreativeSDKFoundation.h \
    -scope AdobeCreativeSDKFoundation.framework/Headers \
    -c -F .</b></pre>
