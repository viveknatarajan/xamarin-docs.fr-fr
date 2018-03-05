---
title: "Créer manuellement les Packages NuGet pour Xamarin"
description: "Cette page contient des conseils pour aider à générer des packages NuGet qui ciblent la plateforme de Xamarin."
ms.topic: article
ms.prod: xamarin
ms.assetid: a5964686-5fc6-4280-b087-7ba27cc1c8bf
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: 76f35819b00302f4a586643798afbd27416d3997
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="manually-creating-nuget-packages-for-xamarin"></a>Créer manuellement les Packages NuGet pour Xamarin

_Cette page contient des conseils pour aider à générer des packages NuGet qui ciblent la plateforme de Xamarin._

> [!NOTE]
> Xamarin Studio 6.2 (et Visual Studio pour Mac) incluent la possibilité de _automatiquement_ générer des packages NuGet à partir de la bibliothèque PCL, .NET Standard ou partagé les projets.
> Reportez-vous à la [bibliothèques multiplateformes pour le partage de Code](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md) guide pour plus de détails.

## <a name="nuget-package-xamarin-profiles"></a>Les profils de Package NuGet Xamarin


Du site Web NuGet [prise en charge de plusieurs Versions du .NET Framework et les profils](https://docs.nuget.org/create/enforced-package-conventions) explique comment prendre en charge de différentes infrastructures de Microsoft et de profils, mais n’inclut pas les noms de framework cible utilisés par Xamarin.

L’infrastructure cible Xamarin principal en cours d’utilisation est aujourd'hui :

* **MonoAndroid** -Xamarin.Android
* **Xamarin.iOS** -Xamarin.iOS [API unifiée](~/cross-platform/macios/unified/index.md) (prend en charge 64 bits)
* **Xamarin.Mac** -profil mobile de Xamarin.Mac, qui est équivalente à la surface Xamarin.iOS et Xamarin.Android API.

Il existe également une cible pour les e/s plus anciens [Classic API](~/cross-platform/macios/unified/index.md):

* **MonoTouch** -Classic API iOS

A **.nuspec** fichier ciblant tous ces éléments peut ressembler à :

```xml
<files>
    <file src="Mac\bin\Release\*.dll" target="lib\Xamarin.Mac20" />
    <file src="iOS\bin\Release\*.dll" target="lib\Xamarin.iOS10" />
    <file src="Android\bin\Release\*.dll" target="lib\MonoAndroid10" />
    <file src="iOSClassic\bin\Release\*.dll" target="lib\MonoTouch10" />
</files>
```

Ci-dessus ignore toutes les bibliothèques de classes portables.

La plupart des **.nuspec** fichiers spécifient le numéro de version du framework cible, mais il est facultatif si votre assembly fonctionne avec toutes les versions de cette infrastructure cible. Par conséquent, si votre cible a été **lib\MonoAndroid** , cela signifie qu’il fonctionne avec n’importe quelle version de Xamarin.Android.

Vous pouvez spécifier la version avec un ensemble de nombres sans virgule décimale, ou vous pouvez spécifier à l’aide de la virgule décimale. Sans la virgule décimale NuGet est simplement prendre chaque nombre et les transformer en une version en insérant un '.' entre chaque chiffre.

Dans le ci-dessus « MonoAndroid10 » signifie « Android 1.0 ». Cela signifie simplement du projet [framework cible](~/android/app-fundamentals/android-api-levels.md) doit être MonoAndroid version 1.0 ou ultérieure. La version est spécifiée dans le `<TargetFrameworkVersion>` élément dans le fichier projet.

Pour préciser :

- **MonoAndroid403** correspond à Android 4.0.3 et versions ultérieures (c.-à-d. API niveau 15)
- **Xamarin.iOS10** correspond à Xamarin.iOS 1.0 et ultérieure
- **Xamarin.iOS1.0** correspond également à Xamarin.iOS 1.0 et ultérieure


## <a name="pcl-nugets-with-platform-dependencies"></a>NuGets PCL avec des dépendances de plateforme

PCL profils sont limitées dans le .NET framework API, ils peuvent accéder, et ils certainement ne peut pas accéder au code spécifique à la plateforme. Ces liens 3 rd-party traitent des différentes approches de création de packages NuGet qui utilisent la bibliothèque de classes portables et des API natives pour assurer la compatibilité de Xamarin et d’autres plates-formes :

- [Comment effectuer le travail de bibliothèques de classes portables pour vous](http://blogs.msdn.com/b/dsplaisted/archive/2012/08/27/how-to-make-portable-class-libraries-work-for-you.aspx)
- [Astuce PCL leurre et commutateur](http://log.paulbetts.org/the-bait-and-switch-pcl-trick/)
- [Création d’une PCL NuGet qui fonctionne avec Xamarin.iOS](http://www.jimbobbennett.io/creating-a-nuget-pcl-that-works-with-xamarin-ios/)

Cette externe [liste des profils de la bibliothèque de classes portables avec leur nom de cible de NuGet](http://embed.plnkr.co/03ck2dCtnJogBKHJ9EjY) est également une référence utile.

## <a name="examples"></a>Exemples

Certains exemples open source que vous pouvez faire référence à :

- [**ModernHttpClient** ](https://www.nuget.org/packages/modernhttpclient/) : écrire votre application à l’aide de System.Net.Http, mais supprimer cette bibliothèque dans passera considérablement plus rapide (vue [source](https://github.com/paulcbetts/ModernHttpClient)).
- [**Splat** ](https://www.nuget.org/packages/Splat/) – une bibliothèque pour rendre les choses inter-plateformes qui doit être (vue [source](https://github.com/paulcbetts/Splat)).
- [**NGraphics** ](https://www.nuget.org/packages/NGraphics/) -bibliothèque multiplateforme pour le rendu des graphiques vectoriels sur .NET (vue [source](https://github.com/praeclarum/NGraphics/blob/master/NGraphics.nuspec)).


## <a name="related-links"></a>Liens associés

- [Nugetizer-3000 automatisée de la création de Nuget](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md)
- [Mise à jour NuGets pour iOS 64 bits](http://blog.xamarin.com/how-to-update-nuget-packages-for-64-bit/)
- [Y compris un NuGet dans votre projet](/visualstudio/mac/nuget-walkthrough/index.md)
