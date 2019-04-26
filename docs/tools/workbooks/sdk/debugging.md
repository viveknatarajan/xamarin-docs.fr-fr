---
title: Débogage des intégrations
description: Ce document décrit comment déboguer des intégrations de Xamarin Workbooks, côté agent et côté client sur Windows et Mac.
ms.prod: xamarin
ms.assetid: 90143544-084D-49BF-B44D-7AF943668F6C
author: lobrien
ms.author: laobri
ms.date: 06/19/2018
ms.openlocfilehash: 86d9c6af93e7f59eb0e819730e46324688df7566
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61293711"
---
# <a name="debugging-integrations"></a>Débogage des intégrations

## <a name="debugging-agent-side-integrations"></a>Débogage des intégrations de côté de l’agent

Débogage des intégrations de côté de l’agent est mieux effectué en utilisant les méthodes de journalisation fournies par le `Log` classe dans `Xamarin.Interactive.Logging`. Consultez le [ `API docs` ](https://developer.xamarin.com/api/type/Xamarin.Interactive.Logging.Log/) pour les méthodes à appeler.

Sur macOS, les messages de journal s’affichent dans les deux le menu de visionneuse du journal (**fenêtre > visionneuse du journal**) et dans le journal du client. Sur Windows, messages apparaissent uniquement dans le journal du client, car il n’existe aucune visionneuse du journal.

Le journal du client est aux emplacements suivants sur macOS et Windows :

- Mac : `~/Library/Logs/Xamarin/Workbooks/Xamarin Workbooks {date}.log`
- Windows : `%LOCALAPPDATA%\Xamarin\Workbooks\logs\Xamarin Workbooks {date}.log`

Il faut être conscient est que lors du chargement des intégrations via habituelles `#r` mécanisme pendant le développement, l’assembly de l’intégration est récupéré en tant qu’un _dépendance_ du classeur et empaqueté avec lui, si un chemin d’accès absolu est non utilisé. Cela peut entraîner des modifications s’affichent ne pas propagation, comme si la reconstruction de l’intégration n’avait aucun effet.

## <a name="debugging-client-side-integrations"></a>Débogage des intégrations de côté client

Comme les intégrations côté client sont écrites en JavaScript et chargées dans notre surface du navigateur web (voir la [architecture](~/tools/workbooks/sdk/architecture.md) documentation), la meilleure façon de les déboguer à l’aide des outils de développement WebKit sur Mac, ou à l’aide du sélecteur de F12 sur Windows .

Les deux ensembles d’outils permettent d’afficher le code source JavaScript/TypeScript, de définir des points d’arrêt, d’afficher la sortie de la console et d’inspecter et de modifier le modèle DOM.

### <a name="mac"></a>Mac

Pour activer les outils de développement pour Xamarin Workbooks sur Mac, exécutez la commande suivante dans votre terminal :

```shell
defaults write com.xamarin.Workbooks WebKitDeveloperExtras -bool true
```

puis redémarrez Xamarin Workbooks. Une fois que vous le faites, vous devez voir **Inspecter un élément** s’affichent dans le menu contextuel et un nouveau **développeur** volet sera disponible dans les préférences de classeurs. Cette option vous permet de choisir si vous souhaitez que les outils de développement ouverts au démarrage :

[![Volet de développeur](debugging-images/developer-pane-small.png)](debugging-images/developer-pane.png#lightbox)

Cette préférence est redémarrage seule ainsi, vous devrez redémarrer le client de classeurs pour qu’elle prenne effet sur les classeurs. Activer les outils de développement via le menu contextuel ou les préférences affiche l’interface familière de Safari :

[![Outils de développement de Safari](debugging-images/mac-dev-tools.png)](debugging-images/mac-dev-tools.png#lightbox)

Pour plus d’informations sur l’utilisation des outils de développement de Safari, consultez le [documentation d’inspecteur WebKit][webkit-docs].

### <a name="windows"></a>Windows

Sur Windows, l’équipe IE fournit un outil appelé « Sélecteur F12 » qui est un débogueur distant pour les instances d’Internet Explorer embedded. Vous pouvez trouver l’outil à l’emplacement suivant :

```shell
C:\Windows\System32\F12\F12Chooser.exe
```

Exécution Sélecteur F12, vous devriez voir l’instance qui alimente la surface de client de classeurs dans la liste. Choisissez et le F12 familière des outils de débogage à partir d’Internet Explorer seront affiche, attaché au client :

[![Outils F12](debugging-images/windows-dev-tools.png)](debugging-images/windows-dev-tools.png#lightbox)

[webkit-docs]: https://trac.webkit.org/wiki/WebInspector
