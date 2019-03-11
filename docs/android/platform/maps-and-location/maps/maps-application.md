---
title: Lancement de l’Application de mappages
description: Guide pratique pour lancer l’application Maps intégrée à partir de votre application Xamarin.Android.
ms.prod: xamarin
ms.assetid: 929EACB8-8950-50E1-093C-43FB5F1F1CD5
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/25/2018
ms.openlocfilehash: fa32783617fce99514560677184f17be904cd42d
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57670319"
---
# <a name="launching-the-maps-application"></a>Lancement de l’Application de mappages

Pour travailler avec des mappages dans Xamarin.Android, le plus simple consiste à tirer parti de l’application de cartes intégrées indiquée ci-dessous :

[![Capture d’écran de l’exemple d’application Google Maps intégrée](maps-application-images/01-mapsapplication.png)](maps-application-images/01-mapsapplication.png#lightbox)

Lorsque vous utilisez l’application de cartes, la carte ne fera pas partie de votre application. Au lieu de cela, votre application lance l’application maps et charger la carte en externe. La section suivante examine comment utiliser Xamarin.Android pour lancer des cartes comme celui illustré ci-dessus.


## <a name="creating-the-intent"></a>Création de l’intention

Utilisation avec les cartes d’application est aussi simple que la création d’une intention avec un URI approprié, la définition de l’action à ActionView et l’appelant la méthode StartActivity. Par exemple, le code suivant lance l’application de cartes centrée à une donnée de latitude et une longitude :

```csharp
var geoUri = Android.Net.Uri.Parse ("geo:42.374260,-71.120824");
var mapIntent = new Intent (Intent.ActionView, geoUri);
StartActivity (mapIntent);
```

Ce code est tout ce qui est nécessaire pour lancer le mappage indiqué dans la capture d’écran précédente. Outre la spécification de latitude et longitude, le schéma d’URI pour les cartes prend en charge plusieurs autres options.


## <a name="geo-uri-scheme"></a>Schéma d’URI de la zone géographique

Le code ci-dessus permet de créer un URI le schéma de la zone géographique. Ce schéma d’URI prend en charge plusieurs formats, comme indiqué ci-dessous :

-   `geo:latitude,longitude` &ndash; Ouvre l’application maps centrée sur un LAT./lon. 

-   `geo:latitude,longitude?z=zoom` &ndash; Ouvre les mappages d’application centrée sur un LAT./lon et zoom au niveau spécifié. Le niveau de zoom peut varier de 1 à 23 : 1 affiche la terre entière et 23 est le niveau de zoom le plus proche.

-   `geo:0,0?q=my+street+address` &ndash; Ouvre l’application de mappages à l’emplacement d’une adresse postale. 

-   `geo:0,0?q=business+near+city` &ndash; Ouvre l’application maps et affiche les résultats de recherche annoté. 


Les versions de l’URI qui prennent une requête (à savoir les rues adresse ou recherche termes) utilisent service de geocoder de Google pour extraire l’emplacement qui est ensuite affiché sur la carte. Par exemple, l’URI `geo:0,0?q=coop+Cambridge` entraîne le mappage indiqué ci-dessous :

[![Capture d’écran montrant Google Maps avec un terme de recherche](maps-application-images/02-mapsearch.png)](maps-application-images/02-mapsearch.png#lightbox)



Pour plus d’informations sur les schémas d’URI géographique, consultez [afficher un emplacement sur une carte](https://developer.android.com/guide/components/intents-common.html#Maps).


## <a name="street-view"></a>Vue rue

Outre le schéma de la zone géographique, Android également en charge le chargement des vues (rue) à partir d’une intention. Voici un exemple de l’application de vue postale lancée à partir de Xamarin.Android :

[![Capture d’écran de l’exemple d’une vue de rue](maps-application-images/03-streetview.png)](maps-application-images/03-streetview.png#lightbox)

Pour lancer une vue rue, utilisez simplement le `google.streetview` schéma d’URI, comme illustré dans le code suivant :

```csharp
var streetViewUri = Android.Net.Uri.Parse (
       "google.streetview:cbll=42.374260,-71.120824&cbp=1,90,,0,1.0&mz=20");  
var streetViewIntent = new Intent (Intent.ActionView, streetViewUri);  
StartActivity (streetViewIntent);
```

Le schéma d’URI google.streetview ci-dessus prend la forme suivante :

```csharp
google.streetview:cbll=lat,lng&cbp=1,yaw,,pitch,zoom&mz=mapZoom
```

Comme vous pouvez le voir, il existe plusieurs paramètres pris en charge, comme indiqué ci-dessous :

-   `lat` &ndash; Latitude de l’emplacement à afficher dans la vue de rue.

-   `lng` &ndash; Longitude de l’emplacement à afficher dans la vue de rue.

-   `pitch` &ndash; Angle de panorama vue rue, mesurée à partir du centre en degrés où 90 degrés est directement vers le bas et de-90 degrés est vers le haut.

-   `yaw` &ndash; Centre de vue de panorama vue rue, mesuré dans le sens horaire en degrés à partir du Nord.

-   `zoom` &ndash; Zoom multiplicateur de panorama de vue rue, où 1.0 = zoom normal, 2.0 = zoom 2 x 3.0 = 4 zoom x, etc.

-   `mz` &ndash; Le niveau de zoom de carte qui sera utilisé lors du passage à l’application de mappages à partir de la vue rue.


Utilisation d’intégrés mappe application ou la vue (rue) est un moyen simple pour ajouter rapidement la prise en charge du mappage. Toutefois, les API de cartes d’Android offre un contrôle plus précis sur l’expérience de mappage.
