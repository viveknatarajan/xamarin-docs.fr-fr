---
title: Principes de base application Xamarin.Mac
description: Ce document contient des liens vers des guides qui décrivent les différents concepts nécessaires pour comprendre lors du développement d’applications Xamarin.Mac.
ms.prod: xamarin
ms.assetid: 5A36B3A7-F197-4AC3-A40D-B2C49362FF06
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 12/17/2015
ms.openlocfilehash: e085cf33615d216e1ce9963254050ef2b623ae40
ms.sourcegitcommit: d0da5ce4158239abd2b36f67550e9b475055766a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39320802"
---
# <a name="xamarinmac-application-fundamentals"></a>Principes de base application Xamarin.Mac

## <a name="common-patterns-and-idiomsmacapp-fundamentalspatternsmd"></a>[Modèles et idiomes courants](~/mac/app-fundamentals/patterns.md)

Tout au long de l’API Apple exposées via c#, certains modèles et idiomes apparu indéfiniment. Si vous êtes habitué avec la programmation avec Xamarin.iOS, il peuvent vous paraître familiers. Documentation fait souvent référence à ces modèles et idiomes d’à plusieurs reprises, afin d’avoir une connaissance approfondie des vous permettra de sens de la documentation que vous trouverez.

## <a name="understanding-mac-apismacapp-fundamentalsmac-apismd"></a>[Présentation des API Mac](~/mac/app-fundamentals/mac-apis.md)

Pour une grande partie de votre temps à développer avec Xamarin.Mac, vous pouvez donc réfléchir, lire et écrire en c#, sans se soucier de bien avec les API Objective-C sous-jacente. Cependant, parfois, allez à lire la documentation de l’API d’Apple, traduire une réponse de Stack Overflow à une solution à votre problème, ou que vous comparer à un exemple existant.

## <a name="console-appsmacapp-fundamentalsconsolemd"></a>[Applications console](~/mac/app-fundamentals/console.md)

Vous pouvez également créer des applications de console « headless » qui accèdent aux API de macOS native à l’aide de Xamarin.Mac.

## <a name="working-with-xib-filesmacapp-fundamentalsxibmd"></a>[Utilisation des fichiers .xib](~/mac/app-fundamentals/xib.md)

Cet article aborde l’utilisation avec les fichiers .xib créés dans l’Interface Builder de Xcode pour créer et gérer des interfaces utilisateur pour une application Xamarin.Mac.

## <a name="storyboardxib-less-user-interface-designmacapp-fundamentalsxibless-uimd"></a>[.Storyboard/.XIB moins de conception de l’interface utilisateur](~/mac/app-fundamentals/xibless-ui.md)

Cet article aborde la création d’interface utilisateur d’une application Xamarin.Mac sans utiliser Interface Builder de Xcode avec fichiers .storyboard ou .xib directement à partir de code c#.

## <a name="working-with-imagesmacapp-fundamentalsimagemd"></a>[Utilisation d’images](~/mac/app-fundamentals/image.md)

Cet article aborde l’utilisation des images et icônes dans une application Xamarin.Mac. Il aborde la création et gestion des images nécessaires pour créer l’icône de votre application et l’utilisation d’images dans le code c# et Interface Builder de Xcode.

## <a name="data-binding-and-key-value-codingmacapp-fundamentalsdatabindingmd"></a>[Liaison de données et le codage de clé-valeur](~/mac/app-fundamentals/databinding.md)

Cet article couvre l’utilisation de la clé-valeur de codage et en observant pour permettre la liaison de données pour les éléments d’interface utilisateur dans l’Interface Builder de Xcode clé-valeur. À l’aide de cette technique, vous réduisez considérablement la quantité de code c# qui doit être écrit pour votre application Xamarin.Mac. 

## <a name="working-with-databasesmacapp-fundamentalsdatabasesmd"></a>[Utilisation des bases de données](~/mac/app-fundamentals/databases.md)

Cet article couvre l’utilisation de la clé-valeur de codage et en observant pour permettre la liaison de données avec un accès direct aux bases de données SQLite pour les éléments d’interface utilisateur dans l’Interface Builder de Xcode clé-valeur. Il couvre également l’utilisation de la SQLite.NET ORM pour fournir l’accès aux données de SQLite.

## <a name="working-with-copy-and-pastemacapp-fundamentalscopy-pastemd"></a>[Utilisation de copier et coller](~/mac/app-fundamentals/copy-paste.md)

Cet article aborde l’utilisation avec la table de montage pour fournir les copier et coller dans une application Xamarin.Mac. Il montre comment travailler avec des types de données standard qui peuvent être partagés entre plusieurs applications et comment prendre en charge des données personnalisées dans une application donnée.

## <a name="sandboxing-a-xamarinmac-appmacapp-fundamentalssandboxingmd"></a>[Sandboxing d’une application Xamarin.Mac](~/mac/app-fundamentals/sandboxing.md)

Cet article traite de sandboxing une application Xamarin.Mac relatives à la version sur l’App Store. Il couvre tous les éléments qui entrent dans le bac à sable : répertoires de conteneur, droits, déterminées par l’utilisateur des autorisations, séparation de privilèges et mise en œuvre du noyau.

## <a name="playing-sound-with-avaudioplayermacapp-fundamentalssoundsmd"></a>[Lecture audio avec AVAudioPlayer](~/mac/app-fundamentals/sounds.md)

Cet article explique comment utiliser une classe d’assistance pour contrôler la lecture du son à l’aide d’un AVAudioPlayer.

## <a name="reporting-bugsmacapp-fundamentalstroubleshootingmd"></a>[Signalement des bogues](~/mac/app-fundamentals/troubleshooting.md)

Parfois, nous sommes bloqués pendant que vous travaillez sur un projet, l’impossibilité d’obtenir une API permettant de travailler comme nous voulons soit en tentant de contourner un bogue. Notre objectif chez Xamarin est pour vous permettre de réussir à écrire vos applications mobiles et de bureau, et nous vous avons fourni quelques ressources pour aider.
