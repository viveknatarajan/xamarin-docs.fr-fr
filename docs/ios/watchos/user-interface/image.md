---
title: Contrôle de l’image
ms.prod: xamarin
ms.assetid: B741C207-3427-46F3-9C90-A52BF8933FA4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 370b9f2a57716de876c7e883afdaf445186fb577
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="image-control"></a>Contrôle de l’image

watchOS fournit un [ `WKInterfaceImage` ](https://developer.xamarin.com/api/type/WatchKit.WKInterfaceImage/) contrôle à afficher des images et des animations simples. Certains contrôles peuvent également avoir une image d’arrière-plan (par exemple, les boutons, les groupes et les contrôleurs d’interface).

![](image-images/image-walkway.png "Affichage d’une image de l’Apple Watch") ![ ] (image-images/image-animation.png "Apple Watch avec animation simple")
<!-- watch image courtesy of http://infinitapps.com/bezel/ -->

Images du catalogue asset permet d’ajouter des images pour les applications de surveillance Kit.
Uniquement **@2x** versions sont requises, car toutes les observer les appareils ont rétine.

![](image-images/asset-universal-sml.png "Seulement 2 x sont requis, car toutes les observer les appareils ont rétine")

Il est conseillé de vérifier que les images proprement dites sont la taille appropriée pour l’affichage de la surveillance. *Éviter* utilisant des images de tailles incorrecte (ceux qui sont particulièrement volumineuse) et mise à l’échelle pour les afficher sur la surveillance.

Vous pouvez utiliser les tailles espion Kit (38 et 42mm) dans une image du catalogue asset pour spécifier différentes images pour chaque taille d’affichage.

![](image-images/asset-watch-sml.png "Vous pouvez utiliser les tailles espion Kit 38 et 42mm dans une image du catalogue asset pour spécifier différentes images pour chaque taille d’affichage")


## <a name="images-on-the-watch"></a>Images sur la surveillance

Le moyen le plus efficace pour afficher des images consiste à *les inclure dans le projet d’application espion* et les afficher à l’aide de la `SetImage(string imageName)` (méthode).

Par exemple, le [WatchKitCatalog](https://developer.xamarin.com/samples/WatchKitCatalog/) exemple a un nombre d’images ajoutées à un catalogue dans le projet d’application espion :

![](image-images/asset-whale-sml.png "L’exemple WatchKitCatalog a un nombre d’images ajoutées à un catalogue dans le projet d’application espion")

Il peuvent être efficacement chargées et affichées sur la surveillance à l’aide `SetImage` avec le paramètre de nom de chaîne :

```csharp
myImageControl.SetImage("Whale");
myOtherImageControl.SetImage("Worry");
```

### <a name="background-images"></a>Images d'arrière-plan

La même logique s’applique à la `SetBackgroundImage (string imageName)` sur la `Button`, `Group`, et `InterfaceController` classes. Meilleures performances sont obtenue en stockant les images dans l’application espion elle-même.


## <a name="images-in-the-watch-extension"></a>Images dans l’Extension de surveillance

Outre le chargement des images qui sont stockées dans l’application de surveillance proprement dit, vous pouvez envoyer des images à partir de l’offre d’extension à l’application de surveillance pour l’affichage (ou vous pouvez télécharger des images à partir d’un emplacement distant et afficher celles).

Pour charger des images à partir de l’extension de surveillance, créez `UIImage` instances, puis appelez `SetImage` avec la `UIImage` objet.

Par exemple, le [WatchKitCatalog](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/) exemple a une image nommée **Bumblebee** dans le projet d’extension de surveillance :

![](image-images/asset-bumblebee-sml.png "L’exemple WatchKitCatalog a une image nommée Bumblebee dans le projet d’extension espion")

Provoque le code suivant :

- l’image qui est chargée en mémoire, et
- affiche sur la surveillance.

```csharp
using (var image = UIImage.FromBundle ("Bumblebee")) {
    myImageControl.SetImage (image);
}
```


## <a name="animations"></a>Animations

Pour animer un ensemble d’images, qu’ils doivent tous commencent par le même préfixe et ont un suffixe numérique.

Le [WatchKitCatalog](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/) exemple contient une série d’images numérotées dans le projet d’application Espion avec la **Bus** préfixe :

![](image-images/asset-bus-animation-sml.png "L’exemple de WatchKitCatalog contient une série d’images numérotées dans le projet d’application Espion avec le préfixe de Bus")

Pour afficher ces images sous la forme d’une animation, d’abord charger l’image à l’aide de `SetImage` avec le nom de préfixe et d’un appel puis `StartAnimating`:

```csharp
animatedImage.SetImage ("Bus");
animatedImage.StartAnimating ();
```

Appeler `StopAnimating` sur le contrôle image à arrêter l’animation en boucle :

```csharp
animatedImage.StopAnimating ();
```


<a name="cache" />

## <a name="appendix-caching-images-watchos-1"></a>Annexe : Mise en cache d’Images (watchOS 1)

> [!IMPORTANT]
> les applications watchOS 3 s’exécutent entièrement sur l’appareil. Les informations suivantes sont watchOS 1 uniquement pour les applications.

Si l’application utilise à plusieurs reprises une image qui est stockée dans l’extension (ou a été téléchargée), il est possible de mettre en cache l’image dans le stockage de la surveillance, pour augmenter les performances pour les écrans suivants.

Utilisez le `WKInterfaceDevice`s `AddCachedImage` méthode pour transférer l’image à la surveillance, puis utiliser `SetImage` avec le paramètre de nom d’image sous forme de chaîne pour l’afficher :

```csharp
var device = WKInterfaceDevice.CurrentDevice;
using (var image = UIImage.FromBundle ("Bumblebee")) {
    if (!device.AddCachedImage (image, "Bumblebee")) {
            Console.WriteLine ("Image cache full.");
        } else {
            cachedImage.SetImage ("Bumblebee");
        }
    }
}
```

Vous pouvez interroger le contenu du cache d’image à l’aide de code `WKInterfaceDevice.CurrentDevice.WeakCachedImages`.


### <a name="managing-the-cache"></a>Gestion du Cache

La taille d’environ 20 Mo de cache. Il est conservé entre les redémarrages de l’application, et quand elle ne soit saturé il vous incombe d’effacer les fichiers à l’aide de `RemoveCachedImage` ou `RemoveAllCachedImages` méthodes sur le `WKInterfaceDevice.CurrentDevice` objet.



## <a name="related-links"></a>Liens associés

- [WatchKitCatalog (exemple)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Document d’Image d’Apple](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/Images.html)
