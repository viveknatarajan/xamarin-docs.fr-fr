---
title: "Mise à jour des applications iOS existante"
description: "Suivez ces étapes pour mettre à jour d’une application Xamarin.iOS existante pour utiliser l’API unifiée."
ms.topic: article
ms.prod: xamarin
ms.assetid: 303C36A8-CBF4-48C0-9412-387E95024CAB
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 3114b2abdc96bc7d122ca0b86e1c53977f6df1be
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="updating-existing-ios-apps"></a>Mise à jour des applications iOS existante

_Suivez ces étapes pour mettre à jour d’une application Xamarin.iOS existante pour utiliser l’API unifiée._

Mise à jour d’une application existante pour utiliser l’API unifiée nécessite des modifications au fichier projet lui-même, ainsi que pour les espaces de noms et les API utilisées dans le code d’application.

## <a name="the-road-to-64-bits"></a>Déplacement à 64 Bits

Les nouvelles API unifiée sont requis pour prendre en charge les architectures 64 bits à partir d’une application mobile Xamarin.iOS. À partir du 1er février 2015 Apple nécessite que tous les envois d’application nouvelle à l’iTunes App Store prend en charge les architectures 64 bits.

Xamarin fournit des outils pour Visual Studio pour Mac et Visual Studio automatiser le processus de migration de l’API classique à l’API unifiée, ou vous pouvez convertir les fichiers projet manuellement. Alors que les outils automatique d’à l’aide est fortement recommandé, cet article couvre les deux méthodes.

### <a name="before-you-start"></a>Avant de commencer...

Avant de mettre à jour de votre code existant à l’API unifiée, il est vivement recommandé d’éliminer toutes *avertissements de compilation*. Nombreux *avertissements* dans l’API classique deviendra erreurs une fois que vous migrez vers unifié. Il est plus facile de les corriger avant de commencer, car les messages du compilateur à partir de l’API classique fournissent souvent des indications sur les éléments à mettre à jour.

## <a name="automated-updating"></a>Automatisée de mise à jour

Une fois que les avertissements ont été résolus, sélectionnez un projet iOS dans Visual Studio pour Mac ou Visual Studio et choisissez **migrer vers une API unifiée de Xamarin.iOS** à partir de la **projet** menu. Exemple :

![](updating-ios-apps-images/beta-tool1.png "Choisir de migrer d’API unifiée de Xamarin.iOS dans le menu projet")

Vous devez accepter cet avertissement avant de pouvoir exécute la migration automatisée (évidemment, vous devez vous assurer vous contrôlez les sauvegardes/source avant de commencer cette adventure) :

![](updating-ios-apps-images/beta-tool2.png "Acceptez cet avertissement avant de pouvoir exécute la migration automatisée")

L’outil automatise fondamentalement toutes les étapes décrites dans le **mise à jour manuellement** section présentés ci-dessous et est la méthode recommandée pour convertir un projet existant de Xamarin.iOS à l’API unifiée.

## <a name="steps-to-update-manually"></a>Étapes pour mettre à jour manuellement

Là encore, une fois que les avertissements ont été résolus, procédez comme suit pour mettre à jour manuellement les applications Xamarin.iOS pour utiliser la nouvelle API unifiée :

### <a name="1-update-project-type--build-target"></a>1. Type de projet de mise à jour et la cible de génération

Modifier la version de projet dans votre **csproj** fichiers à partir de `6BC8ED88-2882-458C-8E55-DFD12B67127B` à `FEACFBD2-3405-455C-9665-78FE426C6842`. Modifier la **csproj** fichier dans un éditeur de texte, en remplaçant le premier élément de la `<ProjectTypeGuids>` élément, comme indiqué :

![](updating-ios-apps-images/csproj.png "Modifiez le fichier csproj dans un éditeur de texte, en remplaçant le premier élément dans l’élément ProjectTypeGuids comme")

Modifier la **importation** élément contenant `Xamarin.MonoTouch.CSharp.targets` à `Xamarin.iOS.CSharp.targets` comme indiqué :

![](updating-ios-apps-images/csproj2.png "Modifier l’élément d’importation contenant Xamarin.MonoTouch.CSharp.targets à Xamarin.iOS.CSharp.targets comme")

### <a name="2-update-project-references"></a>2. Mettre à jour les références de projet

Développez le projet application iOS **références** nœud. Elle affiche initialement un * interrompue - **monotouch** référence similaire à cette capture d’écran (étant donné que nous venons de modifier le type de projet) :

![](updating-ios-apps-images/references.png "Elle affichent initialement une référence monotouch rompu similaire à cette capture d’écran, car le type de projet est modifié")

Avec le bouton droit sur le projet d’application iOS à **modifier les références**, puis cliquez sur le **monotouch** référencer et supprimez-le à l’aide du bouton « X » rouge.

![](updating-ios-apps-images/references-delete-monotouch-sml.png "Avec le bouton droit sur le projet d’application iOS pour modifier les références, puis cliquez sur la référence monotouch et supprimez-le à l’aide de la croix rouge bouton X")

Maintenant accédez à la fin de la liste des références et des graduations le **Xamarin.iOS** assembly.

![](updating-ios-apps-images/references-add-xamarinios-sml.png "Accédez maintenant à la fin de la liste de références et les graduations de l’assembly Xamarin.iOS")

Appuyez sur **OK** pour enregistrer des modifications de références de projet.

### <a name="3-remove-monotouch-from-namespaces"></a>3. Supprimer les MonoTouch d’espaces de noms

Supprimer le **MonoTouch** préfixe à partir d’espaces de noms dans `using` instructions ou partout où un nom de classe a été entièrement qualifié (par exemple). `MonoTouch.UIKit` devient simplement `UIKit`).

### <a name="4-remap-types"></a>4. Remapper les Types

[Les types natifs](~/cross-platform/macios/nativetypes.md) ont été introduites qui remplacent certains Types qui ont été précédemment utilisés, tels que les instances de `System.Drawing.RectangleF` avec `CoreGraphics.CGRect` (par exemple). Vous trouverez la liste complète des types sur le [types natifs](~/cross-platform/macios/nativetypes.md) page.

### <a name="5-fix-method-overrides"></a>5. Corrigez les substitutions de méthode

Certains `UIKit` méthodes ont eu leur signature modifiée pour utiliser le nouveau [types natifs](~/cross-platform/macios/nativetypes.md) (tel que `nint`). Si les sous-classes personnalisés substituez ces méthodes, les signatures ne correspondront plus et produiront des erreurs. Corrigez ces substitutions de méthode en modifiant la sous-classe pour correspondre à la nouvelle signature à l’aide des types natifs.

Exemples sont modifier `public override int NumberOfSections (UITableView tableView)` pour retourner `nint` et en modifiant les type de retour et les types de paramètre dans `public override int RowsInSection (UITableView tableView, int section)` à `nint`.

## <a name="considerations"></a>Éléments à prendre en considération

Les considérations suivantes doivent être prises en compte lors de la conversion d’un projet existant de Xamarin.iOS à partir de l’API classique à la nouvelle API unifiée si cette application s’appuie sur un ou plusieurs composants ou le NuGet Package.

### <a name="components"></a>Composants

Tout composant que vous avez inclus dans votre application également devront être mis à jour à l’API unifiée ou que vous obtenez un conflit lorsque vous essayez de compiler. Pour les composants inclus, remplacez la version actuelle par une nouvelle version à partir du magasin de composant Xamarin qui prend en charge l’API unifiée et effectuer un nettoyage de build. Tout composant qui n’a pas encore été converti par l’auteur, affiche un 32 bits d’avertissement uniquement dans le magasin de composants.

### <a name="nuget-support"></a>Prise en charge par NuGet

Pendant que nous avons contribué modifications NuGet pour travailler avec la prise en charge de l’API unifiée, il n’a pas été une nouvelle version de NuGet, donc nous examinons l’évaluation de l’obtention de NuGet pour reconnaître les nouvelles API.

En attendant, tout comme les composants, vous devez passer n’importe quel NuGet Package que vous avez inclus dans votre projet vers une version qui prend en charge les API unifiée et effectuer un nettoyage de build par la suite.

> [!IMPORTANT]
> **Remarque :** si vous avez une erreur dans le formulaire _« erreur 3 ne peut pas inclure 'monotouch.dll' et 'Xamarin.iOS.dll' dans le même projet Xamarin.iOS - 'Xamarin.iOS.dll' est référencé explicitement, alors que 'monotouch.dll' est référencé par ' xxx, Version = 0.0.000, Culture = neutral, PublicKeyToken = null' «_ après la conversion de votre application à l’API unifiée, il est généralement parce qu’un composant ou un NuGet Package dans le projet n’a pas été mis à jour à l’API unifiée. Vous devez supprimer le composant existant/NuGet, mettre à jour vers une version qui prend en charge les API unifiée, puis effectuez un nettoyage de build.

## <a name="enabling-64-bit-builds-of-xamarinios-apps"></a>L’activation de 64 bits génère des applications de Xamarin.iOS

Pour une application mobile Xamarin.iOS qui a été convertie à l’API unifiée, le développeur doit toujours autorisent la création de l’application pour les ordinateurs 64 bits à partir des Options de l’application. Consultez le **activation 64 génère de Xamarin.iOS applications bits** de la [considérations relatives à la plate-forme 32/64 bits](~/cross-platform/macios/32-and-64/index.md#enable-64) génère des documents pour obtenir des instructions détaillées sur l’activation de 64 bits.

## <a name="finishing-up"></a>Fin de la

Vous choisissez d’utiliser la méthode automatique ou manuelle pour convertir les API unifiée de votre application Xamarin.iOS à partir de la classique, ou non, il existe plusieurs instances qui requièrent d’autres, une intervention manuelle. Veuillez consulter notre [conseils pour le Code de mise à jour à l’API unifiée](~/cross-platform/macios/unified/updating-tips.md) de document pour les problèmes connus et des solutions de contournement.

## <a name="related-links"></a>Liens associés

- [Conseils pour la mise à jour du code vers l’API unifiée](~/cross-platform/macios/unified/updating-tips.md)
- [Utilisation de types natifs dans des applications multiplateformes](~/cross-platform/macios/native-types-cross-platform.md)
- [Différences d’API unifiée de vs classiques](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
- [Migration vers l’API unifiée (vidéo)](http://university.xamarin.com/lightninglectures/migrating-to-the-unified-api)
