---
title: "Android.Support.v7.AppCompat - Aucune ressource trouvée correspondant au nom donné : attr 'android:actionModeShareDrawable'"
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5814069C-FC43-41DE-B5A5-024D05E59929
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: ff2a586773e33a1f4cf78657c3c69c22e79ed047
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61153371"
---
# <a name="androidsupportv7appcompat---no-resource-found-that-matches-the-given-name-attr-androidactionmodesharedrawable"></a>Android.Support.v7.AppCompat - Aucune ressource trouvée correspondant au nom donné : attr 'android:actionModeShareDrawable'

1. Veillez à que télécharger les dernière extras, ainsi que le Android 5.0 SDK via le gestionnaire Android SDK (API 21).

2. Assurez-vous que vous compilez votre application avec compileSdkVersion réglée sur 21. Vous pouvez éventuellement définir la targetSdkVersion 21 également.

3. Si vous avez besoin d’une version antérieure comme API 19, téléchargez la version respective, consultez la page Nuget :

[https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)

*Remarque* : Si vous installez manuellement ce via la Console du Gestionnaire de Package, assurez-vous que vous installez également la même version de Xamarin.Android.Support.v4

[https://www.nuget.org/packages/Xamarin.Android.Support.v4/](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)

Référence de dépassement de capacité de pile : [https://stackoverflow.com/questions/26431676/appcompat-v721-0-0-no-resource-found-that-matches-the-given-name-attr-andro](https://stackoverflow.com/questions/26431676/appcompat-v721-0-0-no-resource-found-that-matches-the-given-name-attr-andro)

## <a name="see-also"></a>Voir aussi

- [Quels packages Android SDK dois-je installer ?](~/android/troubleshooting/questions/install-android-sdk-packages.md)

