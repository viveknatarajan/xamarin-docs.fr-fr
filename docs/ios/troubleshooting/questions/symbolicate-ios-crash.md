---
title: Où puis-je trouver le fichier .dSYM symbolicate journaux iOS ?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: CB8607B9-FFDA-4617-8210-8E43EC512588
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/09/2018
ms.openlocfilehash: 60d897be8739ff5b78a322bc4ea3f43011785bb5
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2018
---
# <a name="where-can-i-find-the-dsym-file-to-symbolicate-ios-crash-logs"></a>Où puis-je trouver le fichier .dSYM symbolicate journaux iOS ?

Lorsque vous créez une application iOS avec Visual Studio pour Mac ou Visual Studio 2017, le fichier .dSYM qui est nécessaire pour les rapports d’incidents de symbolicate sera placé dans la même hiérarchie de répertoires en tant que fichier de projet de votre application (.csproj). L’emplacement exact dépend des paramètres de génération de votre projet :

- Si vous avez activé les builds spécifiques à l’appareil, le le .dSYM peut être trouvé dans le répertoire suivant :

    **&lt;répertoire du projet&gt;/bin/&lt;plateforme&gt;/&lt;configuration&gt;/device-builds /&lt;périphérique&gt; - &lt; version de système d’exploitation&gt;/**

    Par exemple :
  
    **TestApp/bin/iPhone/Release/device-builds/iphone8.4-11.3.1/**

- Si vous n’avez pas activé les builds spécifiques à l’appareil, vous pouvez trouver le .dSYM dans le répertoire suivant :

    **&lt;répertoire du projet&gt;/bin/&lt;plateforme&gt;/&lt;configuration&gt;/**

    Par exemple :

    **TestApp/bin/iPhone/version /**

> [!NOTE]
> Dans le cadre du processus de génération, Visual Studio 2017 copie le fichier .dSYM à partir de l’hôte de build Mac à Windows. Si vous ne voyez pas un fichier .dSYM sous Windows, veillez à ce que vous avez configuré les paramètres de génération de votre application à [créer un fichier .ipa](~/ios/deploy-test/app-distribution/ipa-support.md).

## <a name="see-also"></a>Voir aussi

- [Symbolicating iOS fichiers incident (Xamarin.iOS)](http://jmillerdev.net/symbolicating-ios-crash-files-xamarin-ios/)
- [Démystification iOS journaux de blocage d’Application](https://www.raywenderlich.com/23704/demystifying-ios-application-crash-logs)

