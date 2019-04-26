---
title: Encodages d’internationalisation dans Xamarin.iOS
description: Ce document décrit les encodages d’internationalisation dans Xamarin.iOS, en traitant les encodages disponibles et comment les ajouter à une application.
ms.prod: xamarin
ms.assetid: F5117294-28BB-4583-B6A0-A339B050FDE1
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 04/28/2017
ms.openlocfilehash: db24c8677b0a3099193132575e92bc43a4c31dea
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61251019"
---
# <a name="internationalization-encodings-in-xamarinios"></a>Encodages d’internationalisation dans Xamarin.iOS

Pas tous les encodages sont inclus dans la bibliothèque de classes Xamarin.iOS par défaut.

Pour réduire la taille de l’application, Xamarin.iOS n’inclut pas de n’importe quel encodage spécifique, et vous devez demander à mtouch à inclure les assemblys contenant la prise en charge pour l’encodage que vous avez besoin.

Cette opération est effectuée en sélectionnant les encodages supplémentaires à partir du volet de la Build/avancé iOS dans Visual Studio pour Mac ou Visual Studio :

 [![](encodings-images/00.png "En sélectionnant les encodages supplémentaires")](encodings-images/00.png#lightbox)

 [![](encodings-images/00a.png "En sélectionnant les encodages supplémentaires")](encodings-images/00a.png#lightbox)

Vous pouvez sélectionner une d’elles :

-  CJC : pour Chineese, japonais et coréen
-  mideast : Arabe, hébreu, turc et Latin5.
-  Autres : Cyrillique, Baltique, vietnamien, ukrainien et thaï
-  rares : EBCDIC encodages et autres pages de codes rares
-  ouest des États-Unis : Langues latines, Pâques et Europe occidentale
-  all


 <a name="cjk" />


## <a name="cjk"></a>cjk

-  CP51932
-  CP932
-  CP936
-  CP949
-  CP950
-  CP54936


 <a name="mideast" />


## <a name="mideast"></a>mideast

-  CP1254
-  CP1255
-  CP1256
-  CP28596
-  CP28598
-  CP28599
-  CP38598


 <a name="other" />


## <a name="other"></a>other

-  CP1251
-  CP1257
-  CP1258
-  CP20866
-  CP21866
-  CP28594
-  CP28595
-  CP57002
-  CP874


 <a name="rare" />


## <a name="rare"></a>rares

-  CP1026
-  CP1047
-  CP1140
-  CP1141
-  CP1142
-  CP1143
-  CP1144
-  CP1145
-  CP1146
-  CP1147
-  CP1148
-  CP1149
-  CP20273
-  CP20277
-  CP20278
-  CP20280
-  CP20284
-  CP20285
-  CP20290
-  CP20297
-  CP20420
-  CP20424
-  CP20871
-  CP21025
-  CP37
-  CP500
-  CP708
-  CP852
-  CP855
-  CP857
-  CP858
-  CP862
-  CP864
-  CP866
-  CP869
-  CP870
-  CP875


 <a name="west" />


## <a name="west"></a>Ouest des États-Unis

-  CP10000
-  CP10079
-  CP1250
-  CP1252
-  CP1253
-  CP28592
-  CP28593
-  CP28597
-  CP28605
-  CP437
-  CP850
-  CP860
-  CP861
-  CP863
-  CP865

