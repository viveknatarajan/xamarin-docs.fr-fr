---
title: Native iOS, Mac et projets de liaisons de références
description: Références natives vous donne la possibilité d’incorporer un framework natif dans Xamarin.iOS, Xamarin.Mac ou projet de liaison.
ms.prod: xamarin
ms.assetid: E53185FB-CEF5-4AB5-94F9-CC9B57C52300
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 3a497d0bb4674014b8063cb1fbc91eec6e7ae5ea
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61201545"
---
# <a name="native-references-in-ios-mac-and-bindings-projects"></a>Références natives dans iOS, Mac et les projets de liaisons

_Références natives vous donne la possibilité d’incorporer un framework natif dans un projet Xamarin.iOS ou Xamarin.Mac ou d’un projet de liaison._

Depuis iOS 8.0, il a été possible de créer une structure incorporée afin de partager du code entre les extensions d’application et de l’application principale dans Xcode. À l’aide de la fonctionnalité de référence Native il sera possible d’utiliser ces frameworks incorporés (créées avec Xcode) dans Xamarin.iOS.
 
> [!IMPORTANT]
> Il n’est pas possible de créer des frameworks incorporés à partir de n’importe quel type de projets Xamarin.iOS ou Xamarin.Mac, des références natives n’autorisent la consommation des encadrements natifs (Objective-C).

<a name="Terminology" />

## <a name="terminology"></a>Terminologie

Dans iOS 8 (et versions ultérieures), **Frameworks incorporés** peuvent être incorporés Frameworks statiquement liés et liés de manière dynamique. Pour distribuer correctement, vous devez le faire dans les infrastructures de « fat » incluant tous leurs _tranches_ pour chaque architecture de l’appareil que vous souhaitez prendre en charge avec votre application.

<a name="Static-vs-Dynamic-Frameworks" />

### <a name="static-vs-dynamic-frameworks"></a>Vs statiques. Infrastructures dynamiques

**Infrastructures statiques** sont liés à la compilation où **infrastructures dynamique** sont liées à l’exécution et c’est pourquoi, peut être modifié sans la lier à nouveau. Si vous avez utilisé n’importe quel Framework 3 rd-party antérieures à iOS 8, que vous utilisiez un **Framework statique** qui a été compilé dans votre application. Consultez d’Apple [programmation de bibliothèque dynamique](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/DynamicLibraries/100-Articles/OverviewOfDynamicLibraries.html#//apple_ref/doc/uid/TP40001873-SW1) documentation pour plus d’informations.

<a name="Embedded-vs-System-Frameworks" />

### <a name="embedded-vs-system-frameworks"></a>Vs incorporés. Infrastructures de système

**Incorporé infrastructures** sont inclus dans votre offre groupée d’applications et sont uniquement accessibles à votre application spécifique par le biais de son sandbox. **Les infrastructures de système** sont stockés au niveau du système d’exploitation et sont disponibles pour toutes les applications sur l’appareil. Actuellement, seulement Apple a la possibilité de créer des infrastructures de niveau de système d’exploitation.

<a name="Thin-vs-Fat-Frameworks" />

### <a name="thin-vs-fat-frameworks"></a>Visual Studio léger. Infrastructures FAT

**Mince infrastructures** contiennent uniquement le code compilé pour une architecture système spécifique où **infrastructures Fat** contiennent du code pour plusieurs architectures. Chaque base de code spécifique de l’architecture compilé dans une infrastructure est appelé un _tranche_. Par conséquent, par exemple, si nous disposions d’une infrastructure qui a été compilée pour deux iOS architectures de simulateur (i386 et X86_64), il contiendrait deux tranches.

Si vous avez tenté de distribuer ce exemple Framework avec votre application, il serait s’exécute correctement sur le simulateur, mais échoue sur l’appareil dans la mesure où le Framework ne contient-elle pas les tranches de code spécifique pour un appareil iOS. Pour vous assurer qu’une infrastructure fonctionnera dans toutes les instances, il faudrait également inclure des tranches spécifiques au périphérique tels qu’arm64, armv7 et armv7s.

<a name="Working-with-Embedded-Frameworks" />

## <a name="working-with-embedded-frameworks"></a>Utilisation de Frameworks incorporés

Il existe deux étapes qui doivent être effectuées pour travailler avec les Frameworks incorporés dans une application Xamarin.iOS ou Xamarin.Mac : Création d’une infrastructure Fat et l’incorporation de l’infrastructure.

<a name="Overview" />

### <a name="creating-a-fat-framework"></a>Création d’une infrastructure Fat

Comme indiqué ci-dessus, pour être en mesure d’utiliser une infrastructure incorporé dans votre application, il doit être un Framework Fat qui inclut toutes les architectures système tranches pour les appareils de votre application s’exécutera sur.

Lorsque l’infrastructure et les applications qui l’utilisent se trouvent dans le même projet Xcode, cela n’est pas un problème car Xcode générera l’infrastructure et l’application avec les mêmes paramètres de build. Étant donné que les applications Xamarin ne peut pas créer de Frameworks incorporés, cette technique ne peut pas être utilisée.

Pour résoudre ce problème, le `lipo` outil de ligne de commande peut être utilisé pour fusionner deux ou plusieurs infrastructures dans un Framework Fat contenant tous les secteurs nécessaires. Pour plus d’informations sur l’utilisation de la `lipo` commande, reportez-vous à notre [liaison de bibliothèques natives](~/ios/platform/native-interop.md) documentation.

<a name="Embedding-a-Framework" />

### <a name="embedding-a-framework"></a>Incorporation d’une infrastructure

L’étape de suivi sont requis pour incorporer un framework dans un projet Xamarin.iOS ou Xamarin.Mac à l’aide des références natives :

1. Créer un nouveau ou ouvrez un projet Xamarin.iOS, Xamarin.Mac ou liaison existant.
2. Dans le **l’Explorateur de solutions**, avec le bouton droit sur le nom du projet et sélectionnez **ajouter** > **ajouter une référence Native**: 

    [![](native-references-images/ref01.png "Dans l’Explorateur de solutions, avec le bouton droit sur le nom du projet et sélectionnez Ajouter une référence Native")](native-references-images/ref01.png#lightbox)
3. À partir de la **Open** boîte de dialogue, sélectionnez le nom de l’infrastructure Native que vous souhaitez incorporer, cliquez sur le **Open** bouton : 

    [![](native-references-images/ref02.png "Sélectionnez le nom de l’infrastructure Native pour incorporer et cliquez sur le bouton Ouvrir")](native-references-images/ref02.png#lightbox)
4. Le framework est ajouté à l’arborescence du projet : 

    [![](native-references-images/ref03.png "Le framework sera ajouté à l’arborescence de projets")](native-references-images/ref03.png#lightbox)

Lorsque le projet est compilé, le Framework natif sera incorporé dans le bundle de l’application.

<a name="App-Extensions-and-Embedded-Frameworks" />

## <a name="app-extensions-and-embedded-frameworks"></a>Extensions d’application et de Frameworks incorporés

En interne les Xamarin.iOS peut tirer parti de cette fonctionnalité pour créer un lien avec le runtime Mono en tant qu’infrastructure (lorsque la cible de déploiement est > = iOS 8.0), ce qui réduit considérablement pour les applications avec des extensions de taille de l’application (étant donné que le runtime Mono est inclus qu’une seule fois pour l’ensemble de votre application offre groupée, au lieu d’une fois pour l’application de conteneur et une fois pour chaque extension).

Extensions seront liée avec le runtime Mono en tant qu’infrastructure, car toutes les extensions nécessitent iOS 8.0.

Applications n’ayant pas les extensions et les applications qui ciblent iOS 

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a examiné en détail à l’incorporation d’un Framework natif dans une application Xamarin.iOS ou Xamarin.Mac.

