---
title: 'Xamarin.Essentials : résolution des problèmes'
description: Ce document décrit comment résoudre les problèmes rencontrés lors du développement avec la bibliothèque Xamarin.Essentials.
ms.assetid: 2E474FAF-F841-4E3C-B815-F7ABD8EE3361
author: jamesmontemagno
ms.author: jamont
ms.date: 06/26/2018
ms.openlocfilehash: 8cb18ab029d2fd161c60fda7e130f319b8f0c3ab
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37947372"
---
# <a name="xamarinessentials-troubleshooting"></a>Xamarin.Essentials : résolution des problèmes

![Version préliminaire NuGet](~/media/shared/pre-release.png)

## <a name="error-version-conflict-detected-for-xamarinandroidsupportcompat"></a>Erreur : Version conflit est détecté pour Xamarin.Android.Support.Compat

L’erreur suivante peut se produire lors de la mise à jour les packages NuGet (ou l’ajout d’un nouveau package) avec un projet Xamarin.Forms qui utilise Xamarin.Essentials :

```
NU1107: Version conflict detected for Xamarin.Android.Support.Compat. Reference the package directly from the project to resolve this issue.
 MyApp -> Xamarin.Essentials 0.8.0-preview -> Xamarin.Android.Support.CustomTabs 27.0.2.1 -> Xamarin.Android.Support.Compat (= 27.0.2.1)
 MyApp -> Xamarin.Forms 3.1.0.583944 -> Xamarin.Android.Support.v4 25.4.0.2 -> Xamarin.Android.Support.Compat (= 25.4.0.2).
```

Le problème est les dépendances qui ne correspondent pas pour les deux packages de NuGet. Cela peut être résolu en ajoutant manuellement une version spécifique de la dépendance (dans ce cas **Xamarin.Android.Support.Compat**) qui peut prendre en charge les deux.

Pour ce faire, ajoutez le package NuGet qui est la source du conflit manuellement et utiliser le **Version** liste pour sélectionner une version spécifique. Actuellement version 27.0.2.1 du Xamarin.Android.Support.Compat & Xamarin.Android.Support.Core.Util NuGet résout cette erreur.

Reportez-vous à [ce billet de blog](https://redth.codes/how-to-fix-the-dreaded-version-conflict-nuget-error-in-your-xamarin-android-projects/) pour plus d’informations et une vidéo sur la façon de résoudre le problème.

Si vous rencontrez des problèmes ou les rechercher un bogue envoyez le rapport sur le [Xamarin.Essentials référentiel](http://github.com/xamarin/Essentials).
