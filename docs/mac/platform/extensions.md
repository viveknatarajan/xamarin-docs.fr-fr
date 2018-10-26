---
title: Prise en charge de l’Extension Xamarin.Mac
description: Ce document décrit la prise en charge de Xamarin.Mac pour les extensions de recherche, le partage et aujourd'hui. Il examine les limitations et problèmes connus, des liens vers une procédure pas à pas et l’exemple d’application et fournit des conseils pour travailler avec des extensions.
ms.prod: xamarin
ms.assetid: 4148F1BE-DFA0-46B6-9FCD-425A6541F510
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 60b981a764a2656210730ae0602ff32dc580cd0a
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50117562"
---
# <a name="xamarinmac-extension-support"></a>Prise en charge de l’Extension Xamarin.Mac

Dans la version préliminaire de Xamarin.Mac 2.10 prise en charge a été ajoutée pour plusieurs points d’extension macOS :

- Finder
- Partager
- Aujourd'hui

<a name="Limitations-and-Known-Issues" />

## <a name="limitations-and-known-issues"></a>Limitations et problèmes connus

Les éléments suivants sont les limitations et connaître les problèmes qui peuvent se produire lors du développement d’extensions dans Xamarin.Mac :

* Il n’existe actuellement aucune prise en charge débogage dans Visual Studio pour Mac. Débogage tous les devra être répétée **NSLog** et **Console**. Consultez la section de conseils ci-dessous pour plus d’informations.
* Les extensions doivent être contenues dans une application hôte, qui, lorsqu’une exécution avec un Registre avec le système. Elles doivent ensuite être activées dans le **Extension** section de **Préférences système**. 
* Certains blocages de l’extension peuvent déstabiliser l’application hôte et provoquer un comportement étrange. En particulier, **Finder** et **aujourd'hui** section de la **centre de notifications** peut devenir « bloqué » et ne plus répondre. Cela a déjà été rencontré dans les projets d’extension dans Xcode ainsi et apparaît sans rapport avec Xamarin.Mac. Ceci peut souvent être observé dans le journal système (via **Console**, consultez les conseils de détails) imprimer les messages d’erreur de façon répétée. Le redémarrage de macOS s’affiche pour résoudre ce problème.

<a name="Tips" />

## <a name="tips"></a>Conseils

Les conseils suivants peuvent être utiles lorsque vous travaillez avec des extensions dans Xamarin.Mac :

- Comme Xamarin.Mac ne prend actuellement pas en charge les extensions de débogage, l’expérience de débogage dépendent principalement d’exécution et `printf` telles que des instructions. Toutefois, les extensions exécutées dans un processus de bac à sable, donc `Console.WriteLine` n’agit pas comme il le fait dans d’autres applications Xamarin.Mac. Appel [ `NSLog` directement](https://gist.github.com/chamons/e2e409013a449cfbe1f2fbe5547f6554) génère des messages de débogage dans le journal système.
- Toutes les exceptions non interceptées seront bloque le processus d’extension, en fournissant uniquement une petite quantité d’informations utiles dans le **journal système**. Habillage du code problématique dans un `try/catch` (Exception) qui bloquent `NSLog`du avant de lever à nouveau peut être utile.
- Le **journal système** sont accessibles à partir de la **Console** app sous **Applications** > **utilitaires**:

    [![](extensions-images/extension02.png "Le journal système")](extensions-images/extension02.png#lightbox)
- Comme indiqué ci-dessus, l’application hôte d’extension en cours d’exécution inscrit avec le système. Suppression de l’offre groupée d’applications avec annuler son inscription. 
- Si des versions « parasites » des extensions de l’application sont inscrits, utilisez la commande suivante pour localiser les (donc ils peuvent être supprimés) : `plugin kit -mv`


<a name="Walkthrough-and-Sample-App" />

## <a name="walkthrough-and-sample-app"></a>Procédure pas à pas et l’exemple d’application

Dans la mesure où le développeur crée et utiliser les extensions de Xamarin.Mac dans la même façon que les extensions de Xamarin.iOS, reportez-vous à notre [Introduction aux Extensions](~/ios/platform/extensions.md) documentation pour plus d’informations.

Un exemple de projet Xamarin.Mac contenant un petit, obtenir des exemples fonctionnels de chaque type d’extension figurent [ici](https://developer.xamarin.com/samples/mac/ExtensionSamples/).

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a examiné en rapide à l’utilisation avec des extensions dans une application de version Xamarin.Max 2.10 (et supérieure).

## <a name="related-links"></a>Liens associés

- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [ExtensionSamples](https://developer.xamarin.com/samples/mac/ExtensionSamples/)
- [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
