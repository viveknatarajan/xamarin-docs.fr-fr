---
title: Comment mettre à niveau un package NuGet ?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 2375F833-A630-471E-B8E9-5AD2CB81F264
author: asb3993
ms.author: amburns
ms.date: 05/08/2018
ms.openlocfilehash: 72fdf7246b148fa95ea312284957072ecda47121
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351214"
---
# <a name="how-do-i-downgrade-a-nuget-package"></a>Comment mettre à niveau un package NuGet ?

Visual Studio pour Mac et Visual Studio ont des fonctionnalités pour sélectionner des versions antérieures des packages et les installer automatiquement ; semblable à la façon dont la mise à jour des packages fonctionne. Ces étapes sont décrites ci-dessous.

## <a name="visual-studio"></a>Visual Studio
1. Accédez à **Outils > Gestionnaire de Package NuGet > Console du Gestionnaire de Package**
2. Définissez le projet sous **projet par défaut**
3. Utilisez la syntaxe suivante :

    > Install-Package [PackageName]-Version [tab pour le menu de la version]

Vous pouvez également copier/coller la commande exacte à partir de la page du package NuGet. Exemple pour Xamarin.Forms : [https://www.nuget.org/packages/Xamarin.Forms/](https://www.nuget.org/packages/Xamarin.Forms/)

## <a name="visual-studio-for-mac"></a>Visual Studio pour Mac
1. Dans votre projet, cliquez sur le dossier de packages et sélectionnez **ajouter des Packages**
2. Dans le searchbar, vous pouvez utiliser la syntaxe suivante pour rechercher vos packages requis :

    `[PackageName] version:*`

### <a name="examples"></a>Exemples 
- Répertorie tous les packages de Xamarin.Forms : 

    `Xamarin.Forms version:`
- Répertorie tous les packages de 1.4.x Xamarin.Forms : 

    `Xamarin.Forms version:1.4`

*Remarque : Si vous ajoutez un espace entre `version:` & le numéro de version, la recherche se comportera comme si aucune version n’a été spécifiée.*

