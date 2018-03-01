---
title: "Une erreur IBTool : Impossible d’effectuer l’opération."
ms.topic: article
ms.prod: xamarin
ms.assetid: A804EBC4-2BBF-4A98-A4E8-A455DB2E8A17
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: dd668859428da1abfa3a8e46a0810b2de6645fe2
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="ibtool-error-the-operation-couldnt-be-completed"></a>Une erreur IBTool : Impossible d’effectuer l’opération.

## <a name="fixed-in-xcode-611"></a>Fixe dans Xcode 6.1.1

Apple [fixe](https://developer.apple.com/library/content/documentation/Xcode/Conceptual/RN-Xcode-Archive/Chapters/xc6_release_notes.html#//apple_ref/doc/uid/TP40016994-CH4-SW1) cela `ibtool` bogue dans Xcode 6.1.1, par conséquent, la mise à niveau à Xcode 6.1.1 ou une version ultérieure est le plus simple.

* * *

## <a name="description-of-the-problem"></a>Description du problème

Le `ibtool` commande dans Xcode 6.0 avait un bogue sur OS X 10.10 Yosemite. Xamarin.iOS utilise de Xcode `ibtool` pour compiler des plans conceptuels et `XIB` fichiers.

Vous trouverez plus d’informations sur le bogue par rapport à Xcode sur les serveurs suivants post de débordement de pile : [http://stackoverflow.com/questions/25754763/cant-open-storyboard](http://stackoverflow.com/questions/25754763/cant-open-storyboard)

### <a name="error-message"></a>Message d'erreur

> Le document « MainStoryboard.storyboard » n’a pas pu être ouvert. Impossible d’effectuer l’opération. (erreur com.apple.InterfaceBuilder -1.)

## <a name="workarounds-for-xcode-60"></a>Solutions de contournement (Xcode 6.0)

### <a name="option-1-manage-all-uiimageviewimage-properties-in-code"></a>Option 1 : Gérer tous les `UIImageView.Image` propriétés dans le code

Plutôt que de définir la `Image` propriété d’un `UIImageView` dans la table de montage séquentiel ou `.xib` fichier, vous pouvez définir la propriété dans un de l’affichage du cycle de vie substituez les méthodes dans le contrôleur d’affichage (par exemple, dans `ViewDidLoad()`). Voir aussi [utilisation des Images](~/ios/app-fundamentals/images-icons/index.md) pour obtenir des conseils sur l’utilisation de `UIImage.FromBundle()` et `UIImage.FromFile()`.

### <a name="option-2-move-all-of-the-image-resources-to-the-top-level-resources-folder"></a>Option 2 : Déplacer toutes les ressources de l’image au niveau supérieur `Resources` dossier

Après avoir déplacé les images au niveau supérieur `Resources` dossier, vous devez mettre à jour le plan conceptuel et `.xib` fichiers à utiliser les nouveaux chemins de l’image.

### <a name="option-3-set-the-logicalname-for-any-problematic-image-assets-so-they-are-copied-to-the-top-level-of-theapp-bundle"></a>Option 3 : Définir le `LogicalName` pour toutes les ressources d’image problématique afin qu’ils sont copiés vers le niveau supérieur de la`.app` offre groupée

Par exemple, supposons que l’original `.csproj` fichier contient l’entrée suivante :

`<BundleResource Include="Resources\Images\image.png" />`

Vous pouvez modifier cet élément et ajouter un `LogicalName` afin que l’image est copiée à la place pour le niveau supérieur de la `.app `offre groupée :

```xml
<BundleResource Include="Resources\Images\image.png">
    <LogicalName>image.png</LogicalName>
</BundleResource>
```

Dans Visual Studio pour Mac le `LogicalName` peut également être définie à l’aide de la `Resource ID` champ pour l’image sous **vue > remplit > Propriétés**. (Voir aussi : [http://stackoverflow.com/questions/16938250/xamarin-studio-folder-structure-issue-in-ios-project/16951545#16951545](http://stackoverflow.com/questions/16938250/xamarin-studio-folder-structure-issue-in-ios-project/16951545#16951545))

Après cette modification, vous devez mettre à jour le plan conceptuel et `.xib` fichiers à utiliser les nouveaux chemins de l’image de niveau supérieur. Visual Studio pour Mac met met automatiquement à jour la liste des saisies semi-automatiques pour le `Image` propriété dans le concepteur iOS. Dans Visual Studio, vous devez modifier le chemin d’accès à la main. Le concepteur iOS affichera alors cela comme une image manquante, mais le projet sera générer et exécuter correctement.

### <a name="next-steps"></a>Étapes suivantes

Pour obtenir une assistance pour nous contacter, ou si ce problème persiste même après utilisant les informations ci-dessus, consultez [les options de support sont disponibles pour Xamarin ?](~/cross-platform/troubleshooting/support-options.md) pour plus d’informations sur les options de contact, des suggestions, ainsi que comment entrer un bogue nouveau si nécessaire. 

