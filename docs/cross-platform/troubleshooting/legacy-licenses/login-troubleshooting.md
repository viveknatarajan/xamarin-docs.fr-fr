---
title: Pourquoi ne puis-je pas consigner dans Xamarin dans Visual Studio ou Visual Studio pour Mac ?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 6EF2B553-5DF9-41CC-B68F-77A7F64572FA
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: cb750a7c282ecab6e2193bb554e470086868e018
ms.sourcegitcommit: 6f7033a598407b3e77914a85a3f650544a4b6339
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/07/2018
---
# <a name="why-cant-i-log-into-xamarin-in-visual-studio-or-visual-studio-for-mac"></a>Pourquoi ne puis-je pas consigner dans Xamarin dans Visual Studio ou Visual Studio pour Mac ?

> [!IMPORTANT]
> Ce guide ne s’applique pas à la plupart des utilisateurs MSDN car ils ne sont pas requis ou se connecter à des comptes de Xamarin, sauf si à l’aide de la [stocker les composants Xamarin](https://components.xamarin.com/) ou [Visual Studio pour Mac (Mac)](~/cross-platform/get-started/requirements.md). Titulaires de licence MSDN doivent faire référence à ce [guide des Options de licences](~/cross-platform/get-started/requirements.md) à la place.



## <a name="overview"></a>Vue d'ensemble
Il existe quelques causes courantes qui peuvent vous empêcher de se connecter à votre compte Xamarin dans l’IDE. Vous trouverez ci-dessous la liste des correctifs et les problèmes connus.

### <a name="finding-the-login-screen"></a>Recherche de l’écran de connexion

Pour référence, les écrans d’ouverture de session sont trouvent ici :

- Visual Studio pour Mac
   - Angle supérieur droit de l’écran d’accueil
   - **Visual Studio pour Mac > compte** (Mac)
   - **Outils > compte** (Windows)
- Visual Studio
   - **Outils > compte Xamarin**

## <a name="the-ide-is-connecting-but-the-account-screen-isnt-showing-correct-login-information"></a>La connexion de l’IDE, mais l’écran de compte ne s’affiche pas les informations de connexion correct

Ce problème est généralement résolu en une resynchronisation manuelle de licence.
Suivez les instructions dans cet article : [comment faire manuellement des licences Xamarin Resynchronisez ?](~/cross-platform/troubleshooting/legacy-licenses/resync-licenses.md)

## <a name="invalid-account-information"></a>Informations de compte non valide

Si vous accédez au site Web de Xamarin [page de connexion](https://store.xamarin.com/Login?from=%2faccount%2f), vous pouvez essayer de journalisation avec vos informations d’identification de compte en cours.
La page contient également des liens pour réinitialiser votre mot de passe et pour créer un nouveau compte.

## <a name="account-is-valid-but-the-ide-cant-connect"></a>Le compte est valide, mais l’IDE ne peut pas se connecter.

Il s’agit généralement lorsque le pare-feu ou autres paramètres de sécurité bloquent l’IDE à partir de l’accès à des points de terminaison nécessaires.
Les serveurs d’activation doivent accéder aux données suivantes :

> activation.xamarin.com store.xamarin.com auth.xamarin.com

Toutefois, plusieurs autres points de terminaison sont importants pour les processus de développement général, tels que les mises à jour, l’obtention de packages NuGet, etc. Pour cette raison, il est recommandé de s’assurer que *tous les* des points de terminaison sont ajoutés à partir de la [guide de configuration de pare-feu Xamarin](~/cross-platform/get-started/installation/firewall.md).

### <a name="ios-in-xamarin-studio-windows"></a>iOS dans Xamarin Studio Windows
développement iOS n’est pas prise en charge de Xamarin Studio pour Windows. Lorsque vous accédez à l’écran de connexion, la licence iOS ne s’affichera pas.

Au lieu de cela, la connexion via Xamarin Studio (Mac) ou Visual Studio avec une licence héritée. Notez que les utilisateurs MSDN sur Windows n’avez pas besoin de se connecter.

## <a name="additional-support"></a>Prise en charge supplémentaire

Si les scénarios ci-dessus ne pas décrire votre situation ou corrigez le problème, reportez-vous à ces [prennent en charge des options](https://www.xamarin.com/support).
