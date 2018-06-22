---
title: Erreur du concepteur avec RegisterServicePort d’iOS
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 929A0080-B126-4744-BF88-A4A1EFBB6CC2
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 62d4a06c62bffb23566f4f59f42a0c980f417c45
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30776714"
---
# <a name="ios-designer-error-with-registerserviceport"></a>Erreur du concepteur avec RegisterServicePort d’iOS

## <a name="sample-error"></a>Exemple d’erreur
> System.AggregateException : Une ou plusieurs erreurs se sont produites, ce---> System.SystemException : RegisterServicePort (com.xamarin.MTHosting.2a0b1, com.apple.PowerManagement.control) : noyau retourné : -308 (-308) : serveur (ipc/mig) a été arrêtée

## <a name="explanation"></a>Explication
Erreurs avec `RegisterServicePort` et messages d’erreur semblables comme ci-dessus sont généralement un problème avec les logiciels espions/programmes malveillants sur l’ordinateur. Pensez au [le commentaire sur ce rapport de bogue](https://bugzilla.xamarin.com/show_bug.cgi?id=21907#c4) pour plus d’informations, ainsi que le lien vers le [forum de discussion Apple](https://discussions.apple.com/thread/5596008) comment supprimer une infection. 

Pour vous aider à diagnostiquer le problème, ouvrez l’application macOS **Console** et supprimer tous les fichiers à l’intérieur de la **les rapports de diagnostic utilisateur** section [ http://screencast.com/t/y9i3NKcuMy ](http://screencast.com/t/y9i3NKcuMy). Démarrez Visual Studio pour Mac, puis essayez d’utiliser le concepteur. Si les nouveaux fichiers journaux apparaissent dans cette section une fois que le concepteur n’a pas pu initialiser, veuillez enregistrer ces pour nous à analyser.  

Notez également le plus important à vérifier est ce fichier : 
> /usr/lib/libimckit.dylib

Quel que soit les résultats ci-dessus, si ce fichier existe, le problème de logiciels espions/programmes malveillants qui est présent sur votre ordinateur.  

Le lien suivant présente les étapes pour supprimer les logiciels espions / : [http://www.thesafemac.com/arg-genieo/](http://www.thesafemac.com/arg-genieo/)  

