---
title: Rubriques d’intégration avancée
ms.prod: xamarin
ms.assetid: 002CE0B1-96CC-4AD7-97B7-43B233EF57A6
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.openlocfilehash: b669c61300b1d808be5467358b2ec16a9bc194a8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="external-integrations"></a>Intégrations externes

Les assemblys d’intégration doivent référencer la [ `Xamarin.Workbooks.Integrations` NuGet][nuget]. Découvrez notre [documentation de démarrage rapide](~/tools/workbooks/sdk/index.md) pour plus d’informations sur la mise en route avec le package NuGet.

Intégration du client est également prises en charge et est initiées en plaçant les fichiers JavaScript ou CSS portant le même nom que l’assembly de l’intégration de l’agent dans le même répertoire. Par exemple, si l’assembly d’intégration de l’agent (qui fait référence à NuGet) est nommé `SampleExternalIntegration.dll`, puis `SampleExternalIntegration.js` et `SampleExternalIntegration.css` seront intégrés dans le client ainsi s’ils existent. Intégration du client est facultatifs.

L’intégration externe lui-même peut être insérée comme un NuGet, fournie et référencée directement dans l’application qui héberge l’agent, ou simplement placé en même temps qu’un `.workbook` fichier qui souhaite consommer.

Intégrations externes (l’agent et le client) dans les packages NuGet seront chargées automatiquement lorsque le package est référencé, conformément à la documentation de démarrage rapide, tandis que les assemblys d’intégration livrés avec le classeur devrez référencer ainsi :

```csharp
#r "SampleExternalIntegration.dll"
```

Lorsque vous référencez une intégration de cette façon, il ne sera pas chargé par le client immédiatement&mdash;vous devez appeler du code à partir de l’intégration pour qu’elle soit à charger. Nous allons être ciblant ce bogue à l’avenir.

Le `Xamarin.Interactive` PCL fournit une intégration important quelques API. Chaque intégration doit fournir au moins un point d’entrée de l’intégration :

```csharp
using Xamarin.Interactive;

[assembly: AgentIntegration (typeof (AgentIntegration))]

class AgentIntegration : IAgentIntegration
{
    const string TAG = nameof (AgentIntegration);

    public void IntegrateWith (IAgent agent)
    {
        // hook into IAgent APIs
    }
}
```

À ce stade, une fois que l’assembly de l’intégration est référencé, le client charge implicitement les fichiers intégration JavaScript et CSS.

## <a name="apis"></a>API

Comme avec n’importe quel assembly qui est référencé par un classeur ou dynamique inspecter la session, un de ses API publiques sont accessible à la session. Par conséquent, il est important de disposer d’une surface API sûre et pratique pour les utilisateurs à Explorer.

L’assembly de l’intégration est effectivement un pont entre une application ou le Kit de développement logiciel d’intérêt et la session. Il peut fournir de nouvelles API qui ont un sens en particulier dans le cadre d’un classeur ou dynamique inspecter la session, ou ne fournir aucune API publique et simplement exécuter « en arrière-plan » des tâches comme générant l’objet [représentations](~/tools/workbooks/sdk/representations.md).

> [!NOTE]
> API qui doit être publique, mais ne doit pas être exposés via IntelliSense peut être marqués avec classiques `[EditorBrowsable (EditorBrowsableState.Never)]` attribut.

[nuget]: https://nuget.org/packages/Xamarin.Workbooks.Integration
