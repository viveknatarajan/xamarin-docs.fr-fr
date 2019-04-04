---
title: Portable Visual Basic.NET
description: Ce guide explique comment Visual Basic peut être utilisé pour écrire des projets de bibliothèque de classes Portable (PCL) qui peuvent être utilisés dans les solutions ciblant Xamarin.iOS et Xamarin.Android.
ms.prod: xamarin
ms.assetid: f264c632-8feb-4015-a5e5-cb9c681c787d
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: e4c8c43b4df1a7bfc5436f14564c6d0164216c46
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57669243"
---
# <a name="portable-visual-basicnet"></a>Portable Visual Basic.NET

Projets Xamarin iOS et Android ne pas en mode natif prennent en charge Visual Basic ; Toutefois, les développeurs peuvent utiliser les bibliothèques de classes portables pour migrer du code Visual Basic existant pour iOS et Android, ou d’écrire une partie significative de leur logique d’application dans Visual Basic. Les applications Xamarin.Forms peuvent être entièrement créées en Visual Basic (à l’exclusion des convertisseurs personnalisés, les services de dépendance et code-behind XAML).

## <a name="requirements"></a>Spécifications

Prise en charge de bibliothèque de classes portable a été ajouté dans Xamarin.Android 4.10.1, Xamarin.iOS 7.0.4 et Xamarin Studio 4.2, ce qui signifie que tous les projets Xamarin créés avec ces outils peuvent intégrer des assemblys de bibliothèque de classes portable Visual Basic.

Pour créer et compiler les bibliothèques de classes portables Visual Basic, vous devez utiliser Visual Studio sur Windows (Visual Studio 2012 ou version ultérieure).

> [!NOTE]
> Bibliothèques de Visual Basic ne peuvent être créés et compilé à l’aide de Visual Studio. Xamarin.iOS et Xamarin.Android ne prennent pas en charge le langage Visual Basic.
>
> Si vous travaillez uniquement dans Visual Studio, vous pouvez référencer le projet Visual Basic à partir de projets Xamarin.iOS et Xamarin.Android.
>
> Si vos projets iOS et Android doivent également être chargés dans Visual Studio pour Mac, vous devez référencer l’assembly de sortie à partir de la bibliothèque PCL Visual Basic.


## <a name="creating-a-visual-basicnet-pcl"></a>Création d’une bibliothèque de classes portable Visual Basic.NET

Cette section décrit comment créer une bibliothèque de classes Portable Visual Basic à l’aide de Visual Studio.
La bibliothèque peut ensuite être référencée dans d’autres projets, y compris les applications Xamarin.iOS, Xamarin.Android et Xamarin.Forms.

### <a name="creating-a-pcl"></a>Création d’une bibliothèque de classes portable

Lors de l’ajout d’une bibliothèque de classes portable Visual Basic dans Visual Studio, vous devez choisir un profil qui décrit votre bibliothèque doit être compatible avec les plateformes. Les profils sont expliquées dans l’Introduction de document de la bibliothèque de classes portable.

Les étapes pour créer une bibliothèque de classes portable et choisir son profil sont :

1.  Dans le **nouveau projet** s’affiche, sélectionnez le **Visual Basic > bibliothèque de classes (Portable)** option :

    [![](images/image1-sml.png "Créer la nouvelle bibliothèque Portable Visual Basic")](images/image1.png#lightbox)

1.  Visual Studio vous invite immédiatement par le code suivant **ajouter une bibliothèque de classes Portable** boîte de dialogue afin que le profil peut être configuré. Cochez les plateformes que vous devez prendre en charge et appuyez sur **OK**.

    [![](images/image2-sml.png "Sélectionnez le profil PCL en choisissant des plateformes")](images/image2.png#lightbox)

1.  Le projet de bibliothèque de classes portable Visual Basic s’affiche comme indiqué dans le **l’Explorateur de solutions** comme suit :

    [![](images/image3-sml.png "Projet de bibliothèque de classes portable Visual Studio vide")](images/image3.png#lightbox)


La bibliothèque PCL est prête pour l’ajout de code Visual Basic. Les projets de bibliothèque de classes portable peuvent être référencés par d’autres projets (projets d’Application, les projets de bibliothèque et même d’autres projets de bibliothèque de classes portable).

### <a name="editing-the-pcl-profile"></a>Modification du profil de bibliothèque de classes portable

Le profil PCL (qui contrôle quelles plateformes n’est compatible avec la bibliothèque PCL) peuvent être affiché et modifié en effectuant un clic droit sur le projet et en choisissant **Propriétés > bibliothèque > modification...** . La boîte de dialogue est illustré dans cette capture d’écran :

 [![](images/image4-sml.png "Modifier le profil de la bibliothèque de classes portable dans Propriétés du projet")](images/image4.png#lightbox)

Si le profil est modifié une fois que le code a déjà été ajouté à la bibliothèque de classes portable, il est possible que la bibliothèque n’est plus compilées si le code référence des fonctionnalités qui ne font pas partie du profil qui vient d’être sélectionné.


## <a name="summary"></a>Récapitulatif

Cet article a montré comment utiliser le code Visual Basic dans les applications Xamarin à l’aide de Visual Studio et les bibliothèques de classes portables. Même si Xamarin ne prend pas en charge directement les Visual Basic, compilation Visual Basic dans une bibliothèque de classes portable permet au code écrit avec Visual Basic à inclure dans les applications iOS et Android.

Les pages suivantes décrivent comment utiliser des bibliothèques de classes portables Visual Basic.NET en mode natif ou les applications Xamarin.Forms :

- [Création d’applications natives Xamarin.iOS et Xamarin.Android qui utilisent du VB](native-apps.md)
- [Création d’applications Xamarin.Forms avec VB](xamarin-forms.md)


## <a name="related-links"></a>Liens associés

- [TaskyPortableVB (exemple)](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/TaskyPortableVB)
- [XamarinFormsVB (sample)](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/XamarinFormsVB)
- [Le développement multiplateforme avec le Kit de développement .NET Framework (Microsoft)](https://msdn.microsoft.com/library/gg597391(v=vs.110).aspx)
