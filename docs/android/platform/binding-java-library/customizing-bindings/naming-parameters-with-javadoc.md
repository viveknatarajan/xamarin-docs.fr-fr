---
title: "Paramètres d’affectation de noms avec Javadoc"
description: "Cet article explique comment récupérer les noms de paramètres dans un projet de liaison Java à l’aide de Javadoc généré à partir du projet Java."
ms.topic: article
ms.prod: xamarin
ms.assetid: 59E8EF16-1322-486A-BB16-353804B77356
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/20/2017
ms.openlocfilehash: 84dfe88e912241eb0024143bca568ae75e5bfa28
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="naming-parameters-with-javadoc"></a>Paramètres d’affectation de noms avec Javadoc

_Cet article explique comment récupérer les noms de paramètres dans un projet de liaison Java à l’aide de Javadoc généré à partir du projet Java._

<a name="Overview" />

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
> **Remarque :** est une grande partie de la variance dans la sortie JavaDoc. La barre d’outils. Chaîne d’outils JAR liaison ne prend pas en charge chaque combinaison possible unique et, par conséquent, certains paramètres ne peut pas être correctement nommé.

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article couvert comment utiliser Javadoc dans un projet de liaison de Java pour fournir des noms de paramètre de signification pour les API liées. 
