---
title: La mise à jour des applications Mac existantes
description: Ce document décrit les étapes qui doivent être remplies pour mettre à jour une application Xamarin.Mac à partir de l’API classique à l’API unifiée.
ms.prod: xamarin
ms.assetid: 26673CC5-C1E5-4BAC-BEF4-9A386B296FD5
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 5e6034b079bba5e884872e4f2096d677fd3641d0
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61213526"
---
# <a name="updating-existing-mac-apps"></a>La mise à jour des applications Mac existantes

La mise à jour une application existante pour utiliser l’API unifiée nécessite des modifications au fichier projet lui-même ainsi que pour les espaces de noms et les API utilisées dans le code d’application.

## <a name="the-road-to-64-bits"></a>La route à 64 Bits

Les nouvelles API unifiée sont requis pour prendre en charge les architectures 64 bits à partir d’une application Xamarin.Mac. À compter du 1er février 2015 Apple exige que tous les envois d’application nouveau vers le Store d’applications Mac prennent en charge les architectures 64 bits.

Xamarin fournit des outils pour Visual Studio pour Mac et Visual Studio automatiser le processus de migration à partir de l’API classique à l’API unifiée ou vous pouvez convertir les fichiers projet manuellement. Bien que les outils automatique à l’aide est fortement recommandé, cet article décrit les deux méthodes.

### <a name="before-you-start"></a>Avant de commencer...

Avant de mettre à jour de votre code existant vers l’API unifiée, il est vivement recommandé que vous supprimez toutes les *avertissements de compilation*. Nombreux *avertissements* dans l’API classique deviendra erreurs une fois que vous migrez vers unifiée. Il est plus facile de les corriger avant de commencer, car les messages du compilateur à partir de l’API classique fournissent souvent des indications sur les éléments à mettre à jour.

## <a name="automated-updating"></a>Mise à jour automatisée

Une fois que les avertissements ont été résolus, sélectionnez un projet Mac dans Visual Studio pour Mac ou Visual Studio et choisissez **migrer vers l’API unifiée Xamarin.Mac** à partir de la **projet** menu. Exemple :

![](updating-mac-apps-images/beta-tool1.png "Choisissez de migrer vers API unifiée Xamarin.Mac dans le menu projet")

Vous devrez acceptez cet avertissement avant l’exécution de la migration automatisée (évidemment, vous devez vous assurer vous contrôlez sauvegardes/source avant de passer à cette aventure) :

![](updating-mac-apps-images/migrate01.png "Acceptez cet avertissement avant l’exécution de la migration automatisée")

Il existe deux types de Framework cible pris en charge qui peuvent être sélectionnés lors de l’utilisation de l’API unifiée dans une application Xamarin.Mac :

- **Xamarin.Mac Mobile Framework** -il s’agit du .NET framework optimisé utilisé par Xamarin.iOS et Xamarin.Android prenant en charge un sous-ensemble de la version complète **Desktop** framework. Il s’agit de l’infrastructure recommandée, car elle fournit les fichiers binaires de moyenne plus petits en raison du comportement de liaison supérieure.
- **Xamarin.Mac .NET Framework 4.5** -cette infrastructure est là encore, un sous-ensemble de la **Desktop** framework. Toutefois, il réduit nettement moins de la version complète **Desktop** framework postérieure à la **Mobile** framework et doit _« fonctionnent simplement »_ avec la plupart des Packages NuGet ou des bibliothèques tierces 3e. Cela permet au développeur de consommer standard **Desktop** assemblys pendant que vous utilisez toujours un framework pris en charge, mais cette option génère des groupes d’applications de plus grande. Il s’agit du framework recommandé où les assemblys .NET 3e partie sont utilisés qui ne sont pas compatibles avec le **Xamarin.Mac Mobile Framework**. Pour obtenir la liste des assemblys pris en charge, consultez notre [assemblys](~/cross-platform/internals/available-assemblies.md) documentation.

Pour plus d’informations sur les Frameworks cibles et les implications en matière de sélection d’une cible spécifique pour votre application Xamarin.Mac, veuillez consulter notre [Frameworks cibles](~/mac/platform/target-framework.md) documentation. 

L’outil automatise essentiellement toutes les étapes décrites dans le **mise à jour manuellement** section présentés ci-dessous et est la méthode conseillée de conversion d’un projet Xamarin.Mac existant vers l’API unifiée.

## <a name="steps-to-update-manually"></a>Étapes pour mettre à jour manuellement

Là encore, une fois que les avertissements ont été résolus, procédez comme suit pour mettre à jour manuellement les applications pour utiliser la nouvelle API unifiée Xamarin.Mac :

### <a name="1-update-project-type--build-target"></a>1. Type de projet de mise à jour & cible de génération

Modifier la version de projet dans votre **csproj** fichiers à partir de `42C0BBD9-55CE-4FC1-8D90-A7348ABAFB23` à `A3F8F2AB-B479-4A4A-A458-A89E7DC349F1`. Modifier le **csproj** fichier dans un éditeur de texte, en remplaçant le premier élément dans le `<ProjectTypeGuids>` élément, comme indiqué :

![](updating-mac-apps-images/csproj.png "Modifiez le fichier csproj dans un éditeur de texte, en remplaçant le premier élément dans l’élément ProjectTypeGuids comme indiqué")

Modifier le **importation** élément contenant `Xamarin.Mac.targets` à `Xamarin.Mac.CSharp.targets` comme indiqué :

![](updating-mac-apps-images/csproj2.png "Modifier l’élément Import qui contient Xamarin.Mac.targets à Xamarin.Mac.CSharp.targets comme indiqué")

Ajoutez les lignes suivantes de code après le `<AssemblyName>` élément :

```xml
<TargetFrameworkVersion>v2.0</TargetFrameworkVersion>
<TargetFrameworkIdentifier>Xamarin.Mac</TargetFrameworkIdentifier>

```

Exemple :

![](updating-mac-apps-images/csproj3.png "Ajoutez ces lignes de code après l’élément de < nom_assembly >")

### <a name="2-update-project-references"></a>2. Mettre à jour les références de projet

Développez le projet application Mac **références** nœud. Elle affiche initialement un * interrompue - **XamMac** référence identique à cette capture d’écran (étant donné que nous venons de modifier le type de projet) :

![](updating-mac-apps-images/references.png "Il affichent initialement une référence rompue-XamMac similaire à cette capture d’écran")

Cliquez sur le **icône d’engrenage** en regard de la **XamMac** entrée et sélectionnez **supprimer** pour supprimer la référence incorrecte.

Ensuite, avec le bouton droit sur le **références** dossier dans le **l’Explorateur de solutions** et sélectionnez **modifier les références**. Faites défiler vers le bas de la liste de références et cochez la case outre **Xamarin.Mac**.

![](updating-mac-apps-images/references2.png "Faites défiler vers le bas de la liste de références et cochez la case en plus de Xamarin.Mac")

Appuyez sur **OK** pour enregistrer des modifications de références de projet.

### <a name="3-remove-monomac-from-namespaces"></a>3. Supprimer MonoMac d’espaces de noms

Supprimer le **MonoMac** préfixe à partir d’espaces de noms dans `using` instructions ou partout où un nom de classe a été entièrement qualifié (par ex. `MonoMac.AppKit` devient simplement `AppKit`).

### <a name="4-remap-types"></a>4. Remapper les Types

[Les types natifs](~/cross-platform/macios/nativetypes.md) ont été introduites qui remplacent certains Types qui ont précédemment été utilisés, tels que les instances de `System.Drawing.RectangleF` avec `CoreGraphics.CGRect` (par exemple). Vous trouverez la liste complète des types sur le [types natifs](~/cross-platform/macios/nativetypes.md) page.

### <a name="5-fix-method-overrides"></a>5. Corriger les substitutions de méthode

Certains `AppKit` méthodes ont leur signature modifié pour utiliser le nouveau [types natifs](~/cross-platform/macios/nativetypes.md) (tel que `nint`). Si les sous-classes personnalisés substituer ces méthodes les signatures ne correspondront plus et produiront des erreurs. Corrigez ces substitutions de méthode en modifiant la sous-classe pour correspondre à la nouvelle signature à l’aide des types natifs. 

## <a name="considerations"></a>Éléments à prendre en considération

Les considérations suivantes doivent être prises en compte lors de la conversion d’un projet Xamarin.Mac existant à partir de l’API classique à la nouvelle API unifiée si cette application s’appuie sur un ou plusieurs composants ou un NuGet Package. 

### <a name="components"></a>Composants

N’importe quel composant que vous avez inclus dans votre application doit également être mis à jour vers l’API unifiée ou vous obtiendrez un conflit lorsque vous essayez de compiler. Pour les composants inclus, remplacez la version actuelle par une nouvelle version à partir du Store de composant Xamarin qui prend en charge de l’API unifiée et effectuer un nettoyage de build. Tout composant qui n’a pas encore été converti par l’auteur, affichera un 32 bits uniquement avertissement dans le magasin de composants.

### <a name="nuget-support"></a>Prise en charge par NuGet

Bien que nous contribué les modifications apportées à NuGet pour travailler avec la prise en charge de l’API unifiée, il n’a pas été une nouvelle version de NuGet, donc nous évaluons l’obtention de NuGet pour reconnaître les nouvelles API. 

En attendant, tout comme les composants, vous devrez passer n’importe quel NuGet Package que vous avez inclus dans votre projet vers une version qui prend en charge les API unifiée et effectuer un nettoyage de build par la suite.

> [!IMPORTANT]
> Si vous avez une erreur dans le formulaire _« erreur 3 ne peut pas inclure 'monomac.dll' et 'Xamarin.Mac.dll' dans le même projet Xamarin.Mac : 'Xamarin.Mac.dll' est référencé explicitement, alors que « monomac.dll » est référencé par ' xxx, Version = 0.0.000, Culture = neutral, PublicKeyToken = null » «_ après la conversion de votre application à l’API unifiée, il est généralement dû à avoir un composant ou un NuGet Package dans le projet qui n’a pas été mis à jour vers l’API unifiée. Vous devez supprimer le composant existant/NuGet, mise à jour vers une version qui prend en charge les API unifiée, puis effectuez une génération propre.

## <a name="enabling-64-bit-builds-of-xamarinmac-apps"></a>L’activation de 64 bits génère des applications de Xamarin.Mac

Pour une application mobile Xamarin.Mac qui a été convertie vers l’API unifiée, le développeur doit toujours autorisent la création de l’application pour les ordinateurs 64 bits à partir des Options de l’application. Consultez la **l’activation de 64 bits s’appuie de Xamarin.Mac des applications** de la [considérations relatives à la plateforme 32/64 bits](~/cross-platform/macios/32-and-64/index.md) génère de document pour obtenir des instructions détaillées sur l’activation de 64 bits.
    
## <a name="finishing-up"></a>En cours d’achèvement

Si vous choisissez d’utiliser la méthode automatique ou manuelle pour convertir les API unifiée de votre application Xamarin.Mac du modèle Classic, il existe plusieurs instances qui requièrent d’autres, une intervention manuelle. Consultez notre [conseils pour la mise à jour le Code vers l’API unifiée](~/cross-platform/macios/unified/updating-tips.md) de documents pour les problèmes connus et des solutions de contournement.

## <a name="related-links"></a>Liens associés

- [Conseils pour la mise à jour du code vers l’API unifiée](~/cross-platform/macios/unified/updating-tips.md)
- [Utilisation de types natifs dans des applications multiplateformes](~/cross-platform/macios/native-types-cross-platform.md)
- [Différences d’API unifiée de vs classiques](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
