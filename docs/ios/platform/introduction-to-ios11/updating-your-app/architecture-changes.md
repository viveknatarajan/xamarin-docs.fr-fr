---
title: Modifications de l’architecture
description: Explorer les nouvelles fonctionnalités d’iOS 11
ms.prod: xamarin
ms.assetid: 55F62F3F-8570-402B-B7D9-2875F76CB946
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/13/2016
ms.openlocfilehash: 6dd874fd803d2b3de21b0b604cbc2bafa5bf1aec
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="architecture-changes"></a>Modifications de l’architecture

_Explorer les nouvelles fonctionnalités d’iOS 11_

Un des changements plus importants que vous devez connaître avec iOS 11 est la désapprobation de prise en charge 32 bits pour les applications, comme détaillé dans [Apple](https://developer.apple.com/news/?id=06282017b) communiqué de presse. Toutes les nouvelles applications et mises à jour des applications existantes doivent prendre en charge 64 bits. les applications 32 bits **lancera pas** dans iOS 11.

Pour mettre à jour votre application dans Visual Studio pour Mac, procédez comme suit :

1. Avec le bouton droit sur le nom du projet pour ouvrir **Options du projet**.
2. Accédez à la **Build iOS** onglet.
3. Définir le **architectures prises en charge** liste déroulante **x86_64** pour le **déboguer | iPhoneSimulator** et **version | iPhoneSimulator**:

    ![Modifier la liste déroulante des architectures simulateur](architecture-changes-images/image1.png)

4. Pour les appareils iOS, passez à la configuration **déboguer | iPhone** et définir le **prise en charge des architectures** liste déroulante **ARM64**:

    ![Modifier la liste déroulante des architectures de périphérique](architecture-changes-images/image2.png)

5. Passez à la configuration **version | iPhone** et définir le **architectures prises en charge** liste déroulante **ARM64**.

Pour plus d’informations sur les architectures 32 bits et 64 bits, consultez le [considérations relatives à la plate-forme 32/64 bits](~/cross-platform/macios/32-and-64/index.md#ios) guide.

## <a name="related-links"></a>Liens associés

- [Nouveautés dans iOS 11 (Apple)](https://developer.apple.com/ios/)
- [Page du produit mis à jour App Store (Apple)](https://developer.apple.com/app-store/product-page/)
- [Mise à jour de votre application pour iOS 11 (WWDC) (vidéo)](https://developer.apple.com/videos/play/wwdc2017/204/)
