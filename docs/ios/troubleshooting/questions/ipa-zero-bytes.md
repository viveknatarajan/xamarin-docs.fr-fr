---
title: Le fichier IPA contient 0 octet
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 376BBA27-8694-4E63-9976-BF60349D42D8
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 4835c980b6b11c92ec1c81dea69f229aa5652275
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61420933"
---
# <a name="ipa-file-is-0-bytes"></a>Le fichier IPA contient 0 octet

> [!IMPORTANT]
> Ce problème a été résolu dans les versions récentes de Xamarin. Toutefois, si le problème se produit sur la dernière version du logiciel, veuillez soumettre un [nouveau bogue](~/cross-platform/troubleshooting/questions/howto-file-bug.md) avec votre contrôle de version complet intégral et les informations de générer la sortie de journal.



Certains problèmes se sont produites dans les versions précédentes de Xamarin qui peut entraîner le fichier IPA sur Windows à être de 0 octet. 

### <a name="fixed-in-xamarin-for-visual-studio-311584"></a>Résolu dans Xamarin pour Visual Studio 3.11.584 
- [Bogue 24416 - configuration de génération « Ad Hoc » à partir de la ligne de commande n’envoie pas copie IPA pour Windows](https://bugzilla.xamarin.com/show_bug.cgi?id=24416)
- [Bogue 24417 - modification de « propriétés du projet -> iOS IPA Options -> nom du Package « empêche IPA soient copiés vers Windows](https://bugzilla.xamarin.com/show_bug.cgi?id=24417)
- [Bogue 29822 - [XVS.iOS 3.11] paramètre « Build » nombre différent de « Version » numéro causes IPA ne pas à copier sur Windows](https://bugzilla.xamarin.com/show_bug.cgi?id=29822)

### <a name="fixed-in-xamarin-for-visual-studio-410496"></a>Résolu dans Xamarin pour Visual Studio 4.1.0.496
- [Bogue 27989 - afficher le fichier ipa sur la Build échoue de serveur si le nom d’Assembly ne correspond pas le nom du projet](https://bugzilla.xamarin.com/show_bug.cgi?id=27989)
