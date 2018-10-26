---
title: Comment puis-je copier les fichiers de sortie IPA dans le dossier de dépôt TFS ?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: B0F1E09E-7315-45BA-B7FF-44D2063EE19C
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 74e2f2219dcb0908edce7f109844932639038b25
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50113038"
---
# <a name="how-can-i-copy-ipa-output-files-to-the-tfs-drop-folder"></a>Comment puis-je copier les fichiers de sortie IPA dans le dossier de dépôt TFS ?

Ouvrez le `.csproj` de fichiers pour le projet d’application iOS dans un éditeur de texte et puis ajoutez les lignes suivantes à la fin (juste avant la fermeture `</Project>` balise) :

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

- C’est la même technique générale abordée sur [puis-je modifier le chemin de sortie du fichier IPA ?](~/ios/troubleshooting/questions/ipa-output-path.md). Les deux points importants sont à définir `$(TF_BUILD_BINARIESDIRECTORY)` comme dossier de destination et pour ajouter une condition supplémentaire donc `CopyIpa` s’exécutera uniquement pour les builds de TFS.

- Pour obtenir une description de `TF_BUILD_BINARIESDIRECTORY` consultez [prédéfini les variables de génération](https://docs.microsoft.com/azure/devops/pipelines/build/variables).

## <a name="additional-references"></a>Références supplémentaires

- [Documentation sur l’installation de TFS pour une utilisation avec Xamarin](https://docs.microsoft.com/azure/devops/repos/tfvc/overview)
- [Tâche de génération Azure DevOps : Xamarin.Android](https://docs.microsoft.com/azure/devops/pipelines/tasks/build/xamarin-android)
- [Tâche de génération Azure DevOps : Xamarin.iOS](https://docs.microsoft.com/azure/devops/pipelines/tasks/build/xamarin-ios)

### <a name="next-steps"></a>Étapes suivantes

Ce document décrit le comportement actuel à compter de Xamarin 3.11.666 pour Visual Studio et l’hôte de build Xamarin.iOS 8.10.3 sur le Mac. Pour obtenir une assistance supplémentaire, pour nous contacter, ou si le reste de ce problème même après utilisant les informations ci-dessus, veuillez consulter [quelles options de prise en charge sont disponibles pour Xamarin ?](~/cross-platform/troubleshooting/support-options.md) pour plus d’informations sur les options de contact, des suggestions, ainsi que comment signaler un bogue nouveau si nécessaire.
