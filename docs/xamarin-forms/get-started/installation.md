---
title: Configuration requise pour Xamarin.Forms
description: Configuration requise pour la plateforme et le développement pour Xamarin.Forms.
ms.prod: xamarin
ms.assetid: eecaf6a5-567c-49b2-ac83-2a195596c5bf
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/19/2018
ms.openlocfilehash: ce3f2bcf6acc36239fc431bb7f5edece15d2e139
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2018
---
# <a name="xamarinforms-requirements"></a>Configuration requise pour Xamarin.Forms

_Configuration requise pour la plateforme et le développement pour Xamarin.Forms._

Reportez-vous à l’article [Installation](~/cross-platform/get-started/installation/index.md) pour obtenir une vue d’ensemble des pratiques d’installation et de configuration qui s’appliquent sur toutes les plateformes.

## <a name="target-platforms"></a>Plateformes cibles

Les applications Xamarin.Forms peuvent être écrites pour les systèmes d’exploitation suivants :

-  iOS 8 ou ultérieur
-  Android 4.0.3 (API 15) ou ultérieur ([plus de détails](#android))
-  Plateforme Windows universelle Windows 10 ([plus de détails](#windows10))
-  *Windows 8.1/Windows Phone 8.1 WinRT (DÉCONSEILLÉ)*
-  *Windows Phone 8 Silverlight (DÉPRÉCIÉ)*

Il est supposé que les développeurs connaissent les [bibliothèques de classes portables](~/cross-platform/app-fundamentals/pcl.md) et les [projets partagés](~/cross-platform/app-fundamentals/shared-projects.md).

<a name="android" />

### <a name="android"></a>Android

Les dernières versions d’Android SDK Tools et de la plateforme d’API Android doivent être installées. Vous pouvez effectuer une mise à jour vers les versions les plus récentes à l’aide du [Gestionnaire Android SDK](~/android/get-started/installation/android-sdk.md).

De plus, la version cible/de compilation pour les projets Android **doit** avoir la valeur *Utiliser la dernière plateforme installée*. La version minimale peut toutefois être API 15 pour pouvoir continuer à prendre en charge les appareils qui utilisent Android 4.0.3 et ultérieur. Ces valeurs sont définies dans les **Options du projet** :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

**Options du projet > Application > Propriétés de l’application**

![](installation-images/options-android-vs-sml.png "Options de build Android dans Visual Studio")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

**Générer > Général**

![](installation-images/options-general-sml.png "Générer > Général")

**Générer > Application Android**

![](installation-images/options-android-sml.png "Générer > Application Android")

-----

<a name="windows10" />

### <a name="universal-windows-platform"></a>Plateforme Windows universelle

Les projets de plateforme Windows universelle (UWP) Windows 10 ne sont pas ajoutés lors de la création d’une solution sur macOS. Pour obtenir des instructions sur l’ajout de ces projets à une solution existante, consultez [Configuration de projets Windows](~/xamarin-forms/platform/windows/installation/index.md).

## <a name="development-system-requirements"></a>Configuration requise pour le développement

Les applications Xamarin.Forms peuvent être développées sur macOS et Windows. Toutefois, Windows et Visual Studio sont nécessaires pour produire des versions Windows de l’application.

## <a name="mac-system-requirements"></a>Configuration requise Mac

Vous pouvez utiliser Visual Studio pour Mac pour développer des applications Xamarin.Forms sur OS X El Capitan (10.11) ou version ultérieure. Pour développer des applications iOS, nous vous recommandons d’installer au moins le SDK iOS 10 et Xcode 8.

> [!NOTE]
>  Les applications Windows ne peuvent pas être développées sur macOS.

## <a name="windows-system-requirements"></a>Configuration requise Windows

Les applications Xamarin.Forms pour iOS et Android peuvent être générées sur toute installation Windows qui prend en charge le développement Xamarin. Visual Studio 2017 ou version plus récente doit être exécuté sur Windows 7 ou ultérieur. Un Mac en réseau est requis pour le développement iOS.

### <a name="universal-windows-platform-uwp"></a>Plateforme Windows universelle (UWP)

Le développement d’applications Xamarin.Forms pour UWP nécessite :

* Windows 10 (Fall Creators Update recommandé)

* Visual Studio 2017

* [SDK Windows 10](https://dev.windows.com/downloads/windows-10-sdk)

Les projets UWP sont inclus dans les solutions Xamarin.Forms créées dans Visual Studio 2015 et Visual Studio 2017.
Vous pouvez également [ajouter une application de plateforme Windows universelle (UWP)](~/xamarin-forms/platform/windows/installation/index.md) à une solution Xamarin.Forms existante.
