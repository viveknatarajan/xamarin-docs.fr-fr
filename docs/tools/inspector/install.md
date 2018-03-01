---
title: Installation et configuration requise
ms.topic: article
ms.prod: xamarin
ms.assetid: 81174493-02D3-4FF5-AD57-04F3288A7F94
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 03/29/2017
ms.openlocfilehash: a587935e35882ed1dc68817fbbe1ae3e91200f29
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="installation-and-requirements"></a>Installation et configuration requise

<script> var inspectorOnLoad = fonction () {var primaryTextBase = « Les classeurs Xamarin & Inspecteur pour » ; var secondaryTextBase = « ou télécharger pour » ; var inspectorDownloadUrlMac = « https://dl.xamarin.com/interactive/XamarinInteractive.pkg » ; var inspectorDownloadUrlWin = « https://dl.xamarin.com/interactive/XamarinInteractive.msi » ;

  var aPrimary = document.getElementById("inspector-download-primary"); var aSecondary = document.getElementById("inspector-download-secondary");

  var aMac = aPrimary ; var aWin = aSecondary ; var macTextBase = primaryTextBase ; var winTextBase = secondaryTextBase ;

  if (/win/i.test(navigator.platform.toLowerCase())) { aMac = aSecondary; aWin = aPrimary; macTextBase = secondaryTextBase; winTextBase = primaryTextBase; }

  aMac.href = inspectorDownloadUrlMac ; aMac.text = macTextBase + « Mac » ; aWin.href = inspectorDownloadUrlWin ; aWin.text = winTextBase + « Windows » ; };

document.addEventListener("DOMContentLoaded", inspectorOnLoad);
</script>

## <a name="download-and-installation"></a>Téléchargement et Installation

<ol>
  <li>Téléchargez et installez <a href="https://dl.xamarin.com/interactive/XamarinInteractive.pkg" id="inspector-download-primary">Xamarin classeurs & Inspecteur pour Mac</a> (<a href="https://dl.xamarin.com/interactive/XamarinInteractive.msi" id="inspector-download-secondary">ou téléchargement pour Windows</a>).
  </li>
  <li><a href="~/tools/inspector/inspect.md"> Vérifiez que votre propre application !</a>
    </li>
</ol>

## <a name="requirements"></a>Configuration requise

### <a name="supported-operating-systems"></a>Supported Operating Systems

- **Mac** -OS X 10.11 ou supérieur
- **Windows** -Windows 7 ou version ultérieure (avec Internet Explorer ou supérieure à 11 et .NET 4.6.1 ou supérieur)

### <a name="supported-ides"></a>Prise en charge IDE

- Xamarin Studio 6.2 ou supérieure
- Visual Studio pour Mac Preview 4 ou supérieur
- Visual Studio 2015 avec Xamarin 4.3.x ou supérieur
- Visual Studio 2017, avec une charge de travail de Xamarin

Inspection de l’application en temps réel est disponible pour les clients d’entreprise.

<a name="supported-platforms" />

### <a name="supported-app-platforms"></a>Plateformes d’applications pris en charge

<table>
<thead>
  <tr>
    <th>Plateforme d’application</th>
    <th>Prise en charge IDE</th>
    <th>Notes</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Mac (unifié)</td>
    <td>Prise en charge uniquement sur Mac</td>
    <td/>
  </tr>
  <tr>
    <td>iOS (unifié)</td>
    <td>Prise en charge dans XS et Visual Studio</td>
    <td>Inspection des applications iOS à partir de Windows requiert la même version de l’inspecteur de peuvent également être installés sur l’hôte de build Mac.</td>
  </tr>
  <tr>
    <td>Android</td>
    <td>Prise en charge dans XS et Visual Studio</td>
    <td>
      <ul>
        <li>Doivent cibler Android > = 4.0.3</li>
        <li>Doit avoir fastdev activé</li>
        <li>Doit utiliser des émulateurs de Google, Visual Studio ou Xamarin Android. Les émulateurs Android 7 n’autorise pas de contrôle pour l’instant.</li>
      </ul>
    </td>
  </tr>
  <tr>
    <td>WPF</td>
    <td>Uniquement pris en charge dans Visual Studio sous Windows</td>
    <td/>
  </tr>
</tbody>
</table>

<a name="reporting-bugs" />

## <a name="reporting-bugs"></a>Signalement des bogues

Bogues doivent être signalées directement via Visual Studio :

- **Aide → envoyer des commentaires → signaler un problème**

Merci d’inclure toutes les informations suivantes :

### <a name="platform-version-information"></a>Informations de Version de plateforme

Ces informations sont essentielles.

Visual Studio pour Mac

- **Visual Studio → sur les informations de copie de Visual Studio → Afficher détails →**
- Collez dans le rapport de bogue

Xamarin Studio

- **Xamarin Studio → sur Xamarin Studio → afficher des détails → copier les informations**
- Collez dans le rapport de bogue

Visual Studio

- **Aide > à propos de Visual Studio > Copier les informations**
- Faites-nous savoir votre version de système d’exploitation et que vous exécutez Windows 32 bits ou 64 bits.

### <a name="log-files"></a>Fichiers journaux

Attachez toujours IDE et l’inspecteur de fichiers journaux du client.

Client de l’inspecteur

- Mac : `~/Library/Logs/Xamarin/Inspector/Xamarin Inspector {date}.log`
- Windows : `%LOCALAPPDATA%\Xamarin\Inspector\logs\Xamarin Inspector {date}.log`

1.4.x offre également la possibilité de sélectionner le fichier journal dans le Finder (macOS) ou de l’Explorateur (Windows) directement à partir du menu principal :

- **Fichier d’aide → révéler journal**

Visual Studio pour Mac

- `~/Library/Logs/VisualStudio/7.0/Ide.log`

Xamarin Studio

- `~/Library/Logs/XamarinStudio-6.0/Ide.log`

Visual Studio

- `%LOCALAPPDATA%\Xamarin\Logs\{VS version}\Inspector {date}.log`
- Le contenu de Visual Studio `Output` volet peut également être informatif.

### <a name="project-settings"></a>Paramètres du projet

Si vous pouvez attacher le `.csproj` pour le projet que vous voulez inspecter, il serait très utile. Il est plus facile que posées par les paramètres individuels.

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

## <a name="uninstall"></a>Désinstaller

### <a name="windows"></a>Windows

Selon la façon dont vous avez acquis & Inspecteur de classeurs, vous devrez peut-être exécuter deux procédures de désinstallation. Veuillez vérifier ces deux éléments pour complètement désinstaller le logiciel.

#### <a name="visual-studio-installer"></a>Programme d’installation de Visual Studio

Si vous avez Visual Studio 2017, ouvrez « Programme d’installation de Visual Studio » et rechercher dans les « Composants individuels » pour « Les classeurs Xamarin ». Si elle est activée, désactivez-la et puis cliquez sur « Modifier » à désinstaller.

#### <a name="system-uninstall"></a>Désinstallation du système

Si vous avez installé les classeurs & Inspecteur vous-même avec un programme d’installation téléchargé, elle devra être désinstallées via les **applications & fonctionnalités** page des paramètres système sur Windows 10 ou via **ajouter/supprimer des programmes**dans le panneau de contrôle des versions plus anciennes de Windows.

> **Démarrer → Paramètres → système → applications et fonctionnalités**

![](install-images/windows-remove.png "Xamarin des classeurs et l’inspecteur comme indiqué dans « Applications et fonctionnalités »")

**Vous devez néanmoins suivre la procédure pour Installer Visual Studio pour les classeurs que & Inspecteur de ne pas réinstallé sans votre connaissance.**

### <a name="macos"></a>macOS

En commençant par [1.2.2](https://developer.xamarin.com/releases/interactive/interactive-1.2/), les classeurs Xamarin & Inspecteur peuvent être désinstallé à partir d’un terminal en exécutant :

```bash
sudo /Library/Frameworks/Xamarin.Interactive.framework/Versions/Current/uninstall
```

Le programme de désinstallation décrit en détail les fichiers et répertoires qu’il supprime et demander confirmation avant de continuer.

Passez le `-help` l’argument de la `uninstall` script pour des scénarios plus avancés.

Pour les versions antérieures, vous devez supprimer manuellement les éléments suivants :

1. Supprimez l’application Workbooks dans`"/Applications/Xamarin Workbooks.app"`
2. Supprimez l’application Inspector dans`"Applications/Xamarin Inspector.app"`
2. Supprimez les compléments : `"~/Library/Application Support/XamarinStudio-6.0/LocalInstall/Addins/Xamarin.Interactive"` et `"~/Library/Application Support/XamarinStudio-6.0/LocalInstall/Addins/Xamarin.Inspector"`
3. Supprimez Inspector et les fichiers de prise en charge ici : `/Library/Frameworks/Xamarin.Interactive.framework` et `/Library/Frameworks/Xamarin.Inspector.framework`

