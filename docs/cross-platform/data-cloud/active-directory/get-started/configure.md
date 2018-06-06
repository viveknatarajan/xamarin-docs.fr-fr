---
title: Étape 2. Configurer l’accès au Service pour les applications mobiles
description: Ce document décrit la façon de fournir une application Xamarin avec l’accès à une application Azure sécurisée par Azure Active Directory.
ms.prod: xamarin
ms.assetid: 8A14A457-F72E-4B08-B4B6-801F7619F893
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 2a9baab9215ae2d30e4daf6800a116c95165da42
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34780098"
---
# <a name="step-2-configure-service-access-for-mobile-application"></a>Étape 2. Configurer l’accès au Service pour les applications mobiles

Chaque fois que toutes les ressources, par exemple, l’application web, un service web, un etc. doit être sécurisé par Azure Active Directory, il doit être enregistré. Tous les services ou applications sécurisées peuvent être consultés sous **Applications** onglet. Ici, vous pouvez sélectionner l’application qui doit être accessibles à partir de l’application mobile et accordez l’accès à celui-ci.

1. Sur le **configurer** onglet, recherchez **autorisations à d’autres applications** section :

  ![](configure-images/2.1-configure.png "Sous l’onglet Configurer, recherchez des autorisations pour la section d’autres applications")

2.  Cliquez sur **ajouter application** bouton. Dans l’écran suivant contextuelle, vous devez voir la liste de toutes les applications qui sont sécurisés par Azure Active Directory. Sélectionnez les applications qui doit être accessible à partir de l’application mobile.

  ![](configure-images/2.2-add-application.png "Sélectionnez les applications qui doit être accessible à partir de l’application mobile")

3. Une fois l’application, une fois encore sélectionner l’application nouvellement ajouté dans **autorisations à d’autres applications** section et de donner les droits appropriés.

  ![](configure-images/2.3-permissions.png "Après avoir sélectionné l’application, une fois encore sélectionner l’application qui vient d’être ajouté dans les autorisations de la section d’autres applications et donner les droits appropriés")

4. Enfin, **enregistrer** la configuration. Ces services doivent maintenant être disponibles dans les applications mobiles !



## <a name="related-links"></a>Liens associés

- [Exemple de Microsoft NativeClient](https://github.com/AzureADSamples/NativeClient-MultiTarget-DotNet)
