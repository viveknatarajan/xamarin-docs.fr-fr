---
title: "Débogage des intégrations"
ms.topic: article
ms.prod: xamarin
ms.assetid: 90143544-084D-49BF-B44D-7AF943668F6C
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.openlocfilehash: a0873c6b902e29174da5e27a09e8f580d6d69eb7
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="debugging-integrations"></a>Débogage des intégrations

## <a name="debugging-agent-side-integrations"></a>Le débogage côté agent intégrations

Débogage côté agent intégrations mieux faire, utilisez les méthodes de journalisation fournies par le `Log` classe dans `Xamarin.Interactive.Logging`. Consultez le [ `API docs` ](https://developer.xamarin.com/api/type/Xamarin.Interactive.Logging.Log/) pour les méthodes à appeler.

Sur macOS, des messages de journal apparaissent dans les deux le visualiseur du journal (**fenêtre > visionneuse du journal**) et dans le journal du client. Sous Windows, les messages s’affichent uniquement dans le journal du client, car il n’existe aucune visionneuse du journal.

Le journal du client est aux emplacements suivants sur macOS et Windows :

- Mac : `~/Library/Logs/Xamarin/Inspector/Xamarin Inspector {date}.log`
- Windows : `%LOCALAPPDATA%\Xamarin\Inspector\logs\Xamarin Inspector {date}.log`

Une chose à savoir est que lors du chargement des intégrations via classiques `#r` mécanisme pendant le développement, l’assembly d’intégration est récupéré comme un _dépendance_ du classeur et empaqueté avec elle, si un chemin d’accès absolu est non utilisé. Cela peut entraîner des modifications s’affichent ne pas propager, comme si la reconstruction de l’intégration n’avait aucun effet.

## <a name="debugging-client-side-integrations"></a>Débogage des intégrations côté client

Comme les intégrations côté client sont écrites en JavaScript et chargées dans la surface du navigateur web (consultez la [architecture](~/tools/workbooks/sdk/architecture.md) documentation), la meilleure façon de les déboguer à l’aide des outils de développement de WebKit sur Mac, ou à l’aide du sélecteur de F12 sur Windows .

Les deux ensembles d’outils permettent d’afficher la source de JavaScript/TypeScript, définir des points d’arrêt, afficher la sortie de la console, inspecter et modifier le modèle DOM.

### <a name="mac"></a>Mac

Pour activer les outils de développement pour les classeurs Xamarin sur Mac, exécutez la commande suivante dans votre terminal :

```shell
defaults write com.xamarin.Inspector WebKitDeveloperExtras -bool true
```

puis redémarrez les classeurs de Xamarin. Une fois que vous procédez ainsi, vous devez voir **Inspecter un élément** s’affichent dans le menu contextuel et un nouveau **développeur** volet seront disponible dans les préférences de classeurs. Cette option vous permet de choisir si vous souhaitez que les outils de développement ouverts au démarrage :

[![Volet de développeur](debugging-images/developer-pane-small.png)](debugging-images/developer-pane.png#lightbox)

Cette option est uniquement redémarrage ainsi, vous devez redémarrer le client de classeurs afin qu’il prenne effet sur les classeurs. Activer les outils de développement via le menu contextuel ou les préférences affiche l’interface utilisateur familière de Safari :

[![Outils de développement Safari](debugging-images/mac-dev-tools.png)](debugging-images/mac-dev-tools.png#lightbox)

Pour plus d’informations sur les outils de développement Safari, consultez le [documentation d’inspecteur WebKit][webkit-docs].

### <a name="windows"></a>Windows

Sous Windows, l’équipe IE fournit un outil appelé « Sélecteur F12 » qui est un débogueur distant pour les instances d’Internet Explorer incorporés. Vous pouvez trouver l’outil à l’emplacement suivant :

```shell
C:\Windows\System32\F12\F12Chooser.exe
```

Exécution F12 sélecteur et vous devez voir l’instance embedded optimisant la surface du client de classeurs dans la liste. Choisissez et le F12 familier des outils de débogage à partir d’Internet Explorer seront affiche, attachée au client :

[![Outils F12](debugging-images/windows-dev-tools.png)](debugging-images/windows-dev-tools.png#lightbox)

[webkit-docs]: https://trac.webkit.org/wiki/WebInspector