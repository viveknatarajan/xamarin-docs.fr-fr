---
title: Azure Active Directory
description: Ce document décrit les étapes qui doivent être remplies pour permettre à une application mobile pour s’authentifier auprès d’Azure Active Directory.
ms.prod: xamarin
ms.assetid: 70B3C2AB-CB4D-420C-9CFA-20CCFA0E3C78
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: ca33422817f19dbb0a04e8870800d3f5efa8af2a
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34780062"
---
# <a name="azure-active-directory"></a>Azure Active Directory

_Inscrire une application pour utiliser Azure Active Directory_

Azure Active Directory permet aux développeurs de sécuriser les ressources telles que des fichiers, les liens et les API Web utilisant le même compte d’organisation par les employés pour se connecter à leurs systèmes ou de vérifier leurs messages électroniques.

Développement d’applications mobiles qui peuvent s’authentifier auprès d’Azure Active Directory implique trois étapes.
Les deux premières étapes sont généralement les mêmes, quel que soit les services que vous envisagez d’utiliser. La troisième étape est différente pour chaque type de service :

  1. [L’inscription auprès d’Azure Active Directory](~/cross-platform/data-cloud/active-directory/get-started/register.md) sur la *windowsazure.com* portal, puis
  2. [Configurer les services](~/cross-platform/data-cloud/active-directory/get-started/configure.md).
  3. Développer des applications mobiles à l’aide des services.

Voici quelques exemples de services différents, vous pouvez accéder à :

- [API Graph](~/cross-platform/data-cloud/active-directory/graph.md)
- API web
- Office 365


## <a name="conclusion"></a>Conclusion

À l’aide de la procédure ci-dessus, vous pouvez authentifier vos applications mobiles dans Azure Active Directory. La bibliothèque d’authentification Active Directory (ADAL) rend plus facile avec moins de lignes de code, tout en conservant la plupart du code identiques et ce qui rend partageable entre les plateformes.



## <a name="related-links"></a>Liens associés

- [Exemple de Microsoft NativeClient](https://github.com/AzureADSamples/NativeClient-MultiTarget-DotNet)
