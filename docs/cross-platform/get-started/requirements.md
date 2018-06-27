---
title: Configuration système requise
description: Ce document indique la configuration système requise pour générer des applications avec Xamarin sur les ordinateurs Mac et Windows. Il contient également des liens vers des instructions d’installation.
ms.prod: xamarin
ms.assetid: dd344d57-18e2-42a5-8c15-3f5be4123c72
author: asb3993
ms.author: amburns
ms.date: 08/28/2017
ms.openlocfilehash: 04db2fe4e3385c55ecf653b002b909f16e99a101
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34780722"
---
# <a name="system-requirements"></a>Configuration système requise

_Composants requis pour l’utilisation de Xamarin_

Les produits Xamarin s’appuient sur les SDK de plateforme Apple et Google pour cibler iOS ou Android, dans le but de s’aligner sur leurs exigences système. Cette page concerne la compatibilité des systèmes avec la plateforme Xamarin. Elle indique quels environnements de développement et versions de SDK utiliser selon le système.

- [Environnements de développement](#devenv)
- [Configuration requise pour Mac OS](#mac)
- [Configuration requise pour Windows](#windows)

Pour plus d’informations sur l’obtention des logiciels et des SDK nécessaires, consultez les [instructions d’installation](#install).

<a name="devenv" />

## <a name="development-environments"></a>Environnements de développement

Ce tableau liste les combinaisons d’outils de développement et de systèmes d’exploitation que vous pouvez utiliser pour chaque type de plateforme :

[!include[](~/cross-platform/includes/development-environment.md)]


> [!NOTE]
> Pour développer des applications iOS sur des ordinateurs Windows, vous devez disposer d’un [ordinateur Mac accessible sur le réseau](~/ios/get-started/installation/windows/connecting-to-mac/index.md), pour une compilation et un débogage distants. Cela fonctionne également si Visual Studio est exécuté sur une machine virtuelle Windows installée sur un ordinateur Mac.

<a name="mac" />

## <a name="macos-requirements"></a>Configuration requise pour Mac OS

L’utilisation d’un ordinateur Mac pour le développement Xamarin nécessite les logiciels et SDK suivants. Vérifiez la version de votre système d’exploitation et suivez les instructions relatives au [programme d’installation de Xamarin](#install).

[!include[](~/cross-platform/includes/macos-requirements.md)]

> [!NOTE]
> Xcode peut être installé (et mis à jour) sur [developer.apple.com](https://developer.apple.com/xcode/download/) ou via l’App Store Mac.

### <a name="testing--debugging-on-macos"></a>Test et débogage sur Mac OS

Les applications mobiles Xamarin peuvent être déployées sur des appareils physiques via une connexion USB à des fins de test et de débogage (les applications Xamarin.Mac peuvent être testées directement sur l’ordinateur de développement ; les applications Apple Watch doivent être d’abord déployées sur l’iPhone apparié).

[!include[](~/cross-platform/includes/macos-testing.md)]


<a name="windows" />

## <a name="windows-requirements"></a>Configuration requise pour Windows

L’utilisation d’un ordinateur Windows pour le développement Xamarin nécessite les logiciels et SDK suivants.
Vérifiez la version de votre système d’exploitation. Vérifiez également que vous n’utilisez pas une version *Express* de Visual Studio. Si c’est le cas, effectuez une mise à jour vers l’édition *Community*.
Les programmes d’installation pour Visual Studio 2015 et 2017 incluent une option permettant d’installer Xamarin automatiquement.

[!include[](~/cross-platform/includes/windows-requirements.md)]


> [!NOTE]
>
>* Xamarin pour Visual Studio prend en charge toutes les éditions de Visual Studio 2015 et 2017 (Community, Professional et Enterprise).
>
>* Si vous souhaitez développer des applications Xamarin.Forms pour la plateforme Windows universelle (UWP), vous devez utiliser Windows 10 avec Visual Studio 2015 et 2017.


### <a name="testing--debugging-on-windows"></a>Test et débogage sur Windows

Les applications mobiles Xamarin peuvent être déployées sur des appareils physiques via une connexion USB à des fins de test et de débogage (les appareils iOS doivent être connectés à l’ordinateur Mac, et non à l’ordinateur qui exécute Visual Studio).

[!include[](~/cross-platform/includes/windows-testing.md)]


> [!NOTE]
>
>* [Téléchargement de l’émulateur Windows Phone 8.1](https://www.microsoft.com/download/details.aspx?id=43719).
>* L’émulateur Windows Phone 10 est inclus dans le SDK UWP de Visual Studio 2015.

<a name="install" />

## <a name="installation-instructions"></a>Instructions d'installation

Vous pouvez télécharger la dernière version de Xamarin pour Mac OS sur [xamarin.com/download](http://xamarin.com/download). Pour Windows, suivez les instructions d’installation de [Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio).

La liste complète des versions actuelles de nos produits est disponible sur la [page des versions actuelles](http://developer.xamarin.com/releases/current/). Cette page présente également les versions de produits (et les liens vers les notes de publication) de nos canaux bêta et alpha.

Les instructions d’[installation](~/cross-platform/get-started/installation/index.md) de chaque plateforme sont disponibles ici :

- [Xamarin.iOS](~/ios/get-started/installation/index.md)
- [Xamarin.Android](~/android/get-started/installation/index.md)
- [Xamarin.Mac](~/mac/get-started/installation.md)

Vous pouvez également consulter des informations supplémentaires concernant les [exigences Xamarin.Forms et les plateformes prises en charge](~/xamarin-forms/get-started/installation.md).


## <a name="related-links"></a>Liens associés

- [Télécharger Xamarin](https://xamarin.com/download/)
- [Versions actuelles](https://developer.xamarin.com/releases/current/)
