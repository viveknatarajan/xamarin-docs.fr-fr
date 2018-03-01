---
title: "Il est possible de créer une archive .xcarchive à partir de Visual Studio ?"
ms.topic: article
ms.prod: xamarin
ms.assetid: 417D84FB-1BA9-4DB9-A683-66E960BA3D0D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: bb8f9f8f58eb0b1251b31254b3798ba2c2b5c39b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="is-it-possible-to-create-a-xcarchive-archive-from-visual-studio"></a>Il est possible de créer une archive .xcarchive à partir de Visual Studio ?

## <a name="for-xamarin-4"></a>Pour Xamarin 4

À compter de Xamarin 4.x, il est désormais possible de créer un `.xcarchive` à partir de Windows en définissant le `ArchiveOnBuild` propriété `true`. Par exemple, à l’aide de `MSBuild` sur la ligne de commande :

```bash
msbuild /p:Configuration=Release /p:ServerAddress=10.211.55.2 /p:ServerUser=xamUser /p:Platform=iPhone /p:ArchiveOnBuild=true /t:"Build" MyProject.csproj
```

Le `.xcarchive` sera placée dans le `$HOME/Library/Developer/Xcode/Archives` répertoire sur l’hôte de build Mac Xcode et Xamarin Studio recherche aux archives de l’affichage créé précédemment.

Consultez ce [Xamarin Forums valider](https://forums.xamarin.com/discussion/comment/156635/#Comment_156635) pour certaines brèves Remarques supplémentaires sur le `ArchiveOnBuild` propriété. Consultez la documentation [génère de ligne de commande de Xamarin.iOS sur Windows](~/ios/get-started/installation/windows/connecting-to-mac/index.md) pour plus d’informations sur la `ServerAddress` et `ServerUser` propriétés.

* * *

## <a name="for-xamarin-3-and-earlier"></a>Xamarin 3 ou version antérieure

L’extension de Visual Studio Xamarin 3.x ne fournit pas un mécanisme permettant de produire `.xcarchive` archive. Ceci dit, la logique utilisée pour créer `.xcarchive` archives dans Xamarin Studio sur Mac [est décrit ici](https://bugzilla.xamarin.com/show_bug.cgi?id=35#c5), de sorte que vous pouvez probablement créer votre propre `.xcarchive` manuellement si vous voulez.

Mais il est important de noter que vous n’avez pas besoin une `.xcarchive` à soumettre à l’App Store. Vous pouvez envoyer un fichier IPA tant qu’il est signé avec un profil de Distribution App Store (pas un profil de Distribution Ad Hoc).

En fait, vous pouvez simplement compresser le `.app` groupement (qui est signé avec un profil de Distribution de stocker application) et envoyer ces `.zip` fichier vers le magasin d’applications.

Dans les deux cas, vous pouvez utiliser l’application de chargeur d’Application à soumettre l’application (plutôt que Xcode).

