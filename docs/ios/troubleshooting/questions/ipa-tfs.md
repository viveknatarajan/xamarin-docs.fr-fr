---
title: "Comment puis-je copier des fichiers de sortie IPA dans le dossier de dépôt TFS ?"
ms.topic: article
ms.prod: xamarin
ms.assetid: B0F1E09E-7315-45BA-B7FF-44D2063EE19C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: c89d81434cac43505c4f0341a10aaf4fc99407fe
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="how-can-i-copy-ipa-output-files-to-the-tfs-drop-folder"></a>Comment puis-je copier des fichiers de sortie IPA dans le dossier de dépôt TFS ?

Ouvrez le `.csproj` du fichier pour le projet d’application iOS dans un éditeur de texte, puis ajoutez les lignes suivantes à la fin (juste avant la fermeture `</Project>` balise) :

```xml
<PropertyGroup>
    <CreateIpaDependsOn>
        $(CreateIpaDependsOn);
        CopyIpa
    </CreateIpaDependsOn>
</PropertyGroup>

<Target Name="CopyIpa"
    Condition="'$(OutputType)' == 'Exe'
        And '$(ComputedPlatform)' == 'iPhone'
        And '$(BuildIpa)' == 'true'
        And '$(TF_BUILD)' == 'true'">
    <Copy
        SourceFiles="$(OutputPath)$(IpaPackageName)"
        DestinationFolder="$(TF_BUILD_BINARIESDIRECTORY)"
    />
</Target>
```

## <a name="notes"></a>Notes

-   Ceci est la même technique générale sur [puis-je modifier le chemin de sortie du fichier IPA ?](~/ios/troubleshooting/questions/ipa-output-path.md). Les deux points importants sont à définir `$(TF_BUILD_BINARIESDIRECTORY)` comme dossier de destination et pour ajouter une condition supplémentaire donc `CopyIpa` s’exécutera uniquement pour les versions de TFS.

-   Pour obtenir une description de `TF_BUILD_BINARIESDIRECTORY` consultez [https://msdn.microsoft.com/en-us/library/hh850448.aspx](https://msdn.microsoft.com/en-us/library/hh850448.aspx).

## <a name="additional-references"></a>Références supplémentaires

- [Documentation sur l’installation de TFS pour une utilisation avec Xamarin](https://docs.microsoft.com/vsts/tfvc/overview)
- [Tâche de génération TFS : Xamarin.Android](https://docs.microsoft.com/en-us/vsts/build-release/tasks/build/xamarin-android)
- [Tâche de génération TFS : Xamarin.iOS](https://docs.microsoft.com/en-us/vsts/build-release/tasks/build/xamarin-ios)

### <a name="next-steps"></a>Étapes suivantes
Ce document décrit le comportement actuel à partir de Xamarin 3.11.666 pour Visual Studio et générer le Xamarin.iOS 8.10.3 sur le Mac. Pour obtenir une assistance pour nous contacter, ou si ce problème persiste même après utilisant les informations ci-dessus, consultez [les options de support sont disponibles pour Xamarin ?](~/cross-platform/troubleshooting/support-options.md) pour plus d’informations sur les options de contact, des suggestions, ainsi que comment entrer un bogue nouveau si nécessaire. 



