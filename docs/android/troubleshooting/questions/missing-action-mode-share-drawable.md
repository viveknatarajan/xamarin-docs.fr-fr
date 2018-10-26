---
title: "Android.Support.v7.AppCompat - aucune ressource trouvée qui correspond au nom donné : attr 'android : actionModeShareDrawable'"
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5814069C-FC43-41DE-B5A5-024D05E59929
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: fea681ac3b99abed09d3d3e745bd4bf6015970df
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50112414"
---
# <a name="androidsupportv7appcompat---no-resource-found-that-matches-the-given-name-attr-androidactionmodesharedrawable"></a>Android.Support.v7.AppCompat - aucune ressource trouvée qui correspond au nom donné : attr 'android : actionModeShareDrawable'

1. Veillez à que télécharger les dernière extras, ainsi que le Android 5.0 SDK via le gestionnaire Android SDK (API 21).

2. Assurez-vous que vous compilez votre application avec compileSdkVersion réglée sur 21. Vous pouvez éventuellement définir la targetSdkVersion 21 également.

3. Si vous avez besoin d’une version antérieure comme API 19, téléchargez la version respective, consultez la page Nuget :

[https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)

*Remarque*: Si vous installez manuellement ce via la Console du Gestionnaire de Package, assurez-vous que vous installez également la même version de Xamarin.Android.Support.v4

[https://www.nuget.org/packages/Xamarin.Android.Support.v4/](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)

Référence de dépassement de capacité de pile : [http://stackoverflow.com/questions/26431676/appcompat-v721-0-0-no-resource-found-that-matches-the-given-name-attr-andro](http://stackoverflow.com/questions/26431676/appcompat-v721-0-0-no-resource-found-that-matches-the-given-name-attr-andro)

## <a name="see-also"></a>Voir aussi

- [Quels packages Android SDK dois-je installer ?](~/android/troubleshooting/questions/install-android-sdk-packages.md)

