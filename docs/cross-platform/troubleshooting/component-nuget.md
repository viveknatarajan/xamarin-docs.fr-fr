---
title: "Mise à jour des références de composants à NuGet"
description: "Remplacez votre composant fait référence avec les packages NuGet pour durable à vos applications."
ms.topic: article
ms.prod: xamarin
ms.assetid: 9E6C986F-3FBA-4599-8367-FB0C565C0ADE
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 11/22/2017
ms.openlocfilehash: f3dbfb52d4fbcb4dd65f695a862f6b041d2b22c0
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="updating-component-references-to-nuget"></a>Mise à jour des références de composants à NuGet

_Remplacez votre composant fait référence avec les packages NuGet pour durable à vos applications._

Ce guide explique comment mettre à jour des solutions existantes de Xamarin pour modifier les références de composants dans les packages NuGet.

- [Composants qui contiennent les packages NuGet](#contain)
- [Composants avec les remplacements de NuGet](#replace)

La plupart des composants se répartissent dans les catégories ci-dessus.
Si vous utilisez un composant qui ne semble pas disposer d’un package NuGet équivalent, lire la [composants sans un chemin de migration NuGet](#require-update) section ci-dessous.

Pour plus d’instructions sur l’ajout de packages NuGet de, reportez-vous à ces pages [Windows](https://docs.microsoft.com/nuget/quickstart/use-a-package) ou [Mac](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough).

<a name="contain" />

## <a name="components-that-contain-nuget-packages"></a>Composants qui contiennent les packages NuGet

De nombreux composants contiennent déjà des packages NuGet et le chemin de migration consiste simplement à supprimer la référence du composant.

Vous pouvez déterminer si le composant inclut déjà un package NuGet en double-cliquant sur le composant dans la solution :

![Nœud de composants développé](component-nuget-images/solution-sml.png)

Le **Packages** onglet répertorie tous les packages NuGet inclus dans le composant :

![Onglet de packages contient NuGet](component-nuget-images/packages-tab-sml.png)

Notez que la **assemblys** onglet sera vide :

![Onglet assemblys est vide](component-nuget-images/assemblies-tab-empty-sml.png)

### <a name="updating-the-solution"></a>Mise à jour de la Solution

Pour mettre à jour votre solution, vous devez supprimer la **composant** entrée à partir de la solution :

![Supprimer des composants](component-nuget-images/delete-component-sml.png)

Le package NuGet restera répertorié dans le **Packages** nœud et votre application seront compiler et exécuter comme d’habitude. À l’avenir, les mises à jour de ce package seront effectuées la **Nuget** fonctionnalité de mise à jour :

![Mettre à jour le package NuGet](component-nuget-images/nuget-update-sml.png)


<a name="replace" />

## <a name="components-with-nuget-replacements"></a>Composants avec les remplacements de NuGet

Si la page des informations de composant **assemblys** onglet contient les entrées, comme indiqué ci-dessous, vous devez rechercher le package NuGet équivalent manuellement.

![Contient des assemblys](component-nuget-images/assemblies-tab-sml.png)

Notez que la **Packages** onglet sera probablement vide :

![](component-nuget-images/packages-tab-empty-sml.png)

_Il peut contenir des dépendances de NuGet, mais vous pouvez les ignorer._


Pour confirmer le remplacement NuGet package existe, effectuez une recherche sur [NuGet.org](https://www.nuget.org/packages), en utilisant le nom du composant, ou bien par l’auteur.

Par exemple, vous pouvez trouver le courant **sqlite-net-pcl** package en recherchant des :

- [`sqlite-net-pcl`](https://www.nuget.org/packages?q=sqlite-net-pcl) – le nom du produit.
- [`praeclarum`](https://www.nuget.org/packages?q=praeclarum) – le profil de l’auteur.


### <a name="updating-the-solution"></a>Mise à jour de la Solution

Une fois que vous avez vérifié que le composant est disponible dans NuGet, procédez comme suit :

#### <a name="delete-the-component"></a>Supprimer le composant.

Cliquez avec le bouton droit sur le composant dans la solution et choisissez **supprimer**:

![Supprimer des composants](component-nuget-images/remove-component-sml.png)

Cette opération supprimera le composant et toutes les références. Cela arrêtera votre build, sauf si vous ajoutez le package NuGet équivalent pour le remplacer.

#### <a name="add-the-nuget-package"></a>Ajoutez le package NuGet

1. Avec le bouton droit sur le **Packages** nœud et choisissez **ajouter des Packages en cours...** .
2. Recherchez le remplacement de NuGet par nom ou l’auteur :

  ![](component-nuget-images/nuget-search-sml.png)

3. Appuyez sur **ajouter Package**.

Le package NuGet est ajouté à votre projet, ainsi que toutes les dépendances.
Cela devrait résoudre la build. Si la génération continue à échouer, examinez chaque erreur pour voir si les différences d’API entre le composant et le package NuGet.

<a name="require-update" />

## <a name="components-without-a-nuget-migration-path"></a>Composants sans un chemin de migration de NuGet

Ne doit pas si vous ne trouvez pas immédiatement un remplacement pour les composants utilisés dans votre application. Les composants existants continueront de fonctionner dans Visual Studio 15.5 et le **composants** nœud s’affiche dans votre solution, comme d’habitude.

Les versions ultérieures de Visual Studio, toutefois, seront _pas_ restaurer ou mettre à jour des composants.
Cela signifie que si vous ouvrez la solution sur un nouvel ordinateur, le composant ne sera pas téléchargé et installé ; et l’auteur ne seront pas en mesure de vous fournir des mises à jour. Vous devez planifier :

* Extraire les assemblys à partir du composant et de les référencer directement dans votre projet.
* Contactez l’auteur du composant et demandez de migrer vers NuGet.
* Examinez les packages NuGet autre ou rechercher le code source si le composant est open source.

De nombreux fournisseurs de composants fonctionnent toujours sur la migration vers NuGet, et d’autres utilisateurs (y compris les produits disponibles dans le commerce) peuvent être enquête sur les options de livraison de remplacement.


## <a name="related-links"></a>Liens associés

- [Installer et utiliser un package NuGet (Windows)](https://docs.microsoft.com/nuget/quickstart/use-a-package)
- [Y compris un package NuGet (Mac)](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)
