---
title: Consommation et la création de plug-ins de Xamarin.Forms
description: Cet article explique comment consommer et créer des plug-ins de Xamarin.Forms. Plug-ins sont généralement utilisés pour exposer facilement les fonctionnalités de la plateforme native.
ms.prod: xamarin
ms.assetid: 8A06A420-A9D0-4BCB-B9AF-3AEA6A648A8B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/05/2018
ms.openlocfilehash: ac8e5323a2a2e05ac03294bb6919e8dfadc93655
ms.sourcegitcommit: 7eed80186e23e6aff3ddbbf7ce5cd1fa20af1365
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/11/2018
ms.locfileid: "51526518"
---
# <a name="consuming-and-creating-xamarinforms-plugins"></a>Consommation et la création de plug-ins de Xamarin.Forms

De nombreuses fonctionnalités de plateforme native qui existent sur toutes les plateformes sont mais ont des API légèrement différentes. La première pour les développeurs à utiliser ces fonctionnalités consiste en créant une interface abstraite d’inter-plateformes, puis en implémentant cette interface dans les différentes plateformes. L’application Xamarin.Forms puis accède à ces implémentations de plateforme à l’aide de [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md).

Les développeurs peuvent partager ce travail en écrivant un _plug-in_ et la publiant sur NuGet.

> [!NOTE]
> Nombreuses fonctionnalités inter-plateformes auparavant disponibles uniquement par le biais de plug-ins font désormais partie de l’open source **[Xamarin.Essentials](~/essentials/index.md)** bibliothèque. Ces fonctionnalités incluent : état de la batterie, compass, détecteurs de mouvement, géolocalisation, synthèse vocale et bien plus encore. À l’avenir, **Xamarin.Essentials** sera la principale source de fonctionnalités multiplateformes pour les applications Xamarin.Forms. Bien que les développeurs peuvent toujours créer et publier des plug-ins, envisagez de contribuer à **Xamarin.Essentials**.

## <a name="finding-and-adding-plugins"></a>Recherche et ajout de plug-ins

La Communauté Xamarin a créé plusieurs plug-ins inter-plateformes compatibles avec Xamarin.Forms. Vous trouverez une grande collection à :

[**Plug-ins de Xamarin**](https://github.com/xamarin/XamarinComponents)

Pour obtenir un guide pour l’ajout de packages NuGet à votre projet, consultez notre procédure pas à pas sur [, y compris un package NuGet dans votre projet](/visualstudio/mac/nuget-walkthrough/).

## <a name="creating-plugins"></a>Création de plug-ins

Il est également possible de créer et publier vos propres plug-ins comme les packages Nuget (et les composants Xamarin). Nombreux plug-ins existants sont open source pour pouvoir consulter leur code pour comprendre la façon dont ils ont été writtern.

Par exemple, la liste des plug-ins ci-dessous sont tous les ouvrir la source, et qu’ils correspondent à certains exemples dans le [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md) section :

- **Synthèse vocale** James montemagno &ndash; [GitHub](https://github.com/jamesmontemagno/TextToSpeechPlugin) et [NuGet  ](https://www.nuget.org/packages/Xam.Plugins.TextToSpeech)
- **État de la batterie** James montemagno &ndash; [GitHub](https://github.com/jamesmontemagno/BatteryPlugin) et [NuGet](https://www.nuget.org/packages/Xam.Plugin.Battery)

Ces projets Github peuvent fournir un bon point de départ pour la création de votre propre plug-ins inter-plateformes, comme suivre ces instructions pour [création d’un plug-in pour Xamarin](https://github.com/xamarin/XamarinComponents#create-a-plugin-for-xamarin).

### <a name="structuring-cross-platform-plugin-projects"></a>Structuration des projets de plug-in inter-plateformes

Bien qu’il n’existe aucune exigence particulière pour la conception d’un package NuGet, il existe quelques conseils pour créer un package pour des applications multiplateformes.

Dans le passé, un plug-in multiplate-forme était généralement constitué des composants suivants :

- Bibliothèque de classes portable avec une Interface qui représente l’API pour le plug-in
- iOS, Android et Universal Windows Platform (UWP) des bibliothèques avec une implémentation de l’Interface de classes.

Lecture James Montemagno [billet de blog](https://blog.xamarin.com/creating-reusable-plugins-for-xamarin-forms/) décrivant le processus de création de plug-ins pour Xamarin.

Plus récemment, les plug-ins peuvent être créés avec une seule plateforme multi-ciblées. Cette approche est décrite de James Montemagno [billet de blog](https://montemagno.com/converting-xamarin-libraries-to-sdk-style-multi-targeted-projects/). Cette approche est utilisée dans les plug-ins de James Montemagno mentionné ci-dessus, et est également le format utilisé dans **Xamarin.Essentials**.

Il est préférable d’éviter de référencer Xamarin.Forms directement à partir d’un plug-in.
Cela peut créer des problèmes de conflit de version lorsque d’autres développeurs tentent d’utiliser le plug-in. Au lieu de cela, essayez de concevoir l’API afin qu’il peut être utilisé par n’importe quelle application Xamarin ou .NET.

### <a name="publishing-nuget-packages"></a>Publication de Packages NuGet

Les packages NuGet ont un **nuspec** fichier, qui est un fichier xml qui définit quelles parties de votre projet sont publiés dans le package. Le **nuspec** fichier inclut également des informations sur le package, telles qu’id, title et auteurs.

Consultez [documentation de NuGet](/nuget/create-packages/creating-a-package.md) pour plus d’informations sur la création et publication de packages NuGet.

## <a name="related-links"></a>Liens associés

- [Création de plug-ins réutilisables pour Xamarin.Forms](https://blog.xamarin.com/creating-reusable-plugins-for-xamarin-forms)
- [Utilisation de le & développement de plug-ins pour Xamarin (vidéo)](https://university.xamarin.com/guestlectures/using-developing-plugins-for-xamarin)
