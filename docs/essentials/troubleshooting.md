---
title: 'Xamarin.Essentials : résolution des problèmes'
description: Ce document décrit comment résoudre les problèmes rencontrés lors du développement avec la bibliothèque Xamarin.Essentials.
ms.assetid: 2E474FAF-F841-4E3C-B815-F7ABD8EE3361
author: jamesmontemagno
ms.author: jamont
ms.date: 06/26/2018
ms.openlocfilehash: 3dba315aec2475cb334110ba7555f773f4165aa1
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37066732"
---
# <a name="xamarinessentials-troubleshooting"></a>Xamarin.Essentials : résolution des problèmes

![Version préliminaire de NuGet](~/media/shared/pre-release.png)

## <a name="error-version-conflict-detected-for-xamarinandroidsupportcompat"></a>Erreur : Version conflit est détecté pour Xamarin.Android.Support.Compat

L’erreur suivante peut se produire lors de la mise à jour les packages NuGet (ou l’ajout d’un nouveau package) à un projet de Xamarin.Forms par Xamarin.Essentials :

```
NU1107: Version conflict detected for Xamarin.Android.Support.Compat. Reference the package directly from the project to resolve this issue. 
 MyApp -> Xamarin.Essentials 0.7.0.17-preview -> Xamarin.Android.Support.CustomTabs 27.0.2 -> Xamarin.Android.Support.Compat (= 27.0.2) 
 MyApp -> Xamarin.Forms 3.1.0.583944 -> Xamarin.Android.Support.v4 25.4.0.2 -> Xamarin.Android.Support.Compat (= 25.4.0.2).
```

Il s’agit des dépendances incompatibles pour les deux NuGets. Cela peut être résolu en ajoutant manuellement une version spécifique de la dépendance (dans ce cas **Xamarin.Android.Support.Compat**) qui peut prendre en charge les deux.

Pour ce faire, ajoutez le NuGet est la source du conflit manuellement et utilisez le **Version** liste pour sélectionner une version spécifique. Version 27.0.2 de Xamarin.Android.Support.Compat NuGet résoudre actuellement cette erreur.

Reportez-vous à [ce billet de blog](https://redth.codes/how-to-fix-the-dreaded-version-conflict-nuget-error-in-your-xamarin-android-projects/) pour plus d’informations et une vidéo sur la façon de résoudre le problème.

Si vous rencontrez des problèmes ou de la recherche d’un bogue signalez-le sur le [référentiel GitHub de Xamarin.Essentials](http://github.com/xamarin/Essentials).
