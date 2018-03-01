---
title: "Où puis-je trouver le fichier .dSYM symbolicate journaux iOS ?"
ms.topic: article
ms.prod: xamarin
ms.assetid: CB8607B9-FFDA-4617-8210-8E43EC512588
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 223e1977bb6229760d6428fdca2f5372b1e25c23
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="where-can-i-find-the-dsym-file-to-symbolicate-ios-crash-logs"></a>Où puis-je trouver le fichier .dSYM symbolicate journaux iOS ?

Lorsque vous générez des applications iOS à partir de visual studio, le fichier .dSYM qui peut être utilisé pour les rapports d’incidents de symbolicate se termine sur l’hôte de build au chemin d’accès :
```
    /Users/<username>/Library/Caches/Xamarin/mtbs/builds/<appname>/<guid>/bin/iPhone/<configuration>
```

Notez que la `~/Library` dossier est masqué par défaut dans le Finder, ainsi, si vous devez utiliser du Finder **accédez > accédez au dossier** menu, puis entrez : `~/Library/Caches/Xamarin/mtbs/builds/` pour ouvrir le dossier.  

Ou bien vous pouvez afficher le `~/Library` dossier à l’aide de la **afficher les Options de vue** Panneau de configuration pour votre dossier de base. Si vous sélectionnez votre dossier de base dans la barre latérale dans le Finder et que vous utilisez le menu Finder **Affichage > Afficher les Options de vue** (ou cmd-j), puis vous verrez une case à cocher pour **afficher le dossier de bibliothèque**.


### <a name="see-also"></a>Voir aussi
- Étapes étendus pour symbolicating iOS crash rapports : [http://jmillerdev.net/symbolicating-ios-crash-files-xamarin-ios/](http://jmillerdev.net/symbolicating-ios-crash-files-xamarin-ios/)
- [Démystification iOS journaux de blocage d’Application](https://www.raywenderlich.com/23704/demystifying-ios-application-crash-logs)
