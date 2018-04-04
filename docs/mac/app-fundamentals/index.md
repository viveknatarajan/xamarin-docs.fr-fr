---
title: Principes de base des applications
description: Ce document liens vers des guides qui décrivent les différents concepts nécessaires pour comprendre le développement d’applications de Xamarin.Mac.
ms.prod: xamarin
ms.assetid: 5A36B3A7-F197-4AC3-A40D-B2C49362FF06
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 12/17/2015
ms.openlocfilehash: 807cf0e16cafc00483cecfc578367bc110657672
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="application-fundamentals"></a>Principes de base des applications

## <a name="common-patterns-and-idiomsmacapp-fundamentalspatternsmd"></a>[Des modèles et des idiomes courants](~/mac/app-fundamentals/patterns.md)

Dans les API Apple exposés via c#, certains langages et les modèles de démarrage indéfiniment. Si vous avez aucune expérience en programmation avec Xamarin.iOS, il peuvent sembler familières. Documentation fait souvent référence à ces et idiomes à plusieurs reprises, afin d’avoir une connaissance approfondie des vous permettra de sens de la documentation que vous trouverez.

## <a name="understanding-mac-apismacapp-fundamentalsmac-apismd"></a>[Compréhension des API Mac](~/mac/app-fundamentals/mac-apis.md)

Pour une grande partie de votre temps à développer avec Xamarin.Mac, vous pouvez considérer, lire et écrire en c#, sans trop vous soucier avec les API sous-jacentes Objective-C. Cependant, parfois, allez à lire la documentation de l’API à partir d’Apple, traduire une réponse à partir de débordement de pile pour une solution pour votre problème, ou que vous comparer à un exemple existant.

## <a name="working-with-xib-filesmacapp-fundamentalsxibmd"></a>[Utilisation des fichiers de .xib](~/mac/app-fundamentals/xib.md)

Cet article traite de travail avec les fichiers .xib créés dans le Générateur de Xcode Interface pour créer et gérer des interfaces utilisateur pour une application Xamarin.Mac.

## <a name="storyboardxib-less-user-interface-designmacapp-fundamentalsxibless-uimd"></a>[.Storyboard/.XIB moins de conception de l’interface utilisateur](~/mac/app-fundamentals/xibless-ui.md)

Cet article décrit la création d’interface utilisateur d’une application Xamarin.Mac directement à partir de code c# sans utiliser le Générateur de Xcode Interface avec les fichiers .storyboard ou .xib.

## <a name="working-with-imagesmacapp-fundamentalsimagemd"></a>[Utilisation des images](~/mac/app-fundamentals/image.md)

Cet article décrit l’utilisation des images et des icônes dans une application Xamarin.Mac. Traite de la création et en conservant les images nécessaires pour créer une icône et l’utilisation d’images dans le code c# et Interface Builder de Xcode de votre application.

## <a name="data-binding-and-key-value-codingmacapp-fundamentalsdatabindingmd"></a>[Liaison de données et la clé-valeur de codage](~/mac/app-fundamentals/databinding.md)

Cet article couvre l’utilisation de la clé-valeur de codage et observer pour permettre la liaison de données pour les éléments d’interface utilisateur dans le Générateur de Xcode Interface clé-valeur. À l’aide de cette technique, vous réduisez considérablement la quantité de code c# qui doive être écrites pour votre application Xamarin.Mac. 

## <a name="working-with-databasesmacapp-fundamentalsdatabasesmd"></a>[Utilisation des bases de données](~/mac/app-fundamentals/databases.md)

Cet article couvre l’utilisation de la clé-valeur de codage et observer pour permettre la liaison de données avec un accès direct aux bases de données SQLite pour les éléments d’interface utilisateur dans le Générateur de Xcode Interface clé-valeur. Elle traite également à l’aide de l’ORM SQLite.NET pour fournir l’accès aux données de SQLite.

## <a name="working-with-copy-and-pastemacapp-fundamentalscopy-pastemd"></a>[Utilisation de copier et coller](~/mac/app-fundamentals/copy-paste.md)

Cet article décrit l’utilisation de la table de montage pour fournir de copie et de coller dans une application Xamarin.Mac. Il montre comment travailler avec les types de données standard qui peuvent être partagés entre plusieurs applications et la prise en charge des données personnalisées dans une application donnée.

## <a name="sandboxing-a-xamarinmac-appmacapp-fundamentalssandboxingmd"></a>[Sandboxing une application Xamarin.Mac](~/mac/app-fundamentals/sandboxing.md)

Cet article traite des sandboxing une application Xamarin.Mac version sur l’App Store. Elle couvre tous les éléments nécessaires à la mise en sandbox : répertoires de conteneur, droits, déterminé par l’utilisateur des autorisations, privilège la séparation et mise en œuvre du noyau.

## <a name="playing-sound-with-avaudioplayermacapp-fundamentalssoundsmd"></a>[Lecture du son avec AVAudioPlayer](~/mac/app-fundamentals/sounds.md)

Cet article explique comment utiliser une classe d’assistance pour contrôler la lecture de son à l’aide d’un AVAudioPlayer.

## <a name="reporting-bugsmacapp-fundamentalstroubleshootingmd"></a>[Signalement des bogues](~/mac/app-fundamentals/troubleshooting.md)

Nous avons tous utilisée comme parfois bloquées tout en travaillant sur un projet sur l’incapacité à obtenir une API pour fonctionner comme nous voulons ou en essayant de contourner un bogue. Notre objectif à Xamarin consiste à réussir lors de l’écriture de vos applications de bureau et mobiles, et nous vous avons fourni des ressources à l’aide.
