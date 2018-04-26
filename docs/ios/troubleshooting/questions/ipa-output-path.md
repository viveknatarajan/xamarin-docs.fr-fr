---
title: Puis-je modifier le chemin de sortie du fichier IPA ?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F5E5DCC6-F7CC-48E2-89E8-709E9C269502
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 9c80a209279a2f032eb6c9efcba1398ca0e267a5
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/26/2018
---
# <a name="can-i-change-the-output-path-of-the-ipa-file"></a>Puis-je modifier le chemin de sortie du fichier IPA ?

## <a name="for-cycle-7-and-higher"></a>Pour le Cycle de 7 et versions ultérieures
Oui, vous pouvez utiliser les cibles de MSBuild personnalisées d’effectuer cette opération. L’option la plus simple consiste probablement à copier le `.ipa` fichier une fois qu’il a été généré.

Ces étapes fonctionne pour n’importe quel projet iOS qui utilise le moteur de génération MSBuild sur Mac ou Windows. (Remarque : tous les projets d’API unifiée utilisent le moteur de génération MSBuild.)

1. Ouvrez le `.csproj` du fichier pour le projet d’application iOS dans un éditeur de texte, puis ajoutez les lignes suivantes à la fin (juste avant la fermeture `</Project>` balise) :
    
    ```
    <PropertyGroup>
           <CreateIpaDependsOn>
           $(CreateIpaDependsOn);
            CopyIpa
           </CreateIpaDependsOn>
    </PropertyGroup>
    
    <Target Name="CopyIpa"
        Condition="'$(OutputType)' == 'Exe'
            And '$(ComputedPlatform)' == 'iPhone'
            And '$(BuildIpa)' == 'true'">
        <Copy
            SourceFiles="$(IpaPackagePath)"
            DestinationFolder="$(OutputPath)"
        />
    </Target>
    ```

2. Définir le DestinationFolder au dossier de sortie souhaité. Comme d’habitude, vous pouvez utiliser les propriétés MSBuild (comme $(OutputPath)) dans cet argument si vous le souhaitez.

## <a name="notes"></a>Notes
- Le `CreateIpaDependsOn` propriété est définie dans le `Xamarin.iOS.Common.targets` fichier faisant partie de Xamarin.iOS. Il se comporte comme décrit sous *substitution 'DependsOn' propriétés* sur [ https://msdn.microsoft.com/library/ms366724.aspx ](https://msdn.microsoft.com/library/ms366724.aspx).

- Vous pouvez utiliser un **déplacer** tâches plutôt qu’un **copie** de tâches si votre par défaut. Si vous choisissez l’option et si vous travaillez sur Windows, vous devez utiliser le nom qualifié complet de tâches `<Microsoft.Build.Tasks.Move>` afin d’éviter une ambiguïté avec le XamarinVS tâches de génération.

## <a name="for-versions-before-xamarin-studio-6005174--xamarin-for-visual-studio-410530"></a>Pour les versions antérieures de Xamarin Studio 6.0.0.5174 | Xamarin pour Visual Studio 4.1.0.530

Oui, vous pouvez utiliser les cibles de MSBuild personnalisées d’effectuer cette opération. L’option la plus simple consiste probablement à copier le `.ipa` fichier une fois qu’il a été généré.

Ces étapes fonctionne pour n’importe quel projet iOS qui utilise le moteur de génération MSBuild sur Mac ou Windows. (Remarque : tous les projets d’API unifiée utilisent le moteur de génération MSBuild.)

1. Ouvrez le `.csproj` du fichier pour le projet d’application iOS dans un éditeur de texte, puis ajoutez les lignes suivantes à la fin (juste avant la fermeture `</Project>` balise).

    ```csharp
    <PropertyGroup>
        <CreateIpaDependsOn>
            $(CreateIpaDependsOn);
            CopyIpa
        </CreateIpaDependsOn>
    </PropertyGroup>
    
    <Target Name="CopyIpa"
        Condition="'$(OutputType)' == 'Exe'
            And '$(ComputedPlatform)' == 'iPhone'
            And '$(BuildIpa)' == 'true'">
        <Copy
            SourceFiles="$(OutputPath)$(IpaPackageName)"
            DestinationFolder="/Users/macuser/Desktop/"
        />
    </Target>
    ```

2. Définir le `DestinationFolder` dans le dossier de sortie souhaité. Comme d’habitude, vous pouvez utiliser les propriétés MSBuild (comme `$(OutputPath)`) au sein de cet argument si vous le souhaitez.

## <a name="notes"></a>Notes
- Le `CreateIpaDependsOn` propriété est définie dans le `Xamarin.iOS.Common.targets` fichier faisant partie de Xamarin.iOS. Il se comporte comme décrit sous *propriétés « DependsOn » de substitution* sur [ https://msdn.microsoft.com/library/ms366724.aspx ](https://msdn.microsoft.com/library/ms366724.aspx).

- Vous pouvez utiliser un **déplacer** tâches plutôt qu’un **copie** de tâches si votre par défaut. Si vous choisissez l’option et si vous travaillez sur Windows, vous devez utiliser le nom qualifié complet de tâches `<Microsoft.Build.Tasks.Move>` afin d’éviter une ambiguïté avec le XamarinVS tâches de génération.
