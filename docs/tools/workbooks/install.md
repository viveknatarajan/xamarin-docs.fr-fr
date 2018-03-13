---
title: Installation et configuration requise
ms.topic: article
ms.prod: xamarin
ms.assetid: 9D4E10E8-A288-4C6C-9475-02969198C119
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.openlocfilehash: abc9f9402b55a11e313b9938f07f37e5329b55b6
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/12/2018
---
# <a name="installation-and-requirements"></a>Installation et configuration requise

<script> var inspectorOnLoad = fonction () {var primaryTextBase = « Xamarin classeurs » ; var secondaryTextBase = « ou télécharger pour » ; var inspectorDownloadUrlMac = « https://dl.xamarin.com/interactive/XamarinInteractive.pkg » ; var inspectorDownloadUrlWin = » https://DL.xamarin.com/interactive/XamarinInteractive.msi » ;

  var aPrimary = document.getElementById("inspector-download-primary"); var aSecondary = document.getElementById("inspector-download-secondary");

  var aMac = aPrimary ; var aWin = aSecondary ; var macTextBase = primaryTextBase ; var winTextBase = secondaryTextBase ;

  if (/win/i.test(navigator.platform.toLowerCase())) { aMac = aSecondary; aWin = aPrimary; macTextBase = secondaryTextBase; winTextBase = primaryTextBase; }

  aMac.href = inspectorDownloadUrlMac ; aMac.text = macTextBase + « Mac » ; aWin.href = inspectorDownloadUrlWin ; aWin.text = winTextBase + « Windows » ; };

document.addEventListener("DOMContentLoaded", inspectorOnLoad);
</script>

<a name="install" />

## <a name="download-and-install"></a>Téléchargez et installez

<ol>
  <li>Vérifiez le <a href="#Requirements"> exigences</a> ci-dessous.</li>
  <li>Téléchargez et installez <a href="https://dl.xamarin.com/interactive/XamarinInteractive.pkg" id="inspector-download-primary">Xamarin des classeurs pour Mac</a> (<a href="https://dl.xamarin.com/interactive/XamarinInteractive.msi" id="inspector-download-secondary">ou téléchargement pour Windows</a>).
  </li>
  <li>Démarrer <a href="~/tools/workbooks/workbook.md"> manipulé</a> avec des classeurs ou essayer la <a href="https://developer.xamarin.com/workbooks/">exemples</a>.
    </li>
</ol>

## <a name="requirements"></a>Configuration requise

#### <a name="supported-operating-systems"></a>Supported Operating Systems

- **Mac** -OS X 10.11 ou supérieur
- **Windows** -Windows 7 ou version ultérieure (avec Internet Explorer ou supérieure à 11 et .NET 4.6.1 ou supérieur)

#### <a name="supported-app-platforms"></a>Plateformes d’applications pris en charge

<table>
<thead>
  <tr>
    <th>Plateforme d’application</th>
    <th>Prise en charge du système d’exploitation</th>
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
    <td>Prise en charge sur Mac et Windows</td>
    <td>
      <ul>
        <li>Xamarin.iOS 11.0 et Xcode 9.0 ou version ultérieure doivent être installé sur le Mac.</li>
        <li>IOS classeurs en cours d’exécution sur Windows requiert un hôte de build Mac exécutant tous les éléments ci-dessus et le <a href="~/tools/ios-simulator.md">exécutée à distance iOS Simulator</a> installé sur Windows.</li>
      </ul>
    </td>
  </tr>
  <tr>
    <td>Android</td>
    <td>Prise en charge sur Mac et Windows</td>
    <td>Doit utiliser émulateur Google, Visual Studio ou Xamarin Android, avec un appareil virtuel > = 5.0</td>
  </tr>
  <tr>
    <td>WPF</td>
    <td>Prise en charge uniquement sur Windows</td>
    <td/>
  </tr>
  <tr>
    <td>Console (.NET Framework)</td>
    <td>Prise en charge sur Mac et Windows</td>
    <td/>
  </tr>
  <tr>
    <td>Console (.NET Core)</td>
    <td>Prise en charge sur Mac et Windows</td>
    <td/>
  </tr>
</tbody>
</table>

## <a name="reporting-bugs"></a>Signalement des bogues

Veuillez [signaler des problèmes sur GitHub][bugs]et inclure toutes les informations suivantes :

### <a name="log-files"></a>Fichiers journaux

Attachez toujours les fichiers journaux du client classeurs :

- Mac : `~/Library/Logs/Xamarin/Workbooks/Xamarin Workbooks {date}.log`
- Windows : `%LOCALAPPDATA%\Xamarin\Workbooks\logs\Xamarin Workbooks {date}.log`

1.4.x offre également la possibilité de sélectionner le fichier journal dans le Finder (macOS) ou de l’Explorateur (Windows) directement à partir du menu principal :

- **Fichier d’aide → révéler journal**

#### <a name="log-paths-for-workbooks-13-and-earlier"></a>Journal des chemins d’accès pour les classeurs 1.3 et versions antérieures :

- Mac : `~/Library/Logs/Xamarin/Inspector/Xamarin Inspector {date}.log`
- Windows : `%LOCALAPPDATA%\Xamarin\Inspector\logs\Xamarin Inspector {date}.log`

### <a name="platform-version-information"></a>Informations de Version de plateforme

Il est très utile de connaître les détails sur votre système d’exploitation et Xamarin produits installés.

Dans le menu principal dans les classeurs :

* **Les informations de Version copie →**

#### <a name="instructions-for-workbooks-13-and-earlier"></a>Instructions pour les classeurs 1.3 et versions antérieures :

Visual Studio pour Mac

- **Visual Studio → sur les informations de copie de Visual Studio → Afficher détails →**
- Collez dans le rapport de bogue

Visual Studio

- **Aide → sur Visual Studio → copier les informations**
- Faites-nous savoir votre version de système d’exploitation et que vous exécutez Windows 32 bits ou 64 bits.

### <a name="samples"></a>Exemples

Si vous pouvez attacher ou un lien vers le `.workbooks` fichier que vous rencontrez des problèmes, et qui peut aider à résoudre le bogue plus rapidement.

### <a name="devices"></a>Appareils

Si vous rencontrez des problèmes de connexion de votre iOS ou un classeur Android et ont déjà été [notre page Dépannage](~/tools/workbooks/troubleshooting/index.md), nous aurons besoin de connaître :

- Nom du périphérique que vous essayez de vous connecter à
- Version du système d’exploitation de votre appareil
- Android : Vérifiez que vous utilisez x86 émulateur
- Android : Quelle plate-forme émulateur utilisez-vous ? Émulateur Google ?
  Émulateur Android de Visual Studio ? Xamarin Android Player ?
- iOS sur Windows : version du simulateur iOS à distance de Xamarin vous disposez (Vérifiez `Add/Remove Programs` dans `Control Panel`) ?
- iOS sur Windows : Veuillez également fournir les informations de Version de plateforme pour votre hôte de build Mac
- L’appareil a-t-il la connectivité de réseau (vérification via un navigateur web)

[bugs]: https://github.com/Microsoft/workbooks/issues/new

## <a name="uninstall"></a>Désinstaller

### <a name="windows"></a>Windows

Selon la façon dont vous avez acquis & Inspecteur de classeurs, vous devrez peut-être exécuter deux procédures de désinstallation. Veuillez vérifier ces deux éléments pour complètement désinstaller le logiciel.

#### <a name="visual-studio-installer"></a>Programme d’installation de Visual Studio

Si vous avez Visual Studio 2017, ouvrez **le programme d’installation de Visual Studio**, puis en regardant dans **des composants individuels** pour **Xamarin classeurs**. Si elle est activée, désactivez-la, puis **modifier** à désinstaller.

#### <a name="system-uninstall"></a>Désinstallation du système

Si vous avez installé les classeurs & Inspecteur vous-même avec un programme d’installation téléchargé, elle devra être désinstallées via les **applications & fonctionnalités** page des paramètres système sur Windows 10 ou via **ajouter/supprimer des programmes**dans le panneau de contrôle des versions plus anciennes de Windows.

> **Démarrer → Paramètres → système → applications et fonctionnalités**

![](install-images/windows-remove.png "Classeurs de Xamarin et comme indiqué dans l’inspecteur &quot;applications &amp; fonctionnalités&quot;")

**Vous devez néanmoins suivre la procédure pour Installer Visual Studio pour les classeurs que & Inspecteur de ne pas réinstallé sans votre connaissance.**

<a name="uninstall-macos" />

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

## <a name="downgrading"></a>La rétrogradation

L’identificateur de lot pour `/Applications/Xamarin Workbooks.app` a été remplacée par `com.xamarin.Inspector` à `com.xamarin.Workbooks` dans la version 1.4 pour faciliter une future fractionnement de programmes d’installation de Xamarin classeurs & Inspecteur.

En raison d’un bogue dans les programmes d’installation plus anciens, il n’est pas possible de rétrograder les versions 1.4 ou ultérieure à l’aide de la 1.3.2 ou programmes d’installation plus anciens.

Rétrogradation de 1.4 ou plus récent à 1.3.2 ou antérieure :

1. [Désinstaller manuellement les classeurs & Inspecteur](#macOS)
2. Exécutez le 1.3.2 ou antérieure `.pkg` programme d’installation