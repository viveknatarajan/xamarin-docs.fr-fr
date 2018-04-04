---
title: Mise à jour d’applications Mac existantes
description: Suivez ces étapes pour mettre à jour d’une application Xamarin.Mac existante pour utiliser l’API unifiée.
ms.prod: xamarin
ms.assetid: 26673CC5-C1E5-4BAC-BEF4-9A386B296FD5
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: bdd3b01212d8c1c9949d6aa183c38c83d0afe831
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="updating-existing-mac-apps"></a>Mise à jour d’applications Mac existantes

_Suivez ces étapes pour mettre à jour d’une application Xamarin.Mac existante pour utiliser l’API unifiée._

Mise à jour d’une application existante pour utiliser l’API unifiée nécessite des modifications au fichier projet lui-même, ainsi que pour les espaces de noms et les API utilisées dans le code d’application.

## <a name="the-road-to-64-bits"></a>Déplacement à 64 Bits

Les nouvelles API unifiée sont requis pour prendre en charge les architectures 64 bits à partir d’une application Xamarin.Mac. À partir du 1er février 2015 Apple nécessite que tous les envois d’application nouveau vers le magasin d’applications Mac prennent en charge les architectures 64 bits.

Xamarin fournit des outils pour Visual Studio pour Mac et Visual Studio automatiser le processus de migration de l’API classique à l’API unifiée, ou vous pouvez convertir les fichiers projet manuellement. Alors que les outils automatique d’à l’aide est fortement recommandé, cet article couvre les deux méthodes.

### <a name="before-you-start"></a>Avant de commencer...

Avant de mettre à jour de votre code existant à l’API unifiée, il est vivement recommandé d’éliminer toutes *avertissements de compilation*. Nombreux *avertissements* dans l’API classique deviendra erreurs une fois que vous migrez vers unifié. Il est plus facile de les corriger avant de commencer, car les messages du compilateur à partir de l’API classique fournissent souvent des indications sur les éléments à mettre à jour.

## <a name="automated-updating"></a>Automatisée de mise à jour

Une fois que les avertissements ont été résolus, sélectionnez un projet Mac existant dans Visual Studio pour Mac ou Visual Studio et choisissez **migrer vers une API unifiée de Xamarin.Mac** à partir de la **projet** menu. Par exemple :

![](updating-mac-apps-images/beta-tool1.png "Choisir de migrer d’API unifiée de Xamarin.Mac dans le menu projet")

Vous devez accepter cet avertissement avant de pouvoir exécute la migration automatisée (évidemment, vous devez vous assurer vous contrôlez les sauvegardes/source avant de commencer cette adventure) :

![](updating-mac-apps-images/migrate01.png "Acceptez cet avertissement avant de pouvoir exécute la migration automatisée")

Il existe deux types de Framework cible pris en charge qui peuvent être sélectionnés lors de l’utilisation de l’API unifiée dans une application Xamarin.Mac :

- **Xamarin.Mac Mobile Framework** -il s’agit du même analysé .NET framework utilisée par Xamarin.iOS et Xamarin.Android prenant en charge un sous-ensemble de la version complète **bureau** framework. Il s’agit de l’infrastructure recommandée, car elle fournit des binaires moyenne plus petits en raison du comportement de liaison supérieure.
- **Xamarin.Mac .NET 4.5 Framework** -cette infrastructure est à nouveau, un sous-ensemble de la **bureau** framework. Toutefois, il réduit beaucoup moins de la version complète **Desktop** framework que le **Mobile** framework et doit _« travail simplement »_ avec la plupart des Packages NuGet ou des bibliothèques tierces 3e. Cela permet au développeur de consommer standard **bureau** assemblys toujours à l’aide d’une infrastructure de prise en charge, mais cette option génère des groupes d’applications de plus grandes. Il s’agit le framework recommandé où les assemblys .NET 3e partie utilisés qui ne sont pas compatibles avec la **Xamarin.Mac Mobile Framework**. Pour obtenir la liste des assemblys pris en charge, consultez notre [assemblys](~/cross-platform/internals/available-assemblies.md) documentation.

Pour plus d’informations sur l’infrastructure cible et les implications en matière de sélection d’une cible spécifique pour votre application Xamarin.Mac, veuillez consulter notre [infrastructure cible](~/mac/platform/target-framework.md) documentation. 

L’outil automatise fondamentalement toutes les étapes décrites dans le **mise à jour manuellement** section présentés ci-dessous et est la méthode recommandée pour convertir un projet Xamarin.Mac existant à l’API unifiée.

## <a name="steps-to-update-manually"></a>Étapes pour mettre à jour manuellement

Là encore, une fois que les avertissements ont été résolus, procédez comme suit pour mettre à jour manuellement les applications Xamarin.Mac pour utiliser la nouvelle API unifiée :

### <a name="1-update-project-type--build-target"></a>1. Type de projet de mise à jour et la cible de génération

Modifier la version de projet dans votre **csproj** fichiers à partir de `42C0BBD9-55CE-4FC1-8D90-A7348ABAFB23` à `A3F8F2AB-B479-4A4A-A458-A89E7DC349F1`. Modifier la **csproj** fichier dans un éditeur de texte, en remplaçant le premier élément de la `<ProjectTypeGuids>` élément, comme indiqué :

![](updating-mac-apps-images/csproj.png "Modifiez le fichier csproj dans un éditeur de texte, en remplaçant le premier élément dans l’élément ProjectTypeGuids comme")

Modifier la **importation** élément contenant `Xamarin.Mac.targets` à `Xamarin.Mac.CSharp.targets` comme indiqué :

![](updating-mac-apps-images/csproj2.png "Modifier l’élément d’importation contenant Xamarin.Mac.targets à Xamarin.Mac.CSharp.targets comme")

Ajoutez les lignes suivantes du code après le `<AssemblyName>` élément :

```xml
<TargetFrameworkVersion>v2.0</TargetFrameworkVersion>
<TargetFrameworkIdentifier>Xamarin.Mac</TargetFrameworkIdentifier>

```

Exemple :

![](updating-mac-apps-images/csproj3.png "Ajoutez ces lignes de code après l’élément < nom_assembly >")

### <a name="2-update-project-references"></a>2. Mettre à jour les références de projet

Développez le projet application Mac **références** nœud. Elle affiche initialement un * interrompue - **XamMac** référence similaire à cette capture d’écran (étant donné que nous venons de modifier le type de projet) :

![](updating-mac-apps-images/references.png "Elle affichent initialement une référence XamMac rompu similaire à cette capture d’écran")

Cliquez sur le **icône d’engrenage** en regard de la **XamMac** entrée et sélectionnez **supprimer** pour supprimer la référence incorrecte.

Ensuite, avec le bouton droit sur le **références** dossier dans le **l’Explorateur de solutions** et sélectionnez **modifier les références**. Faites défiler vers le bas de la liste de références, puis cochez outre **Xamarin.Mac**.

![](updating-mac-apps-images/references2.png "Faites défiler vers le bas de la liste de références, puis cochez outre Xamarin.Mac")

Appuyez sur **OK** pour enregistrer des modifications de références de projet.

### <a name="3-remove-monomac-from-namespaces"></a>3. Supprimer les MonoMac d’espaces de noms

Supprimer le **MonoMac** préfixe à partir d’espaces de noms dans `using` instructions ou partout où un nom de classe a été entièrement qualifié (par exemple). `MonoMac.AppKit` devient simplement `AppKit`).

### <a name="4-remap-types"></a>4. Remapper les Types

[Les types natifs](~/cross-platform/macios/nativetypes.md) ont été introduites qui remplacent certains Types qui ont été précédemment utilisés, tels que les instances de `System.Drawing.RectangleF` avec `CoreGraphics.CGRect` (par exemple). Vous trouverez la liste complète des types sur le [types natifs](~/cross-platform/macios/nativetypes.md) page.

### <a name="5-fix-method-overrides"></a>5. Corrigez les substitutions de méthode

Certains `AppKit` méthodes ont eu leur signature modifiée pour utiliser le nouveau [types natifs](~/cross-platform/macios/nativetypes.md) (tel que `nint`). Si les sous-classes personnalisés substituez ces méthodes, les signatures ne correspondront plus et produiront des erreurs. Corrigez ces substitutions de méthode en modifiant la sous-classe pour correspondre à la nouvelle signature à l’aide des types natifs. 

## <a name="considerations"></a>Éléments à prendre en considération

Les considérations suivantes doivent être prises en compte lors de la conversion d’un projet Xamarin.Mac existant à partir de l’API classique à la nouvelle API unifiée si cette application s’appuie sur un ou plusieurs composants ou le NuGet Package. 

### <a name="components"></a>Composants

Tout composant que vous avez inclus dans votre application également devront être mis à jour à l’API unifiée ou que vous obtenez un conflit lorsque vous essayez de compiler. Pour les composants inclus, remplacez la version actuelle par une nouvelle version à partir du magasin de composant Xamarin qui prend en charge l’API unifiée et effectuer un nettoyage de build. Tout composant qui n’a pas encore été converti par l’auteur, affiche un 32 bits d’avertissement uniquement dans le magasin de composants.

### <a name="nuget-support"></a>Prise en charge par NuGet

Pendant que nous avons contribué modifications NuGet pour travailler avec la prise en charge de l’API unifiée, il n’a pas été une nouvelle version de NuGet, donc nous examinons l’évaluation de l’obtention de NuGet pour reconnaître les nouvelles API. 

En attendant, tout comme les composants, vous devez passer n’importe quel NuGet Package que vous avez inclus dans votre projet vers une version qui prend en charge les API unifiée et effectuer un nettoyage de build par la suite.

> [!IMPORTANT]
> Si vous avez une erreur dans le formulaire _« erreur 3 ne peut pas inclure 'monomac.dll' et 'Xamarin.Mac.dll' dans le même projet Xamarin.Mac - 'Xamarin.Mac.dll' est référencé explicitement, alors que 'monomac.dll' est référencé par ' xxx, Version = 0.0.000, Culture = neutral, PublicKeyToken = null' «_ après la conversion de votre application à l’API unifiée, il est généralement parce qu’un composant ou un NuGet Package dans le projet n’a pas été mis à jour à l’API unifiée. Vous devez supprimer le composant existant/NuGet, mettre à jour vers une version qui prend en charge les API unifiée, puis effectuez un nettoyage de build.

## <a name="enabling-64-bit-builds-of-xamarinmac-apps"></a>L’activation de 64 bits génère des applications de Xamarin.Mac

Pour une application mobile Xamarin.Mac qui a été convertie à l’API unifiée, le développeur doit toujours autorisent la création de l’application pour les ordinateurs 64 bits à partir des Options de l’application. Consultez le **activation 64 génère de Xamarin.Mac applications bits** de la [considérations relatives à la plate-forme 32/64 bits](~/cross-platform/macios/32-and-64/index.md) génère des documents pour obtenir des instructions détaillées sur l’activation de 64 bits.
    
## <a name="finishing-up"></a>Fin de la

Vous choisissez d’utiliser la méthode automatique ou manuelle pour convertir les API unifiée de votre application Xamarin.Mac à partir de la classique, ou non, il existe plusieurs instances qui requièrent d’autres, une intervention manuelle. Veuillez consulter notre [conseils pour le Code de mise à jour à l’API unifiée](~/cross-platform/macios/unified/updating-tips.md) de document pour les problèmes connus et des solutions de contournement.

## <a name="related-links"></a>Liens associés

- [Conseils pour la mise à jour du code vers l’API unifiée](~/cross-platform/macios/unified/updating-tips.md)
- [Utilisation de types natifs dans des applications multiplateformes](~/cross-platform/macios/native-types-cross-platform.md)
- [Différences d’API unifiée de vs classiques](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
