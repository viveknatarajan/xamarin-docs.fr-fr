---
title: Il est possible de créer une archive .xcarchive à partir de Visual Studio ?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 417D84FB-1BA9-4DB9-A683-66E960BA3D0D
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 04/03/2018
ms.openlocfilehash: 952777b3178b82657c8f64d5882c532c188c7a79
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50107233"
---
# <a name="is-it-possible-to-create-a-xcarchive-archive-from-visual-studio"></a>Il est possible de créer une archive .xcarchive à partir de Visual Studio ?

## <a name="for-xamarin-4"></a>Pour Xamarin 4

À compter de Xamarin 4.x, il est désormais possible de créer un `.xcarchive` à partir de Windows en définissant le `ArchiveOnBuild` propriété `true`. Par exemple, à l’aide de `MSBuild` sur la ligne de commande :

```bash
msbuild /p:Configuration=Release /p:ServerAddress=10.211.55.2 /p:ServerUser=xamUser /p:Platform=iPhone /p:ArchiveOnBuild=true /t:"Build" MyProject.csproj
```

Le `.xcarchive` sera placée dans le `$HOME/Library/Developer/Xcode/Archives` répertoire sur l’hôte de build Mac Xcode et Xamarin Studio rechercher aux archives de l’affichage créé précédemment.

Consultez ce [valider des Forums de Xamarin](https://forums.xamarin.com/discussion/comment/156635/#Comment_156635) pour certains brèves Remarques supplémentaires sur le `ArchiveOnBuild` propriété. Consultez la documentation [ligne de commande Xamarin.iOS s’appuie sur Windows](~/ios/get-started/installation/windows/connecting-to-mac/index.md) pour plus d’informations sur la `ServerAddress` et `ServerUser` propriétés.

* * *

## <a name="for-xamarin-3-and-earlier"></a>Pour Xamarin 3 et versions antérieures

L’extension de Visual Studio 3.x Xamarin ne fournit pas un mécanisme pour produire `.xcarchive` archive. Ceci dit, la logique utilisée pour créer `.xcarchive` archives dans Xamarin Studio sur Mac [est décrite ici](https://bugzilla.xamarin.com/show_bug.cgi?id=35#c5), de sorte que vous pouvez probablement créer votre propre `.xcarchive` manuellement si vous souhaitiez.

Mais il est important de noter que vous n’avez pas besoin un `.xcarchive` à envoyer à l’App Store. Vous pouvez envoyer un fichier IPA tant qu’il est signé avec un profil de Distribution App Store (pas un profil de Distribution Ad Hoc).

En fait, vous pouvez simplement compresser le `.app` bundle (qui est signé avec un profil de Distribution App Store) et la soumets `.zip` fichier sur l’app store.

Dans les deux cas, vous pouvez utiliser l’application de chargeur d’Application pour envoyer l’application (plutôt que Xcode).

