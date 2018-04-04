---
title: Plug-ins
description: Ajouter facilement des fonctionnalités natives pour les applications de Xamarin.Forms
ms.prod: xamarin
ms.assetid: 8A06A420-A9D0-4BCB-B9AF-3AEA6A648A8B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/07/2016
ms.openlocfilehash: 5770d13c46998872752820b7a0cbb222a04c3ff8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="plugins"></a>Plug-ins

Il existe de nombreuses fonctionnalités de plateforme natifs qui existent sur toutes les plateformes, mais ont des API légèrement différentes. Les développeurs écrivent des plug-ins pour créer une interface multiplateforme abstraite pour les fonctionnalités qu’ils peuvent également partager avec d’autres utilisateurs.

Ces fonctionnalités sont notamment : état de la batterie, compass, capteurs de mouvement, géolocalisation, synthèse vocale et bien plus encore. Plug-ins autorisent ces fonctionnalités facilement accessibles par les applications de Xamarin.Forms.

## <a name="finding-and-adding-plugins"></a>Recherche et ajout de plug-ins

La Communauté Xamarin a créé plusieurs plug-ins d’inter-plateformes compatibles avec Xamarin.Forms - une grande collection peut se trouve à :

[**Plug-ins de Xamarin**](https://github.com/xamarin/plugins)

Pour un guide sur l’ajout de packages NuGet à votre projet, consultez notre procédure pas à pas sur [, y compris un package NuGet dans votre projet](/visualstudio/mac/nuget-walkthrough/).


## <a name="creating-plugins"></a>Création de plug-ins

Il est également possible de créer et publier vos propres plug-ins comme packages Nuget (et composants Xamarin). Nombreux plug-ins existants sont open source pour pouvoir consulter leur code pour comprendre la façon dont ils ont été writtern.

Par exemple, la liste des plug-ins ci-dessous sont tous les ouvrir la source, et qu’ils correspondent aux exemples dans le [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md) section :

- **Synthèse vocale** par James Montemagno &ndash; [GitHub](https://github.com/jamesmontemagno/Xamarin.Plugins/tree/master/TextToSpeech) et [NuGet](https://www.nuget.org/packages/Xam.Plugin.Battery)
- **État de la batterie** par James Montemagno &ndash; [GitHub](https://github.com/jamesmontemagno/Xamarin.Plugins/tree/master/Battery) et [NuGet](https://www.nuget.org/packages/Xam.Plugins.TextToSpeech/)

Ces projets Github peuvent fournir un bon point de départ pour la création de vos propres plug-ins inter-plateformes, comme suivre ces instructions pour [création d’un plug-in pour Xamarin](https://github.com/xamarin/plugins#create-a-plugin-for-xamarin).

### <a name="structuring-cross-platform-plugin-projects"></a>Structuration des projets de plug-in multiplateforme

Bien qu’il n’existe aucune exigence particulière pour la conception d’un package NuGet, il existe quelques indications pour la création d’un package pour des applications multiplateformes.

Un plug-in multiplateforme doit généralement comporter des composants suivants :

- Bibliothèque de classes portables avec une Interface qui représente l’API pour le plug-in,
- bibliothèques avec une implémentation de l’Interface de la classe iOS, Android et Windows.

Lecture James Montemagno [billet de blog](https://blog.xamarin.com/creating-reusable-plugins-for-xamarin-forms/) décrivant le processus de création de plug-ins pour Xamarin.

Il est un préférable d’éviter de référencer Xamarin.Forms directement à partir d’un plug-in.
Cela peut créer des problèmes de conflit de version lorsque d’autres développeurs tentent d’utiliser le plug-in. Au lieu de cela, essayez de concevoir l’API afin qu’il peut être utilisé par n’importe quelle application Xamarin ou .NET.

### <a name="publishing-nuget-packages"></a>Publication de Packages NuGet

Les packages NuGet ont un **nuspec** fichier, qui est un fichier xml qui définit quelles parties de votre projet sont publiés dans le package. Le **nuspec** fichier inclut également des informations sur le package, telles qu’id, title et auteurs.

Consultez [documentation de NuGet](http://docs.nuget.org/create/creating-and-publishing-a-package) pour plus d’informations sur la création et la publication des packages NuGet.


## <a name="related-links"></a>Liens associés

- [Création de plug-ins réutilisables pour Xamarin.Forms](https://blog.xamarin.com/creating-reusable-plugins-for-xamarin-forms)
- [En utilisant les & développement des plug-ins pour Xamarin (vidéo)](https://university.xamarin.com/guestlectures/using-developing-plugins-for-xamarin)
