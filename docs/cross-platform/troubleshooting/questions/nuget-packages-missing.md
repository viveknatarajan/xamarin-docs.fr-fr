---
title: Erreur de packages manquants après la mise à jour de packages NuGet
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: D61CC966-1D4A-49A5-8A6F-41572E28329B
author: asb3993
ms.author: amburns
ms.date: 05/08/2018
ms.openlocfilehash: 7cb802dd60d4e4879a260ff56d4f94ea5acb2965
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50674845"
---
# <a name="missing-packages-error-after-updating-nuget-packages"></a>Erreur de packages manquants après la mise à jour de packages NuGet

Ce problème a principalement été signalé sur les solutions d’application exemple Xamarin.Forms, mais le risque de ce problème peut se produire sur n’importe quel projet qui utilise les packages NuGet. 

Si, après la mise à jour les packages Nuget dans votre projet ou solution, vous voyez une erreur qui fait référence à l’ancien numéros de version de package, telles que :

```csharp
Error: This project references NuGet package(s) that are missing on this computer.
Enable NuGet Package Restore to download them.  
For more information, see http://go.microsoft.com/fwlink/?LinkID=322105

The missing file is ../../packages/Xamarin.Forms.1.3.1.6296/build/portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10/Xamarin.Forms.targets. (FormsGallery)
```

Dans cet exemple *Xamarin.Forms.1.3.1.6296* ancien numéro de version qui a été supprimée avec la mise à jour du package Nuget.

Cela peut se produire si les éléments XML dans le fichier .csproj qui font référence à l’ancien numéro de version de package avaient été ajoutées manuellement ou modifié, Nuget ne supprimer ni les mettre à jour s’ils avaient été manuellement ajoutés/modifiés, donc le projet est maintenant recherche de packages qui ont été supprimé. 

Pour résoudre ce problème, modifier manuellement l’ou les fichiers .csproj et à supprimer tous les éléments qui font référence à l’ancien numéro de version. 

Exemples d’éléments à supprimer (s’ils ont l’ancien numéro de version de package) :

```xml
<Reference Include="Xamarin.Forms.Maps">
    <HintPath>..\..\packages\Xamarin.Forms.Maps.1.3.1.6296\lib\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.Maps.dll</HintPath>
</Reference>

<Import Project="..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets" Condition="Exists('..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets')" />
<Error Condition="!Exists('..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets')" Text="$([System.String]::Format('$(ErrorText)', '..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets'))" />
```