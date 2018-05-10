---
title: Références natives
description: Références natives vous donne la possibilité d’incorporer un Framework natif dans un projet de Xamarin.iOS ou Xamarin.Mac ou d’un projet de liaison.
ms.prod: xamarin
ms.assetid: E53185FB-CEF5-4AB5-94F9-CC9B57C52300
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: c56e392420debb21998363cfffa288aec51691ea
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2018
---
# <a name="native-references"></a>Références natives

_Références natives vous donne la possibilité d’incorporer un Framework natif dans un projet de Xamarin.iOS ou Xamarin.Mac ou d’un projet de liaison._


Depuis iOS 8.0, il a été possible de créer une structure incorporée pour partager du code entre les extensions d’application et de l’application principale dans Xcode. À l’aide de la fonctionnalité de référence Native il sera possible d’utiliser ces infrastructures incorporés (créées avec Xcode) dans Xamarin.iOS.
 
> [!IMPORTANT]
> Il ne sera pas possible de créer des structures incorporées à partir de n’importe quel type de Xamarin.iOS ou Xamarin.Mac projets, références natives ne permettent la consommation des encadrements natifs (Objective-C).




<a name="Terminology" />

## <a name="terminology"></a>Terminologie

Dans iOS 8 (et versions ultérieures), **infrastructures incorporé** peuvent être incorporés infrastructures statiquement liées et liées dynamiquement. Pour les distribuer correctement, vous devez le faire dans les infrastructures de « fat » tous les leurs _tranches_ pour chaque architecture de périphérique que vous souhaitez prendre en charge avec votre application.

<a name="Static-vs-Dynamic-Frameworks" />

### <a name="static-vs-dynamic-frameworks"></a>Ports statiques et. Infrastructures dynamiques

**Infrastructures statiques** sont liés à la compilation où **infrastructures dynamique** sont liées à l’exécution et, par conséquent, peut être modifié sans le lier à nouveau. Si vous avez utilisé n’importe quelle infrastructure 3 rd-party avant iOS 8, que vous utilisiez un **Framework statique** qui a été compilé dans votre application. Consultez d’Apple [de programmation bibliothèque dynamique](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/DynamicLibraries/100-Articles/OverviewOfDynamicLibraries.html#//apple_ref/doc/uid/TP40001873-SW1) documentation pour plus de détails.

<a name="Embedded-vs-System-Frameworks" />

### <a name="embedded-vs-system-frameworks"></a>Vs incorporés. Infrastructures de système

**Incorporé infrastructures** sont inclus dans votre offre groupée d’applications et sont uniquement accessibles à votre application spécifique via son sandbox. **Les infrastructures système** sont stockés au niveau du système d’exploitation et sont disponibles pour toutes les applications sur l’appareil. Actuellement, uniquement Apple a la possibilité de créer des infrastructures de niveau de système d’exploitation.

<a name="Thin-vs-Fat-Frameworks" />

### <a name="thin-vs-fat-frameworks"></a>Vs dynamiques. Infrastructures FAT

**Dynamique des infrastructures** contient uniquement le code compilé pour une architecture spécifique du système où **infrastructures Fat** contenir du code de plusieurs architectures. Chaque base de code spécifique à l’architecture compilé dans une infrastructure est appelé un _tranche_. Ainsi, par exemple, si nous avions une infrastructure qui a été compilée pour deux iOS architectures de simulateur (i386 et X86_64), il contient deux tranches.

Si vous avez tenté de distribuer ce exemple Framework avec votre application, il serait s’exécute correctement sur le simulateur, mais échoue sur l’appareil, car le Framework ne contient-elle pas les tranches spécifiques au code pour un appareil iOS. Pour garantir une infrastructure dans toutes les instances, il devra également inclure des tranches spécifiques au périphérique tels qu’arm64, armv7 et armv7s.

<a name="Working-with-Embedded-Frameworks" />

## <a name="working-with-embedded-frameworks"></a>Utilisation des structures incorporées

Il existe deux étapes qui doivent être effectuées pour travailler avec des infrastructures incorporé dans une application Xamarin.iOS ou Xamarin.Mac : création d’une infrastructure Fat et l’incorporation de l’infrastructure.

<a name="Overview" />

### <a name="creating-a-fat-framework"></a>Création d’une infrastructure Fat

Comme indiqué ci-dessus, pour être en mesure d’utiliser une structure incorporée dans votre application, il doit être une infrastructure Fat qui inclut toutes les architectures système tranches pour votre application s’exécutera sur les appareils.

Lorsque l’infrastructure et l’application consommatrice se trouvent dans le même projet Xcode, cela n’est pas un problème comme Xcode générera l’infrastructure et l’application qui utilise les mêmes paramètres de build. Étant donné que les applications Xamarin ne peut pas créer des structures incorporées, cette technique ne peut pas être utilisée.

Pour résoudre ce problème, le `lipo` outil de ligne de commande peut être utilisé pour fusionner deux ou plusieurs infrastructures dans une infrastructure Fat contenant tous les secteurs nécessaires. Pour plus d’informations sur l’utilisation de la `lipo` de commande, consultez notre [lier des bibliothèques natives](~/ios/platform/native-interop.md) documentation.

<a name="Embedding-a-Framework" />

### <a name="embedding-a-framework"></a>Incorporation d’une infrastructure

L’étape suivante sont nécessaires pour incorporer une structure dans un projet Xamarin.iOS ou Xamarin.Mac à l’aide des références natives :

1. Créer un nouveau ou ouvrir un projet existant de Xamarin.iOS, Xamarin.Mac ou liaison.
2. Dans le **l’Explorateur de solutions**, avec le bouton droit sur le nom du projet et sélectionnez **ajouter** > **ajouter une référence Native**: 

    [![](native-references-images/ref01.png "Dans l’Explorateur de solutions, avec le bouton droit sur le nom du projet et sélectionnez Ajouter une référence Native")](native-references-images/ref01.png#lightbox)
3. À partir de la **ouvrir** boîte de dialogue, sélectionnez le nom de l’infrastructure Native que vous souhaitez incorporer, cliquez sur le **ouvrir** bouton : 

    [![](native-references-images/ref02.png "Sélectionnez le nom de l’infrastructure Native pour incorporer, cliquez sur le bouton Ouvrir")](native-references-images/ref02.png#lightbox)
4. Le framework vont être ajouté à l’arborescence du projet : 

    [![](native-references-images/ref03.png "Le framework sera ajouté à l’arborescence de projets")](native-references-images/ref03.png#lightbox)

Lorsque le projet est compilé, le Framework natif seront incorporé dans l’offre groupée de l’application.

<a name="App-Extensions-and-Embedded-Frameworks" />

## <a name="app-extensions-and-embedded-frameworks"></a>Extensions d’application et les structures incorporées

En interne les Xamarin.iOS peut tirer parti de cette fonctionnalité pour créer un lien avec le runtime Mono en tant qu’infrastructure (lorsque la cible de déploiement est > = iOS 8.0), ce qui réduit la taille de l’application considérablement pour les applications avec les extensions (étant donné que le runtime Mono est inclus une seule fois pour l’application entière offre groupée, au lieu d’une fois pour l’application de conteneur, puis une fois pour chaque extension).

Extensions de lien avec le runtime Mono, en tant qu’infrastructure, car toutes les extensions nécessitent iOS 8.0.

Applications qui n’ont les extensions et les applications iOS de cette cible 

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a pris une présentation détaillée à l’incorporation d’une infrastructure native dans une application Xamarin.iOS ou Xamarin.Mac.

