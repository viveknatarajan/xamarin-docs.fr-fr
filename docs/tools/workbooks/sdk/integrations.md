---
title: Rubriques de l’intégration avancée
description: Ce document décrit les rubriques avancées relatives aux intégrations de Xamarin Workbooks. Il aborde le package NuGet de Xamarin.Workbook.Integrations et exposition d’API au sein d’un classeur de Xamarin.
ms.prod: xamarin
ms.assetid: 002CE0B1-96CC-4AD7-97B7-43B233EF57A6
author: lobrien
ms.author: laobri
ms.date: 03/30/2017
ms.openlocfilehash: 56ee709b78b8587c2717dc9d25a6357041812d23
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50104204"
---
# <a name="advanced-integration-topics"></a>Rubriques de l’intégration avancée

Les assemblys d’intégration doivent référencer le [ `Xamarin.Workbooks.Integrations` NuGet][nuget]. Découvrez notre [documentation de démarrage rapide](~/tools/workbooks/sdk/index.md) pour plus d’informations sur la mise en route avec le package NuGet.

Intégrations de client sont également pris en charge et sont initiées en plaçant des fichiers JavaScript ou CSS portant le même nom que l’assembly de l’intégration de l’agent dans le même répertoire. Par exemple, si l’assembly d’intégration de l’agent (qui fait référence à NuGet) est nommé `SampleExternalIntegration.dll`, puis `SampleExternalIntegration.js` et `SampleExternalIntegration.css` seront intégrés dans le client ainsi s’ils existent. Intégrations de client sont facultatifs.

L’intégration externe lui-même peut être empaquetée en tant qu’un package NuGet, fournie et référencée directement à l’intérieur de l’application qui héberge l’agent, ou simplement placés en même temps qu’un `.workbook` fichier qui souhaite utiliser.

Intégrations externes (agent et client) dans les packages NuGet seront automatiquement chargées lorsque le package est référencé, conformément à la documentation de démarrage rapide, tandis que les assemblys d’intégration fournis en même temps que le classeur devrez référencez-le ainsi :

```csharp
#r "SampleExternalIntegration.dll"
```

Lors du référencement d’une intégration de cette façon, il ne sera pas chargé par le client immédiatement&mdash;vous devrez appeler du code à partir de l’intégration de charge. Nous allons être ciblant ce bogue à l’avenir.

Le `Xamarin.Interactive` PCL assure une intégration important quelques API. Chaque intégration doit fournir au moins un point d’entrée de l’intégration :

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

À ce stade, une fois que l’assembly de l’intégration est référencé, le client charge implicitement les fichiers de l’intégration JavaScript et CSS.

## <a name="apis"></a>API

Comme avec n’importe quel assembly est référencé par un classeur ou en direct inspecter la session, un de ses API publiques sont accessible à la session. Par conséquent, il est important de disposer d’une surface d’API cohérent et sécurisée pour les utilisateurs à Explorer.

L’assembly de l’intégration est effectivement un pont entre une application ou de kit de développement logiciel d’intérêt et de la session. Il peut fournir de nouvelles API qui sont pertinents en particulier dans le contexte d’un classeur ou en direct inspecter la session, ou ne fournir aucune API publique et simplement effectuer des tâches « dans les coulisses » comme produisant l’objet [représentations](~/tools/workbooks/sdk/representations.md).

> [!NOTE]
> API qui doivent être publique, mais ne doivent pas être exposés via IntelliSense peuvent être marquées avec habituelles `[EditorBrowsable (EditorBrowsableState.Never)]` attribut.

[nuget]: https://nuget.org/packages/Xamarin.Workbooks.Integration
