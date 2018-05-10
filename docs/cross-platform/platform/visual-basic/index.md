---
title: Portable Visual Basic.NET
description: Ce guide explique comment Visual Basic peut être utilisé pour écrire des projets de bibliothèque de classes Portable (PCL) qui peuvent être utilisés dans les solutions ciblant Xamarin.iOS et Xamarin.Android.
ms.prod: xamarin
ms.assetid: f264c632-8feb-4015-a5e5-cb9c681c787d
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 71ee153135df97d3b4fa149d3d788d3b940fe944
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2018
---
# <a name="portable-visual-basicnet"></a>Portable Visual Basic.NET

Projets Xamarin iOS et Android ne pas en mode natif prennent en charge Visual Basic. Toutefois, les développeurs peuvent utiliser les bibliothèques de classes portables pour la migration de code Visual Basic existant pour iOS et Android, ou d’écrire une partie importante de leur logique d’application dans Visual Basic. Xamarin.Forms applications peuvent être créées uniquement dans Visual Basic (à l’exclusion des convertisseurs personnalisés et services de dépendance avec code-behind XAML).

## <a name="requirements"></a>Spécifications

Prise en charge de bibliothèque de classes portable a été ajoutée dans Xamarin.Android 4.10.1, Xamarin.iOS 7.0.4 et Xamarin Studio 4.2, ce qui signifie que tous les projets Xamarin créés avec ces outils peuvent incorporer des assemblys de bibliothèque de classes portables Visual Basic.

Pour créer et compiler des bibliothèques de classes portables Visual Basic, vous devez utiliser Visual Studio sur Windows (Visual Studio 2012 ou version ultérieure).

> [!NOTE]
> Bibliothèques Visual Basic ne peuvent être créés et compilé à l’aide de Visual Studio. Xamarin.iOS et Xamarin.Android ne prennent pas en charge du langage Visual Basic.
>
> Si vous travaillez uniquement dans Visual Studio, vous pouvez référencer le projet Visual Basic à partir de projets Xamarin.iOS et Xamarin.Android.
>
> Si vos projets iOS et Android doivent également être chargés dans Visual Studio pour Mac, vous devez référencer l’assembly de sortie à partir de la bibliothèque PCL Visual Basic.


## <a name="creating-a-visual-basicnet-pcl"></a>Création d’une bibliothèque de classes Visual Basic.NET portables

Cette section décrit comment créer une bibliothèque de classes Portable Visual Basic à l’aide de Visual Studio.
La bibliothèque peut ensuite être référencée dans d’autres projets, y compris les applications Xamarin.iOS, Xamarin.Android et Xamarin.Forms.

### <a name="creating-a-pcl"></a>Création d’une bibliothèque de classes portables

Lors de l’ajout d’une bibliothèque PCL Visual Basic dans Visual Studio, vous devez choisir un profil qui décrit votre bibliothèque doit être compatible avec les plateformes. Les profils sont expliqués dans l’Introduction de document de la bibliothèque de classes portables.

Les étapes pour créer une bibliothèque de classes portables et choisir son profil sont :

1.  Dans le **nouveau projet** écran, sélectionnez le **Visual Basic > bibliothèque de classes (Portable)** option :

    [![](images/image1-sml.png "Créer la nouvelle bibliothèque Portable Visual Basic")](images/image1.png#lightbox)

1.  Visual Studio vous invite immédiatement par le code suivant **ajouter une bibliothèque de classes portables** boîte de dialogue afin que le profil peut être configuré. Cochez les plateformes que vous avez besoin pour prendre en charge, puis appuyez sur **OK**.

    [![](images/image2-sml.png "Sélectionnez le profil de bibliothèque PCL en choisissant les plateformes")](images/image2.png#lightbox)

1.  Le projet Visual Basic PCL s’affichent comme illustré dans le **l’Explorateur de solutions** comme suit :

    [![](images/image3-sml.png "Projet Visual Studio PCL vide")](images/image3.png#lightbox)


La bibliothèque PCL est prête pour l’ajout de code Visual Basic. Les projets de bibliothèque de classes portables peuvent être référencés par d’autres projets (projets d’Application, les projets de bibliothèque et même d’autres projets de bibliothèque PCL).

### <a name="editing-the-pcl-profile"></a>Modification du profil de la bibliothèque de classes portables

Le profil de bibliothèque de classes portables (qui contrôle la bibliothèque PCL est compatible avec les plateformes sur lesquelles) peuvent être affiché et modifié en cliquant sur le projet et en choisissant **Propriétés > bibliothèque > modifier...** . La boîte de dialogue est présenté dans cette capture d’écran :

 [![](images/image4-sml.png "Modifier le profil de la bibliothèque de classes portables dans Propriétés du projet")](images/image4.png#lightbox)

Si le profil est modifié une fois que le code a déjà été ajouté à la bibliothèque PCL, il est possible que la bibliothèque n’est plus compilées si le code fait référence à des fonctionnalités qui ne font pas partie du profil qui vient d’être sélectionné.


## <a name="summary"></a>Récapitulatif

Cet article a montré comment utiliser le code Visual Basic dans les applications Xamarin avec Visual Studio et les bibliothèques de classes portables. Même si Xamarin ne prend pas en charge directement les Visual Basic, la compilation de Visual Basic dans une bibliothèque PCL permet au code écrit en Visual Basic à inclure dans les applications Android et iOS.

Les pages suivantes décrivent comment utiliser Visual Basic.NET PCLs en mode natif ou les applications Xamarin.Forms :

- [Création d’applications natives Xamarin.iOS et Xamarin.Android qui utilisent Visual Basic](native-apps.md)
- [Création d’applications avec Visual Basic Xamarin.Forms](xamarin-forms.md)


## <a name="related-links"></a>Liens associés

- [TaskyPortableVB (exemple)](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/TaskyPortableVB)
- [XamarinFormsVB (exemple)](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/XamarinFormsVB)
- [Le développement multiplateforme avec le .NET Framework (Microsoft)](http://msdn.microsoft.com/library/gg597391(v=vs.110).aspx)
