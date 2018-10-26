---
title: Utilisation de tailles d’écran dans Xamarin.Android et l’usure du système d’exploitation
ms.prod: xamarin
ms.assetid: 77831169-C663-4D42-B742-B8B556B1DA4B
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/25/2018
ms.openlocfilehash: 9fc22a3c08b60a8474b006f1c9225155b9705507
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50113116"
---
# <a name="working-with-screen-sizes"></a>Utilisation de tailles d’écran

Les appareils Android Wear peuvent avoir un rectangulaire ou un affichage round, ce qui peut également être de différentes tailles.

![Affiche des captures d’écran d’usure rectangulaire et round](screen-sizes-images/moyeu-wear.png)

## <a name="identifying-screen-type"></a>Identifiant le Type d’écran

La bibliothèque de prise en charge de l’usure fournit certains contrôles qui vous aident à détectent et s’adapter aux formes d’écran différentes, telles que `WatchViewStub` et `BoxInsetLayout`.

N’oubliez pas que d’autres prennent en charge les contrôles de bibliothèque (tel que `GridViewPager`) *automatiquement* détecter de forme de l’écran eux-mêmes et ne doit pas être ajouté comme enfants des contrôles décrit ci-dessous.

### <a name="watchviewstub"></a>WatchViewStub

Consultez le [WatchViewStub](https://developer.xamarin.com/samples/WatchViewStub/) exemple pour voir comment détecter le type d’écran et d’afficher une disposition différente pour chaque type.

Le fichier de disposition principale contient un `android.support.wearable.view.WatchViewStub` qui fait référence à des dispositions différentes pour les écrans rectangulaires et arrondis à l’aide de la `app:rectLayout` et `app:roundLayout` attributs :

```xml
<android.support.wearable.view.WatchViewStub
    xmlns:app="http://schemas.android.com/apk/res-auto"
  android:layout_width="match_parent"
  android:layout_height="match_parent"
  android:id="@+id/stub"
  app:rectLayout="@layout/rect_layout"
  app:roundLayout="@layout/round_layout" />
```

La solution contient différentes dispositions pour chaque style qui sera activée au moment de l’exécution :

![Fichiers figurant sous ressources/mise en page](screen-sizes-images/solution.png)


### <a name="boxinsetlayout"></a>BoxInsetLayout

Plutôt que de créer des dispositions différentes pour chaque type d’écran, vous pouvez également créer une vue unique qui s’adapte aux écrans arrondis ou rectangulaires.

Cela [Google exemple](https://developer.android.com/training/wearables/ui/layouts.html#same-layout) montre comment utiliser le `BoxInsetLayout` pour la même disposition écrans rectangulaires et arrondis.


## <a name="wear-ui-designer"></a>Concepteur d’interface utilisateur d’usure

Le concepteur Android Xamarin prend en charge les écrans rectangulaires et arrondis :

![Sélection de l’écran Android Wear carré dans le concepteur Android Xamarin](screen-sizes-images/design-screen-type.png)

L’aire de conception de style rectangulaire est illustré ici :

![Aire de conception de style rectangulaire](screen-sizes-images/design-rect.png) 

L’aire de conception dans le style d’arrondi est indiqué ici :

![Aire de conception dans le style d’arrondi](screen-sizes-images/design-round.png)


## <a name="wear-simulator"></a>Simulateur d’usure

Le **Gestionnaire d’émulateur Google** contient les définitions d’appareil pour les deux types d’écran. Vous pouvez créer des émulateurs rectangulaires et round pour tester votre application.

![Porter des définitions d’appareil indiquées dans le Gestionnaire d’émulateur Google](screen-sizes-images/emulator-devices.png)

Pour un écran rectangulaires, l’émulateur s’affiche comme suit :

![Émulateur de rendu d’un écran rectangulaire](screen-sizes-images/recipe-2.png) 

Il s’affiche sous la forme pour un écran arrondi :

![Émulateur de rendu d’un écran arrondi](screen-sizes-images/recipe-2-round.png)

## <a name="video"></a>Vidéo

[Les applications plein écran pour Android Wear](https://www.youtube.com/watch?v=naf_WbtFAlY) de [developers.google.com](https://www.youtube.com/channel/UC_x5XG1OV2P6uZZ5FSM9Ttw).

