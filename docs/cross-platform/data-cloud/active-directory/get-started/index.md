---
title: Azure Active Directory
description: Inscrire une application pour utiliser Azure Active Directory
ms.prod: xamarin
ms.assetid: 70B3C2AB-CB4D-420C-9CFA-20CCFA0E3C78
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: ff076aa894f7f406c79d2a19b6055d2feeb99bd0
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
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
- Office365


## <a name="conclusion"></a>Conclusion

À l’aide de la procédure ci-dessus, vous pouvez authentifier vos applications mobiles dans Azure Active Directory. La bibliothèque d’authentification Active Directory (ADAL) rend plus facile avec moins de lignes de code, tout en conservant la plupart du code identiques et ce qui rend partageable entre les plateformes.



## <a name="related-links"></a>Liens associés

- [Exemple de Microsoft NativeClient](https://github.com/AzureADSamples/NativeClient-MultiTarget-DotNet)
