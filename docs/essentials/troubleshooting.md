---
title: 'Xamarin.Essentials: Résolution des problèmes'
description: Ce document explique comment résoudre les problèmes rencontrés lors d’un développement avec la bibliothèque Xamarin.Essentials.
ms.assetid: 2E474FAF-F841-4E3C-B815-F7ABD8EE3361
author: jamesmontemagno
ms.author: jamont
ms.date: 04/02/2019
ms.openlocfilehash: a9546923c9a17aade58d830ea2504406ad09d7d7
ms.sourcegitcommit: 495680e74c72e7c570e68cde95d3d3643b1fcc8a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/02/2019
ms.locfileid: "58870077"
---
# <a name="xamarinessentials-troubleshooting"></a>Xamarin.Essentials: Résolution des problèmes

## <a name="error-version-conflict-detected-for-xamarinandroidsupportcompat"></a>Erreur : Conflit de version détecté pour Xamarin.Android.Support.Compat

L’erreur suivante peut se produire lors de la mise à jour des packages NuGet (ou de l’ajout d’un nouveau package) avec un projet Xamarin.Forms qui utilise Xamarin.Essentials :

```error
NU1107: Version conflict detected for Xamarin.Android.Support.Compat. Reference the package directly from the project to resolve this issue. 
 MyApp -> Xamarin.Essentials 1.1.0 -> Xamarin.Android.Support.CustomTabs 28.0.0.1 -> Xamarin.Android.Support.Compat (= 28.0.0.1) 
 MyApp -> Xamarin.Forms 3.1.0.583944 -> Xamarin.Android.Support.v4 25.4.0.2 -> Xamarin.Android.Support.Compat (= 25.4.0.2).
```

Le problème se rapporte à des dépendances incompatibles pour les deux packages NuGet. Pour résoudre ce problème, vous pouvez ajouter manuellement une version spécifique de la dépendance (dans ce cas **Xamarin.Android.Support.Compat**) capable de prendre en charge les deux.

Pour ce faire, ajoutez manuellement le package NuGet qui est la source du conflit et utilisez la liste **Version** pour sélectionner une version spécifique. Actuellement, la version 28.0.0.1 du package NuGet Xamarin.Android.Support.Compat & Xamarin.Android.Support.Core.Util résoudra cette erreur.

Reportez-vous à [ce billet de blog](https://redth.codes/how-to-fix-the-dreaded-version-conflict-nuget-error-in-your-xamarin-android-projects/) pour plus d’informations et une vidéo sur la façon de résoudre le problème.

Si vous rencontrez un problème ou trouvez un bogue, signalez-le dans le [dépôt GitHub Xamarin.Essentials](https://github.com/xamarin/Essentials).
