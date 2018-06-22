---
title: Paramètres d’affectation de noms avec Javadoc
description: Cet article explique comment récupérer les noms de paramètres dans un projet de liaison Java à l’aide de Javadoc généré à partir du projet Java.
ms.prod: xamarin
ms.assetid: 59E8EF16-1322-486A-BB16-353804B77356
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/20/2017
ms.openlocfilehash: 7517e46c5b66123dc4e12fb5562c59f569f249aa
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30766892"
---
# <a name="naming-parameters-with-javadoc"></a>Paramètres d’affectation de noms avec Javadoc

_Cet article explique comment récupérer les noms de paramètres dans un projet de liaison Java à l’aide de Javadoc généré à partir du projet Java._


## <a name="overview"></a>Vue d'ensemble

Lors de la liaison d’une bibliothèque Java existante, certaines métadonnées sur l’API liée sont perdue. En particulier les noms des paramètres de méthodes. Les noms de paramètre sont affichés en tant que `p0`, `p1`, etc. C’est parce que le Java `.class` fichiers ne conservent pas les noms de paramètres qui ont été utilisés dans le code source Java. 

Un projet de liaison Xamarin.Android Java peut fournir les noms de paramètre s’il a accès au code HTML Javadoc à partir de la bibliothèque d’origine. 

## <a name="integrating-javadoc-html-into-a-java-binding-project"></a>L’intégration de Javadoc HTML dans un projet de liaison de Java

Intégrer le code HTML Javadoc dans un projet Java de liaison est un processus manuel est constitué des étapes suivantes : 

1.  Télécharger le Javadoc pour la bibliothèque
2.  Modifier la `.csproj` et ajoutez un `<JavaDocPaths>` propriété :
3.  Nettoyer et régénérer le projet

Une fois cette opération est effectuée, les noms de paramètre Java d’origine doivent être présents dans les API liées à un projet de liaison de Java. 


> [!NOTE]
> Il existe une grande partie de la variance dans la sortie JavaDoc. La barre d’outils. Chaîne d’outils JAR liaison ne prend pas en charge chaque combinaison possible unique et, par conséquent, certains paramètres ne peut pas être correctement nommé.


## <a name="summary"></a>Récapitulatif

Cet article couvert comment utiliser Javadoc dans un projet de liaison de Java pour fournir des noms de paramètre de signification pour les API liées. 

