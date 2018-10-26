---
title: Où puis-je trouver le fichier .dSYM pour générer les symboles journaux d’incidents iOS ?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: CB8607B9-FFDA-4617-8210-8E43EC512588
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/09/2018
ms.openlocfilehash: 0b8f3aa736cba6e70fbf346766499c23a9bbe270
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50114065"
---
# <a name="where-can-i-find-the-dsym-file-to-symbolicate-ios-crash-logs"></a>Où puis-je trouver le fichier .dSYM pour générer les symboles journaux d’incidents iOS ?

Lorsque vous générez une application iOS avec Visual Studio pour Mac ou Visual Studio 2017, le fichier .dSYM qui est nécessaire pour générer les symboles des rapports d’incident sera placé dans la même hiérarchie de répertoire en tant que fichier de projet de votre application (.csproj). L’emplacement exact dépend des paramètres de build de votre projet :

- Si vous avez activé des builds spécifiques aux appareils, vous pouvez trouver le .dSYM dans le répertoire suivant :

    **&lt;répertoire du projet&gt;/bin/&lt;plateforme&gt;/&lt;configuration&gt;/device-builds /&lt;appareil&gt; - &lt; version du système d’exploitation&gt;/**

    Exemple :
  
    **TestApp/bin/iPhone/Release/device-builds/iphone8.4-11.3.1/**

- Si vous n’avez pas activé les builds spécifiques à l’appareil, vous pouvez trouver le .dSYM dans le répertoire suivant :

    **&lt;répertoire du projet&gt;/bin/&lt;plateforme&gt;/&lt;configuration&gt;/**

    Exemple :

    **TestApp/bin/iPhone/version /**

> [!NOTE]
> Dans le cadre du processus de génération, Visual Studio 2017 copie le fichier .dSYM à partir de l’hôte de build Mac pour Windows. Si vous ne voyez pas un fichier .dSYM sur Windows, veillez à ce que vous avez configuré les paramètres de génération de votre application à [créer un fichier .ipa](~/ios/deploy-test/app-distribution/ipa-support.md).

## <a name="see-also"></a>Voir aussi

- [Symbolicating iOS se bloquer des fichiers (Xamarin.iOS)](http://jmillerdev.net/symbolicating-ios-crash-files-xamarin-ios/)
- [Démystification de journaux se bloquer des applications d’iOS](https://www.raywenderlich.com/23704/demystifying-ios-application-crash-logs)

