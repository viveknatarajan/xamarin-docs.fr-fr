---
title: Erreur de concepteur iOS avec RegisterServicePort
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 929A0080-B126-4744-BF88-A4A1EFBB6CC2
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 04/03/2018
ms.openlocfilehash: fc4c143d6b5f7c211d24e6e3ed2ed3bb8d264410
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61421968"
---
# <a name="ios-designer-error-with-registerserviceport"></a>Erreur de concepteur iOS avec RegisterServicePort

## <a name="sample-error"></a>Exemple d’erreur
> System.AggregateException: Une ou plusieurs erreurs se sont produites---> System.SystemException : RegisterServicePort (com.xamarin.MTHosting.2a0b1, com.apple.PowerManagement.control) : Noyau retourné : -308 (-308) : serveur (ipc/mig) a été arrêtée

## <a name="explanation"></a>Explication
Erreurs avec `RegisterServicePort` et messages d’erreur similaires comme ci-dessus sont généralement un problème avec les logiciels espions/contre les programmes malveillants sur l’ordinateur. Pensez au [le commentaire sur ce rapport de bogue](https://bugzilla.xamarin.com/show_bug.cgi?id=21907#c4) pour plus d’informations, ainsi que le lien vers le [forum de discussion Apple](https://discussions.apple.com/thread/5596008) sur la suppression d’une infection. 

Pour aider à diagnostiquer le problème, ouvrez l’application macOS **Console** et supprimer tous les fichiers à l’intérieur de la **les rapports de diagnostic utilisateur** section [ http://screencast.com/t/y9i3NKcuMy ](http://screencast.com/t/y9i3NKcuMy). Démarrez Visual Studio pour Mac, puis essayez d’utiliser le concepteur. Si les nouveaux fichiers journaux apparaissent dans cette section, une fois que le concepteur n’a pas pu initialiser, enregistrez ces valeurs pour nous à analyser.  

Veuillez noter la chose la plus importante à vérifier est ce fichier : 
> /usr/lib/libimckit.dylib

Quel que soit les résultats ci-dessus, si ce fichier existe, le problème de logiciels espions/programmes malveillants mentionnés ci-dessus est présent sur votre ordinateur.  

Le lien suivant présente les étapes pour les supprimer les logiciels espions / : [http://www.thesafemac.com/arg-genieo/](http://www.thesafemac.com/arg-genieo/)  

