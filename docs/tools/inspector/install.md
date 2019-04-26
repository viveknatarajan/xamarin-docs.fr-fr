---
title: Configuration requise et Installation de l’inspecteur
description: Ce document décrit comment installer Xamarin Inspector et traite le système d’exploitation pris en charge, IDE et plateformes d’applications.
ms.prod: xamarin
ms.assetid: 81174493-02D3-4FF5-AD57-04F3288A7F94
author: lobrien
ms.author: laobri
ms.date: 06/19/2018
ms.openlocfilehash: 2bbf0bda42b7bce483d9d036ebf39314dcb73072
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61373467"
---
# <a name="inspector-installation-and-requirements"></a>Configuration requise et Installation de l’inspecteur

## <a name="download-and-installation"></a>Téléchargement et Installation

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

1. Téléchargez et installez [Visual Studio Enterprise](https://visualstudio.microsoft.com/vs/) et sélectionnez le **développement Mobile en .NET** charge de travail.
1. [Connectez-vous](https://docs.microsoft.com/visualstudio/ide/signing-in-to-visual-studio) pour activer votre abonnement Enterprise.
1. [Inspecter](~/tools/inspector/inspect.md) votre propre application !

# <a name="macostabmacos"></a>[macOS](#tab/macos)

1. Téléchargez et installez [Visual Studio pour Mac](https://visualstudio.microsoft.com/vs/mac/).
1. [Connectez-vous](https://docs.microsoft.com/visualstudio/mac/activation) pour activer votre abonnement Enterprise.
1. [Inspecter](~/tools/inspector/inspect.md) votre propre application !

-----

## <a name="requirements"></a>Configuration requise

### <a name="supported-operating-systems"></a>Supported Operating Systems

- **Mac** -OS X 10.11 ou supérieure
- **Windows** -Windows 7 ou version ultérieure (avec Internet Explorer 11 ou ultérieur et de .NET 4.6.1 ou version ultérieure)

### <a name="supported-ides"></a>IDE pris en charge

- Visual Studio pour Mac
- Visual Studio 2017 avec **développement Mobile en .NET** charge de travail

Inspection de l’application en direct est disponible pour les clients d’entreprise.

<a name="supported-platforms" />

### <a name="supported-app-platforms"></a>Plateformes d’applications pris en charge

|Plateforme d’application|Prise en charge de l’IDE|Notes|
|--- |--- |--- |
|Mac|Uniquement pris en charge dans Visual Studio pour Mac|
|iOS|Prise en charge dans Visual Studio 2017 et Visual Studio pour Mac| Comportement de l’éditeur de liens doit être définie sur **ne pas lier** (sous **Build iOS** options projets) |
|Android|Prise en charge dans Visual Studio 2017 et Visual Studio pour Mac|Doit cibler Android > = 4.0.3, avec **fastdev** activé.<br />Doit utiliser des émulateurs de Google, Visual Studio ou Xamarin Android. Les émulateurs Android 7 n’autorise pas l’inspection pour l’instant.|
|WPF|Uniquement pris en charge dans Visual Studio 2017|

<a name="reporting-bugs" />

## <a name="reporting-bugs"></a>Signalement des bogues

Bogues doivent être signalés directement par le biais de Visual Studio :

- **Aide > envoyer des commentaires > signaler un problème**

Veuillez inclure toutes les informations suivantes :

### <a name="platform-version-information"></a>Informations de Version de plateforme

Ces informations sont vitales.

Visual Studio pour Mac

- **Visual Studio > à propos de Visual Studio > Afficher les détails > Copier les informations**
- Collez dans un rapport de bogues

Visual Studio

- **Aide > à propos de Visual Studio > Copier les informations**
- Donnez-nous votre version de système d’exploitation et que vous exécutez Windows 32 bits ou 64 bits.

### <a name="log-files"></a>Fichiers journaux

Attachez toujours IDE et inspecteur de fichiers journaux du client.

Client de l’inspecteur

- Mac : `~/Library/Logs/Xamarin/Inspector/Xamarin Inspector {date}.log`
- Windows : `%LOCALAPPDATA%\Xamarin\Inspector\logs\Xamarin Inspector {date}.log`

1.4.x comprend également la possibilité de sélectionner le fichier journal dans Finder (macOS) ou l’Explorateur (Windows) directement dans le menu principal :

- **Aide > Afficher le fichier journal**

Visual Studio pour Mac

- `~/Library/Logs/VisualStudio/7.0/Ide.log`

Visual Studio

- `%LOCALAPPDATA%\Xamarin\Logs\{VS version}\Inspector {date}.log`
- Le contenu de Visual Studio **sortie** volet peut également être informative.

### <a name="project-settings"></a>Paramètres du projet

Si vous pouvez attacher le **.csproj** pour le projet que vous voulez inspecter, il peut être extrêmement utile. Cela est plus facile que de parler de paramètres individuels.

Veuillez également confirmer que vous êtes dans la configuration Debug.

### <a name="selected-devices"></a>Appareils sélectionnés

Pour Android et iOS, il est essentiel que nous savons quel appareil que vous déboguez sur lorsque vous voulez inspecter. Nous devons savoir :

- Nom du périphérique, comme indiqué dans votre IDE
- Version du système d’exploitation de votre appareil
- Android : Vérifiez que vous utilisez un x86 émulateur
- Android : Quelle plate-forme émulateur utilisez-vous ? Émulateur Google ? Émulateur Android de Visual Studio ? Xamarin Android Player ?
- L’application que vous déboguez correctement s’affichent et fonctionnent dans l’appareil ?
- L’appareil a-t-il la connectivité réseau (vérification via le navigateur web) ?

[client-bugs]: https://github.com/Microsoft/workbooks/issues/new
