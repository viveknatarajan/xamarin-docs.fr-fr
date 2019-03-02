---
title: Instructions de configuration du pare-feu Xamarin
description: Ce document fournit une liste d’hôtes qui doivent figurer dans la liste verte dans votre pare-feu pour permettre le fonctionnement de Xamarin dans un environnement d’entreprise.
ms.prod: xamarin
ms.assetid: 658f699b-8cca-48f7-ae54-fa956384b6d6
author: asb3993
ms.author: amburns
ms.date: 10/05/2018
ms.openlocfilehash: 68689ce7d92a038d0724e1441f68fddcb1d0bba8
ms.sourcegitcommit: d62732ce6f3f9d8dc929d72d4acac3e592cba073
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57199655"
---
# <a name="xamarin-firewall-configuration-instructions"></a>Instructions de configuration du pare-feu Xamarin

_Une liste des ordinateurs hôtes que vous devez ajouter à la liste verte de votre pare-feu pour permettre l’utilisation de la plateforme Xamarin par votre entreprise._

Pour que les produits Xamarin puissent être installés et fonctionner correctement, certains points de terminaison doivent être accessibles pour télécharger les outils et les mises à jour nécessaires. Si vos paramètres de pare-feu ou ceux de votre entreprise sont stricts, vous pouvez rencontrer des problèmes avec l’installation, la gestion des licences ou certains composants, par exemple. Ce document liste quelques points de terminaison connus qui doivent être ajoutés à la liste verte de votre pare-feu pour que Xamarin puisse fonctionner. Cette liste ne comprend pas les points de terminaison qui sont nécessaires pour les outils tiers inclus dans le téléchargement. Si vous rencontrez encore des problèmes après avoir étudié cette liste, reportez-vous aux guides de dépannage de l’installation pour les produits Apple ou Android.

## <a name="endpoints-to-whitelist"></a>Points de terminaison à ajouter à la liste verte

### <a name="xamarin-installer"></a>Programme d’installation de Xamarin

Les adresses connues suivantes doivent être ajoutées à la liste pour que les logiciels soient installés correctement à l’aide de la dernière version du programme d’installation Xamarin :

- xamarin.com (manifestes du programme d’installation)
- dl.xamarin.com (emplacement de téléchargement du paquet)
- dl.google.com (pour télécharger Android SDK)
- download.oracle.com (JDK)
- visualstudio.com (emplacement de téléchargement des paquets d’installation)
- go.microsoft.com (résolution de l’URL d’installation)
- aka.ms (résolution de l’URL d’installation)

Si vous utilisez un ordinateur Mac et rencontrez des problèmes d’installation avec Xamarin.Android, vérifiez que Mac OS est en mesure de télécharger Java.

### <a name="nuget-including-xamarinforms"></a>NuGet (notamment Xamarin.Forms)

Les adresses suivantes doivent être ajoutées pour permettre l’accès à NuGet (Xamarin.Forms est empaqueté au format NuGet) :

- www\.nuget.org (pour accéder à NuGet)
- az320820.vo.msecnd.net (téléchargements NuGet)
- dl-ssl.google.com (composants Google pour Android et Xamarin.Forms)

### <a name="software-updates"></a>Mises à jour logicielles

Les adresses suivantes doivent être ajoutées à la liste pour que les mises à jour logicielles puissent être téléchargées correctement :

- software.xamarin.com (service de mise à jour)
- download.visualstudio.microsoft.com
- dl.xamarin.com

## <a name="xamarin-mac-agent"></a>Mac Agent Xamarin

Pour connecter Visual Studio à un Build Host Mac avec Mac Agent Xamarin, vous devez ouvrir le port SSH. Par défaut, il s’agit du **port 22**.

## <a name="summary"></a>Récapitulatif

Ce guide a couvert les points de terminaison qui doivent être ajoutés à la liste verte pour que les produits Xamarin puissent être installés et mis à jour correctement sur votre ordinateur.
