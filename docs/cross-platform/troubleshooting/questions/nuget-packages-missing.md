---
title: "Erreur de packages manquant après la mise à jour les packages Nuget"
ms.topic: article
ms.prod: xamarin
ms.assetid: D61CC966-1D4A-49A5-8A6F-41572E28329B
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: f330590132e4881484eeae3efafe570d991a509a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="missing-packages-error-after-updating-nuget-packages"></a>Erreur de packages manquant après la mise à jour les packages Nuget

Ce problème a principalement été signalé sur les solutions d’application exemple Xamarin.Forms, mais le risque de ce problème peut se produire sur n’importe quel projet qui utilise les packages NuGet. 

Si, après la mise à jour les packages Nuget dans votre projet ou solution, vous voyez une erreur qui fait référence à des numéros de version de package ancien, tel que :

```csharp
Error: This project references NuGet package(s) that are missing on this computer.
Enable NuGet Package Restore to download them.  
For more information, see http://go.microsoft.com/fwlink/?LinkID=322105

The missing file is ../../packages/Xamarin.Forms.1.3.1.6296/build/portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10/Xamarin.Forms.targets. (FormsGallery)

```

Dans cet exemple *Xamarin.Forms.1.3.1.6296* ancien numéro de version qui a été supprimé avec la mise à jour du package Nuget.

Cela peut se produire si les éléments XML dans le fichier .csproj qui font référence à l’ancien numéro de version du package avaient été ajoutés manuellement ou modifié, Nuget ne sera pas supprimer ou mettre à jour s’ils avaient été ajouté/modifié manuellement, pour le projet est maintenant affiché pour les packages qui ont été supprimé. 

Pour résoudre ce problème, modifier manuellement les fichiers .csproj et à supprimer tous les éléments qui font référence à l’ancien numéro de version. 

Exemples d’éléments à supprimer (s’ils ont l’ancien numéro de version de package) :

```xml
<Reference Include="Xamarin.Forms.Maps">
    <HintPath>..\..\packages\Xamarin.Forms.Maps.1.3.1.6296\lib\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.Maps.dll</HintPath>
</Reference>

<Import Project="..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets" Condition="Exists('..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets')" />
<Error Condition="!Exists('..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets')" Text="$([System.String]::Format('$(ErrorText)', '..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets'))" />

```

