---
title: Utilisation de tailles d’écran dans le système d’exploitation Xamarin.Android et usure
ms.prod: xamarin
ms.assetid: 77831169-C663-4D42-B742-B8B556B1DA4B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/25/2018
ms.openlocfilehash: 40e7850ffe239b0ede43e4d0cd3c6da08bce3a40
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
ms.locfileid: "32436074"
---
# <a name="working-with-screen-sizes"></a>Utilisation de tailles d’écran

Les appareils Android usure peuvent avoir un rectangle ou un affichage round, ce qui peut également être de différentes tailles.

![Affiche des captures d’écran d’usure rectangulaire et arrondi](screen-sizes-images/moyeu-wear.png)

## <a name="identifying-screen-type"></a>Identifiant le Type d’écran

La bibliothèque de prise en charge d’usure fournit certains contrôles qui vous aident à détectent et s’adapter aux formes d’écran différentes, telles que `WatchViewStub` et `BoxInsetLayout`.

Gardez à l’esprit que d’autres prennent en charge les contrôles de bibliothèque (tel que `GridViewPager`) *automatiquement* détecter forme de l’écran eux-mêmes et ne doit pas être ajouté comme des contrôles enfants est décrit ci-dessous.

### <a name="watchviewstub"></a>WatchViewStub

Consultez le [WatchViewStub](https://developer.xamarin.com/samples/WatchViewStub/) pour voir comment détecter le type d’écran et d’afficher une disposition différente pour chaque type.

Le fichier de mise en page principale contient une `android.support.wearable.view.WatchViewStub` qui fait référence à des dispositions différentes pour les écrans rectangulaires et arrondis à l’aide de la `app:rectLayout` et `app:roundLayout` attributs :

```xml
<android.support.wearable.view.WatchViewStub
    xmlns:app="http://schemas.android.com/apk/res-auto"
  android:layout_width="match_parent"
  android:layout_height="match_parent"
  android:id="@+id/stub"
  app:rectLayout="@layout/rect_layout"
  app:roundLayout="@layout/round_layout" />
```

La solution contient des dispositions différentes pour chaque style qui sera activée au moment de l’exécution :

![Fichiers figurant sous ressources/mise en page](screen-sizes-images/solution.png)


### <a name="boxinsetlayout"></a>BoxInsetLayout

Plutôt que de créer des dispositions différentes pour chaque type d’écran, vous pouvez également créer une vue unique qui s’adapte aux écrans rectangulaires ou arrondis.

Cela [Google exemple](https://developer.android.com/training/wearables/ui/layouts.html#same-layout) montre comment utiliser le `BoxInsetLayout` pour la même disposition rectangulaires et arrondis des écrans.


## <a name="wear-ui-designer"></a>Le Concepteur de l’interface utilisateur d’usure

Le concepteur Android Xamarin prend en charge les écrans rectangulaires et arrondis :

![Sélection de l’écran Android usure carré dans le concepteur Android Xamarin](screen-sizes-images/design-screen-type.png)

L’aire de conception dans le style rectangulaire est illustrée ici :

![Aire de conception de style rectangulaire](screen-sizes-images/design-rect.png) 

L’aire de conception dans le style d’arrondi est illustrée ici :

![Aire de conception de style round](screen-sizes-images/design-round.png)


## <a name="wear-simulator"></a>Porter le simulateur

Le **Gestionnaire de l’émulateur Google** contient les définitions de périphériques pour les deux types de l’écran. Vous pouvez créer des émulateurs rectangulaires et arrondis pour tester votre application.

![Porter des définitions de périphériques indiquées dans le Gestionnaire de l’émulateur Google](screen-sizes-images/emulator-devices.png)

Pour un écran rectangulaire, l’émulateur sera restitué comme suit :

![Émulateur de rendu d’un écran rectangulaire](screen-sizes-images/recipe-2.png) 

Il sera restitué comme suit pour un écran arrondi :

![Émulateur de rendu d’un écran round](screen-sizes-images/recipe-2-round.png)

## <a name="video"></a>Vidéo

[Les applications en plein écran pour Android usure](https://www.youtube.com/watch?v=naf_WbtFAlY) de [developers.google.com](https://www.youtube.com/channel/UC_x5XG1OV2P6uZZ5FSM9Ttw).

