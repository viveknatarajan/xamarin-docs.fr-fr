---
title: "Désinstallation de Xamarin"
description: "Désinstallation des produits Xamarin sur un ordinateur"
ms.topic: article
ms.prod: xamarin
ms.assetid: b83a85ec-842a-444c-8f82-c2464eda099b
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 04/08/2017
ms.openlocfilehash: d2db4af7dd13611075bc2b100470b5fb3ba83118
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="uninstalling-xamarin"></a>Désinstallation de Xamarin

> [!IMPORTANT]
> Cet article explique comment désinstaller Xamarin Studio ou autre produit Xamarin sur un ordinateur Mac ou Windows. Pour plus d’informations sur la désinstallation de Visual Studio pour Mac, consultez [Désinstallation de Visual Studio pour Mac](https://docs.microsoft.com/visualstudio/mac/uninstall) sur docs.microsoft.com.

# <a name="overview"></a>Vue d'ensemble

Plusieurs produits Xamarin permettent de développer des applications multiplateformes, notamment des applications autonomes comme Xamarin Studio, mais également des extensions d’autres applications telles que le support Xamarin dans Visual Studio.

Ce guide explique comment supprimer la fonctionnalité Xamarin sur Mac OS, ou à partir de Visual Studio sur Windows :

1.  [Désinstallation de Xamarin Studio](#uninstallxamarinstudio)
  1.  [Désinstallation de Mono](#uninstallmono)
  1.  [Désinstallation de Xamarin.Android](#uninstallandroid)
  1.  [Désinstallation de Xamarin.iOS](#uninstallios)
  1.  [Désinstallation de Xamarin.Mac](#uninstallmac)
  2.  [Désinstallation d’Inspector et de Workbooks](#uninstallworkbooks)
1.  [Désinstallation de Xamarin sur un système Windows](#uninstallwindows)
  1.  [Désinstallation de Xamarin à partir de Visual Studio 2015 et versions antérieures](#uninstallvs2015)
  1.  [Désinstallation de Xamarin à partir de Visual Studio 2017](#uninstallvs2017)
1.  [Désinstallation de Visual Studio pour Mac](#uninstallvsmac)

Si vous devez réinstaller Xamarin à l’aide d’Universal Installer, il est recommandé de toujours redémarrer l’ordinateur avant de procéder.

# <a name="uninstalling-xamarin-on-mac"></a>Désinstallation de Xamarin sur un système Mac

Ce guide peut être utilisé pour désinstaller chaque produit individuellement en accédant à la section appropriée. La totalité des outils Xamarin peuvent être désinstallés en suivant ce guide jusqu’au bout.

Pour plus d’informations sur l’utilisation du [script de désinstallation](http://download.xamarin.com/developer/cross-platform/xamarin_uninstall.sh), passez directement à la section [Utilisation du script de désinstallation](#uninstallscript) au bas de ce guide.

<a name="uninstallxamarinstudio" />

## <a name="uninstall-xamarin-studio"></a>Désinstaller Xamarin Studio

Pour désinstaller Xamarin Studio sur un système Mac, la première chose à faire est de localiser **Xamarin Studio.app** dans le répertoire **/Applications**, puis de le faire glisser dans la **Corbeille**. Vous pouvez aussi cliquer avec le bouton droit et sélectionner **Placer dans la Corbeille**, comme illustré ci-dessous :

 [ ![](uninstalling-xamarin-images/image1.png "Vous pouvez aussi cliquer avec le bouton droit et sélectionner Placer dans la Corbeille, comme illustré ici")](uninstalling-xamarin-images/image1.png)

La suppression de ce bundle d’applications a pour effet de désinstaller Xamarin Studio. Toutefois, d’autres fichiers relatifs à Xamarin peuvent subsister dans le système de fichiers.

Pour supprimer toutes les traces de Xamarin Studio, les commandes suivantes doivent être exécutées dans Terminal :

```bash
sudo rm -rf "/Applications/Xamarin Studio.app"
rm -rf ~/Library/Caches/XamarinStudio-*
rm -rf ~/Library/Preferences/XamarinStudio-*
rm -rf ~/Library/Logs/XamarinStudio-*
rm -rf ~/Library/XamarinStudio-*
```

<a name="uninstallmono" />

## <a name="uninstall-mono-sdk-mdk"></a>Désinstaller le SDK Mono (MDK)

Mono est une implémentation open source de Microsoft .NET Framework. Il est utilisé par tous les produits Xamarin (Xamarin.iOS, Xamarin.Android et Xamarin.Mac) pour permettre le développement de ces plateformes en C#.

> [!IMPORTANT]
> Remarque : d’autres applications en dehors de Xamarin utilisent également Mono, par exemple, Unity. Vérifiez qu’il n’existe pas d’autres dépendances de Mono avant de le désinstaller.

Pour supprimer le framework Mono sur une machine, exécutez les commandes suivantes dans Terminal :

```bash
sudo rm -rf /Library/Frameworks/Mono.framework
sudo pkgutil --forget com.xamarin.mono-MDK.pkg
```

<a name="uninstallandroid" />

## <a name="uninstall-xamarinandroid"></a>Désinstaller Xamarin.Android

Plusieurs éléments sont nécessaires pour l’installation et l’utilisation de Xamarin.Android, comme Android SDK et le Java SDK. Pour plus d’informations sur les composants nécessaires, consultez le guide d’[installation manuelle](https://docs.microsoft.com/visualstudio/mac/installation/).

Utilisez les commandes suivantes pour supprimer Xamarin.Android :

```bash
sudo rm -rf /Developer/MonoDroid
rm -rf ~/Library/MonoAndroid
sudo pkgutil --forget com.xamarin.android.pkg
sudo rm -rf /Library/Frameworks/Xamarin.Android.framework
```

### <a name="uninstall-android-sdk-and-java-sdk"></a>Désinstaller Android SDK et le Java SDK

Android SDK est nécessaire pour le développement d’applications Android. Pour supprimer l’intégralité des composants du kit Android SDK, recherchez le fichier dans **~/Library/Developer/Xamarin/**, puis placez-le dans la **Corbeille**, comme illustré ci-dessous :

 [ ![](uninstalling-xamarin-images/image2.png "Pour supprimer l’intégralité des composants du kit Android SDK, recherchez le fichier, puis placez-le dans la Corbeille, comme illustré ici")](uninstalling-xamarin-images/image2.png)

Il n’est pas nécessaire de désinstaller le SDK Java (JDK), car il est déjà préempaqueté dans Mac OS X.

<a name="uninstallios" />

## <a name="uninstall-xamarinios"></a>Désinstaller Xamarin.iOS

Xamarin.iOS permet le développement d’applications iOS à l’aide des langages C# et F# avec Xamarin Studio pour Mac.
Xamarin Build Host était également installé automatiquement avec les versions antérieures de Xamarin.iOS, pour permettre les développements iOS dans Visual Studio. Pour désinstaller les deux d’une machine, suivez les étapes ci-dessous :

Utilisez les commandes suivantes dans Terminal pour supprimer tous les fichiers Xamarin.iOS d’un système de fichiers :

```bash
rm -rf ~/Library/MonoTouch
sudo rm -rf /Library/Frameworks/Xamarin.iOS.framework
sudo rm -rf /Developer/MonoTouch
sudo pkgutil --forget com.xamarin.monotouch.pkg
sudo pkgutil --forget com.xamarin.xamarin-ios-build-host.pkg
```

### <a name="uninstall-the-mac-build-host"></a>Désinstaller le Build Host Mac

Remarque : Le Build Host a peut-être déjà été supprimé si vous avez effectué une mise à jour vers Xamarin 4. Pour supprimer l’application Build Host, exécutez la commande suivante dans Terminal :

```bash
sudo rm -rf "/Applications/Xamarin.iOS Build Host.app"
```

Le processus Build Host ou la tâche `launchd` peuvent encore être exécutés ou en train d’écouter certains ports.
Vous pouvez vérifier leur état en exécutant `launchctl list | grep com.xamarin.mtvs.buildserver` dans Terminal.

```bash
sudo launchctl unload /Library/LaunchAgents/com.xamarin.mtvs.buildserver.plist
sudo rm -f /Library/LaunchAgents/com.xamarin.mtvs.buildserver.plist
```

<a name="uninstallmac" />

## <a name="uninstall-xamarinmac"></a>Désinstaller Xamarin.Mac

Une fois que Xamarin Studio a été correctement désinstallé, Xamarin.Mac peut être supprimé de votre machine à l’aide des deux commandes suivantes pour supprimer respectivement le produit et la licence de votre Mac :

```bash
sudo rm -rf /Library/Frameworks/Xamarin.Mac.framework
rm -rf ~/Library/Xamarin.Mac
```

<a name="uninstallworkbooks" />

## <a name="uninstall-workbooks-and-inspector"></a>Désinstaller Workbooks et Inspector

La commande Bash suivante supprime Xamarin Inspector et Xamarin Workbooks version 1.2.2 et ultérieures :

```bash
sudo /Library/Frameworks/Xamarin.Interactive.framework/Versions/Current/uninstall
```

Pour les versions antérieures, consultez le guide de désinstallation de [Workbooks](~/tools/workbooks/install.md#uninstall-macos).

## <a name="uninstall-the-xamarin-installer"></a>Désinstaller Xamarin Installer

Utilisez les commandes suivantes pour supprimer toutes les traces de Xamarin Universal Installer :

```bash
rm -rf ~/Library/Caches/XamarinInstaller/
rm -rf ~/Library/Logs/XamarinInstaller/
rm -rf ~/Library/Preferences/Xamarin/
```

<a name="uninstallscript" />

## <a name="using-the-uninstall-script"></a>Utilisation du script de désinstallation

Le [script de désinstallation](http://download.xamarin.com/developer/cross-platform/xamarin_uninstall.sh) va supprimer Xamarin de l’ordinateur. Pour utiliser le script de désinstallation :

1.  Téléchargez le script de désinstallation et notez l’emplacement de téléchargement. Par défaut, il s’agit du répertoire **/Downloads**.
1.  Ouvrez **Terminal**, puis accédez au répertoire de travail où le script a été téléchargé :

        $ cd /location/of/file

1. Rendez le script exécutable et exécutez-le avec **sudo** :

        $ chmod +x ./xamarin_uninstall.sh
        $ sudo ./xamarin_uninstall.sh

1. Enfin, supprimez le script de désinstallation.

À ce stade, Xamarin doit être désinstallé de votre ordinateur.

<a name="uninstallwindows" />

# <a name="uninstalling-xamarin-on-windows"></a>Désinstallation de Xamarin sur un système Windows

<a name="uninstallvs2015" />

## <a name="visual-studio-2015-and-earlier"></a>Visual Studio 2015 et versions antérieures

Sur les ordinateurs Windows, Xamarin peut être désinstallé via le **Panneau de configuration**. Accédez à **Programmes et fonctionnalités** ou à **Programmes > Désinstaller un programme**, comme illustré ci-dessous :

 [ ![](uninstalling-xamarin-images/image3.png "Accédez à Programmes et fonctionnalités ou à Programmes > Désinstaller un programme, comme illustré ici")](uninstalling-xamarin-images/image3.png) [ ![](uninstalling-xamarin-images/image4.png "Accédez à Programmes et fonctionnalités ou à Programmes > Désinstaller un programme, comme illustré ici")](uninstalling-xamarin-images/image4.png)

Pour désinstaller Xamarin Studio, recherchez **Xamarin Studio 5.x.x** dans la liste des programmes, puis cliquez sur le bouton **Désinstaller**. Pour supprimer l’extension Xamarin pour Visual Studio et les SDK, recherchez **Xamarin** dans la liste des programmes, puis cliquez sur **Désinstaller**. Ces désinstallations sont illustrées dans la capture d’écran ci-dessous :

 [ ![](uninstalling-xamarin-images/image4a.png "Ces désinstallations sont illustrées dans cette capture d’écran")](uninstalling-xamarin-images/image4a.png)

Les programmes suivants peuvent également être désinstallés dans le but de supprimer l’intégralité des composants Xamarin :

-  Kit de développement logiciel Android SDK


  [ ![](uninstalling-xamarin-images/image5.png "Les programmes suivants peuvent également être désinstallés dans le but de supprimer l’intégralité des composants Xamarin")](uninstalling-xamarin-images/image5.png)
-  GTK#


  [ ![](uninstalling-xamarin-images/image6.png "Les programmes suivants peuvent également être désinstallés dans le but de supprimer l’intégralité des composants Xamarin")](uninstalling-xamarin-images/image6.png)
-  Xamarin Universal Installer


 [ ![](uninstalling-xamarin-images/image7.png "Les programmes suivants peuvent également être désinstallés dans le but de supprimer l’intégralité des composants Xamarin")](uninstalling-xamarin-images/image7.png)
-  Le SDK Java (avant de le supprimer, vérifiez que d’autres programmes n’en dépendent pas)


 [ ![](uninstalling-xamarin-images/image8.png "Avant de supprimer le SDK Java, vérifiez que d’autres programmes n’en dépendent pas")](uninstalling-xamarin-images/image8.png)

Pour désinstaller complètement Visual Studio, suivez les [instructions de Microsoft](https://msdn.microsoft.com/library/mt720585.aspx).


<a name="uninstallvs2017" />

# <a name="visual-studio-2017"></a>Visual Studio 2017

Xamarin peut être désinstallé à partir de Visual Studio 2017, à l’aide de l’application Visual Studio Installer :

1. Utilisez le **menu Démarrer** pour ouvrir **Visual Studio Installer**.

  [ ![](uninstalling-xamarin-images/vs2017-01-sml.png "Lancez Visual Studio Installer")](uninstalling-xamarin-images/vs2017-01.png)

1. Appuyez sur le bouton **Modifier** pour l’instance que vous souhaitez modifier.

  [ ![](uninstalling-xamarin-images/vs2017-02-sml.png "Appuyez sur le bouton Modifier")](uninstalling-xamarin-images/vs2017-02.png)

1. Sous l’onglet **Charges de travail**, décochez l’option  **Développement mobile en .NET** (dans la section **Mobile et jeux**).

  [ ![](uninstalling-xamarin-images/vs2017-03-sml.png "Décochez la charge de travail Développement mobile en .NET")](uninstalling-xamarin-images/vs2017-03.png)

1. Cliquez sur le bouton **Modifier** situé en bas à droite de la fenêtre.
1. Visual Studio Installer va supprimer les composants qui ont été désélectionnés (Visual Studio 2017 doit être fermé pour que Visual Studio Installer puisse apporter les modifications nécessaires).

  [ ![](uninstalling-xamarin-images/vs2017-04-sml.png "Appuyez sur le bouton Modifier")](uninstalling-xamarin-images/vs2017-04.png)

Pour supprimer les composants Xamarin un à un (par exemple, Profiler ou Workbooks), accédez à l’onglet **Composants individuels** à l’étape 3, puis décochez les composants de votre choix :

[ ![](uninstalling-xamarin-images/vs2017-components-sml.png "Désinstallez les composants un à un")](uninstalling-xamarin-images/vs2017-components.png)

Pour désinstaller entièrement Visual Studio 2017, choisissez **Désinstaller** dans le menu en regard du bouton **Lancer** (représenté par trois barres superposées).

[ ![](uninstalling-xamarin-images/vs2017-uninstall-sml.png "Désinstallez entièrement Visual Studio")](uninstalling-xamarin-images/vs2017-uninstall.png)

> [!IMPORTANT]
> **AVERTISSEMENT :** si vous avez installé deux instances (ou plus) de Visual Studio côte à côte (SxS), par exemple une Version et une Préversion, la désinstallation d’une instance peut supprimer certaines fonctionnalités Xamarin des autres instances de Visual Studio, notamment :
>
> - Xamarin Profiler
> - Xamarin Workbooks/Inspector
> - Xamarin Remote iOS Simulator
> - SDK Apple Bonjour
>
> Sous certaines conditions, la désinstallation de l’une des instances côte à côte peut entraîner la suppression de ces fonctionnalités. Cela peut alors dégrader les performances de la plateforme Xamarin sur les instances restantes de Visual Studio.
>
>Pour résoudre ce problème, vous pouvez exécuter l’option **Réparer** de Visual Studio Installer, dans le but de réinstaller les composants manquants.

<a name="uninstallvsmac" />

# <a name="uninstalling-visual-studio-for-mac"></a>Désinstallation de Visual Studio pour Mac

Si vous souhaitez désinstaller Visual Studio pour Mac mais continuer d’utiliser Xamarin Studio, recherchez **Visual Studio.app** dans le répertoire **/Applications**, puis faites-le glisser dans la Corbeille. Vous pouvez aussi cliquer avec le bouton droit et sélectionner **Placer dans la Corbeille**, comme illustré ci-dessous :

 [ ![](uninstalling-xamarin-images/image9.png "Cliquez avec le bouton droit sur l’icône Visual Studio, puis sélectionnez Placer dans la Corbeille")](uninstalling-xamarin-images/image9.png)

Pour désinstaller complètement Xamarin de votre ordinateur, désinstallez d’abord Visual Studio pour Mac, puis suivez les étapes indiquées dans la section [Désinstaller Xamarin Studio](#uninstallxamarinstudio).

# <a name="summary"></a>Récapitulatif

Dans cet article, nous avons vu comment désinstaller complètement Xamarin d’un système Mac à l’aide de commandes Terminal. Nous avons également vu comment désinstaller Xamarin d’un système Windows via l’option **Programmes et fonctionnalités** pour Visual Studio 2015 et versions antérieures, et à l’aide de **Visual Studio Installer** pour Visual Studio 2017.


## <a name="related-links"></a>Liens associés

- [Désinstaller le script (échantillon)](http://download.xamarin.com/developer/cross-platform/xamarin_uninstall.sh)
