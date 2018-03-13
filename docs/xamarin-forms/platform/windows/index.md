---
title: "Fonctionnalités de la plateforme Windows"
ms.topic: article
ms.prod: xamarin
ms.assetid: F6EA9E49-FB3E-442F-AF13-B7AD0C80D11F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/20/2017
ms.openlocfilehash: f77290a8c780d7dd5c936af576b39228d91687aa
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="windows-platform-features"></a>Fonctionnalités de la plateforme Windows

Développement d’applications de Xamarin.Forms pour les plateformes Windows requiert Visual Studio. Le [page spécifications](~/xamarin-forms/get-started/installation.md) contient plus d’informations sur les conditions préalables.

![](images/allhanselman.png "Xamarin.Forms les Applications s’exécutant sous Windows")

## <a name="platform-support"></a>Prise en charge de plateforme

Les modèles Xamarin.Forms disponibles dans Visual Studio contiennent un projet Windows par défaut :

* **Applications de la plateforme Windows universelle** -Xamarin.Forms applications peuvent également être optimisées pour Windows 10. Les applications universelle (UWP) peuvent exécuter sur le téléphone, tablette et appareils de bureau.

Si vous avez installé les options correctes de développement dans Visual Studio, il est également possible de [ajouter](installation/index.md) ces types pour prendre en charge des versions antérieures de Windows de projets :

* **Windows 8.1** : vous pouvez déployer des applications Xamarin.Forms tablettes et des facteurs de forme bureau comme une application Windows 8.1 de projet à l’aide de contrôles de WinRT.
* **Windows Phone 8.1** -Xamarin.Forms a une prise en charge complète pour la plateforme Windows Phone 8.1 à l’aide de WinRT. L’apparence des applications à l’aide de la prise en charge Windows Phone 8.1 peuvent être différente pour vos applications Windows Phone de Xamarin.Forms antérieures qui étaient basées sur Silverlight.


> [!NOTE]
> Prise en charge 1.x et 2.x Xamarin.Forms _Windows Phone 8 Silverlight_ développement d’applications, toutefois, ce type de projet a été déconseillé.


## <a name="getting-started"></a>Prise en main

Accédez à **fichier > Nouveau > projet** dans Visual Studio et choisissez une de la **Cross-Platform > application vide (Xamarin.Forms)** modèles de prise en main.

Solutions Xamarin.Forms plus anciennes, ou celles créées sur macOS, n’aura pas tous les projets Windows répertoriés ci-dessus (mais ils doivent être ajoutés manuellement).
Si la plateforme Windows que vous souhaitez cibler n’est pas déjà dans votre solution, visitez le [instructions d’installation](installation/index.md) pour ajouter les fenêtres souhaitées projet type/s.


## <a name="samples"></a>Exemples

[Tous les exemples](https://github.com/xamarin/xamarin-forms-book-preview-2) livre de Petzold [ *créer des applications mobiles avec Xamarin.Forms* ](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md) incluent Windows Phone 8.1, Windows 8.1 et projets de plateforme Windows universelle (pour Windows 10).

Le [« Scott Hanselman » application de démonstration](https://github.com/jamesmontemagno/Hanselman.Forms) sont disponibles séparément et inclut également des projets Apple Watch et usure Android (à l’aide de Xamarin.iOS et Xamarin.Android respectivement, Xamarin.Forms ne s’exécute pas sur ces plateformes).


## <a name="related-links"></a>Liens associés

- [Projets d’installation Windows](~/xamarin-forms/platform/windows/installation/index.md)
