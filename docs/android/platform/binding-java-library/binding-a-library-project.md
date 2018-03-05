---
title: "Liaison d’un projet de bibliothèque Eclipse"
description: "Cette procédure pas à pas explique comment utiliser des modèles de projet Xamarin.Android pour lier un projet de bibliothèque Android Eclipse."
ms.topic: article
ms.prod: xamarin
ms.assetid: CEE90F8A-164B-4155-813A-7537A665A7E7
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/14/2017
ms.openlocfilehash: 2048056415e0969e13e305b1dbba8bdb7ffabd30
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="binding-an-eclipse-library-project"></a>Liaison d’un projet de bibliothèque Eclipse

_Cette procédure pas à pas explique comment utiliser des modèles de projet Xamarin.Android pour lier un projet de bibliothèque Android Eclipse._

<a name=overview />

## <a name="overview"></a>Vue d'ensemble

Bien que. Les fichiers AAR deviennent plus en plus de la norme pour la distribution de la bibliothèque Android, dans certains cas, il est nécessaire de créer une liaison pour un *projet de bibliothèque Android*. Les projets de bibliothèque Android sont les projets Android spéciaux qui contiennent du code partageable et les ressources qui peuvent être référencés par les projets d’application Android. En règle générale, vous lier à un projet de bibliothèque Android lors de la création de la bibliothèque dans l’IDE Eclipse.
Cette procédure pas à pas fournit des exemples montrant comment créer un projet de bibliothèque Android. ZIP à partir de la structure de répertoires d’un projet Eclipse.

Projets de bibliothèque Android diffèrent des projets Android régulières car ils ne sont pas compilés dans un fichier APK et ne sont pas sur leurs propres, pouvant être déployé sur un appareil. Au lieu de cela, un projet de bibliothèque Android est destiné à être référencé par un projet d’application Android. Lors de la génération d’un projet d’application Android, le projet de bibliothèque Android est d’abord compilé. Le projet d’application Android est ensuite absorbée dans le projet de bibliothèque Android compilé et inclure le code et les ressources dans l’APK de distribution. En raison de cette différence, la création d’une liaison pour un projet de bibliothèque Android est légèrement différente de celle de la création d’une liaison pour Java. JAR ou. Fichier de AAR.


<a name="Walkthrough" />

## <a name="walkthrough"></a>Procédure pas à pas

Pour utiliser un projet de bibliothèque Android dans un projet de liaison de Xamarin.Android Java, il est nécessaire pour générer le projet de bibliothèque Android dans Eclipse. La capture d’écran suivante montre un exemple d’un projet de bibliothèque Android après la compilation : 

[ ![Exemple de projet de bibliothèque dans Eclipse](binding-a-library-project-images/build-lib-in-eclipse.png)](binding-a-library-project-images/build-lib-in-eclipse.png)

Notez que le code source à partir du projet de bibliothèque Android a été compilé dans une variable temporaire. Fichier JAR nommé **android-mapviewballoons.jar**, et que les ressources ont été copiés sur le **bin/res/soignée** dossier. 

Une fois que le projet de bibliothèque Android a été compilé dans Eclipse, il peut alors être lié à l’aide d’un projet de liaison de Xamarin.Android Java. Première un. Fichier ZIP doit être créé qui contient le **bin** et **res** dossiers du projet de bibliothèque Android. Il est important que vous supprimez les intervenants **soignée** sous-répertoire afin que les ressources se trouvent dans **bin/res**. La capture d’écran suivante montre le contenu d’une telle. Fichier ZIP : 

[ ![Contenu de la bibliothèque Android projet .zip](binding-a-library-project-images/contents-of-zip-file.png)](binding-a-library-project-images/contents-of-zip-file.png)

Cela. Fichier ZIP est ensuite ajouté au projet de liaison Java Xamarin.Android, comme indiqué dans la capture d’écran suivante :

[ ![ZIP, ajouté au projet Java Binding](binding-a-library-project-images/zip-in-binding-project.png)](binding-a-library-project-images/zip-in-binding-project.png)

Notez que l’Action de génération de la. Fichier ZIP a été automatiquement défini sur **LibraryProjectZip**.

Le cas échéant. Fichiers JAR requis par le projet de bibliothèque Android, ils doivent être ajoutés à la **fichiers JAR** dossier du projet de bibliothèque de liaison de Java et le **Action de génération** la valeur **ReferenceJar**. Un exemple de cette peut être observé dans la capture d’écran ci-dessous : 

[ ![Action ReferenceJar la valeur de génération](binding-a-library-project-images/set-to-referencejar.png)](binding-a-library-project-images/set-to-referencejar.png)

Une fois ces étapes terminées, le projet Xamarin.Android Binding de Java peut servir comme décrit précédemment dans ce document.

> [!NOTE]
> **Remarque**: la compilation des projets de bibliothèque Android dans d’autres IDE n’est pas prise en charge pour l’instant. Autres environnements IDE ne peut pas créer la même structure de répertoires ou fichiers dans le **bin** dossier que Eclipse. 

<a name="Summary" /> 

## <a name="summary"></a>Récapitulatif

Dans cet article, nous avons passé en revue le processus de liaison d’un projet de bibliothèque Android. Nous avons créé le projet de bibliothèque Android dans Eclipse, puis nous avons créé un fichier zip à partir de la **bin** et **res** dossiers du projet de bibliothèque Android. Ensuite, nous avons utilisé ce fichier zip pour créer un projet de liaison de Xamarin.Android Java. 

