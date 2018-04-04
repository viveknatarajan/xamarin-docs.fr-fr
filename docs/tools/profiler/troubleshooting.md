---
title: Résolution des problèmes de Xamarin Profiler
description: Résolution des problèmes de Xamarin Profiler
ms.prod: xamarin
ms.assetid: 0060E9D1-C003-4E4C-ADE8-B406978FE891
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 10/27/2017
ms.openlocfilehash: 4844c999ceddcee89d4f45f6e41dd4c7f2caf054
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="xamarin-profiler-troubleshooting"></a>Résolution des problèmes de Xamarin Profiler

_Résolution des problèmes de Xamarin Profiler_

## <a name="logging-and-diagnostics"></a>Journalisation et Diagnostics

L’équipe Xamarin peut aider à effectuer le suivi des problèmes si vous nous fournissez des informations, y compris :

- Une capture du problème, incident, ou échec et de votre flux de travail mènent à ce dernier.
- Sorties de journal (voir ci-dessous).
- Le **.mlpd** générées pour la session de profilage (voir ci-dessous).

### <a name="getting-log-outputs"></a>Obtention des sorties de journal
Sur Mac journaux sont enregistrés dans `~/Library/Logs/Xamarin.Profiler/Profiler.<date>.log`.

Sur Windows, ceux-ci sont enregistrés sur `%appdata%Local//Xamarin/Log/Xamarin.Profiler/Profiler.<date>.log` Veuillez inclure le dernier journal chaque fois que vous envoyez un problème.

Nous ajoutons une journalisation plus mesure que nous, donc cette sortie doit augmenter et devenir plus utile au fil du temps.

<a name="gen_mlpd" />

### <a name="generating-mlpd-files"></a>Génération de fichiers .mlpd

Un **.mlpd** fichier est la sortie compressée du profileur runtime mono. L’interface utilisateur graphique de profileur Xamarin lit les données dans un **.mlpd** et il affiche pour l’utilisateur. **.mlpd** les fichiers sont utiles, outils de débogage pour Xamarin, car elles aider nos ingénieurs diagnostiquer les problèmes que le profileur peut rencontrer avec vos données.

Le **.mlpd** pour la session active est automatiquement enregistrée dans votre Mac `/tmp` active et peuvent être identifiées par l’horodatage. Si vous activez la journalisation, la première sortie sera le chemin d’accès à la **.mlpd** fichier. Le **.mlpd** fichier normalement sera enregistré dans le répertoire de démarrage ~/var/dossiers...

Le **.mlpd** pour une session en cours peut également être enregistrée en choisissant **fichier > Enregistrer sous...** à partir du menu du profileur :

**Visual Studio pour Mac**:

![](troubleshooting-images/image17.png "L’enregistrement de fichier .mlpd dans Visual Studio pour Mac")

**Visual Studio**:

![](troubleshooting-images/image17-vs.png "L’enregistrement du fichier de .mlpd dans Visual Studio")


Il est important de noter que **.mlpd** contiennent de nombreuses informations et la taille du fichier sera volumineux.

## <a name="troubleshooting"></a>Résolution des problèmes

La liste ci-dessous indique courantes pièges, solutions de contournement et trucs et astuces pour l’aide du profileur.

> [!NOTE]
> **Remarque**: vous devez être un Visual Studio **Enterprise** abonné pour déverrouiller cette fonctionnalité soit Visual Studio Enterprise sur Windows ou de Visual Studio pour Mac.

#### <a name="i-cant-see-the-ios-profiler-option-or-it-is-greyed-out-visual-studio-and-visual-studio-for-mac"></a>Je ne peux pas voir l’option de profileur iOS, ou il est grisé [Visual Studio et Visual Studio pour Mac]

Vérifiez les paramètres suivants pour résoudre ce problème :

- Vérifiez que vous utilisez la configuration Debug
- Vérifiez que vous utilisez le SGen Garbage collector.
- Assurez-vous que la plateforme est [pris en charge](~/tools/profiler/index.md#Profiler_Support).
- Assurez-vous que vous disposez de la licence appropriée.
- Assurez-vous que vous êtes connecté dans et correctement authentifié.
- (Visual Studio) Vous devez utiliser [Visual Studio Enterprise](https://www.visualstudio.com/vs/enterprise/) et posséder une licence d’entreprise valide.


#### <a name="i-get-an-error-when-i-try-to-launch-the-profiler"></a>J’obtiens une erreur lorsque vous tentez de lancer le Générateur de profils

Si vous exécutez dans la zone de cette erreur lorsque vous utilisez le profileur dans Visual Studio :

![](troubleshooting-images/error.png "Boîte d’erreur lorsque vous utilisez le profileur dans Visual Studio")

Il s’agit normalement en raison de l’impossibilité de se lancer dans le simulateur / émulateur. Essayez et exécuter l’application normalement, corrigez les problèmes qu’il offre et essayez ensuite d’utiliser le profileur à nouveau.

#### <a name="to-watch-a-specific-thread"></a>Pour surveiller un thread spécifique

Si vous disposez d’un thread que vous souhaitez surveiller en particulier, il est idéal nommer le thread à la tout début de sa création afin que get `ThreadName` au lieu de `0x0`. Par exemple définir le nom de thread en tant que l’interface utilisateur, vous pouvez utiliser le code suivant :


```csharp
RunOnUiThread (() => {
  Thread.CurrentThread.Name  = "UI";
});
```



## <a name="related-links"></a>Liens associés

- [Procédure pas à pas : utilisation du profileur Xamarin](~/tools/profiler/index.md)
- [Mémoire et les performances les meilleures pratiques](~/cross-platform/deploy-test/memory-perf-best-practices.md)
- [Notes de publication](https://developer.xamarin.com/releases/profiler/preview/)
