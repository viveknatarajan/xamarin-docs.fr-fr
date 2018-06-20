---
title: Configuration requise et Installation de l’inspecteur
description: Ce document décrit comment installer l’inspecteur de Xamarin et traite le système d’exploitation pris en charge IDE et plateformes d’applications.
ms.prod: xamarin
ms.assetid: 81174493-02D3-4FF5-AD57-04F3288A7F94
author: topgenorth
ms.author: toopge
ms.date: 06/19/2018
ms.openlocfilehash: f7c5217a9c2d3881ca29094c3186e448975db6a3
ms.sourcegitcommit: d70fcc6380834127fdc58595aace55b7821f9098
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36268967"
---
# <a name="inspector-installation-and-requirements"></a>Configuration requise et Installation de l’inspecteur

## <a name="download-and-installation"></a>Téléchargement et Installation

# <a name="windowstabwindows"></a>[Fenêtres](#tab/windows)

1. Téléchargez et installez [Visual Studio Enterprise](https://www.visualstudio.com/vs/) et sélectionnez le **développement pour appareils mobiles avec .NET** la charge de travail.
1. [Connectez-vous](https://docs.microsoft.com/visualstudio/ide/signing-in-to-visual-studio) pour activer votre abonnement d’entreprise.
1. [Inspecter](~/tools/inspector/inspect.md) votre propre application !

# <a name="macostabmacos"></a>[macOS](#tab/macos)

1. Téléchargez et installez [Visual Studio pour Mac](https://www.visualstudio.com/vs/mac/).
1. [Connectez-vous](https://docs.microsoft.com/visualstudio/mac/activation) pour activer votre abonnement d’entreprise.
1. [Inspecter](~/tools/inspector/inspect.md) votre propre application !

-----

## <a name="requirements"></a>Configuration requise

### <a name="supported-operating-systems"></a>Supported Operating Systems

- **Mac** -OS X 10.11 ou supérieur
- **Windows** -Windows 7 ou version ultérieure (avec Internet Explorer ou supérieure à 11 et .NET 4.6.1 ou supérieur)

### <a name="supported-ides"></a>Prise en charge IDE

- Visual Studio pour Mac
- Visual Studio 2017, avec **développement pour appareils mobiles avec .NET** la charge de travail

Inspection de l’application en temps réel est disponible pour les clients d’entreprise.

<a name="supported-platforms" />

### <a name="supported-app-platforms"></a>Plateformes d’applications pris en charge

|Plateforme d’application|Prise en charge IDE|Notes|
|--- |--- |--- |
|Mac|Uniquement pris en charge dans Visual Studio pour Mac|
|iOS|Prise en charge dans Visual Studio 2017 et Visual Studio pour Mac| |
|Android|Prise en charge dans Visual Studio 2017 et Visual Studio pour Mac|Doivent cibler Android > = 4.0.3, avec **fastdev** activé.<br />Doit utiliser des émulateurs de Google, Visual Studio ou Xamarin Android. Les émulateurs Android 7 n’autorise pas de contrôle pour l’instant.|
|WPF|Uniquement pris en charge dans Visual Studio 2017|

<a name="reporting-bugs" />

## <a name="reporting-bugs"></a>Signalement des bogues

Bogues doivent être signalées directement via Visual Studio :

- **Aide > envoyer des commentaires > signaler un problème**

Merci d’inclure toutes les informations suivantes :

### <a name="platform-version-information"></a>Informations de Version de plateforme

Ces informations sont essentielles.

Visual Studio pour Mac

- **Visual Studio > à propos de Visual Studio > Afficher les détails > Copier les informations**
- Collez dans le rapport de bogue

Visual Studio

- **Aide > à propos de Visual Studio > Copier les informations**
- Faites-nous savoir votre version de système d’exploitation et que vous exécutez Windows 32 bits ou 64 bits.

### <a name="log-files"></a>Fichiers journaux

Attachez toujours IDE et l’inspecteur de fichiers journaux du client.

Client de l’inspecteur

- Mac : `~/Library/Logs/Xamarin/Inspector/Xamarin Inspector {date}.log`
- Windows : `%LOCALAPPDATA%\Xamarin\Inspector\logs\Xamarin Inspector {date}.log`

1.4.x offre également la possibilité de sélectionner le fichier journal dans le Finder (macOS) ou de l’Explorateur (Windows) directement à partir du menu principal :

- **Aide > Afficher le fichier journal**

Visual Studio pour Mac

- `~/Library/Logs/VisualStudio/7.0/Ide.log`

Visual Studio

- `%LOCALAPPDATA%\Xamarin\Logs\{VS version}\Inspector {date}.log`
- Le contenu de Visual Studio **sortie** volet peut également être informatif.

### <a name="project-settings"></a>Paramètres du projet

Si vous pouvez attacher le **.csproj** pour le projet que vous voulez inspecter, il serait très utile. Il est plus facile que posées par les paramètres individuels.

Veuillez également confirmer que vous êtes dans la configuration Debug.

### <a name="selected-devices"></a>Périphériques sélectionnés

Pour Android et iOS, il est essentiel que nous savons quel périphérique que vous effectuez le débogage lorsque vous souhaitez examiner. Nous devons savoir :

- Nom du périphérique, comme indiqué dans votre IDE
- Version du système d’exploitation de votre appareil
- Android : Vérifiez que vous utilisez x86 émulateur
- Android : Quelle plate-forme émulateur utilisez-vous ? Émulateur Google ? Émulateur Android de Visual Studio ? Xamarin Android Player ?
- L’application que vous déboguez correctement s’affichent et fonctionnent dans l’appareil ?
- L’appareil a-t-il la connectivité de réseau (vérification via un navigateur web)

[client-bugs]: https://github.com/Microsoft/workbooks/issues/new
