---
title: Instructions de configuration du pare-feu Xamarin
description: Liste des ordinateurs hôtes que vous devez ajouter à la liste verte de votre pare-feu pour permettre l’utilisation de la plateforme Xamarin dans votre entreprise.
ms.prod: xamarin
ms.assetid: 658f699b-8cca-48f7-ae54-fa956384b6d6
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 12/02/2016
ms.openlocfilehash: bb551b548f241cacfc4cb700d247684c15f6fcf7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="xamarin-firewall-configuration-instructions"></a>Instructions de configuration du pare-feu Xamarin

_Une liste des ordinateurs hôtes que vous devez ajouter à la liste verte de votre pare-feu pour permettre l’utilisation de la plateforme Xamarin par votre entreprise._

Pour que les produits Xamarin puissent être installés et fonctionner correctement, certains points de terminaison doivent être accessibles pour télécharger les outils et les mises à jour nécessaires. Si vos paramètres de pare-feu ou ceux de votre entreprise sont stricts, vous pouvez rencontrer des problèmes avec l’installation, la gestion des licences ou certains composants, par exemple. Ce document liste quelques points de terminaison connus qui doivent être ajoutés à la liste verte de votre pare-feu pour que Xamarin puisse fonctionner. Cette liste ne comprend pas les points de terminaison qui sont nécessaires pour les outils tiers inclus dans le téléchargement. Si vous rencontrez encore des problèmes après avoir étudié cette liste, reportez-vous aux guides de dépannage de l’installation pour les produits Apple ou Android.

## <a name="endpoints-to-whitelist"></a>Points de terminaison à ajouter à la liste verte

### <a name="xamarin-installer"></a>Programme d’installation de Xamarin

Les adresses connues suivantes doivent être ajoutées à la liste pour que les logiciels soient installés correctement à l’aide de la dernière version du programme d’installation Xamarin :

-  xamarin.com (manifestes du programme d’installation)
-  dl.xamarin.com (emplacement de téléchargement du paquet)
-  dl.google.com (pour télécharger Android SDK)
-  download.oracle.com (JDK)
-  visualstudio.com (emplacement de téléchargement des paquets d’installation)
-  go.microsoft.com (résolution de l’URL d’installation)
-  aka.ms (résolution de l’URL d’installation)

Si vous utilisez un ordinateur Mac et rencontrez des problèmes d’installation avec Xamarin.Android, vérifiez que Mac OS est en mesure de télécharger Java.


### <a name="components-store-and-nuget-including-xamarinforms"></a>Magasin de composants et NuGet (y compris Xamarin.Forms)

Les adresses suivantes doivent être ajoutées à la liste pour permettre l’accès au magasin de composants Xamarin et à NuGet (Xamarin.Forms est empaqueté sous la forme d’un NuGet) :

-  components.xamarin.com (pour utiliser le magasin de composants Xamarin)
-  xampubdl.blob.core.windows.net (héberge les téléchargements du magasin de composants)
-  www.nuget.org (pour accéder à NuGet)
-  az320820.vo.msecnd.net (téléchargements NuGet)
-  dl-ssl.google.com (composants Google)


### <a name="software-updates"></a>Mises à jour logicielles

Les adresses suivantes doivent être ajoutées à la liste pour que les mises à jour logicielles puissent être téléchargées correctement :

-  software.xamarin.com (service de mise à jour)
-  download.visualstudio.microsoft.com
-  dl.xamarin.com

## <a name="xamarin-mac-agent"></a>Mac Agent Xamarin

Pour connecter Visual Studio à un Build Host Mac avec Mac Agent Xamarin, vous devez ouvrir le port SSH. Par défaut, il s’agit du **port 22**.

## <a name="summary"></a>Récapitulatif

Ce guide a couvert les points de terminaison qui doivent être ajoutés à la liste verte pour que les produits Xamarin puissent être installés et mis à jour correctement sur votre ordinateur.