---
title: "Résolution des problèmes"
description: "Problèmes connus avec Xamarin Player Live et comment les résoudre."
ms.topic: article
ms.prod: xamarin
ms.assetid: 29A97ADA-80E0-40A1-8B26-C68FFABE7D26
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 05/17/2017
ms.openlocfilehash: d7c5bedb03d7c869be65e3c704bac58a9cdfcbbd
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="troubleshooting"></a>Résolution des problèmes

![Fonctionnalité d’aperçu](~/media/shared/preview.png)

Cet article décrit certains problèmes courants et fournit les étapes pour les corriger.


## <a name="mobile-device-does-not-connect-after-scanning-barcode-or-entering-code"></a>Appareil mobile ne se connecte pas après analyse code-barres (ou une entrée de code)

Se produit lorsque l’appareil mobile en cours d’exécution Xamarin Live Player n’est pas sur le même réseau que l’ordinateur qui exécute l’IDE. Passez en revue les éléments suivants :

- Vérifiez que l’appareil et l’ordinateur sont sur le même réseau Wi-Fi.
  - Si l’ordinateur est également connecté à un réseau câblé, essayez de le débrancher la connexion câblée.
- Le réseau peut être correctement sécurisé (par exemple, certains réseaux d’entreprise), bloquant les ports requis par le lecteur Xamarin Live.
- Fermez l’application Xamarin Player de Live et redémarrez-le.


## <a name="error-while-trying-to-deploy-message-in-ide"></a>Message « Erreur lors de la tentative de déploiement » dans l’IDE

**« IOException : Impossible de lire les données à partir de la connexion de transport : susceptibles de bloquer l’opération sur un socket non bloquant »**

Cette erreur se produite souvent lorsque le périphérique mobile en cours d’exécution Xamarin Live Player ne se trouve pas sur le même réseau que l’ordinateur qui exécute l’IDE ; Cela se produit souvent lorsque vous vous connectez à un appareil qui a été associé précédemment avec succès.

* Vérifiez que l’appareil et l’ordinateur sont sur le même réseau Wi-Fi.
* Le réseau peut être correctement sécurisé (par exemple, certains réseaux d’entreprise), bloquant les ports requis par le lecteur Xamarin Live. Les ports suivants sont requis pour le lecteur de Live Xamarin :
  * 37847 – accès réseau interne 
  * 8090 – accès au réseau externe

## <a name="type-or-namespace-cannot-be-found-message-in-ide"></a>Message de « espace de noms ou le type est introuvable » dans l’IDE

Vérifiez que vous avez sélectionné un **projet de démarrage** qui correspond à votre type d’appareil (iOS ou Android) et que la configuration correspond à ce type de périphérique (par exemple). **Déboguer | iPhone simulateur** pour iOS).

## <a name="constructor-on-type-interpretedxamarinformsbutton-not-found-message-in-player"></a>Message « Constructeur sur le type 'InterpretedXamarin.Forms.Button' non trouvé » dans le lecteur

Certaines classes système ne peut pas être substituées, par exemple :

```csharp
public class SomeCustomButton : Xamarin.Forms.Button { ... }
```

## <a name="mainactivitycs-resourcelayout-does-not-contain-a-definition-for-main"></a>« MainActivity.cs : 'Resource.Layout' ne contient pas de définition pour 'Main' »

Cette erreur se produit pour les projets Android avec des interfaces utilisateur définis dans les fichiers AXML.
Les fichiers AXML ne sont pas pris en charge actuellement dans le lecteur Xamarin Live.

### <a name="android-toolbar-and-tabs-render-incorrectly-using-xamarinforms"></a>Barre d’outils et des onglets Android restituent correctement à l’aide de Xamarin.Forms

Projets de Xamarin.Forms Android doivent utiliser « Toolbar.axml » et « Tabbar.axml » pour les noms des fichiers de mise en page pertinentes. Le modèle par défaut utilise ces noms ; renommant entraîne des problèmes de rendu.


Veuillez signaler d’autres problèmes sur [bugzilla](https://aka.ms/live-player-report-issue).


## <a name="related-links"></a>Liens associés

- [Limitations](~/tools/live-player/limitations.md)
- [Installation](~/tools/live-player/install.md)
