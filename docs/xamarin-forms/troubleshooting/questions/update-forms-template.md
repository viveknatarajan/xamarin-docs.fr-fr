---
title: Puis-je mettre à jour le modèle par défaut de Xamarin.Forms à un package NuGet plus récente ?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 160FBE13-26EB-4B4F-9248-A5CBE58FDD7F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: fce595d7722dcd053f6fc9dcad84dc9a921e55b3
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="can-i-update-the-xamarinforms-default-template-to-a-newer-nuget-package"></a>Puis-je mettre à jour le modèle par défaut de Xamarin.Forms à un package NuGet plus récente ?

Ce guide utilise le modèle de bibliothèque de classes portables Xamarin.Forms par exemple, mais la même méthode générale fonctionne également pour le modèle de projet partagé de Xamarin.Forms. Ce guide est écrit avec l’exemple de mise à jour à partir de Xamarin.Forms 1.5.1.6471 à 2.1.0.6529, mais la même procédure est possible de définir d’autres versions comme la valeur par défaut à la place.

1.  Copiez le modèle d’origine `.zip` à partir de :

    > `C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Xamarin\Xamarin\[Xamarin Version]\T\PT\Cross-Platform\Xamarin.Forms.PCL.zip`

2.  Décompressez le `.zip` vers un emplacement temporaire.

3.  Modifiez toutes les occurrences de l’ancienne version du package Forms vers la nouvelle version que vous souhaitez utiliser.
    *   `FormsTemplate\FormsTemplate.vstemplate`
    *   `FormsTemplate.Android\FormsTemplate.Android.vstemplate`
    *   `FormsTemplate.WinPhone\FormsTemplate.WinPhone.vstemplate`
    *   `FormsTemplate.iOS\FormsTemplate.iOS.vstemplate`

    Exemple : `<package id="Xamarin.Forms" version="1.5.1.6471" />` -> `<package id="Xamarin.Forms" version="2.1.0.6529" />`

4.  Modifier l’élément « name » de la main [fichier de modèle à plusieurs projets](http://msdn.microsoft.com/library/ms185308.aspx) (`Xamarin.Forms.PCL.vstemplate`) pour le rendre unique. Par exemple :
    > <Name>Application vide (Xamarin.Forms Portable) - 2.1.0.6529</Name>

5.  Nouveau compresser le dossier modèle entier. Veillez à respecter la structure de fichier d’origine de le `.zip` fichier. Le `Xamarin.Forms.PCL.vstemplate` fichier doit être en haut de la `.zip` fichier, pas au sein de tous les dossiers.

6.  Créer un sous-répertoire « Applications Mobile » dans votre dossier de modèles Visual Studio par utilisateur :
    > `%USERPROFILE%\Documents\Visual Studio 2013\Templates\ProjectTemplates\Visual C#\Mobile Apps`

7.  Copiez le dossier compressé des modèles dans le nouveau répertoire « Applications Mobile ».

8.  Télécharger le package NuGet correspondant à la version de l’étape 3. Par exemple, [ http://nuget.org/api/v2/package/Xamarin.Forms/2.1.0.6529 ](http://nuget.org/api/v2/package/Xamarin.Forms/2.1.0.6529) (voir aussi [ http://stackoverflow.com/questions/8597375/how-to-get-the-url-of-a-nupkg-file ](http://stackoverflow.com/questions/8597375/how-to-get-the-url-of-a-nupkg-file)), puis copiez-le dans le sous-dossier approprié du dossier d’extensions de Visual Studio de Xamarin :
    > `C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Xamarin\Xamarin\[Xamarin Version]\Packages`
