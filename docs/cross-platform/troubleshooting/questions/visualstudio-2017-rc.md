---
title: Puis-je utiliser Visual Studio 2017 Release Candidate avec Xamarin ?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 8E752F36-F73A-4EFC-9F82-4E18FDE1C9E2
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 18329d226df5d129bd648c82b01e1ea045d131f4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="can-i-use-visual-studio-2017-release-candidate-with-xamarin"></a>Puis-je utiliser Visual Studio 2017 Release Candidate avec Xamarin ?

## <a name="can-i-use-visual-studio-2017-release-candidate-with-xamarin"></a>Puis-je utiliser Visual Studio 2017 Release Candidate avec Xamarin ?

Oui. Vous êtes en mesure de tirer parti de Xamarin via Visual Studio 2017 Release Candidate. Toutefois, notez qu’actuellement, l’installation de Xamarin pour Visual Studio 2017 RC désinstalle les versions antérieures de Xamarin est installé dans Visual Studio 2015/2013. Xamarin pour Visual Studio 2017 ne peut pas être installé en même temps que Xamarin pour Visual Studio 2015/2013 à la suite Visual Studio 2017 déplacement d’empaquetage MSI vers l’utilisation du système de programme d’installation de Visual Studio.

Pendant que l’équipe recherche actuellement dans les façons de contourner ce comportement attendu, nous recommandons d’aux utilisateurs de choisir leur environnement de développement en fonction de leurs besoins. 

> [!IMPORTANT]
> Si vous générez des projets de Xamarin.iOS, assurez-vous que Visual Studio pour Mac sur votre système Mac apparié est sur la même version du canal de Xamarin.iOS.

## <a name="how-do-i-install-xamarin-to-visual-studio-2017-release-candidate"></a>Comment installer Xamarin pour Visual Studio 2017 version finale ?

### <a name="installing-during-the-visual-studio-2017-rc-installer"></a>L’installation pendant le programme d’installation de Visual Studio 2017 RC

* Sélectionnez le **Xamarin** composant dans le cadre de la nouvelle **le programme d’installation de Visual Studio**

  [![](visualstudio-2017-rc-images/install1-sml.png "Écran programme d’installation de Visual Studio 2017 RC")](visualstudio-2017-rc-images/install1-orig.png#lightbox)

Cela installe l’extension Visual Studio pour le développement de Xamarin.iOS et Xamarin.Android.

### <a name="installing-or-reinstalling-xamarin-in-an-existing-installation-of-visual-studio-2017-rc"></a>Lors de l’installation ou de réinstaller Xamarin dans une installation existante de Visual Studio 2017 RC

#### <a name="using-the-visual-studio-installer"></a>À l’aide du programme d’installation de Visual Studio :

1. Recherche de l’application du programme d’installation de Visual Studio

  [![](visualstudio-2017-rc-images/reinstall1-sml.png "Résultats de recherche pour l’Application de programme d’installation de Visual Studio")](visualstudio-2017-rc-images/reinstall1-orig.png#lightbox)

2. Sélectionnez : un. **Développement pour appareils mobiles avec .NET (version préliminaire)** dans l’onglet de charges de travail, ou

  [![](visualstudio-2017-rc-images/reinstall2-sml.png "Onglet de charges de travail de programme d’installation de VS") ](visualstudio-2017-rc-images/reinstall2-orig.png#lightbox) b. **Xamarin** dans les **des composants individuels** onglet

  [![](visualstudio-2017-rc-images/reinstall3-sml.png "Onglet de composants de programme d’installation de Visual Studio")](visualstudio-2017-rc-images/reinstall3-orig.png#lightbox)

#### <a name="using-the-visual-studio-installer-within-visual-studio"></a>À l’aide du programme d’installation de Visual Studio dans Visual Studio :
1. Accédez à la Page de démarrage Visual Studio 2017
2. Cliquez sur **plus les modèles de projet** sous le **nouveau projet** section

    [![](visualstudio-2017-rc-images/reinstall4-sml.png "Page de démarrage de Visual Studio")](visualstudio-2017-rc-images/reinstall4-orig.png#lightbox)
3. Cliquez sur `Open Visual Studio Installer` dans le volet gauche

    [![](visualstudio-2017-rc-images/reinstall5-sml.png "Nouvel écran de projet")](visualstudio-2017-rc-images/reinstall5-orig.png#lightbox)
4. Sélectionnez :
    
    a. **Développement pour appareils mobiles avec .NET (version préliminaire)** dans l’onglet de charges de travail, ou

    [![](visualstudio-2017-rc-images/reinstall2-sml.png "Onglet de charges de travail de programme d’installation de VS") ](visualstudio-2017-rc-images/reinstall2-orig.png#lightbox) b. **Xamarin** dans les **des composants individuels** onglet

    [![](visualstudio-2017-rc-images/reinstall3-sml.png "Onglet de composants de programme d’installation de Visual Studio")](visualstudio-2017-rc-images/reinstall3-orig.png#lightbox)
