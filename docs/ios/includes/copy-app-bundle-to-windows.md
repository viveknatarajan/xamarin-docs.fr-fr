---
ms.openlocfilehash: 5bcf548c0ff907df0913e77a1def2fe27f3eecfd
ms.sourcegitcommit: e7f27ba75cae5099ef053b819b84132a77d4f9e7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2019
ms.locfileid: "52294990"
---

Quand vous créez des applications iOS dans Visual Studio et sur l’agent de build Mac, le bundle .app n’est pas copié sur la machine Windows. Xamarin Tools pour Visual Studio 7.4 ajoute une nouvelle propriété `CopyAppBundle`, qui permet aux builds d’intégration continue (CI) de copier les bundles .app sur Windows.

Pour utiliser cette fonctionnalité, ajoutez la propriété `CopyAppBundle` au fichier .csproj sous le groupe de propriétés auquel vous souhaitez appliquer la fonctionnalité. L’exemple suivant montre comment copier le bundle .app sur l’ordinateur Windows pour une build de **débogage** ciblant **iPhoneSimulator** :

    <PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Debug|iPhoneSimulator' ">
        <CopyAppBundle>true</CopyAppBundle>
    </PropertyGroup>

