---
title: Paramètres d’affectation de noms avec Javadoc
description: Cet article explique comment récupérer les noms de paramètres dans un projet de liaison Java à l’aide de Javadoc généré à partir du projet Java.
ms.prod: xamarin
ms.assetid: 59E8EF16-1322-486A-BB16-353804B77356
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/20/2017
ms.openlocfilehash: e394377043953a297afed36a3ce0747a3e6d1512
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50104412"
---
# <a name="naming-parameters-with-javadoc"></a>Paramètres d’affectation de noms avec Javadoc

_Cet article explique comment récupérer les noms de paramètres dans un projet de liaison Java à l’aide de Javadoc généré à partir du projet Java._


## <a name="overview"></a>Vue d'ensemble

Lors de la liaison d’une bibliothèque Java existante, des métadonnées sur l’API liée sont perdue. En particulier les noms des paramètres aux méthodes. Les noms de paramètre apparaîtront en tant que `p0`, `p1`, etc. Il s’agit, car le Java `.class` fichiers ne conservent pas les noms de paramètres qui ont été utilisés dans le code source Java. 

Un projet de liaison Xamarin.Android Java peut fournir les noms de paramètre s’il a accès au code HTML Javadoc à partir de la bibliothèque d’origine. 

## <a name="integrating-javadoc-html-into-a-java-binding-project"></a>Intégration de Javadoc HTML dans un projet de liaison de Java

L’intégration de l’HTML Javadoc dans un projet Java de liaison est un processus manuel constitué des étapes suivantes : 

1.  Télécharger le Javadoc correspondant pour la bibliothèque
2.  Modifier le `.csproj` fichier, puis ajoutez un `<JavaDocPaths>` propriété :
3.  Nettoyez et regénérez le projet

Une fois cette opération est effectuée, les noms de paramètre Java d’origine doivent être présents dans les API liées par un projet de liaison Java. 


> [!NOTE]
> Il existe une grande quantité de variance dans la sortie JavaDoc. La barre d’outils. Chaîne d’outils de la liaison JAR ne prend pas en charge chaque permutation possibles unique et, par conséquent, certains paramètres ne peut pas être correctement nommé.


## <a name="summary"></a>Récapitulatif

Cet article couvert comment utiliser Javadoc dans un projet de liaison de Java pour fournir des noms de paramètre de signification pour les API liées. 

