---
title: Modifications de l’architecture dans iOS 11
description: Ce document décrit la désapprobation des applications 32 bits dans iOS 11. Il explique comment mettre à jour des applications pour les architectures 64 bits cible.
ms.prod: xamarin
ms.assetid: 55F62F3F-8570-402B-B7D9-2875F76CB946
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/13/2016
ms.openlocfilehash: b7d1df6ed2a8bd480025681ebcbe48acd7592564
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61169354"
---
# <a name="architecture-changes-in-ios-11"></a>Modifications de l’architecture dans iOS 11

Un des plus grands changements que vous devez connaître avec iOS 11 est la désapprobation de prise en charge 32 bits pour les applications, comme détaillé dans [Apple](https://developer.apple.com/news/?id=06282017b) communiqué de presse. Toutes les nouvelles applications et mises à jour des applications existantes doivent prendre en charge 64 bits. les applications 32 bits **ne lancera pas** dans iOS 11.

Pour mettre à jour votre application dans Visual Studio pour Mac, procédez comme suit :

1. Avec le bouton droit sur le nom du projet pour ouvrir **Options du projet**.
2. Accédez à la **Build iOS** onglet.
3. Définir le **architectures prises en charge** liste déroulante pour **x86_64** pour le **déboguer | iPhoneSimulator** et **version | iPhoneSimulator**:

    ![Modifier des architectures de simulateur liste déroulante](architecture-changes-images/image1.png)

4. Pour les appareils iOS, passez à la configuration **Debug | iPhone** et définir le **architectures prises en charge** liste déroulante pour **ARM64**:

    ![Modifier la liste déroulante les architectures](architecture-changes-images/image2.png)

5. Passez à la configuration **mise en production | iPhone** et définir le **architectures prises en charge** liste déroulante pour **ARM64**.

Pour plus d’informations sur les architectures 32 bits et 64 bits, consultez le [considérations relatives à la plateforme 32/64 bits](~/cross-platform/macios/32-and-64/index.md#ios) guide.

## <a name="related-links"></a>Liens associés

- [Quelles sont les nouveautés dans iOS 11 (Apple)](https://developer.apple.com/ios/)
- [Page du produit mis à jour App Store (Apple)](https://developer.apple.com/app-store/product-page/)
- [La mise à jour de votre application pour iOS 11 (WWDC) (vidéo)](https://developer.apple.com/videos/play/wwdc2017/204/)
