---
title: Connexion des préversions de Visual Studio 2019 sur Windows et Mac OS
description: Ce guide fournit des instructions sur l’utilisation de la préversion de Visual Studio 2019 sur Windows pour générer des applications iOS, tout en utilisant la préversion de Visual Studio pour Mac 2019 sur macOS pour héberger vos builds.
ms.prod: xamarin
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 11/04/2018
ms.openlocfilehash: 1eeb79737bd712da64ce34a6b4fe83ce2823e3eb
ms.sourcegitcommit: 01f93a34b466f8d4043cef68fab9b35cd8decee6
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2018
ms.locfileid: "52899362"
---
# <a name="visual-studio-2019-preview-pairing"></a>Appairage des préversions de Visual Studio 2019

![Preview](~/media/shared/preview.png)

La [préversion de Visual Studio pour Mac 2019](https://docs.microsoft.com/visualstudio/mac/install-preview) constitue la première prise en charge complète de l’installation côte à côte de Visual Studio sur macOS. Les développeurs Windows qui génèrent des applications iOS font ainsi face à une nouvelle situation : si leur hôte de build Mac dispose à la fois de Visual Studio 2017 pour Mac (version 7.x) et de la préversion de Visual Studio 2019 pour Mac (version 8.0), lequel de ces produits sert d’hôte de build pour l’installation Windows ?

_Par défaut_, la version stable &ndash; Visual Studio 2017 pour Mac (version 7.7) &ndash; est utilisée par Visual Studio sur Windows, que vous utilisiez Visual Studio 2017 ou la préversion de Visual Studio 2019 sur Windows.

Pour tester correctement les préversions de Visual Studio 2019 sur Windows et macOS :

1. Installez et utilisez la préversion de Visual Studio 2019 (version 16.0) sur votre ordinateur Windows.
2. Installez la préversion de Visual Studio 2019 pour Mac (version 8.0) sur votre Mac.
3. Dans **Visual Studio 2019 pour Mac** :

    a. Choisissez l’élément de menu **Visual Studio > Rechercher les mises à jour**.

    b. Dans la fenêtre **Visual Studio Update**, sélectionnez **Préversion Xamarin** à partir du canal de mise à jour. L’avertissement suivant s’affiche :

    > [!WARNING]
    > Essayez les dernières builds de Visual Studio 2019 pour Mac Preview avec les derniers SDK Xamarin et runtime Mono. **L’installation de builds à partir de ce canal arrête la version Visual Studio 2017 pour Mac.** Utilisez ce canal uniquement si vous générez des applications Xamarin sur un système d’exploitation Windows utilisant des versions Visual Studio 2019 Preview.

    c. Appuyez sur le bouton **Changer de canal** pour activer l’hôte de build de préversion.

4. Suivez les instructions indiquées dans [Appairer avec un Mac pour le développement Xamarin.iOS](index.md) et les [conseils de dépannage](troubleshooting.md) (si nécessaire).