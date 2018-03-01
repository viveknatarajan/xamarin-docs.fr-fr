---
title: "Comment mettre à niveau un package NuGet ?"
ms.topic: article
ms.prod: xamarin
ms.assetid: 2375F833-A630-471E-B8E9-5AD2CB81F264
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: 98d12bd93c50690909ac902a6f2498bcdb94960f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="how-do-i-downgrade-a-nuget-package"></a>Comment mettre à niveau un package NuGet ?

Visual Studio pour Mac et Visual Studio ont des fonctionnalités pour sélectionner les versions antérieures des packages et de les installer automatiquement ; similaire à la mise à jour des packages fonctionne. Ces étapes sont décrites ci-dessous.

## <a name="visual-studio"></a>Visual Studio
1. Accédez à **Outils > Gestionnaire de Package NuGet > Console du Gestionnaire de Package**
2. Définissez le projet sous **projet par défaut**
3. Utilisez la syntaxe suivante :

    > Package d’installation [PackageName]-Version [onglet de menu de la version]

Vous pouvez également copier/coller la commande exacte à partir de la page de NuGet du package. Exemple de Xamarin.Forms : [https://www.nuget.org/packages/Xamarin.Forms/](https://www.nuget.org/packages/Xamarin.Forms/)

## <a name="visual-studio-for-mac"></a>Visual Studio pour Mac
1. Dans votre projet, cliquez sur le dossier de packages et sélectionnez **ajouter des Packages**
2. Dans la searchbar, vous pouvez utiliser la syntaxe suivante pour rechercher vos packages requis :

    `[PackageName] version:*`

### <a name="examples"></a>Exemples 
- Répertorie tous les packages de Xamarin.Forms : 

    `Xamarin.Forms version:`
- Répertorie tous les packages de 1.4.x Xamarin.Forms : 


    `Xamarin.Forms version:1.4`

*Remarque : Si vous ajoutez un espace entre `version:` et le numéro de version, la recherche se comporteront comme si aucune version n’a été spécifiée.*

