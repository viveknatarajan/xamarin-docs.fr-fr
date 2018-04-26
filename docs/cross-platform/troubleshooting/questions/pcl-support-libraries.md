---
title: Comment puis-je afficher les bibliothèques sont prises en charge dans une bibliothèque de classes portables ?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 14FF03BD-AF41-4DB1-B307-2349C13DE7E4
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: 7875fc47b1caac025488b8b71bdbd909844e7823
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/26/2018
---
# <a name="how-can-i-view-what-libraries-are-supported-in-a-pcl"></a>Comment puis-je afficher les bibliothèques sont prises en charge dans une bibliothèque de classes portables ?

- Vous trouverez une vue d’ensemble des différentes fonctionnalités prises en charge par les différentes plateformes cibles de bibliothèque PCL sous le *prise en charge des fonctionnalités* partie de cette page : [http://msdn.microsoft.com/library/gg597391.aspx](https://msdn.microsoft.com/library/gg597391.aspx)

- Une autre option consiste à utiliser le [Analyseur de portabilité .NET](https://visualstudiogallery.msdn.microsoft.com/1177943e-cfb7-4822-a8a6-e56c7905292b) afin d’évaluer si votre bibliothèque existante peut être converti en un profil de bibliothèque de classes portables.

- Une troisième possibilité est pour parcourir le contenu du profil réel que vous utilisez. À l’aide de profil 78 par exemple, vous pouvez accéder ici : `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETPortable\v4.5\Profile\Profile78\` et afficher tous les assemblys qu’il contient.

Quelle que soit la méthode choisie, veuillez Notez que certaines fonctionnalités doit être téléchargé via NuGet et la bibliothèque Microsoft BCL.
