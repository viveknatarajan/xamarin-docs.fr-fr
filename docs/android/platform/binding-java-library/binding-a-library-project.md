---
title: Liaison d’un projet de bibliothèque Eclipse
description: Cette procédure pas à pas explique comment utiliser des modèles de projet Xamarin.Android pour lier un projet de bibliothèque Eclipse Android.
ms.prod: xamarin
ms.assetid: CEE90F8A-164B-4155-813A-7537A665A7E7
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: a5887748eda8af89b9215e3e121db592dfa73935
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60957627"
---
# <a name="binding-an-eclipse-library-project"></a>Liaison d’un projet de bibliothèque Eclipse

_Cette procédure pas à pas explique comment utiliser des modèles de projet Xamarin.Android pour lier un projet de bibliothèque Eclipse Android._


## <a name="overview"></a>Vue d'ensemble

Bien que. Les fichiers AAR sont de plus en plus la norme pour la distribution de bibliothèque Android, dans certains cas, il est nécessaire créer une liaison pour un *projet de bibliothèque Android*. Les projets de bibliothèque Android sont des projets Android spéciales qui contiennent du code partageable et les ressources qui peuvent être référencés par les projets d’application Android. En règle générale, vous liez à un projet de bibliothèque Android lors de la bibliothèque est créée dans l’IDE Eclipse.
Cette procédure pas à pas fournit des exemples montrant comment créer un projet de bibliothèque Android. ZIP à partir de la structure de répertoires d’un projet Eclipse.

Les projets de bibliothèque Android diffèrent des projets Android régulières car ils ne sont pas compilés dans un fichier APK et ne sont pas, sur leurs propres, pouvant être déployées sur un appareil. Au lieu de cela, un projet de bibliothèque Android est destiné à être référencé par un projet d’application Android. Lorsqu’un projet d’application Android est généré, le projet de bibliothèque Android est d’abord compilé. Le projet d’application Android est ensuite absorbée dans le projet de bibliothèque Android compilé et inclure le code et les ressources dans le fichier APK pour la distribution. En raison de cette différence, la création d’une liaison pour un projet de bibliothèque Android est légèrement différente de celle de la création d’une liaison pour Java. JAR ou. Fichier de AAR.



## <a name="walkthrough"></a>Procédure pas à pas

Pour utiliser un projet de bibliothèque Android dans un projet Xamarin.Android Java liaison, il est nécessaire pour générer le projet de bibliothèque Android dans Eclipse. La capture d’écran suivante montre un exemple d’un projet de bibliothèque Android après la compilation : 

[![Exemple de projet de bibliothèque dans Eclipse](binding-a-library-project-images/build-lib-in-eclipse.png)](binding-a-library-project-images/build-lib-in-eclipse.png#lightbox)

Notez que le code source à partir du projet de bibliothèque Android a été compilé pour une table temporaire. Fichier JAR nommé **android-mapviewballoons.jar**, et que les ressources ont été copiés vers le **res/bin/soignée** dossier. 

Une fois le projet de bibliothèque Android a été compilé dans Eclipse, il peut ensuite être liée à l’aide d’un projet Xamarin.Android Java liaison. Première un. Fichier ZIP doit être créé, qui contient le **bin** et **res** dossiers du projet de bibliothèque Android. Il est important que vous supprimez les intervenants **soignée** sous-répertoire afin que les ressources se trouvent dans **bin/res**. La capture d’écran suivante montre le contenu d’une telle. Fichier ZIP : 

[![Contenu de bibliothèque Android projet .zip](binding-a-library-project-images/contents-of-zip-file.png)](binding-a-library-project-images/contents-of-zip-file.png#lightbox)

Cela. Fichier ZIP est ensuite ajouté au projet de liaison Java Xamarin.Android, comme illustré dans la capture d’écran suivante :

[![ZIP ajouté au projet de liaison de Java](binding-a-library-project-images/zip-in-binding-project.png)](binding-a-library-project-images/zip-in-binding-project.png#lightbox)

Notez que l’Action de génération de la. Fichier ZIP a été défini automatiquement sur **LibraryProjectZip**.

Le cas échéant. Les fichiers JAR requis par le projet de bibliothèque Android, ils doivent être ajoutés à la **Jars** dossier du projet de bibliothèque de liaison de Java et le **Action de génération** définie sur **ReferenceJar**. Un exemple de ce peut être consultée dans la capture d’écran ci-dessous : 

[![Action ReferenceJar la valeur de génération](binding-a-library-project-images/set-to-referencejar.png)](binding-a-library-project-images/set-to-referencejar.png#lightbox)

Une fois ces étapes terminées, le projet Xamarin.Android Java Binding utilisable comme décrit précédemment dans ce document.

> [!NOTE]
> Compiler les projets de bibliothèque Android dans d’autres IDE n’est pas pris en charge pour l’instant. Autres IDE ne peut pas créer la même structure de répertoires ou fichiers dans le **bin** dossier comme Eclipse. 


## <a name="summary"></a>Récapitulatif

Dans cet article, nous avons passé en revue le processus de liaison d’un projet de bibliothèque Android. Nous avons créé le projet de bibliothèque Android dans Eclipse, puis nous avons créé un fichier zip à partir de la **bin** et **res** dossiers du projet de bibliothèque Android. Ensuite, nous avons utilisé ce fichier zip pour créer un projet de liaison Java Xamarin.Android. 

