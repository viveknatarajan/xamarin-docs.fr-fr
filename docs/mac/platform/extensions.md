---
title: "Prise en charge de l’Extension de Xamarin.Mac"
description: "Cet article décrit la prise en charge de l’extension dans Xamarin.Mac version 2.10 (et supérieure)."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4148F1BE-DFA0-46B6-9FCD-425A6541F510
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 5ce20322b576b12ff9dfe56ef0bc9d2e1ca27792
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/28/2018
---
# <a name="xamarinmac-extension-support"></a>Prise en charge de l’Extension de Xamarin.Mac

Dans l’aperçu de Xamarin.Mac 2.10 prise en charge a été ajouté pour plusieurs points d’extension macOS :

- Finder
- Partager
- Aujourd'hui

<a name="Limitations-and-Known-Issues" />

## <a name="limitations-and-known-issues"></a>Limitations et problèmes connus

Les éléments suivants sont les limitations et connaître les problèmes qui peuvent se produire lors du développement d’extensions dans Xamarin.Mac :

* Il n’existe actuellement aucune prise en charge débogage dans Visual Studio pour Mac. Débogage de tous les devront être effectuées **NSLog** et **Console**. Consultez la section conseils ci-dessous pour plus d’informations.
* Les extensions doivent être contenues dans une application hôte, qui, lorsqu’une exécution avec le Registre avec le système. Ils doivent ensuite être activés dans le **Extension** section de **Préférences système**. 
* Des pannes de l’extension peuvent déstabiliser l’application hôte et provoquer un comportement étrange. En particulier, **Finder** et le **aujourd'hui** section de la **centre de notifications** peut devenir « bloqué » et ne plus répondre. Cela a déjà été rencontré dans les projets d’extension dans Xcode ainsi et apparaît actuellement lié à Xamarin.Mac. Ceci peut souvent être observé dans le journal système (via **Console**, consultez les conseils pour plus d’informations) imprimer les messages d’erreur de façon répétée. Le redémarrage macOS s’affiche pour résoudre ce problème.

<a name="Tips" />

## <a name="tips"></a>Conseils

Les conseils suivants peuvent être utiles lorsque vous travaillez avec des extensions dans Xamarin.Mac :

- Comme Xamarin.Mac ne prend actuellement pas en charge les extensions de débogage, l’expérience de débogage dépendent principalement d’exécution et `printf` telles que des instructions. Toutefois, les extensions s’exécutent dans un processus de bac à sable, ainsi `Console.WriteLine` n’agit pas comme il le fait dans d’autres applications Xamarin.Mac. Appel de [ `NSLog` directement](https://gist.github.com/chamons/e2e409013a449cfbe1f2fbe5547f6554) produira des messages de débogage dans le journal système.
- Toutes les exceptions non interceptées seront bloque le processus d’extension, qui fournit uniquement une petite quantité d’informations utiles dans le **dans le journal système**. Renvoi à la ligne de code problématique dans un `try/catch` (Exception) qui bloquent `NSLog`d’avant de lever à nouveau peut s’avérer utile.
- Le **dans le journal système** est accessible à partir de la **Console** application sous **Applications** > **utilitaires**:

    [ ![](extensions-images/extension02.png "Le journal système")](extensions-images/extension02.png)
- Comme indiqué ci-dessus, l’application hôte d’extension en cours d’exécution inscrit avec le système. Suppression de l’offre groupée d’application avec annuler son inscription. 
- Si des versions « isolées » des extensions de l’application sont inscrits, utilisez la commande suivante pour localiser les (afin qu’ils peuvent être supprimés) : `plugin kit -mv`


<a name="Walkthrough-and-Sample-App" />

## <a name="walkthrough-and-sample-app"></a>Procédure pas à pas et l’exemple d’application

Étant donné que le développeur crée et travailler avec les extensions Xamarin.Mac dans la même façon que les extensions Xamarin.iOS, reportez-vous à notre [Introduction aux Extensions](~/ios/platform/extensions.md) documentation pour plus de détails.

Un exemple de projet Xamarin.Mac contenant un petit, des exemples fonctionnels de chaque type d’extension sont accessibles [ici](https://developer.xamarin.com/samples/mac/ExtensionSamples/).

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a pris un coup de œil rapide à l’utilisation des extensions dans une application de version Xamarin.Max 2.10 (et supérieure).

## <a name="related-links"></a>Liens associés

- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [ExtensionSamples](https://developer.xamarin.com/samples/mac/ExtensionSamples/)
- [Indications de l’Interface du système d’exploitation X humaines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
